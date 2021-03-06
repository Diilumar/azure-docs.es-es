---
title: Creación y carga de un VHD de Red Hat Enterprise Linux para su uso en Azure Stack | Microsoft Docs
description: Aprenda a crear y cargar un disco duro virtual (VHD) de Azure que contiene un sistema operativo Red Hat Linux.
services: azure-stack
documentationcenter: ''
author: JeffGoldner
manager: BradleyB
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: jeffgo
ms.lastreviewed: 08/15/2018
ms.openlocfilehash: ad0419cee3fc5c838d6d81adf9040432b9feaf07
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242236"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Preparación de una máquina virtual basada en Red Hat para Azure Stack

En este artículo, aprenderá a preparar una máquina virtual de Red Hat Enterprise Linux (RHEL) para usarla en Azure Stack. Las versiones de RHEL que se tratan en este artículo son 7.1+. Los hipervisores de preparación que se tratan en este artículo son Hyper-V, máquina virtual basada en kernel (KVM) y VMware.

Para obtener información de soporte técnico de Red Hat Enterprise Linux, consulte [Red Hat and Azure Stack: Frequently Asked Questions](https://access.redhat.com/articles/3413531) (Red Hat y Azure Stack: preguntas más frecuentes).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Preparación de una máquina virtual basada en Red Hat desde el Administrador de Hyper-V

En esta sección, se supone que ya tiene un archivo ISO en el sitio web de Red Hat y que instaló la imagen RHEL en un disco duro virtual (VHD). Para más información sobre cómo usar el Administrador de Hyper-V para instalar una imagen de sistema operativo, consulte el artículo sobre cómo [instalar el rol de Hyper-V y configurar una máquina virtual](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="rhel-installation-notes"></a>Notas de instalación de RHEL

* Azure Stack no admite el formato VHDX. Azure solo admite VHD fijo. Puede usar el Administrador de Hyper-V para convertir el disco al formato VHD, o puede usar el cmdlet convert-vhd. Si usa VirtualBox, seleccione **Tamaño fijo** a diferencia de la opción predeterminada asignada dinámicamente al crear el disco.
* Azure Stack solo admite máquinas virtuales de la generación 1. Puede convertir una máquina virtual de generación 1 de VHDX para el formato de archivo VHD desde un disco de expansión dinámica a otro de tamaño fijo. No puede cambiar la generación de una máquina virtual. Para más información, consulte [¿Debería crear una máquina virtual de generación 1 o 2 en Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* El tamaño máximo permitido para los discos duros virtuales es de 1023 GB.
* Al instalar el sistema operativo Linux se recomienda usar las particiones estándar en lugar de Logical Volume Manager (LVM) que a menudo viene de forma predeterminada en muchas instalaciones. Esta práctica evita los conflictos de nombres LVM con máquinas virtuales clonadas, especialmente si alguna vez necesita conectar un disco de sistema operativo a otra máquina virtual idéntica para solucionar el problema.
* Se requiere la compatibilidad de kernel para el montaje de sistemas de archivos de formato de disco universal (UDF). En el primer arranque, los medios con formato UDF conectados al invitado pasan la configuración de aprovisionamiento a la máquina virtual Linux. El agente Linux de Azure debe montar el sistema de archivos UDF para leer su configuración y aprovisionar la máquina virtual.
* No configure una partición de intercambio en el disco del sistema operativo. El agente de Linux se puede configurar para crear un archivo de intercambio en el disco de recursos temporal. Puede encontrar más información al respecto en los pasos siguientes.
* En Azure, todos los discos duros virtuales deben tener un tamaño virtual alineado con 1 MB. Al convertir un disco sin formato en un disco duro virtual, tiene que asegurarse de que su tamaño es un múltiplo de 1 MB antes de la conversión. Se puede encontrar más información en los siguientes pasos.
* Azure Stack no admite cloud-init. La máquina virtual debe ser configurada con una versión compatible del agente Linux de Microsoft Azure (WALA).

### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V

1. En el Administrador de Hyper-V, seleccione la máquina virtual.

1. Haga clic en **Conectar** para abrir una ventana de consola de la máquina virtual.

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto según sea necesario:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Ejecute el comando siguiente para asegurarse de que el servicio de red se inicia en el arranque:

    ```bash
    sudo systemctl enable network
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta modificación, abra `/etc/default/grub` en un editor de texto y modifique el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Esto garantiza que todos los mensajes de la consola se envíen al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración. Esta configuración también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC.

   Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños. Se recomienda quitar los parámetros siguientes:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque, que suele ser el predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```sh
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. No cree espacio de intercambio en el disco del sistema operativo.

    El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. El disco de recursos local es un disco temporal que debe vaciarse cuando la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Si quiere anular el registro de la suscripción, ejecute el siguiente comando:

    ```bash
    sudo subscription-manager unregister
    ```

1. Si usa un sistema que se implementó con una entidad de certificación de empresa, la máquina virtual de RHEL no confiará en el certificado raíz de Azure Stack. Se debe colocar en el almacén raíz de confianza. Consulte el artículo sobre cómo [agregar certificados raíz de confianza al servidor](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Haga clic en **Acción**  >  **Apagar** en el Administrador de Hyper-V.

1. Convierta el VHD en un VHD de tamaño fijo con la característica "Editar disco" del Administrador de Hyper-V o con el comando Convert-VHD de PowerShell. El VHD de Linux ya está listo para cargarse en Azure.

## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Preparación de una máquina virtual basada en Red Hat desde KVM

1. Descargue la imagen KVM de RHEL 7 desde el sitio web de Red Hat. Este procedimiento utiliza RHEL 7 como el ejemplo.

1. Establezca una contraseña raíz.

    Genere una contraseña cifrada y copie el resultado del comando:

    ```bash
    openssl passwd -1 changeme
    ```

   Establezca una contraseña raíz con guestfish:

    ```sh
    guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Cambie el segundo campo de usuario raíz de "!!" a la contraseña cifrada.

1. Cree una máquina virtual en KVM a partir de la imagen qcow2. Establezca el tipo de disco en **qcow2** y el modelo de dispositivo de interfaz de red virtual en **virtio**. Después, inicie la máquina virtual e inicie sesión como raíz.

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Ejecute el comando siguiente para asegurarse de que el servicio de red se inicia en el arranque:

    ```bash
    sudo systemctl enable network
    ```

1. Registre la suscripción de RedHat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```bash
    subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta configuración, abra `/etc/default/grub` en un editor de texto y modifique el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Este comando también asegura que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores. El comando también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC.

   Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde todos los registros se envían al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños. Se recomienda quitar los parámetros siguientes:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```bash
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Agregue módulos de Hyper-V a initramfs.

    Edite `/etc/dracut.conf` y agregue contenido:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Recompile initramfs:

    ```bash
    dracut -f -v
    ```

1. Desinstale cloud-init:

    ```bash
    yum remove cloud-init
    ```

1. Asegúrese de que el servidor SSH esté instalado y configurado para iniciarse en el tiempo de arranque:

    ```bash
    systemctl enable sshd
    ```

    Modifique /etc/ssh/sshd_config para que incluya las siguientes líneas:

    ```sh
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```bash
    yum install WALinuxAgent
    ```

    Habilite el servicio waagent:

    ```bash
    systemctl enable waagent.service
    ```

1. No cree espacio de intercambio en el disco del sistema operativo.

    El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. El disco de recursos local es un disco temporal que debe vaciarse cuando la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Para anular el registro de la suscripción (si es necesario), ejecute el siguiente comando:

    ```bash
    subscription-manager unregister
    ```

1. Si usa un sistema que se implementó con una entidad de certificación de empresa, la máquina virtual de RHEL no confiará en el certificado raíz de Azure Stack. Se debe colocar en el almacén raíz de confianza. Consulte el artículo sobre cómo [agregar certificados raíz de confianza al servidor](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Apague la máquina virtual en KVM.

1. Convierta la imagen qcow2 al formato VHD.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: https://bugs.launchpad.net/qemu/+bug/1490611.

    Primero convierta la imagen al formato raw:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Preparación de una máquina virtual basada en Red Hat para VMware

En esta sección se supone que ya instaló una máquina virtual RHEL en VMware. Para más información acerca de cómo instalar un sistema operativo en VMware, consulte [VMware Guest Operating System Installation Guide](https://partnerweb.vmware.com/GOSIG/home.html)(Guía de instalación de sistema operativo invitado de VMware).

* Al instalar el sistema Linux se recomienda usar las particiones estándar en lugar de un LVM, que a menudo viene de forma predeterminada en muchas instalaciones. De este modo se impide que el nombre del LVM entre en conflicto con las máquinas virtuales clonadas, especialmente si en algún momento hace falta adjuntar un disco de sistema operativo a otra máquina virtual para solucionar problemas. Para los discos de datos se puede utilizar LVM o RAID si así se prefiere.
* No configure una partición de intercambio en el disco del sistema operativo. Es posible configurar el agente de Linux para crear un archivo de intercambio en el disco de recursos temporal. Puede encontrar más información al respecto en los pasos a continuación.
* Cuando cree el disco duro virtual, seleccione **Almacenar disco virtual como un solo archivo**.

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Preparar una máquina virtual RHEL 7 desde VMware

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```bash
    sudo chkconfig network on
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta modificación, abra `/etc/default/grub` en un editor de texto y modifique el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo: 

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

    Esta configuración también asegura que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores. Esto también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC. Además, se recomienda quitar los parámetros siguientes:

    ```sh
    rhgb quiet crashkernel=auto
    ```

    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Agregue módulos de Hyper-V a initramfs.

    Edite `/etc/dracut.conf`y agregue contenido:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Recompile initramfs:

    ```bash
    dracut -f -v
    ```

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque. Esta configuración es normalmente el valor predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```sh
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. No cree espacio de intercambio en el disco del sistema operativo.

    El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. Tenga en cuenta que el disco de recursos local es un disco temporal que debe vaciarse cuando la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en `/etc/waagent.conf`:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    NOTE: set this to whatever you need it to be.
    ```

1. Si quiere anular el registro de la suscripción, ejecute el siguiente comando:

    ```bash
    sudo subscription-manager unregister
    ```

1. Si usa un sistema que se implementó con una entidad de certificación de empresa, la máquina virtual de RHEL no confiará en el certificado raíz de Azure Stack. Se debe colocar en el almacén raíz de confianza. Consulte el artículo sobre cómo [agregar certificados raíz de confianza al servidor](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Apague la máquina virtual y convierta el archivo VMDK al formato VHD.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: <https://bugs.launchpad.net/qemu/+bug/1490611>.

    Primero convierta la imagen al formato raw:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Preparación de una máquina virtual basada en Red Hat desde una imagen ISO con un archivo kickstart automáticamente

1. Cree un archivo kickstart que incluye el contenido siguiente y guarde el archivo. Para obtener información más detallada sobre la instalación de Kickstart, consulte la [guía de instalación de Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

    ```sh
    Kickstart for provisioning a RHEL 7 Azure VM

    System authorization information
    auth --enableshadow --passalgo=sha512

    Use graphical install
    text

    Do not run the Setup Agent on first boot
    firstboot --disable

    Keyboard layouts
    keyboard --vckeymap=us --xlayouts='us'

    System language
    lang en_US.UTF-8

    Network information
    network  --bootproto=dhcp

    Root password
    rootpw --plaintext "to_be_disabled"

    System services
    services --enabled="sshd,waagent,NetworkManager"

    System timezone
    timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

    Partition clearing information
    clearpart --all --initlabel

    Clear the MBR
    zerombr

    Disk partitioning information
    part /boot --fstype="xfs" --size=500
    part / --fstyp="xfs" --size=1 --grow --asprimary

    System bootloader configuration
    bootloader --location=mbr

    Firewall configuration
    firewall --disabled

    Enable SELinux
    selinux --enforcing

    Don't configure X
    skipx

    Power down the machine after install
    poweroff

    %packages
    @base
    @console-internet
    chrony
    sudo
    parted
    -dracut-config-rescue

    %end

    %post --log=/var/log/anaconda/post-install.log

    #!/bin/bash

    Register Red Hat Subscription
    subscription-manager register --username=XXX --password=XXX --auto-attach --force

    Install latest repo update
    yum update -y

    Enable extras repo
    subscription-manager repos --enable=rhel-7-server-extras-rpms

    Install WALinuxAgent
    yum install -y WALinuxAgent

    Unregister Red Hat subscription
    subscription-manager unregister

    Enable waaagent at boot-up
    systemctl enable waagent

    Disable the root account
    usermod root -p '!!'

    Configure swap in WALinuxAgent
    sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
    sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

    Set the cmdline
    sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

    Enable SSH keepalive
    sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

    Build the grub cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg

    Configure network
    cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    EOF

    Deprovision and prepare for Azure
    waagent -force -deprovision

    %end
    ```

1. Coloque el archivo kickstart donde el sistema de la instalación puede acceder a él.

1. En el Administrador de Hyper-V, cree una nueva máquina virtual. En la página **Conectar disco duro virtual**, seleccione **Exponer un disco duro virtual más adelante** y complete el Asistente para crear nueva máquina virtual.

1. Abra la configuración de la máquina virtual:

     a. Cargue un nuevo disco duro virtual en la máquina virtual. Asegúrese de seleccionar **Formato VHD** y **Tamaño fijo**.

    b. Adjunte la imagen ISO de instalación a la unidad de DVD.

    c. Configure el BIOS para que arranque desde el CD.

1. Inicie la máquina virtual. Cuando aparezca la guía de instalación, presione la tecla **Tab** para configurar las opciones de arranque.

1. Escriba `inst.ks=<the location of the kickstart file>` al final de las opciones de arranque y presione **Entrar**.

1. Espere hasta que la instalación se complete. Cuando haya finalizado, la máquina virtual se apaga automáticamente. El VHD de Linux ya está listo para cargarse en Azure.

## <a name="known-issues"></a>Problemas conocidos

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>El controlador de Hyper-V no se pudo incluir en el disco RAM inicial al usar un hipervisor de Hyper-V

En algunos casos, los instaladores de Linux pueden no incluir los controladores de Hyper-V en el disco RAM inicial (initrd o initramfs) a menos que Linux detecte que se está ejecutando un entorno de Hyper-V.

Cuando utilice un sistema de virtualización diferente (es decir, Virtualbox, KVM, etc.) para preparar su imagen de Linux, puede que necesite recompilar initrd para asegurarse de que al menos los módulos kernel hv_vmbus y hv_storvsc kernel están disponibles en el disco RAM inicial. Esto es un problema conocido al menos en sistemas basados en el nivel superior de distribución de Red Hat.

Para resolver este problema, agregue los módulos de Hyper-V en initramfs y recompilarlo:

Edite `/etc/dracut.conf` y agregue el siguiente contenido:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

Recompile initramfs:

    ```bash
    dracut -f -v
    ```

Para más información, consulte la información sobre cómo [recompilar initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para usar el disco duro virtual de Red Hat Enterprise Linux para crear nuevas máquinas virtuales de Azure Stack. Si es la primera vez que carga el archivo VHD en Azure Stack, consulte [Creación y publicación de un producto en Marketplace](azure-stack-create-and-publish-marketplace-item.md).

Para más información sobre los hipervisores certificados para ejecutar Red Hat Enterprise Linux, visite el [sitio web de Red Hat](https://access.redhat.com/certified-hypervisors).

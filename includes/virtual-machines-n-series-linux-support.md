---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: b8aba410cd447f14fcce89fee93c5f6a253a34ce
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102479"
---
## <a name="supported-distributions-and-drivers"></a>Distribuciones y controladores admitidos

### <a name="nvidia-cuda-drivers"></a>Controladores NVIDIA CUDA

Los controladores NVIDIA CUDA para máquinas virtuales de las series NC, NCv2, NCv3, ND y NDv2 (y opcionales para la serie NV) solo se admiten en las distribuciones de Linux enumeradas en la tabla siguiente. La información sobre los controladores de CUDA está actualizada en el momento de su publicación. Para ver los controladores de CUDA más recientes, visite el sitio web de [NVIDIA](https://developer.nvidia.com/cuda-zone). Asegúrese de instalar o actualizar los controladores más recientes de CUDA para su distribución. 

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual Linux, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Ubuntu 16.04 LTS o CentOS 7.4 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.

| Distribución | Controlador |
| --- | -- | 
| Ubuntu 16.04 LTS, 18.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3, 7.4, 7.5 o 7.6<br/><br/> HPC basada en CentOS 7.3, 7.4, 7.5 o 7.6, HPC basada en CentOS 7.4 | NVIDIA CUDA 10.0, rama de controlador R410 |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv2 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en máquinas virtuales de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

| Distribución | Controlador |
| --- | -- |
| Ubuntu 16.04 LTS, 18.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3, 7.4, 7.5 o 7.6<br/><br/>Basado en CentOS 7.3, 7.4, 7.5 o 7.6 | NVIDIA GRID 7.1, rama de controlador R410|

> [!WARNING] 
> La instalación de software de terceros en productos de Red Hat puede afectar a los términos de soporte técnico de Red Hat. Vea el [artículo de Knowledgebase de Red Hat](https://access.redhat.com/articles/1067).
>

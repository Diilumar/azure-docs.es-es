---
title: 'Tutorial: Integración de Azure Active Directory con TiViTz | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TiViTz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b97ed88f-7888-4185-be22-41d1f62cbbf1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe1168eb9ea1ba886bb49b1ea2a24e66ff8ae94e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187891"
---
# <a name="tutorial-azure-active-directory-integration-with-tivitz"></a>Tutorial: Integración de Azure Active Directory con TiViTz

En este tutorial, aprenderá a integrar TiViTz con Azure Active Directory (Azure AD).

La integración de TiViTz con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a TiViTz.
- Puede permitir que los usuarios inicien sesión automáticamente en TiViTz (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con TiViTz, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en TiViTz

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de TiViTz desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-tivitz-from-the-gallery"></a>Adición de TiViTz desde la galería
Para configurar la integración de TiViTz en Azure AD, debe agregar TiViTz desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar TiViTz desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **TiViTz**.

    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/tutorial_tivitz_search.png)

1. En el panel de resultados, seleccione **TiViTz** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/tutorial_tivitz_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con TiViTz con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de TiViTz para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de TiViTz.

Para establecer la relación de vínculo, en TiViTz, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con TiViTz, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de TiViTz](#creating-a-tivitz-test-user)**: para tener un homólogo de Britta Simon en TiViTz vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación TiViTz.

**Para configurar el inicio de sesión único de Azure AD con TiViTz, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **TiViTz**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/tivitz-tutorial/tutorial_tivitz_samlbase.png)

1. En la sección **Dominio y direcciones URL de TiViTz**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/tivitz-tutorial/tutorial_tivitz_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.o365.tivitz.com/`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.o365.tivitz.com/`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de TiViTz](mailto:info@tivitz.com) para obtener estos valores. 

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/tivitz-tutorial/tutorial_tivitz_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/tivitz-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en el lado de **TiViTz**, necesita enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de TiViTz](mailto:info@tivitz.com).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/tivitz-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-tivitz-test-user"></a>Creación de un usuario de prueba de TiViTz

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en TiViTz. TiViTz admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a TiViTz, se crea un nuevo usuario, en caso de que no exista.

>[!NOTE]
>Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de TiViTz](mailto:info@tivitz.com).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a TiViTz.

![Asignar usuario][200] 

**Para asignar a Britta Simon a TiViTz, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **TiViTz**.

    ![Configurar inicio de sesión único](./media/tivitz-tutorial/tutorial_tivitz_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TiViTz en el panel de acceso, debería iniciar sesión automáticamente en su aplicación TiViTz.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/tivitz-tutorial/tutorial_general_01.png
[2]: ./media/tivitz-tutorial/tutorial_general_02.png
[3]: ./media/tivitz-tutorial/tutorial_general_03.png
[4]: ./media/tivitz-tutorial/tutorial_general_04.png

[100]: ./media/tivitz-tutorial/tutorial_general_100.png

[200]: ./media/tivitz-tutorial/tutorial_general_200.png
[201]: ./media/tivitz-tutorial/tutorial_general_201.png
[202]: ./media/tivitz-tutorial/tutorial_general_202.png
[203]: ./media/tivitz-tutorial/tutorial_general_203.png


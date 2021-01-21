---
title: Configuración de correo electrónico en modo nativo de SSRS (Administrador de configuración) | Microsoft Docs
description: SQL Server Reporting Services incluye una extensión de entrega por correo electrónico para distribuir informes por correo electrónico.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.emailsettings.F1
helpviewer_keywords:
- SQL11.rsconfigtool.emailsettings.F1
ms.assetid: cdad1529-bfa6-41fb-9863-d9ff1b802577
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31ab6390286c748d8f9cb8f6b98b80c9c53818a1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596936"
---
# <a name="e-mail-settings---reporting-services-native-mode-configuration-manager"></a>Configuración de correo electrónico: Modo nativo de Reporting Services (Administrador de configuración)
SQL Server Reporting Services incluye una extensión de entrega por correo electrónico para distribuir informes por correo electrónico. Según cómo defina la suscripción del correo electrónico, una entrega podría estar compuesta de una notificación, un vínculo, datos adjuntos o un informe incrustado. La extensión de entrega por correo electrónico funciona con la tecnología de servidor de correo existente. El servidor de correo debe ser un servidor SMTP o un reenviador. El servidor de informes se conecta a un servidor SMTP a través de bibliotecas de Collaboration Data Objects (CDO), cdosys.dll, que el sistema operativo proporciona.

La extensión de entrega por correo electrónico del servidor de informes no está configurada de manera predeterminada. Debe usar el Administrador de configuración del servidor de informes para configurar la extensión mínimamente. Para establecer propiedades avanzadas, debe editar el archivo RSReportServer.config. Si no puede configurar el servidor de informes para que utilice esta extensión, puede entregar los informes en una carpeta compartida. Para obtener más información, vea Entrega a recursos compartidos en Reporting Services.

## <a name="configuration-requirements"></a>Requisitos de configuración

- La entrega por correo electrónico del servidor de informes se implementa en Collaboration Data Objects (CDO) y requiere un servidor del Protocolo simple de transferencia de correo (SMTP) local o remoto, o bien un reenviador SMTP. SMTP no se admite en todos los sistemas operativos Windows. Si usa la edición basada en Itanium de Windows Server 2008, no se admite SMTP. Para obtener más información sobre las opciones de configuración que se proporcionan a través de CDO, vea [el tema sobre la coclase Configuration](/previous-versions/exchange-server/exchange-10/ms526939(v=exchg.10)) en MSDN.

La cuenta de autenticación configurada debe tener permiso para enviar correo electrónico en el servidor SMTP.

- La extensión de entrega por correo electrónico utiliza la codificación UTF-8 en los datos adjuntos del correo electrónico. La codificación no se puede modificar; la extensión de representación en HTML solo admite UTF-8.

> [!NOTE] 
> La extensión de entrega por correo electrónico predeterminada no es compatible con la firma digital ni el cifrado de mensajes de correo salientes.

## <a name="setting-configuration-options-for-e-mail-delivery"></a>Establecer opciones de configuración para la entrega por correo electrónico
Para poder utilizar la entrega por correo electrónico del servidor de informes, debe establecer valores de configuración que proporcionen información sobre qué servidor SMTP se debe utilizar.

Para configurar un servidor de informes para la entrega por correo electrónico, siga este procedimiento:

- Use el Administrador de configuración del servidor de informes si solo va a especificar un servidor SMTP y una cuenta de usuario que tenga permiso para enviar correo electrónico. Ésta es la configuración mínima necesaria para configurar la extensión de entrega por correo electrónico del servidor de informes.

- (Opcionalmente) Utilice un procesador de texto para especificar valores adicionales en el archivo RSreportserver.config. Este archivo contiene toda la configuración para la distribución del correo electrónico del servidor de informes. Si utiliza un servidor SMTP adicional o limita la entrega de correo electrónico a hosts específicos, debe configurar opciones adicionales en estos archivos. Para obtener más información sobre cómo buscar y modificar archivos de configuración, consulte [Modificar un archivo de configuración de Reporting Services (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).

> [!NOTE] 
> Las opciones de correo electrónico del servidor de informes se basan en CDO. Si desea obtener más detalles acerca de opciones específicas, puede consultar la documentación de producción de CDO.

## <a name="configure-report-server-e-mail-using-the-report-server-configuration-manager"></a><a name="rsconfigman"/>Configurar el correo electrónico del servidor de informes mediante el Administrador de configuración del servidor de informes

1. Inicie el Administrador de configuración del servidor de informes y conéctese a la instancia del servidor de informes.

2. En **Dirección del remitente**, indique la dirección de correo electrónico que se va a usar en el campo **De:** de un mensaje de correo electrónico generado. 

     Debe especificar una cuenta de usuario que tenga permiso para enviar correo desde el servidor SMTP. El valor que escriba en **Dirección del remitente** se guarda en el campo `<From>` del archivo rsreportserver.config.  

3.  En **Servidor SMTP**, indique el servidor SMTP o la puerta de enlace que se va a usar. 

     Este valor puede ser una dirección IP, un nombre NetBIOS de un equipo de la intranet corporativa o un nombre de dominio completo. El valor que escriba en **Servidor SMTP** se guarda en el campo `<SMTPServer>` del archivo rsreportserver.config.

4. Use la lista desplegable **Autenticación** para especificar cómo autenticarse en el servidor SMTP. Este 

     - **Sin autenticación** significa que se conectará de forma anónima al servidor de correo especificado.
     
          Si selecciona esta opción, `<SendUsing>` se establecerá en un valor de **2** y `<SMTPAuthenticate>` , en un valor de **0** en el archivo rsreportserver.config.
     
     - **Nombre de usuario y contraseña (básico)** permite especificar un nombre de usuario y una contraseña para conectarse al servidor de correo. También puede seleccionar **Usar una conexión segura** para que tenga lugar una conexión cifrada con el servidor de correo.
     
          Si selecciona esta opción, `<SendUsing>` se establecerá en un valor de **2** y `<SMTPAuthenticate>` , en un valor de **1** en el archivo rsreportserver.config. Si selecciona **Usar una conexión segura**, `SMTPUseSSL` se establecerá en **True**. **Nombre de usuario** se establecerá en `<SendUserName>` como un valor cifrado. **Contraseña** se establecerá en `<SendPassword>` como un valor cifrado.
     
     - **Cuenta de servicio del servidor de informes (NTLM)** usará la cuenta de servicio especificada para el servidor de informes. Si usa la cuenta de servicio del servidor de informes para la autenticación, confirme que la cuenta de servicio tiene permisos **Enviar como** en el servidor SMTP.
     
          Si selecciona esta opción, `<SendUsing>` se establecerá en un valor de **2** y `<SMTPAuthenticate>` , en un valor de **2** en el archivo rsreportserver.config.

5. Seleccione **Aplicar**.

6. Opcionalmente, puede ajustar otros campos relativos a la configuración de correo electrónico en el archivo rsreportserver.config.

## <a name="example-report-server-e-mail-configuration"></a>Ejemplo de configuración de correo electrónico del servidor de informes
El ejemplo siguiente muestra las opciones de configuración del archivo RSreportserver.config para un servidor SMTP remoto. Para obtener más información sobre las descripciones de configuración y los valores válidos, consulte [Archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

```
<RSEmailDPConfiguration>
     <SMTPServer>mySMTPServer.Adventure-Works.com</SMTPServer>
     <SMTPServerPort></SMTPServerPort>
     <SMTPAccountName></SMTPAccountName>
     <SMTPConnectionTimeout></SMTPConnectionTimeout>
     <SMTPServerPickupDirectory></SMTPServerPickupDirectory>
     <SMTPUseSSL>False</SMTPUseSSL>
     <SendUsing>2</SendUsing>
     <SMTPAuthenticate>2</SMTPAuthenticate>
     <From>my-rs-email-account@Adventure-Works.com</From>
     <EmbeddedRenderFormats>
          <RenderingExtension>MHTML</RenderingExtension>
     </EmbeddedRenderFormats>
     <PrivilegedUserRenderFormats></PrivilegedUserRenderFormats>
     <ExcludedRenderFormats>
          <RenderingExtension>HTMLOWC</RenderingExtension>
          <RenderingExtension>NULL</RenderingExtension>
          <RenderingExtension>RGDI</RenderingExtension>
     </ExcludedRenderFormats>
     <SendEmailToUserAlias>True</SendEmailToUserAlias>
     <DefaultHostName></DefaultHostName>
     <PermittedHosts>
          <HostName>Adventure-Works.com</HostName>
          <HostName>hotmail.com</HostName>
     </PermittedHosts>
     <SendUserName></SendUserName>
     <SendPassword></SendPassword>
</RSEmailDPConfiguration>
```
## <a name="configuration-options-for-setting-the-to-field-in-a-message"></a>Opciones de configuración para configurar el campo Para: de un mensaje
Las suscripciones definidas por el usuario que se crean según los permisos otorgados por la tarea Administrar suscripciones individuales contienen un nombre de usuario establecido previamente basado en la cuenta de usuario de dominio. Cuando el usuario crea la suscripción, el nombre del destinatario del campo **Para:** se rellena automáticamente a partir de la cuenta de usuario de dominio de la persona que está creando la suscripción.

Si está utilizando un servidor SMTP o reenviador que emplee cuentas de correo electrónico distintas a la cuenta de usuario de dominio, la entrega del informe generará un error cuando el servidor SMTP intente entregar el informe al usuario.

Para solucionar este problema, puede modificar la configuración para permitir a los usuarios escribir un nombre en el campo **Para:** :

1. Abra RSReportServer.config en un editor de texto.

2. Establezca `<SendEmailToUserAlias>` en **False**.

3. Establezca `<DefaultHostName>` en el nombre del Sistema de nombres de dominio (DNS) o la dirección IP del servidor SMTP o reenviador.

4. Guarde el archivo.

## <a name="configuration-options-for-remote-smtp-service"></a>Opciones de configuración para un servicio SMTP remoto
La conexión entre el servidor de informes y un servidor o reenviador SMTP viene determinada por las opciones de configuración siguientes:

- `<SendUsing>` especifica un método para enviar mensajes. Se puede elegir entre un servicio SMTP de red o un directorio de recogida del servicio SMTP local. Para utilizar un servicio SMTP remoto, este valor debe establecerse en **2** en el archivo RSReportServer.config.
- `<SMTPServer>` especifica el servidor o reenviador SMTP remoto. Se trata de un valor necesario si utiliza un servidor o reenviador SMTP remoto.
- `<From>` establece el valor que se muestra en la línea **De:** del mensaje de correo electrónico. Se trata de un valor necesario si utiliza un servidor o reenviador SMTP remoto.

Entre los demás valores que se utilizan para un servicio SMTP remoto se incluyen los siguientes (tenga en cuenta que no es necesario especificarlos a menos que desee reemplazar los valores predeterminados).

- `<SMTPServerPort>` está configurado para el puerto 25 de manera predeterminada.
- `<SMTPAuthenticate>` especifica cómo se conecta el servidor de informes al servidor SMTP remoto. El valor predeterminado es **0** (o sin autenticación). En tal caso, la conexión se realiza a través de un acceso anónimo. En función de su configuración de dominio, es posible que el servidor de informes y el servidor SMTP tengan que ser miembros del mismo dominio.
- Para enviar correo electrónico a listas de distribución restringidas (por ejemplo, listas de distribución que acepten mensajes entrantes solo de cuentas autenticadas), establezca `<SMTPAuthenticate>` en **1** o en **2**. Si se establece en **1**, también necesitará establecer `<SendUserName>` y `<SendPassword>`. Se recomienda hacerlo a través del Administrador de configuración del servidor de informes, ya que cifrará los valores de `<SendUserName>` y `<SendPassword>`.

### <a name="to-configure-a-remote-smtp-service-for-the-report-server"></a>Para configurar un servicio SMTP remoto para el servidor de informes

> [!NOTE] 
> Se recomienda configurar el servidor de correo con el Administrador de configuración del servidor de informes.

1. Compruebe que el servicio Servidor de informes de Windows disponga de permisos **Send As** para el servidor SMTP.

2. Abra el archivo RSReportServer.config en un editor de texto.

3. Compruebe que `<UrlRoot>` se haya establecido en la dirección URL del servidor de informes. Este valor se establece al configurar el servidor de informes y debe mostrar la dirección. Si no es así, escriba la dirección URL del servidor de informes.

4. En la sección Entrega, busque `<RSEmailDPConfiguration>`.
     
5. En `<SMTPServer>`, escriba el nombre del servidor SMTP. Este valor puede ser una dirección IP, un nombre UNC de un equipo de la intranet corporativa o un nombre de dominio completo.

6. Establezca `<SendUsing>` en un valor de **2** para usar la cuenta de servicio para el servidor de informes. Establezca `<SendUsing>` en un valor de **1** para la autenticación básica. Si se establece en **1**, también habrá que proporcionar un valor para `<SendUserName>` y `<SendPassword>`. Si quiere que esos valores se cifren, establezca la autenticación en el Administrador de configuración del servidor de informes.

7. Establezca `<SMTPAuthenticate>` en un valor de **1** Si establece `<SendUsing>` en 1 o en 2.

7. Establezca `<From>`. Debe especificar una cuenta de usuario que tenga permiso para enviar correo desde el servidor SMTP.

8. Guarde el archivo.

     El servidor de informes utilizará la nueva configuración de forma automática, por lo que no necesita reiniciar el servicio. Puede especificar valores adicionales para SMTP con el fin de configurar cómo se utilizará el servidor para la entrega por correo electrónico del servidor de informes.

## <a name="configuration-options-for-local-smtp-service"></a>Opciones de configuración para un servicio SMTP local
Configurar un servicio SMTP local resulta útil si se está probando o solucionando problemas de la entrega por correo electrónico del servidor de informes. El servicio SMTP local no está habilitado de forma predeterminada.

La conexión entre el servidor de informes y un servidor o reenviador SMTP local viene determinada por las opciones de configuración siguientes:

- **SendUsing** se establece en **1**.
- **SMTPServerPickupDirectory** se establece en una carpeta de la unidad local.

  > [!NOTE] 
  > Asegúrese de no establecer SMTPServer si está usando un servidor SMTP local.

- **From** establece el valor que se muestra en la línea **De:** del mensaje de correo electrónico. Este valor es necesario.

### <a name="to-configure-a-local-smtp-service-for-the-report-server"></a>Para configurar un servicio SMTP local para el servidor de informes

1. En el Panel de Control, seleccione **Activar o desactivar las características de Windows** para abrir el **Asistente para agregar roles y características**.

2. Seleccione **Instalación basada en características o en roles** y, luego, **Siguiente**.

3. Seleccione el servidor donde va a instalar Internet Information Server (IIS) y seleccione **Siguiente**.

4. Haga clic en **Siguiente** en la página *Roles de servidor*.
     
5. En la página *Características* , seleccione **Servidor SMTP** y, después, **Siguiente**.

     Si se le pide agregar características necesarias para el servidor SMTP, seleccione **Agregar características**.

6. Seleccione **Siguiente** en la página *Rol de servidor web (IIS)* .

7. Seleccione **Siguiente** en la página *Servicios de rol* .

8. Seleccione **Instalar** en la página **Confirmación** .

9. Confirme que el servicio de Windows **Protocolo simple de transferencia de correo (SMTP)** se está ejecutando en la consola Servicios.

     Para configurar el servidor SMTP local, hay que usar el Administrador de IIS 6.0, que encontrará en Herramientas administrativas.

10. Abra el archivo RSReportServer.config en un editor de texto.

11. Compruebe que `<UrlRoot>` se haya establecido en la dirección URL del servidor de informes. Este valor se establece al configurar el servidor de informes y debe mostrar la dirección. Si no está establecido, escriba la dirección URL del servicio web del servidor de informes.

12. En la sección Entrega, busque `<RSEmailDPConfiguration>`.
     
13. Asegúrese de que `<SMTPServer>` está presente, pero vacío.
     
14. Establezca `<SendUsing>` en 1.
     
14. Establezca `<SMTPAuthenticate>` en 0.
     
15. Establezca `<SMTPServerPickupDirectory>` en la carpeta de recogida del servicio SMTP.
     
     La ubicación predeterminada será *C:\inetpub\mailroot\Pickup*.
     
16. Establezca `<From>`. Esto establece el valor que se muestra en la línea **De:** del mensaje de correo electrónico.
     
17. Guarde el archivo.
  
## <a name="see-also"></a>Consulte también  
[Administrador de configuración del servidor de informes (modo nativo)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
[Modify a Reporting Services Configuration File (rsreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  
[El archivo de configuración RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)
  

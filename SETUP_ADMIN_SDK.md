# Guía: Configuración del Firebase Admin SDK para la Gestión de Usuarios

Para que el panel de administración pueda listar y gestionar usuarios, necesita comunicarse con el **Firebase Admin SDK**, que se ejecuta de forma segura en el servidor a través de una API Route de Next.js.

El paso final y más crucial, que debes hacer manualmente por seguridad, es proporcionar a tu aplicación la "llave secreta" (credenciales de la cuenta de servicio) para que pueda autenticarse como administrador en tu proyecto de Firebase.

Sigue estos 3 pasos:

---

### Paso 1: Obtener tu Llave de Cuenta de Servicio desde Firebase

Esta llave es un archivo JSON que le da a tu servidor acceso de administrador a tu proyecto.

1.  **Abre la Consola de Firebase:** Ve a [https://console.firebase.google.com/](https://console.firebase.google.com/).
2.  **Selecciona tu Proyecto:** Elige el proyecto que estás utilizando para esta aplicación.
3.  **Ve a la Configuración del Proyecto:** Haz clic en el ícono de engranaje (⚙️) junto a "Project Overview" en el menú de la izquierda y selecciona **"Project settings"**.
4.  **Ve a la Pestaña "Service accounts":** Dentro de la configuración, busca y haz clic en la pestaña **"Service accounts"**.
5.  **Genera una Nueva Clave Privada:** Haz clic en el botón que dice **"Generate new private key"**. Aparecerá una advertencia; confirma haciendo clic en **"Generate key"**.
6.  **Guarda el Archivo:** Tu navegador descargará automáticamente un archivo JSON. Este archivo contiene tus credenciales de administrador. Guárdalo en un lugar seguro en tu computadora y **nunca lo compartas públicamente**.

---

### Paso 2: Convertir el Contenido del Archivo JSON a una Sola Línea

Las variables de entorno funcionan mejor si no contienen saltos de línea.

1.  **Abre el archivo JSON** que acabas de descargar con un editor de texto (como VS Code, Sublime Text, o incluso el Bloc de notas).
2.  **Copia todo el contenido** del archivo.
3.  **Pega el contenido** en un convertidor de "JSON a una sola línea". Puedes usar una herramienta en línea como [este sitio (JSON to String)](https://tools.knowledgewalls.com/json-to-string-converter). Pega tu JSON en el cuadro de la izquierda y copia la cadena de texto de una sola línea que aparece a la derecha.

El resultado debería verse algo así (es una sola línea larga):
`{"type": "service_account", "project_id": "tu-proyecto", "private_key_id": "...", ...}`

---

### Paso 3: Configurar la Variable de Entorno (¡El Paso Clave!)

**IMPORTANTE:** Este paso **NO se realiza en la consola de Firebase**. Debes hacerlo en el panel de control del servicio donde tienes alojada tu aplicación web (por ejemplo, Vercel, Netlify, Firebase App Hosting, etc.).

Imagina que tu aplicación es una casa y la llave secreta es la llave de la puerta. No puedes dejar la llave pegada en la puerta (el código). Debes dársela al "conserje" (el servicio de hosting) de forma segura. Las variables de entorno son ese método seguro.

1.  **Identifica dónde alojas tu aplicación:** ¿Estás usando Vercel, Netlify, Firebase App Hosting, u otro servicio? El proceso varía ligeramente dependiendo del proveedor.
2.  **Busca la sección de "Environment Variables" (Variables de Entorno)** en el panel de control de tu proyecto dentro de tu proveedor de hosting. Generalmente se encuentra en la sección de "Settings" (Ajustes) del proyecto.
3.  **Crea una nueva variable de entorno:**
    *   **Nombre:** `FIREBASE_SERVICE_ACCOUNT_JSON` (El nombre debe ser exactamente este, ya que el código está preparado para buscarlo).
    *   **Valor:** Pega la cadena de texto de **una sola línea** que copiaste en el Paso 2.
4.  **Guarda y Vuelve a Desplegar (Redeploy):** Guarda la variable. La mayoría de los proveedores de hosting requerirán que vuelvas a desplegar (o hacer "redeploy") tu aplicación para que los cambios surtan efecto y tu aplicación pueda acceder a esta nueva variable secreta.

¡Y eso es todo! Una vez que la variable esté configurada y tu aplicación se haya vuelto a desplegar, el panel de administración podrá autenticarse de forma segura y empezar a mostrar la lista de usuarios registrados en tu proyecto.

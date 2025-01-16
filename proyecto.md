# **Proyecto segundo trimestre**
Diseñar y desarrollar una aplicación móvil en Android Studio utilizando Firebase como backend. Cada uno elegirá una temática de su interés (gestor de recetas, catálogo de videojuegos, agenda de contactos, etc) A lo largo de cinco semanas, los estudiantes implementarán funcionalidades clave, incluyendo autenticación, almacenamiento en tiempo real, patrones de diseño, pruebas, uso de Material Design y despliegue final de la aplicación.

#### **Duración**
- **Total:** 5 semanas (45 sesiones de 50 minutos).
- **Distribución:** La práctica se desarrollará incrementalmente semana a semana, con nuevas funcionalidades integradas en cada etapa.

---

## **Semana 1: Introducción a Firebase y Pantallas Iniciales**

#### **Objetivo**
Configurar Firebase en un proyecto de Android Studio, implementar autenticación de usuarios y crear una pantalla principal que muestre datos almacenados en Firebase Realtime Database.

#### **Tareas**

1. **Configuración del Proyecto Firebase:**
   - Crear un proyecto en Firebase Console.
   - Registrar la aplicación Android en Firebase.
   - Descargar el archivo `google-services.json` y agregarlo al proyecto en Android Studio.
   - Configurar los archivos `build.gradle` según las instrucciones proporcionadas por Firebase.

2. **Interfaz de Usuario:**
   - **Si el programa no te almacena el usuario en Realtime Database, vuelve a generar el google-services.json y añádelo a tu aplicación de nuevo. En clase comenté que primero había que habilitar el Authenticator y Realtime Database en Firebase, pero me olvidé de reflejarlo aquí.**
   - Elaborar las siguientes pantallas en Figma siguiendo lo aprendido en la tarea de Grupo de Expertos.
    - **RegisterActivity:**
     - Diseñar una pantalla de registro que permita al usuario ingresar los siguientes datos:
       - Nombre completo.
       - Email.
       - Contraseña.
       - Confirmación de contraseña.
       - Teléfono.
       - Dirección.
     - Validar que todos los campos sean completados y que las contraseñas coincidan antes de registrar al usuario.
     - Al registrar al usuario:
       - Guardar el email y la contraseña en Firebase Authentication.
       - Crear un nodo en Firebase Realtime Database bajo el identificador del usuario registrado con los datos adicionales (nombre, teléfono, dirección).

    - **LoginActivity:**
     - Diseñar una pantalla de inicio de sesión con los siguientes elementos:
       - Campo de texto para email.
       - Campo de texto para contraseña.
       - Botón para registrar un nuevo usuario.
       - Botón para iniciar sesión.

    - **DashboardActivity:**
     - Diseñar una pantalla que muestre un único elemento desde Firebase (título, descripción e imagen - será accesible meddiante URL).
     - Agregar un botón de **Logout** en la parte superior derecha de la interfaz. Este botón nos devolverá a LoginActivity

4. **Funcionalidades de Firebase:**
   - **Autenticación:**
     - Implementar el registro de usuarios con email y contraseña utilizando `FirebaseAuth`.
     - Implementar el inicio de sesión para usuarios registrados.
     - Implementar el logout.
   - **Realtime Database:**
     - Configurar reglas de seguridad para permitir acceso solo a usuarios autenticados.
     - Cargar datos iniciales (10) en Firebase Realtime Database mediante un archivo JSON. Estos tienen que tratar una temática común (misma tabla), con campos título, descripción e imagen.
     - Leer uno de los datos cargados inicialmente.


#### **Entregables**
1.GitHub:
Código fuente completo subido al repositorio. Trabaja en la rama semana. Haz un commit por cada tarea significativa terminada. Para la entrega, haz un merge a la rama principal.
Los commits deben comenzar por: "SemanaX: commit que estimes oportuno." (Hay que cambiar la X por el número de semana/etapa en la que estáis en cada caso)

2.Classroom:
Enlace al repositorio.
Carpeta con el proyecto completo de Android Studio
Archivo JSON utilizado para cargar datos iniciales en Firebase.
Capturas de pantalla mostrando distintas pruebas en:
  - Pantalla de inicio de sesión.
  - Pantalla de registro.
  - Pantalla de visualización de datos.

---




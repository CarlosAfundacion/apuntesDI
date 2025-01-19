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
   - Activar Authenticator y Realtime Database.
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
     - Diseñar una pantalla que muestre un único elemento desde Firebase (título, descripción e imagen - será accesible mediante URL).
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
1. GitHub:
Código fuente completo subido al repositorio. Trabaja en la rama semana. Haz un commit por cada tarea significativa terminada. Para la entrega, haz un merge a la rama principal.
Los commits deben comenzar por: "SemanaX: commit que estimes oportuno." (Hay que cambiar la X por el número de semana/etapa en la que estáis en cada caso)

2. Classroom:
Enlace al repositorio.
Carpeta con el proyecto completo de Android Studio
Archivo JSON utilizado para cargar datos iniciales en Firebase.
Vídeo mostrando funcionalidad y pruebas de la aplicación en (No es necesario audio):
  - Pantalla de inicio de sesión.
  - Pantalla de registro.
  - Pantalla de visualización de datos.
  - Logout

---

## **Semana 2: Implementación de MVVM y Pantalla de Detalle**
#### **Objetivo**
Desarrollar una arquitectura basada en el patrón de diseño MVVM (Model-View-ViewModel) para todas las pantallas del proyecto iniciado en la semana 1. Además, implementar una nueva pantalla de detalle (`DetailActivity`) que se abrirá al seleccionar un elemento del `RecyclerView` en la pantalla principal (`DashboardActivity`). La nueva pantalla mostrará el título, la imagen y la descripción del elemento seleccionado.

---

#### **Tareas**
1. **Reestructuración del Proyecto para MVVM**
   - Crear los paquetes `models`, `repositories`, `viewmodels`, y mover las clases a sus ubicaciones correspondientes:
     - **`models/`:** Clases que representan la estructura de datos (e.g., `Recipe`, `Game`, `Contact`).
     - **`repositories/`:** Clases encargadas de interactuar con Firebase para cargar o guardar datos.
     - **`viewmodels/`:** Clases `ViewModel` que exponen los datos observables a las vistas.

2. **Pantalla de Registro (`RegisterActivity`)**
   - Crear un modelo `User.java` para representar los datos del usuario.
   - Implementar un repositorio (`UserRepository`) que gestione la autenticación y el almacenamiento de datos en Firebase.
   - Implementar un `RegisterViewModel` que valide los datos del formulario y gestione el registro de usuarios.
   - Modificar `RegisterActivity` para observar los datos del `RegisterViewModel` y reaccionar a cambios.

3. **Pantalla de Inicio de Sesión (`LoginActivity`)**
   - Agregar funcionalidad al `UserRepository` para gestionar el inicio de sesión.
   - Crear un `LoginViewModel` que exponga métodos para iniciar sesión.
   - Actualizar `LoginActivity` para usar el `LoginViewModel` y observar los resultados de la autenticación.

4. **Pantalla Principal (`DashboardActivity`)**
   - Implementar un `RecyclerView` que muestre una lista de elementos relacionados con la temática seleccionada.
   - Diseñar un modelo que represente los elementos de la lista (e.g., `Recipe`, `Game`, `Contact`).
   - Crear un adaptador para enlazar los datos con el `RecyclerView`.
   - Implementar un repositorio (`DashboardRepository`) que cargue los datos desde Firebase.
   - Crear un `DashboardViewModel` que exponga los datos observables para la lista.
   - Agregar un botón de **Logout** en la parte superior que permita cerrar sesión y volver a la `LoginActivity`.
   - Configurar un evento en el adaptador para abrir el `DetailActivity` al seleccionar un elemento.

5. **Pantalla de Detalle (`DetailActivity`)**
   - Diseñar la interfaz del `DetailActivity` para mostrar el título, la imagen y la descripción del elemento seleccionado.
   - Pasar los datos del elemento seleccionado desde el `DashboardActivity` mediante un `Intent`.

---

#### **Estructura del Proyecto**
```
app/
  src/
    main/
      java/
        com.example.app/
          models/
            User.java
            Recipe.java
          repositories/
            UserRepository.java
            DashboardRepository.java
          viewmodels/
            RegisterViewModel.java
            LoginViewModel.java
            DashboardViewModel.java
          views/
            RegisterActivity.java
            LoginActivity.java
            DashboardActivity.java
            DetailActivity.java
          adapters/
            RecipeAdapter.java
          
```

---

#### **Entregables**

**Concédeme acceso como colaborador a tu proyecto de Firebase**
1. **Código en GitHub:**
   - Rama `semana2`.
   - Realizar commits por cada tarea completada, siguiendo la nomenclatura: `Semana2: [descripción del cambio]`.
   - Para la entrega, mergea semana2 con main.

2. **Documentos y Vídeos:**
   - Video demostrativo con las siguientes funciones, mostrando la estructura del proyecto:
     - Registro de usuario.
     - Inicio de sesión.
     - Visualización de la lista de elementos.
     - Navegación al detalle desde el Dashboard.
     - Logout desde el Dashboard.

3. **Presentación en Classroom:**
   - Enlace al repositorio.
   - Archivo comprimido con el proyecto de Android Studio.




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
   - Configurar un evento en el adaptador para abrir el `DetailActivity` al seleccionar un elemento. [Ayuda de implementación](https://github.com/CarlosAfundacion/apuntesDI/blob/main/Celda%20clickable%20recyclerView.md)

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

## Semana 3: Gestión de Favoritos, Accesibilidad y Configuración de Preferencias

En esta etapa de la práctica incremental, los estudiantes trabajarán en implementar funcionalidades relacionadas con la gestión de favoritos y prácticas de accesibilidad en su aplicación móvil. A continuación, se describen los objetivos, tareas y entregables de esta semana.

---

### **Objetivos**

1. Crea un tema personalizado con Material Theme Builder y aplícalo a tu aplicación.
2. Desde DashboardActivity debe poder navegarse a FavouritesActivity
3. Añadir un RecyclerView en FavouritesActivity que muestre únicamente los elementos marcados como favoritos por el usuario.
4. Integrar un botón flotante (Floating Action Button) en el DetailActivity que permita agregar o eliminar un elemento de la lista de favoritos del usuario.
5. Mejorar la accesibilidad de las pantallas utilizando las prácticas descritas en los apuntes. Añade el contentDescription a las imágenes del RecyclerView
6. Consolidar el uso del patrón MVVM para esta nueva funcionalidad.

---

### **Tareas**

#### **1. Gestión de Favoritos**

1. **Base de Datos:**

   - Crear una estructura en Firebase para almacenar los favoritos del usuario (puedes implementar directamente el código en tu app):
     ```json
     {
       "usuarios": {
         "usuario1": {
           "favoritos": ["idElemento1", "idElemento2"]
         }
       }
     }
     ```
   - Configurar las reglas de seguridad para asegurar que cada usuario solo pueda acceder a sus favoritos.

2. **Floating Action Button en DetailActivity:**

   - Diseñar el botón flotante y agregarlo al layout.
   - Implementar la funcionalidad para:
     - Agregar el elemento actual a la lista de favoritos del usuario en Firebase si no está presente.
     - Eliminarlo si ya está en la lista.
   - Actualizar dinámicamente el estado del botón flotante (e.g., cambiar su ícono dependiendo de si el elemento es favorito o no).

3. **RecyclerView para Favoritos:**

   - Crear un nuevo RecyclerView en el FavouritesActivity que muestre los favoritos del usuario.
   - Diseñar un adaptador específico para esta lista.
   - Observar los cambios en la base de datos en tiempo real y actualizar el RecyclerView dinámicamente.

4. **DashboardActivity:**

   - Añadir navegación a FavouritesActivity.

---

#### **2. Accesibilidad**

1. **contentDescription:**

   - Añadir descripciones en todas las imágenes del RecyclerView para soportar lectores de pantalla como TalkBack.

2. **Contraste:**

   - Revisar y ajustar los colores en las pantallas para cumplir con las pautas de contraste de WCAG. Utilizar herramientas como [**Contrast Checker**](https://webaim.org/resources/contrastchecker/).

3. **Pruebas de Accesibilidad:**

   - Probar la aplicación con TalkBack activado para verificar que los usuarios con discapacidades visuales puedan navegar correctamente y escuchar las descripciones de las imágenes. Para configurar TalkBack en Android Studio, sigue los siguientes [pasos](https://github.com/CarlosAfundacion/apuntesDI/blob/main/TalkBack.md)

---

#### **3. Integración con MVVM**

1. **ViewModels:**

   - Crear o actualizar los ViewModels necesarios para gestionar las listas de elementos y favoritos.
   - Implementar LiveData u otros mecanismos de observación para que las vistas reaccionen automáticamente a los cambios en los datos.

2. **Repositorios:**

   - Crear métodos en los repositorios para leer y escribir en la base de datos de favoritos.

---

#### **4. Uso de Shared Preferences y Dark Mode**

1. **Shared Preferences para Configuración del Modo Oscuro:**

   - Implementar una opción en el menú de configuración para habilitar o deshabilitar el modo oscuro utilizando Shared Preferences.
   - Guardar el estado del modo oscuro (activado/desactivado) en Shared Preferences.
   - Recuperar el estado al iniciar la aplicación y aplicar el tema correspondiente.

2. **Cambio de Tema Dinámico:**

   - Diseñar dos temas diferentes en `styles.xml`: uno claro y otro oscuro.
   - Crear un botón en el DashboardActivity para alternar entre los modos claro y oscuro.
   - Usar Shared Preferences para guardar la selección del usuario y aplicar dinámicamente el tema seleccionado.

3. **Ejemplo de Código:**

   - Guardar preferencia:
     ```java
     SharedPreferences sharedPref = getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
     SharedPreferences.Editor editor = sharedPref.edit();
     editor.putBoolean("darkMode", true); // Cambiar a false si se desactiva
     editor.apply();
     ```
   - Recuperar preferencia al iniciar:
     ```java
     SharedPreferences sharedPref = getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
     boolean darkMode = sharedPref.getBoolean("darkMode", false);
     if (darkMode) {
         setTheme(R.style.DarkTheme);
     } else {
         setTheme(R.style.LightTheme);
     }
     ```
   - Cambiar tema dinámicamente:
     ```java
     Button themeButton = findViewById(R.id.themeButton);
     themeButton.setOnClickListener(view -> {
         boolean isDarkMode = sharedPref.getBoolean("darkMode", false);
         SharedPreferences.Editor editor = sharedPref.edit();
         editor.putBoolean("darkMode", !isDarkMode);
         editor.apply();
         recreate();
     });
     ```

4. **Pruebas de Modo Oscuro:**

   - Verificar que el cambio de tema se aplica correctamente en todas las pantallas.
   - Asegurarse de que los colores y elementos visuales cumplen con los estándares de contraste tanto en modo claro como oscuro.

---

### **Entregables**

1. **Repositorio GitHub:**

   - Código subido a la rama `semana3`.
   - Commits con mensajes descriptivos que comiencen por: `Semana3: [descripción]`.

2. **Vídeo:**

   - Mostrar la funcionalidad:
     - Prueba del modo oscuro
     - Agregar o eliminar favoritos desde el DetailActivity.
     - Visualización de los favoritos en el RecyclerView dedicado.
     - Pruebas de accesibilidad con TalkBack.
     - Mostrar que los colores y fuentes de tu app superan la validación de Contrast Checker.


---

### **Recursos Adicionales**

- [Guía oficial de RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview)
- [Firebase Realtime Database](https://firebase.google.com/docs/database/android/start)
- [Accesibilidad en Android](https://developer.android.com/guide/topics/ui/accessibility)




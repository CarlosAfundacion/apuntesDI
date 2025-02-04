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
2. Añade un botón o switch para activar y desactivar el DarkMode en tu aplcicación.
3. Desde DashboardActivity debe poder navegarse a FavouritesActivity
4. Añadir un RecyclerView en FavouritesActivity que muestre únicamente los elementos marcados como favoritos por el usuario.
5. Integrar un botón flotante (Floating Action Button) en el DetailActivity que permita agregar o eliminar un elemento de la lista de favoritos del usuario.
6. Mejorar la accesibilidad de las pantallas utilizando las prácticas descritas en los apuntes. Añade el contentDescription a las imágenes del RecyclerView
7. Consolidar el uso del patrón MVVM para esta nueva funcionalidad.

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

   - Usa los temas que exportaste de Material Theme Builder, si los temas App
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
        AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
        recreate(); // recrea la activity para que se aplique el tema sin reiniciar.
     } else {
        AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
        recreate(); // recrea la activity para que se aplique el tema sin reiniciar.
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
     - Mostrar que los colores y fuentes de tu app superan la validación de Contrast Checker.


---

### **Recursos Adicionales**

- [Guía oficial de RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview)
- [Firebase Realtime Database](https://firebase.google.com/docs/database/android/start)
- [Accesibilidad en Android](https://developer.android.com/guide/topics/ui/accessibility)

# Semana 4: Migración a Fragments, Navigation Drawer y Configuración de Usuario

## 1. Introducción y Contexto

Hasta la Semana 3 habíamos creado varias Activities, cada una con su funcionalidad:
- `DashboardActivity` (lista principal de elementos).
- `FavouritesActivity` (lista de favoritos).
- `DetailActivity` (detalle de un elemento).
- Otras pantallas como `LoginActivity` y `RegisterActivity`.

Ahora, el objetivo es:
1. **Reemplazar** algunas de estas pantallas por **Fragments**.
2. Utilizar un **Navigation Drawer** (o menú lateral) para navegar fácilmente entre ellas.
3. Añadir una **pantalla de configuración de usuario** (o `ProfileFragment`) que permita cambiar la contraseña y alternar el modo claro/oscuro.
4. Incluir la opción de **Logout** en el menú lateral, que cierre sesión en Firebase y lleve de vuelta a la pantalla de Login.

Este enfoque mejora la arquitectura, reduce código duplicado y ofrece una navegación más fluida para el usuario.

---

## 2. Migración de Activities a Fragments

En las semanas anteriores, cada pantalla estaba implementada como una Activity independiente. Ahora migraremos las siguientes a Fragments:

- `DashboardFragment` (en lugar de `DashboardActivity`)
- `FavouritesFragment` (en lugar de `FavouritesActivity`)
- `DetailFragment` (en lugar de `DetailActivity`)
- `ProfileFragment` (pantalla de usuario: cambio de contraseña, cambio de tema, etc.)

### 2.1. Creación de Fragments

En tu carpeta `views`, crea archivos Java para cada Fragment. Por ejemplo, en Java:

```java
public class DashboardFragment extends Fragment {
    // Constructor vacío
    public DashboardFragment() { }

    // Se infla el layout del fragment
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Infla el layout del fragment
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);
        // Aquí configuras RecyclerView, adapters, etc.
        return view;
    }
}
```

- **Inflar Layout**: Apunta a un layout XML que será `fragment_dashboard.xml`.  
- Haz lo mismo para `FavouritesFragment`, `ProfileFragment` y `DetailFragment`.

> **Nota**: El contenido que tenías en tu antigua `DashboardActivity` (RecyclerView, Adaptador, etc.) se mueve ahora dentro de `DashboardFragment`, adaptado al ciclo de vida de un Fragment.

### 2.2. Layouts para cada Fragment

Crea (o copia) los layouts en `res/layout/`:
- `fragment_dashboard.xml`
- `fragment_favourites.xml`
- `fragment_profile.xml`
- `fragment_detail.xml`

Por ejemplo, `fragment_dashboard.xml` podría incluir tu `RecyclerView` principal:

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <!-- Si usas DataBinding, puedes declarar variables o ViewModel aquí -->
    </data>
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/dashboardRecyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
            
    </LinearLayout>
</layout>
```

---

## 3. Creación de una MainActivity con Navigation Drawer

Ahora necesitamos una **Activity contenedora** que muestre estos Fragments. El patrón más común es un `Navigation Drawer`.

### 3.1. Estructura XML del Navigation Drawer

1. Crea o edita el layout de tu `MainActivity`: `res/layout/activity_main.xml`.
2. Incluye un `DrawerLayout` (contenedor principal) y dentro, un contenedor para tu **Fragment** principal y el `NavigationView`:

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <!-- Si usas Data Binding, declara variables/objetc-->
    </data>

    <androidx.drawerlayout.widget.DrawerLayout
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        
        <!-- Contenedor principal para el Fragment -->
        <FrameLayout
            android:id="@+id/fragmentContainer"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <!-- NavigationView con el menú lateral -->
        <com.google.android.material.navigation.NavigationView
            android:id="@+id/navigationView"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:menu="@menu/drawer_menu" />
            
    </androidx.drawerlayout.widget.DrawerLayout>
</layout>
```

- `DrawerLayout`: Layout raíz que contiene todo.
- `FrameLayout`: Donde vamos colocando los **Fragments** cuando el usuario selecciona alguna sección.
- `NavigationView`: Muestra las opciones del Drawer, referenciando un archivo de menú (ej. `drawer_menu.xml`).

### 3.2. Menú del Drawer: `res/menu/drawer_menu.xml`

Crea el menú del Drawer:

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/nav_dashboard"
        android:title="Dashboard"
        android:icon="@drawable/ic_dashboard" />

    <item
        android:id="@+id/nav_favourites"
        android:title="Favoritos"
        android:icon="@drawable/ic_favorite" />

    <item
        android:id="@+id/nav_profile"
        android:title="Perfil"
        android:icon="@drawable/ic_person" />

    <item
        android:id="@+id/nav_logout"
        android:title="Cerrar Sesión"
        android:icon="@drawable/ic_logout" />
</menu>
```

- Ajusta los `drawable/` de iconos a tu preferencia.

### 3.3. Lógica en MainActivity

Crea tu `MainActivity` y vincúlala al layout. Ejemplo con Data Binding:

```java
public class MainActivity extends AppCompatActivity {

    private ActivityMainBinding binding;  // DataBinding

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        
        // Manejamos los eventos del menú lateral
        binding.navigationView.setNavigationItemSelectedListener(item -> {
            switch (item.getItemId()) {
                case R.id.nav_dashboard:
                    openFragment(new DashboardFragment());
                    break;
                case R.id.nav_favourites:
                    openFragment(new FavouritesFragment());
                    break;
                case R.id.nav_profile:
                    openFragment(new ProfileFragment());
                    break;
                case R.id.nav_logout:
                    logoutUser();
                    break;
            }
            // Al pulsar en un ítem, cerramos el drawer
            binding.drawerLayout.closeDrawers();
            return true;
        });

        // Cargar por defecto el DashboardFragment
        if (savedInstanceState == null) {
            openFragment(new DashboardFragment());
        }
    }

    private void openFragment(Fragment fragment) {
        getSupportFragmentManager()
                .beginTransaction()
                .replace(R.id.fragmentContainer, fragment)
                .commit();
    }

    private void logoutUser() {
        FirebaseAuth.getInstance().signOut();
        // Redireccionar a LoginActivity
        Intent intent = new Intent(this, LoginActivity.class);
        startActivity(intent);
        finish(); // Para que no pueda volver con el botón atrás
    }
}
```

**Puntos clave**:
- `navigationView.setNavigationItemSelectedListener(...)`: Detecta cuándo se pulsa un ítem en el Drawer y ejecuta la acción correspondiente.
- `openFragment(...)`: Realiza el `replace` del contenedor (`fragmentContainer`) con el Fragment que deseamos.
- `logoutUser()`: Cierra sesión de Firebase y dirige al usuario a `LoginActivity`.

---

## 4. Implementación de la Pantalla de Perfil (ProfileFragment)

Una de las novedades es el **cambio de contraseña** y la configuración de **tema**. Vamos a centralizar esto en un `ProfileFragment`.

### 4.1. Layout de `fragment_profile.xml`

Ejemplo sencillo:

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <!-- ViewModel o variables de Data Binding, si aplicas -->
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">
        
        <!-- Campos para cambio de contraseña -->
        <EditText
            android:id="@+id/currentPasswordEditText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Contraseña actual"
            android:inputType="textPassword"/>

        <EditText
            android:id="@+id/newPasswordEditText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Nueva contraseña"
            android:inputType="textPassword"/>

        <Button
            android:id="@+id/changePasswordButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Cambiar contraseña"/>

        <!-- Switch para modo claro/oscuro -->
        <Switch
            android:id="@+id/darkModeSwitch"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Modo oscuro"/>

    </LinearLayout>
</layout>
```

### 4.2. Lógica de cambio de contraseña en ProfileFragment

```java
public class ProfileFragment extends Fragment {

    private EditText currentPasswordEditText, newPasswordEditText;
    private Switch darkModeSwitch;

    public ProfileFragment() { }

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_profile, container, false);

        currentPasswordEditText = view.findViewById(R.id.currentPasswordEditText);
        newPasswordEditText = view.findViewById(R.id.newPasswordEditText);
        darkModeSwitch = view.findViewById(R.id.darkModeSwitch);

        // Estado inicial del switch (obtener de SharedPreferences)
        SharedPreferences prefs = requireActivity().getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
        boolean isDarkMode = prefs.getBoolean("darkMode", false);
        darkModeSwitch.setChecked(isDarkMode);

        // Listener para botón "Cambiar contraseña"
        Button changePasswordButton = view.findViewById(R.id.changePasswordButton);
        changePasswordButton.setOnClickListener(v -> changePassword());

        // Listener para alternar modo oscuro/claro
        darkModeSwitch.setOnCheckedChangeListener((compoundButton, checked) -> toggleDarkMode(checked));

        return view;
    }

    private void changePassword() {
        String currentPass = currentPasswordEditText.getText().toString();
        String newPass = newPasswordEditText.getText().toString();

        // Primero reautenticar al usuario (opcional) o directamente update:
        FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();
        if (user != null && !newPass.isEmpty()) {
            // EJEMPLO: reautenticación con la credencial actual (si quieres ser estricto con la seguridad)
            AuthCredential credential = EmailAuthProvider
                    .getCredential(user.getEmail(), currentPass);

            user.reauthenticate(credential).addOnCompleteListener(task -> {
                if (task.isSuccessful()) {
                    user.updatePassword(newPass).addOnCompleteListener(updateTask -> {
                        if (updateTask.isSuccessful()) {
                            Toast.makeText(getContext(), "Contraseña cambiada con éxito", Toast.LENGTH_SHORT).show();
                        } else {
                            Toast.makeText(getContext(), "Error al cambiar la contraseña", Toast.LENGTH_SHORT).show();
                        }
                    });
                } else {
                    Toast.makeText(getContext(), "La contraseña actual no es correcta", Toast.LENGTH_SHORT).show();
                }
            });
        }
    }

    private void toggleDarkMode(boolean enableDarkMode) {
        // Guardamos la preferencia
        SharedPreferences prefs = requireActivity().getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
        prefs.edit().putBoolean("darkMode", enableDarkMode).apply();

        // Aplicamos el tema
        if (enableDarkMode) {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
        } else {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
        }
        // Recreamos la activity para que se aplique el cambio
        requireActivity().recreate();
    }
}
```

**Explicaciones clave**:
- Para cambiar la contraseña en Firebase se usa `user.updatePassword(newPassword)`.
- O, más recomendable, se pide la contraseña actual y se reautentica con `user.reauthenticate(...)` antes de actualizar.
- El modo oscuro se controla con `AppCompatDelegate.setDefaultNightMode(...)`; guardamos el estado en `SharedPreferences` y `recreate()` la Activity para que recargue el tema.

---

## 5. Navegación al DetailFragment desde el RecyclerView (DashboardFragment)

En la semana anterior, el `DashboardActivity` abría un `DetailActivity` con un Intent. Ahora, dentro de tu `DashboardFragment`, cuando el usuario pulse un ítem, reemplazamos el Fragment actual por el `DetailFragment`.

### 5.1. Pasar datos al DetailFragment

Para pasar datos a un fragment, se suelen usar **Bundle**:

```java
public class DashboardFragment extends Fragment {

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);

        RecyclerView recyclerView = view.findViewById(R.id.dashboardRecyclerView);
        // Configura layout manager, adapter, etc.

        // Ejemplo de adapter con callback al pulsar un ítem
        MyAdapter adapter = new MyAdapter(item -> {
            // Al hacer click en un ítem del RecyclerView
            DetailFragment detailFragment = new DetailFragment();

            // Pasar datos con un Bundle
            Bundle bundle = new Bundle();
            bundle.putString("ITEM_ID", item.getId());
            detailFragment.setArguments(bundle);

            // Reemplazar el fragment
            requireActivity().getSupportFragmentManager()
                    .beginTransaction()
                    .replace(R.id.fragmentContainer, detailFragment)
                    .addToBackStack(null) // Permite volver con el botón "atrás"
                    .commit();
        });

        recyclerView.setAdapter(adapter);

        return view;
    }
}
```

### 5.2. Recibir datos en DetailFragment

En tu `DetailFragment`:

```java
public class DetailFragment extends Fragment {

    private String itemId;  // ID del elemento a mostrar

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_detail, container, false);

        // Recuperar argumentos
        if (getArguments() != null) {
            itemId = getArguments().getString("ITEM_ID");
        }

        // A partir de itemId, carga datos de Firebase o en tu ViewModel
        // Ejemplo: detailViewModel.loadItem(itemId);

        return view;
    }
}
```

De esta forma, la navegación a la pantalla de detalle ya no se hace con un Intent, sino con transacciones de Fragments y Bundle.

---

## 6. Logout desde el Drawer

En la sección `navigationView.setNavigationItemSelectedListener(...)` de `MainActivity` incluimos el caso `R.id.nav_logout` con la llamada:

```java
private void logoutUser() {
    FirebaseAuth.getInstance().signOut();
    Intent intent = new Intent(this, LoginActivity.class);
    startActivity(intent);
    finish();
}
```

Esto cerrará la sesión y volverá a la pantalla de `LoginActivity`, que ya tenías en semanas anteriores.

---

## 7. Consolidación de MVVM

Asegúrate de mantener la **arquitectura** ya establecida:
- **ViewModels** para cada fragment si manejan lógicas diferentes (por ejemplo, `DashboardViewModel`, `FavouritesViewModel`).
- **Repositorios** centralizando la comunicación con Firebase.
- **Models** representando tus datos (`Recipe`, `Game`, `Contact`, etc.).
- Las **Vistas (Fragments)** interactúan con sus ViewModels mediante `LiveData`.

### 7.1. Ejemplo de un Fragment con MVVM

Supongamos `DashboardFragment` usa un `DashboardViewModel`:

```java
public class DashboardFragment extends Fragment {
    private DashboardViewModel viewModel;
    private RecyclerView recyclerView;
    private MyAdapter adapter;

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);

        // Configurar RecyclerView
        recyclerView = view.findViewById(R.id.dashboardRecyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(getContext()));
        adapter = new MyAdapter(item -> {/* Navegar a detalle... */});
        recyclerView.setAdapter(adapter);

        // Obtener ViewModel
        viewModel = new ViewModelProvider(this).get(DashboardViewModel.class);
        // Observar cambios en la lista
        viewModel.getItemsLiveData().observe(getViewLifecycleOwner(), items -> {
            adapter.setItems(items);
            adapter.notifyDataSetChanged();
        });

        return view;
    }
}
```

- `getViewLifecycleOwner()` en Fragments se usa para observar LiveData sin fugas de memoria.
- `DashboardViewModel` podría cargar la lista desde Firebase a través de un `DashboardRepository`.

---

## 8. Ajustes Finales y Buenas Prácticas

1. **Accesibilidad**: Revisa que tus Fragments mantengan `contentDescription` en las imágenes, buen contraste, etc. (Semana 3).
2. **Material Theme**: Si usas Material Theme Builder, confirma que tanto el **modo claro** como el **oscuro** se apliquen correctamente en el `MainActivity`.
3. **Back Stack**: Cuando navegues a un `DetailFragment`, a veces querrás usar `addToBackStack(null)` en la transacción para permitir que el usuario regrese al fragmento anterior con el botón de retroceso.

---

## 9. Resumen de Pasos

1. **Crear/editar** la `MainActivity` con un `DrawerLayout` y un `NavigationView`.  
2. **Definir** el menú del Drawer en `res/menu/drawer_menu.xml`.  
3. **Migrar** `DashboardActivity`, `FavouritesActivity`, `DetailActivity` a sus correspondientes Fragments.  
4. **Crear** el `ProfileFragment` para el usuario: 
   - Cambio de contraseña con `FirebaseAuth`.
   - Toggle de tema oscuro/claro con `SharedPreferences` y `AppCompatDelegate`.
5. **Configurar** la navegación en `MainActivity`:
   - `openFragment(...)` para reemplazar el contenedor.  
   - `logoutUser()` para cerrar sesión.  
6. **Probar** la navegación:
   - Seleccionar Dashboard, Favoritos, Perfil, etc.
   - Realizar logout y volver al Login.  
7. **Mantener** la lógica MVVM (ViewModels + Repositorios).  
8. **Verificar** accesibilidad y tema (claro/oscuro).  

Con esto habrás logrado una estructura más robusta, fácilmente escalable, con navegación centralizada y buenas prácticas de arquitectura.

---
### **Enunciado Semana 4: Migración a Fragments, Navigation Drawer y Configuración de Usuario**

#### **Objetivo**
En esta semana, el objetivo es continuar con la implementación de la aplicación móvil Android que estamos desarrollando. Esta vez, nos enfocaremos en reemplazar las Activities por Fragments, implementar un `Navigation Drawer` para la navegación fluida entre secciones y permitir la configuración del usuario desde el perfil. A lo largo de la semana, implementaremos la funcionalidad para cambiar la contraseña y alternar entre el modo claro y oscuro desde el `ProfileFragment`. También se incluirá la opción de hacer `logout` desde el menú lateral.

#### **Tareas**

1. **Implementación de una `SplashScreen`**
   
3. **Migración de Activities a Fragments**  
   - **DashboardActivity** → `DashboardFragment`
   - **FavouritesActivity** → `FavouritesFragment`
   - **DetailActivity** → `DetailFragment`
   - **ProfileActivity** → `ProfileFragment`

   Asegúrate de que cada `Fragment` cargue la interfaz correspondiente y mantenga la funcionalidad prevista (e.g., lista de elementos en el `DashboardFragment`, favoritos en el `FavouritesFragment`, etc.).

4. **Implementación del Navigation Drawer**
   - Crea un `DrawerLayout` y un `NavigationView` en `MainActivity` para gestionar la navegación entre los diferentes `Fragments`. Puedes usar la plantilla del propio `Android Studio` añadiendo una nueva activity de tipo `NavigationDrawer`.
   - Enlaza cada item del menú con su correspondiente Fragment: `DashboardFragment`, `FavouritesFragment`, `ProfileFragment`, y la opción de hacer `logout`.
   - Utiliza el patrón de `FragmentTransaction` para reemplazar los fragments dentro del `MainActivity`.

5. **ProfileFragment: Cambio de Contraseña y Tema**
   - Implementa un `ProfileFragment` que permita al usuario cambiar su contraseña y alternar entre el modo claro y oscuro.
   - **Cambio de Contraseña**: Usa `FirebaseAuth` para cambiar la contraseña del usuario autenticado.
   - **Modo Oscuro**: Implementa un `Switch` para alternar entre el modo claro y oscuro, y guarda la preferencia usando `SharedPreferences`.

6. **Logout**
   - Añade la opción de `logout` en el menú del `NavigationDrawer`.
   - Al hacer logout, cierra la sesión de `FirebaseAuth` y redirige al usuario de vuelta a la pantalla de login.

7. **Refactorización del código y uso de MVVM**
   - Mantén la arquitectura MVVM para todas las pantallas que impliquen lógica de negocio (e.g., `DashboardViewModel`, `ProfileViewModel`).
   - Implementa `LiveData` para que los cambios de datos en el ViewModel actualicen la interfaz de usuario automáticamente.

8. **Navegación a DetailFragment desde RecyclerView**
   - En `DashboardFragment`, cuando el usuario seleccione un ítem, navega a `DetailFragment` y pasa los datos correspondientes mediante un `Bundle`.

#### **Entregables**

1. **Código en GitHub**
   - Subir el código a la rama `semana4`.
   - Realizar commits atómicos con el prefijo “Semana4: [Descripción del cambio]”.
   - Al finalizar la semana, mergear la rama `semana4` con `main`.

2. **Vídeo Demostrativo**
   - Graba un vídeo demostrativo mostrando las siguientes funcionalidades:
     - La implementación de la `Splash Screen`.
     - Navegación entre los Fragments usando el `Navigation Drawer` (Dashboard, Favoritos, Perfil, Detalle).
     - Cambio de contraseña desde el `ProfileFragment`.
     - Cambio de tema entre claro y oscuro desde el `ProfileFragment`.
     - Logout y retorno a la pantalla de `LoginActivity`.

3. **En Classroom**
   - Subir el enlace al repositorio de GitHub.
   - Subir un archivo ZIP con el proyecto Android Studio.
   - Subir el vídeo demostrativo.



      

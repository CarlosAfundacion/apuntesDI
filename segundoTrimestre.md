**Semana 1: Introducción a Firebase**

### **Objetivo**

En esta semana aprenderás los conceptos básicos de Firebase, incluyendo cómo configurar un proyecto, implementar autenticación y trabajar con la base de datos en tiempo real utilizando el usuario autenticado. Cada instrucción y método se explica en detalle para que puedas aplicar este conocimiento de forma autónoma.

---

### **1. Introducción a Firebase**

Firebase es una plataforma de desarrollo de aplicaciones que proporciona diversas herramientas para crear apps escalables, rápidas y con funcionalidades avanzadas. Trabajaremos principalmente con:

- **Autenticación:** Gestionar usuarios que pueden iniciar sesión en la aplicación.
- **Base de datos en tiempo real (Realtime Database):** Almacenar y sincronizar datos entre usuarios en tiempo real.

---

### **2. Configuración de Firebase en Android Studio**

#### **Paso 1: Crear un proyecto en Firebase**

1. Accede a la [Consola de Firebase](https://console.firebase.google.com/).
2. Haz clic en **Agregar proyecto**.
3. Introduce un nombre para el proyecto (por ejemplo, `MyFirebaseApp`) y completa la configuración.

#### **Paso 2: Registrar tu app Android en Firebase**

1. Selecciona **Agregar app** y elige Android.
2. Ingresa el nombre del paquete de tu app (puedes encontrarlo en `AndroidManifest.xml`, atributo `package`).
3. Descarga el archivo `google-services.json` proporcionado y cópialo en la carpeta `app/` del proyecto.

#### **Paso 3: Configurar Firebase en Android Studio**

1. Sigue los pasos de configuración del gradle que indica Firebase.
2. Sincroniza el proyecto para aplicar los cambios y asegúrate de que no haya errores.

---

### **3. Implementación de Firebase Authentication**

#### **Paso 1: Diseñar la interfaz de usuario**

En `activity_main.xml`, agrega los siguientes elementos:

```xml
<EditText
    android:id="@+id/emailEditText"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Email" />

<EditText
    android:id="@+id/passwordEditText"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Password"
    android:inputType="textPassword" />

<Button
    android:id="@+id/registerButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Register" />

<Button
    android:id="@+id/loginButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Login" />
```

#### **Paso 2: Registro de nuevos usuarios**

En `MainActivity.java`:

1. Importa las bibliotecas necesarias:
   ```java
   import com.google.firebase.auth.FirebaseAuth;
   ```
2. Inicializa Firebase Authentication y configura el botón de registro:
   ```java
   public class MainActivity extends AppCompatActivity {
       private FirebaseAuth mAuth;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           mAuth = FirebaseAuth.getInstance();

           findViewById(R.id.registerButton).setOnClickListener(v -> registerUser());
       }

       private void registerUser() {
           String email = ((EditText) findViewById(R.id.emailEditText)).getText().toString();
           String password = ((EditText) findViewById(R.id.passwordEditText)).getText().toString();

           mAuth.createUserWithEmailAndPassword(email, password)
               .addOnCompleteListener(this, task -> {
                   if (task.isSuccessful()) {
                       Toast.makeText(MainActivity.this, "Usuario registrado correctamente.", Toast.LENGTH_SHORT).show();
                   } else {
                       Toast.makeText(MainActivity.this, "Error en el registro: " + task.getException().getMessage(), Toast.LENGTH_SHORT).show();
                   }
               });
       }
   }
   ```

**Explicación:**

- `FirebaseAuth.getInstance()`: Devuelve la instancia de autenticación de Firebase.
- `createUserWithEmailAndPassword(email, password)`: Registra un nuevo usuario con el correo y contraseña proporcionados.
- `addOnCompleteListener`: Se ejecuta al completar el registro, indicando éxito o error.
- `getException().getMessage()`: Recupera el mensaje del error, si ocurre.

#### **Paso 3: Inicio de sesión de usuarios**

Configura el botón de inicio de sesión:

```java
findViewById(R.id.loginButton).setOnClickListener(v -> loginUser());

private void loginUser() {
    String email = ((EditText) findViewById(R.id.emailEditText)).getText().toString();
    String password = ((EditText) findViewById(R.id.passwordEditText)).getText().toString();

    mAuth.signInWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, task -> {
            if (task.isSuccessful()) {
                Toast.makeText(MainActivity.this, "Inicio de sesión exitoso.", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(MainActivity.this, "Error en autenticación.", Toast.LENGTH_SHORT).show();
            }
        });
}
```

**Explicación:**

- `signInWithEmailAndPassword(email, password)`: Autentica al usuario con las credenciales proporcionadas.
- `Toast`: Muestra un mensaje en pantalla para retroalimentar al usuario.

---

### **4. Firebase Realtime Database**

#### **4.1. Configurar reglas de la base de datos**

En Firebase Console, navega a **Realtime Database** > **Reglas**. Configura las reglas para permitir acceso autenticado:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

#### **4.2. Consultar datos en Firebase**

Para consultar datos de una tabla en Firebase, utiliza un `ValueEventListener`:

```java
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference("users");

ValueEventListener userListener = new ValueEventListener() {
    @Override
    public void onDataChange(@NonNull DataSnapshot dataSnapshot) {
        for (DataSnapshot userSnapshot : dataSnapshot.getChildren()) {
            String userName = userSnapshot.child("name").getValue(String.class);
            Log.d("Firebase", "Nombre del usuario: " + userName);
        }
    }

    @Override
    public void onCancelled(@NonNull DatabaseError databaseError) {
        Log.w("Firebase", "Error al leer datos", databaseError.toException());
    }
};

databaseRef.addListenerForSingleValueEvent(userListener);
```

**Explicación:**

- `DatabaseReference`: Representa una referencia a una ubicación en la base de datos.
- `ValueEventListener`: Es un oyente que se utiliza para leer datos de Firebase. Contiene dos métodos principales:
  - `onDataChange`: Se ejecuta cuando se obtienen datos de la base de datos. Los datos se devuelven como un objeto `DataSnapshot` que permite acceder a los valores almacenados.
  - `onCancelled`: Se ejecuta si ocurre algún error al intentar leer los datos.
- `addListenerForSingleValueEvent`: Lee los datos de la base de datos una única vez en lugar de escuchar cambios en tiempo real.
- `DataSnapshot`: Proporciona acceso a los datos reales en el nodo de la base de datos al que se hace referencia.

#### **4.3. Añadir datos a Firebase**

Para añadir datos, utiliza el método `setValue` y `push`:

```java
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference("users");

String userId = databaseRef.push().getKey();
User newUser = new User(userId, "John Doe", "johndoe@example.com");

databaseRef.child(userId).setValue(newUser).addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Usuario añadido correctamente.");
    } else {
        Log.w("Firebase", "Error al añadir usuario.", task.getException());
    }
});
```

**Explicación:**

- `push()`: Genera una clave única para cada nodo hijo.
- `setValue(Object value)`: Escribe un objeto en la base de datos.
- `addOnCompleteListener`: Informa si la operación fue exitosa o falló.

#### **4.4. Modificar datos existentes**

Para modificar datos, utiliza el método `updateChildren`:

```java
DatabaseReference orderRef = FirebaseDatabase.getInstance().getReference("orders/order1");

Map<String, Object> updates = new HashMap<>();
updates.put("price", 1300);
updates.put("product", "Laptop Pro");

orderRef.updateChildren(updates).addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Orden actualizada correctamente.");
    } else {
        Log.w("Firebase", "Error al actualizar la orden.", task.getException());
    }
});
```

**Explicación:**

- `updateChildren(Map<String, Object>)`: Actualiza múltiples campos de un nodo específico.

#### **4.5. Eliminar datos**

Para eliminar órdenes específicas:

```java
DatabaseReference orderRef = FirebaseDatabase.getInstance().getReference("orders/order2");

orderRef.removeValue().addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Orden eliminada correctamente.");
    } else {
        Log.w("Firebase", "Error al eliminar la orden.", task.getException());
    }
});
```

**Explicación:**

- `removeValue()`: Borra el nodo completo de la base de datos asociado a una orden.

#### **Ejemplo avanzado: Modificar un campo anidado en órdenes**

```java
DatabaseReference reviewRef = FirebaseDatabase.getInstance().getReference("orders/order1/reviews/review1");

Map<String, Object> reviewUpdates = new HashMap<>();
reviewUpdates.put("rating", 4);
reviewUpdates.put("comment", "Muy buen producto, pero un poco caro.");

reviewRef.updateChildren(reviewUpdates).addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Reseña actualizada correctamente.");
    } else {
        Log.w("Firebase", "Error al actualizar la reseña.", task.getException());
    }
});
```

**Explicación:**

- Este ejemplo modifica un nodo más profundo en la estructura JSON, mostrando flexibilidad en Firebase.

---




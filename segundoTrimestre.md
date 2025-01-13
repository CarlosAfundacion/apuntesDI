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

1. Sigue los pasos de configuración del gradle que indica Firebase

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
    android:id="@+id/loginButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Login" />
```

#### **Paso 2: Autenticación de usuarios**

En `MainActivity.java`:

1. Importa las bibliotecas necesarias:
   ```java
   import com.google.firebase.auth.FirebaseAuth;
   import com.google.firebase.auth.FirebaseUser;
   ```
2. Inicializa Firebase Authentication y configura el botón de inicio de sesión:
   ```java
   public class MainActivity extends AppCompatActivity {
       private FirebaseAuth mAuth;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           mAuth = FirebaseAuth.getInstance();

           findViewById(R.id.loginButton).setOnClickListener(v -> loginUser());
       }

       private void loginUser() {
           String email = ((EditText) findViewById(R.id.emailEditText)).getText().toString();
           String password = ((EditText) findViewById(R.id.passwordEditText)).getText().toString();

           mAuth.signInWithEmailAndPassword(email, password)
               .addOnCompleteListener(this, task -> {
                   if (task.isSuccessful()) {
                       FirebaseUser user = mAuth.getCurrentUser();
                       Toast.makeText(MainActivity.this, "Login exitoso: " + user.getEmail(), Toast.LENGTH_SHORT).show();
                   } else {
                       Toast.makeText(MainActivity.this, "Error en autenticación.", Toast.LENGTH_SHORT).show();
                   }
               });
       }
   }
   ```

**Explicación:**

- `FirebaseAuth.getInstance()`: Devuelve la instancia de autenticación.
- `signInWithEmailAndPassword(email, password)`: Verifica las credenciales y autentica al usuario.
- `FirebaseUser.getEmail()`: Recupera el email del usuario autenticado.

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

#### **4.3. Modificar datos**

Para modificar datos existentes, utiliza el método `updateChildren`:

```java
DatabaseReference userRef = FirebaseDatabase.getInstance().getReference("users/user123");

Map<String, Object> updates = new HashMap<>();
updates.put("email", "nuevoemail@example.com");
updates.put("phone", "1234567890");

userRef.updateChildren(updates).addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Datos actualizados correctamente.");
    } else {
        Log.w("Firebase", "Error al actualizar datos.", task.getException());
    }
});
```

#### **4.4. Eliminar datos**

Para eliminar datos, utiliza el método `removeValue`:

```java
DatabaseReference userRef = FirebaseDatabase.getInstance().getReference("users/user123");

userRef.removeValue().addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        Log.d("Firebase", "Usuario eliminado correctamente.");
    } else {
        Log.w("Firebase", "Error al eliminar usuario.", task.getException());
    }
});
```

#### **4.5. Añadir datos**

Para añadir nuevos datos, utiliza el método `push` o `setValue`:

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

#### **4.6. Crear tablas con relaciones**

En Firebase, las tablas se representan como nodos en un árbol JSON. Puedes establecer relaciones creando referencias cruzadas:

- **Estructura de ejemplo:**

```json
{
  "users": {
    "user123": {
      "name": "John Doe",
      "email": "johndoe@example.com",
      "orders": {
        "order1": true,
        "order2": true
      }
    }
  },
  "orders": {
    "order1": {
      "product": "Laptop",
      "price": 1000
    },
    "order2": {
      "product": "Phone",
      "price": 500
    }
  }
}
```

- **Acceso a datos relacionados:**

```java
DatabaseReference userOrdersRef = FirebaseDatabase.getInstance().getReference("users/user123/orders");

userOrdersRef.addListenerForSingleValueEvent(new ValueEventListener() {
    @Override
    public void onDataChange(@NonNull DataSnapshot snapshot) {
        for (DataSnapshot orderSnapshot : snapshot.getChildren()) {
            String orderId = orderSnapshot.getKey();
            DatabaseReference orderRef = FirebaseDatabase.getInstance().getReference("orders/" + orderId);
            orderRef.addListenerForSingleValueEvent(new ValueEventListener() {
                @Override
                public void onDataChange(@NonNull DataSnapshot orderSnapshot) {
                    String product = orderSnapshot.child("product").getValue(String.class);
                    Log.d("Firebase", "Producto: " + product);
                }

                @Override
                public void onCancelled(@NonNull DatabaseError error) {
                    Log.w("Firebase", "Error al leer orden", error.toException());
                }
            });
        }
    }

    @Override
    public void onCancelled(@NonNull DatabaseError error) {
        Log.w("Firebase", "Error al leer órdenes del usuario", error.toException());
    }
});
```

# **Semana 1: Introducción a Firebase**

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
4. Activar Realtime Database y Authenticator.

#### **Paso 2: Registrar tu app Android en Firebase**

1. Selecciona **Agregar app** y elige Android.
2. Ingresa el nombre del paquete de tu app (puedes encontrarlo en el modo de vista `Packages`, con formato `com.example.nombreDeTuApp`).
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

Para añadir datos, utiliza el método `setValue`:

```java
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference("users");

FirebaseUser firebaseUser = auth.getCurrentUser();
            if (firebaseUser != null) {
                String uid = firebaseUser.getUid();
                User newUser = new User(uid, "John Doe", "johndoe@example.com");
                databaseRef.child(uid).setValue(newUser)
                    .addOnCompleteListener(dbTask -> {
                        if (dbTask.isSuccessful()) {
                            Log.d("Firebase", "Usuario añadido a Realtime Database con UID: " + uid);
                        } else {
                            Log.e("Firebase", "Error al añadir usuario a la base de datos", dbTask.getException());
                        }
                    });
            }
        } else {
            Log.e("Firebase", "Error al registrar usuario", task.getException());
        }
});
```

**Explicación:**

- `push()`: Genera una clave única para cada nodo hijo, en el ejemplo usamos la Id única proporcionada por Authentication de Firebase. Si queremos añadir un elemento a otra tabla que no sea la de usuario, usaremos `push().getKey()`para hacerlo.
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

# **Semana 2: Patrones de diseño: MVVM (Model-View-ViewModel)**

---

#### **1. Introducción a los patrones de diseño en Android**

##### **1.1. ¿Qué son los patrones de diseño?**

Los patrones de diseño son soluciones generales y reutilizables a problemas comunes en el desarrollo de software. Ayudan a:
- **Separar responsabilidades**, mejorando la organización del código.
- **Facilitar la escalabilidad**, permitiendo que las aplicaciones crezcan sin comprometer la mantenibilidad.
- **Simplificar las pruebas unitarias**, desacoplando la lógica de negocio de la interfaz de usuario.

##### **1.2. Comparativa: MVVM frente a MVC**

###### **MVC (Model-View-Controller):**
- **Model:** Gestiona los datos y la lógica de negocio.
- **View:** Representa la interfaz de usuario.
- **Controller:** Actúa como intermediario entre el Model y la View, gestionando eventos y actualizando la vista manualmente.

**Problemas del MVC en Android:**
- Las Activities o Fragments suelen actuar como Controllers, lo que genera clases excesivamente grandes y difíciles de mantener ("God Classes").
- Las actualizaciones de la interfaz son manuales, lo que incrementa el riesgo de errores.

###### **MVVM (Model-View-ViewModel):**
- **Model:** Igual que en MVC, contiene los datos y la lógica de negocio.
- **View:** Interfaz de usuario (XML) que observa cambios en el ViewModel mediante `LiveData` y Data Binding.
- **ViewModel:** Intermediario entre el Model y la View. Utiliza observables como `LiveData` para exponer datos a la vista.

**Ventajas del MVVM frente al MVC:**
1. **Sincronización automática:** Los cambios en el ViewModel se reflejan directamente en la interfaz.
2. **Mejor organización:** Las Activities y Fragments son más simples, centándose solo en la configuración de la vista.
3. **Testabilidad:** La lógica de negocio y la gestión de datos están desacopladas de la vista, facilitando las pruebas unitarias.

---

#### **2. Estructura de un proyecto MVVM**

Para implementar el patrón MVVM en Android de manera eficiente, organiza tu proyecto en los siguientes paquetes:

1. **models:** Clases que representan los datos de la aplicación.
   - Ejemplo: `Product.java`, `Order.java`, `User.java`.

2. **repositories:** Clases responsables de interactuar con Firebase Realtime Database u otras fuentes de datos.
   - Ejemplo: `ProductRepository.java`, `OrderRepository.java`, `UserRepository.java`.

3. **viewmodels:** Contiene las clases `ViewModel` que exponen los datos y la lógica a las vistas.
   - Ejemplo: `ProductViewModel.java`, `OrderViewModel.java`, `UserViewModel.java`.

4. **views:** Activities, Fragments y adaptadores para los RecyclerView.
   - Ejemplo: `MainActivity.java`, `ProductAdapter.java`.

5. **utils:** (Opcional) Clases auxiliares.

**Estructura del proyecto:**
```
app/
  src/
    main/
      java/
        com.example.app/
          models/
            Product.java
            Order.java
            User.java
          repositories/
            ProductRepository.java
            OrderRepository.java
            UserRepository.java
          viewmodels/
            ProductViewModel.java
            OrderViewModel.java
            UserViewModel.java
          views/
            MainActivity.java
            ProductAdapter.java
          utils/
            FirebaseUtils.java
```

---

#### **3. Configuraciones iniciales en Android Studio**

1. **Gradle:**
   - Habilitar Data Binding en el archivo `build.gradle` de la app:
     ```gradle
     android {
         buildFeatures {
             dataBinding = true
         }
     }
     ```
    - Modificar compileSdk y targetSdk a la versión 35 (En caso de que no esté descargada os pedirá hacerlo) en el archivo `build.gradle` de la app:
         ```gradle
         android {
             namespace = "com.example.mvvm"
             compileSdk = 35
             defaultConfig {
                 applicationId = "com.example.mvvm"
                 minSdk = 26
                 targetSdk = 35
             ...
             }
         ...
         }
         ```
   - Agregar dependencias necesarias:
     ```gradle
     dependencies {
         implementation ("androidx.lifecycle:lifecycle-extensions:2.2.0")
        implementation ("androidx.lifecycle:lifecycle-livedata-ktx:2.8.7")
        implementation ("androidx.lifecycle:lifecycle-viewmodel-ktx:2.8.7")
        implementation ("androidx.recyclerview:recyclerview:1.4.0")
     }
     ```

---

#### **4. Implementación de un RecyclerView con MVVM para productos**

Un **RecyclerView** es un componente de la biblioteca de Android que permite mostrar una lista o colección de datos de manera eficiente, especialmente cuando se trabaja con grandes cantidades de elementos. Se utiliza en reemplazo del antiguo **ListView** y **GridView** debido a su mayor flexibilidad y rendimiento.

Cuando implementamos un **RecyclerView** siguiendo el patrón **MVVM** (Model-View-ViewModel), se estructura de manera que los datos y la lógica de la interfaz estén separados, siguiendo los principios de arquitectura moderna. Aquí se detallan sus principales componentes y cómo encajan en MVVM:

---

## **Principales componentes de RecyclerView en MVVM**

##### 1. **Modelo (Model)**
El modelo representa la fuente de datos que se mostrará en el RecyclerView. Es una clase que define la estructura de los datos. 
##### 2. **Vista (View)**
En este caso, la **View** está compuesta por:

###### a. **RecyclerView en el XML**
Define el RecyclerView en el layout XML del fragmento o actividad:

###### b. **Adaptador (Adapter)**
El adaptador es responsable de vincular los datos con las vistas. Implementa la lógica para asociar cada elemento del modelo con un ViewHolder.

###### c. **ViewHolder**
Un contenedor que almacena referencias a las vistas dentro de cada elemento del RecyclerView. Se define dentro del adaptador.

###### d. **Layout del ítem**
El layout XML para los elementos individuales del RecyclerView, como `item_user.xml`.

##### 3. **ViewModel**
El ViewModel gestiona la lógica y la fuente de datos que se mostrará en la vista. Proporciona los datos al RecyclerView a través de un `LiveData` o `MutableLiveData`.


##### 4. **Enlazar la Vista y el ViewModel**

En el Fragmento o Actividad, enlazas el ViewModel con el RecyclerView.


---

##### **4.1. Modelo: Product.java**

El modelo `Product` representa la estructura de los datos de un producto tal como se almacena en Firebase Realtime Database y como se utiliza en la aplicación.

```java
public class Product {
    private String id;
    private String name;
    private double price;

    public Product() {}

    public Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    public String getId() { return id; }
    public String getName() { return name; }
    public double getPrice() { return price; }
}
```

**Explicación:**
- **Propiedades:**
  - `id`: Identificador único del producto.
  - `name`: Nombre del producto.
  - `price`: Precio del producto.
- **Constructor vacío:** Necesario para que Firebase pueda deserializar los datos automáticamente.
- **Constructor completo:** Útil para inicializar manualmente un objeto de tipo `Product`.
- **Métodos `get`:** Permiten acceder a las propiedades del objeto, utilizados en Data Binding y lógica de negocio.


##### **4.2. Repositorio: ProductRepository.java**

El repositorio `ProductRepository` se encarga de interactuar con Firebase Realtime Database para obtener los datos de los productos y exponerlos a través de `LiveData`.

```java
import com.google.firebase.database.*;
import androidx.lifecycle.MutableLiveData;
import java.util.ArrayList;
import java.util.List;

public class ProductRepository {
    private final DatabaseReference productRef;

    public ProductRepository() {
        productRef = FirebaseDatabase.getInstance().getReference("products");
    }

    public void getProducts(MutableLiveData<List<Product>> productLiveData) {
        productRef.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot snapshot) {
                List<Product> products = new ArrayList<>();
                for (DataSnapshot child : snapshot.getChildren()) {
                    Product product = child.getValue(Product.class);
                    products.add(product);
                }
                productLiveData.setValue(products);
            }

            @Override
            public void onCancelled(DatabaseError error) {
                // Manejo de errores
            }
        });
    }
}
```

**Explicación:**
- **`DatabaseReference productRef`:** Referencia al nodo `products` en Firebase Realtime Database.
- **`getProducts`:** Método que escucha los cambios en la base de datos y actualiza `MutableLiveData` con la lista de productos.
- **`ValueEventListener`:**
  - **`onDataChange`:** Se ejecuta cuando hay cambios en la base de datos. Recupera los productos y los almacena en una lista.
  - **`onCancelled`:** Maneja posibles errores en la consulta.


##### **4.3. ViewModel: ProductViewModel.java**

El `ProductViewModel` actúa como intermediario entre el repositorio y la vista. Expone los datos observables de los productos mediante `LiveData`.

```java
import androidx.lifecycle.LiveData;
import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;
import java.util.List;

public class ProductViewModel extends ViewModel {
    private final MutableLiveData<List<Product>> productLiveData = new MutableLiveData<>();
    private final ProductRepository productRepository;

    public ProductViewModel() {
        productRepository = new ProductRepository();
        loadProducts();
    }

    public LiveData<List<Product>> getProductLiveData() {
        return productLiveData;
    }

    private void loadProducts() {
        productRepository.getProducts(productLiveData);
    }
}
```

**Explicación:**
- **`MutableLiveData<List<Product>>`:**
  - Un contenedor observable que mantiene la lista de productos.
  - Actualiza automáticamente la vista cuando cambian los datos.
- **`getProductLiveData`:** Expone los datos como `LiveData` para que puedan ser observados desde la vista.
- **`loadProducts`:** Llama al repositorio para obtener los productos y actualizar los datos observables.


##### **4.4. Adaptador del RecyclerView: ProductAdapter.java**

El `ProductAdapter` conecta los datos del modelo con las vistas del RecyclerView.

```java
import android.view.LayoutInflater;
import android.view.ViewGroup;
import androidx.annotation.NonNull;
import androidx.databinding.DataBindingUtil;
import androidx.recyclerview.widget.RecyclerView;
import com.example.app.databinding.ItemProductBinding;
import java.util.List;

public class ProductAdapter extends RecyclerView.Adapter<ProductAdapter.ProductViewHolder> {
    private List<Product> products;

    public ProductAdapter(List<Product> products) {
        this.products = products;
    }

    public void setProducts(List<Product> products) {
        this.products = products;
        notifyDataSetChanged();
    }

    @NonNull
    @Override
    public ProductViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        ItemProductBinding binding = DataBindingUtil.inflate(
            LayoutInflater.from(parent.getContext()),
            R.layout.item_product,
            parent,
            false
        );
        return new ProductViewHolder(binding);
    }

    @Override
    public void onBindViewHolder(@NonNull ProductViewHolder holder, int position) {
        Product product = products.get(position);
        holder.bind(product);
    }

    @Override
    public int getItemCount() {
        return products != null ? products.size() : 0;
    }

    static class ProductViewHolder extends RecyclerView.ViewHolder {
        private final ItemProductBinding binding;

        public ProductViewHolder(@NonNull ItemProductBinding binding) {
            super(binding.getRoot());
            this.binding = binding;
        }

        public void bind(Product product) {
            binding.setProduct(product);
            binding.executePendingBindings();
        }
    }
}
```

**Explicación:**
- **`DataBindingUtil.inflate`:** Infla el diseño XML vinculado al adaptador.
- **`bind`:** Vincula un objeto `Product` con las vistas declaradas en `item_product.xml`.
- **`setProducts`:** Actualiza la lista de productos y notifica cambios al RecyclerView.



###### **Archivo XML: item_product.xml**

Este archivo define la estructura visual de cada elemento del RecyclerView. Utiliza Data Binding para vincular datos del modelo `Product` a los componentes de la interfaz.

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="product"
            type="com.example.app.models.Product" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:id="@+id/productName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{product.name}" />

        <TextView
            android:id="@+id/productPrice"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{String.valueOf(product.price)}" />

    </LinearLayout>
</layout>
```

**Explicación:**
- **`<layout>`:** Habilita Data Binding en este archivo XML.
- **`<variable>`:** Declara la variable `product` de tipo `Product`, accesible en este diseño.
- **`@{}`:** Permite acceder a propiedades del objeto `Product` y vincularlas directamente a las vistas.



##### **4.5. Configuración del RecyclerView en MainActivity.java**

En esta sección se configura el RecyclerView para mostrar los productos cargados desde Firebase Realtime Database. El RecyclerView está vinculado al adaptador `ProductAdapter` y observa los datos expuestos por el `ProductViewModel`.

```java
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.databinding.DataBindingUtil;
import androidx.lifecycle.ViewModelProvider;
import androidx.recyclerview.widget.LinearLayoutManager;
import com.example.app.databinding.ActivityMainBinding;
import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    private ProductViewModel productViewModel;
    private ProductAdapter productAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);

        productAdapter = new ProductAdapter(new ArrayList<>());
        binding.recyclerView.setLayoutManager(new LinearLayoutManager(this));
        binding.recyclerView.setAdapter(productAdapter);

        productViewModel = new ViewModelProvider(this).get(ProductViewModel.class);
        productViewModel.getProductLiveData().observe(this, products -> productAdapter.setProducts(products));
    }
}
```

**Explicación:**
- **`ActivityMainBinding`:**
  - Vincula automáticamente el diseño XML (`activity_main.xml`) con la actividad usando Data Binding.
  - Proporciona acceso directo a los elementos definidos en el XML, como el RecyclerView (`binding.recyclerView`).

- **`ProductAdapter`:**
  - Conecta la lista de productos con las vistas del RecyclerView.
  - El método `setProducts` actualiza la lista de productos y notifica cambios al RecyclerView.

- **`LinearLayoutManager`:**
  - Administra cómo se organizan los elementos en el RecyclerView (en este caso, en una lista vertical).

- **`ProductViewModel`:**
  - El `ViewModel` actúa como intermediario entre la actividad y el repositorio de productos.
  - El método `getProductLiveData` expone un `LiveData` que la actividad observa para recibir actualizaciones automáticas de datos.

###### **Archivo XML: activity_main.xml**

Este archivo define la estructura básica de la actividad y habilita Data Binding.

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

    </LinearLayout>
</layout>
```

**Explicación:**
- **`<layout>`:** Habilita el uso de Data Binding en este diseño XML.
- **`<data>`:** Aunque está vacío en este caso, permite declarar variables y observables que se utilizarán en el diseño.
- **`RecyclerView`:** Componente que muestra la lista de productos.
  - El ID `@+id/recyclerView` se usa para acceder al RecyclerView desde el `ActivityMainBinding`.

# **Semana 3: Desarrollo de Interfaces en Android**

### 1. Accesibilidad en Apps Móviles

#### Importancia
La accesibilidad en aplicaciones móviles garantiza que personas con discapacidades visuales, auditivas, motoras o cognitivas puedan interactuar con las apps de manera efectiva.

#### Normas y Principios
- **WCAG (Web Content Accessibility Guidelines):** Proporcionan un marco de referencia para el diseño accesible.
- **Principios básicos:**
  - **Perceptible:** Contenido visible y claro para todos.
  - **Operable:** Controles fáciles de usar.
  - **Comprensible:** Interfaces intuitivas.
  - **Robusto:** Compatible con tecnologías de asistencia.

#### Buenas prácticas
- Añadir etiquetas como **contentDescription** para lectores de pantalla. Algunos ejemplos de uso incluyen:
  - **ImageView:** Para describir imágenes no textuales.
    ```java
    ImageView imageView = findViewById(R.id.sample_image);
    imageView.setContentDescription("Descripción de la imagen");
    ```
  - **Button:** Para detallar acciones asociadas.
    ```java
    Button button = findViewById(R.id.send_button);
    button.setContentDescription("Enviar formulario");
    ```
  - **EditText:** Para explicar el propósito del campo de entrada.
    ```java
    EditText editText = findViewById(R.id.name_field);
    editText.setContentDescription("Campo para escribir el nombre");
    ```
- Asegurar un buen contraste entre texto y fondo usando herramientas como **Contrast Checker**.
- Probar la app con lectores de pantalla (por ejemplo, TalkBack en Android).
- Evitar el uso de colores como única señal visual.

### 2. Firebase: Manejo de Tablas Relacionadas

#### Introducción
Firebase es una base de datos NoSQL ideal para gestionar relaciones simples entre tablas.

#### Ejemplo de Relación de Favoritos
##### Estructura en Firebase
```json
{
  "usuarios": {
    "usuario1": {
      "nombre": "Juan",
      "favoritos": ["receta1", "receta2"]
    }
  },
  "recetas": {
    "receta1": {
      "nombre": "Tarta de manzana",
      "ingredientes": ["manzana", "harina", "azúcar"]
    }
  }
}
```
##### Código para manejar favoritos
- **Agregar favorito:**
  ```java
  DatabaseReference userFavoritesRef = FirebaseDatabase.getInstance()
          .getReference("usuarios/usuario1/favoritos");
  userFavoritesRef.child("receta3").setValue(true);
  ```
- **Leer favoritos:**
  ```java
  userFavoritesRef.addListenerForSingleValueEvent(new ValueEventListener() {
      @Override
      public void onDataChange(DataSnapshot snapshot) {
          List<String> favorites = new ArrayList<>();
          for (DataSnapshot child : snapshot.getChildren()) {
              favorites.add(child.getKey());
          }
          // Usar los favoritos
      }

      @Override
      public void onCancelled(DatabaseError error) {
          // Manejo de errores
      }
  });
  ```

### 3. Shared Preferences

#### Introducción
Las **Shared Preferences** permiten almacenar configuraciones o datos ligeros que no necesitan una base de datos.

#### Ejemplo de Uso
- **Guardar datos:**
  ```java
  SharedPreferences sharedPref = getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
  SharedPreferences.Editor editor = sharedPref.edit();
  editor.putString("username", "Juan");
  editor.putBoolean("darkMode", true);
  editor.apply();
  ```
- **Recuperar datos:**
  ```java
  SharedPreferences sharedPref = getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
  String username = sharedPref.getString("username", "default");
  boolean darkMode = sharedPref.getBoolean("darkMode", false);
  ```

### 4. Uso de Recursos XML

#### strings.xml
- Permite centralizar textos para facilitar la internacionalización.
  ```xml
  <resources>
      <string name="app_name">Mi App</string>
      <string name="welcome_message">Bienvenido a la aplicación</string>
  </resources>
  ```

#### colors.xml
- Define colores reutilizables en toda la app.
- Puedes ver información a cerca de los colores [aquí](https://es.goodbarber.com/blog/como-elegir-los-colores-adecuados-para-tu-aplicacion-movil-a814/).
  ```xml
  <resources>
      <color name="primary">#6200EE</color>
      <color name="primary_variant">#3700B3</color>
      <color name="secondary">#03DAC6</color>
  </resources>
  ```
- **Uso en layouts con MVVM:**
  ```xml
  <data>
      <variable
          name="viewModel"
          type="com.example.app.ui.MainViewModel" />
  </data>

  <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical"
      android:background="@{viewModel.backgroundColor}">

      <TextView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="@{viewModel.welcomeMessage}" />
  </LinearLayout>
  ```

### 5. Temas y Estilos en Android Studio

#### Conceptos Básicos
- **Estilos:** Colecciones de atributos aplicados a componentes específicos para asegurar uniformidad visual.
  - Ejemplo: Un estilo para botones con color de fondo y texto personalizado.
- **Temas:** Conjunto de estilos aplicados globalmente a una aplicación o actividad.
  - Ejemplo: Tema oscuro para toda la app.

#### Creación
- Definir en **styles.xml:**
  ```xml
  <style name="CustomButtonStyle" parent="Widget.AppCompat.Button">
      <item name="android:background">@color/primaryColor</item>
      <item name="android:textColor">#FFFFFF</item>
      <item name="android:padding">16dp</item>
  </style>
  ```
- Aplicar en XML:
  ```xml
  <Button style="@style/CustomButtonStyle" android:text="Aceptar"/>
  ```
- Definir temas en **AndroidManifest.xml:**
  ```xml
  <application android:theme="@style/AppTheme">
  ```

#### Temas para Modo Nocturno
- Configurar **values-night/styles.xml:**
  ```xml
  <style name="AppTheme" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
      <item name="colorPrimary">#1A237E</item>
      <item name="colorPrimaryVariant">#283593</item>
      <item name="colorAccent">#FF4081</item>
  </style>
  ```

#### Cambio de Tema con Botón

Un botón puede ser usado para cambiar dinámicamente entre temas claro y oscuro en tiempo de ejecución.

##### Código para cambiar tema:
1. **Crear un tema oscuro y uno claro en styles.xml:**
   ```xml
   <style name="ThemeClaro" parent="Theme.MaterialComponents.Light">
       <item name="colorPrimary">#FFFFFF</item>
       <item name="colorOnPrimary">#000000</item>
   </style>

   <style name="ThemeOscuro" parent="Theme.MaterialComponents">
       <item name="colorPrimary">#000000</item>
       <item name="colorOnPrimary">#FFFFFF</item>
   </style>
   ```

2. **Cambiar el tema en el botón dinámicamente:**
   ```java
   Button themeButton = findViewById(R.id.themeButton);
   themeButton.setOnClickListener(view -> {
       boolean isDarkMode = getSharedPreferences("AppConfig", Context.MODE_PRIVATE)
               .getBoolean("darkMode", false);
       SharedPreferences.Editor editor = getSharedPreferences("AppConfig", Context.MODE_PRIVATE).edit();
       editor.putBoolean("darkMode", !isDarkMode);
       editor.apply();

       setTheme(!isDarkMode ? R.style.ThemeOscuro : R.style.ThemeClaro);
       recreate();
   });
   ```
3. **Botón en el XML:**
   ```xml
   <Button
       android:id="@+id/themeButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="Cambiar Tema"/>
   ```

### 6. Introducción a Material Design

Material Design es un sistema de diseño desarrollado por Google que proporciona un lenguaje visual unificado para crear interfaces consistentes y atractivas. Combina principios del diseño clásico con innovación tecnológica para garantizar experiencias de usuario intuitivas y fluidas.

#### Principios Fundamentales de Material Design
1. **Jerarquía Visual:**
   - Utiliza colores, tamaños y posiciones para dirigir la atención del usuario hacia elementos importantes.
2. **Movimiento Significativo:**
   - Las transiciones y animaciones deben ser fluidas y reflejar la interacción del usuario.
3. **Diseño Adaptativo:**
   - Las interfaces deben ajustarse a diferentes tamaños de pantalla y orientaciones.
4. **Consistencia:**
   - Usa componentes estándar para que el usuario pueda anticipar el comportamiento de la interfaz.

#### Cómo Aplicar Material Design
1. **Configurar el Tema Base:**
   - En el archivo `styles.xml`, define un tema base que herede de **MaterialComponents.**
   ```xml
   <style name="AppTheme" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
       <item name="colorPrimary">@color/primary</item>
       <item name="colorPrimaryVariant">@color/primaryDark</item>
       <item name="colorAccent">@color/accent</item>
   </style>
   ```
2. **Integrar Componentes Clave:**
   - **AppBarLayout:** Barra superior para mostrar el título y acciones principales.
     ```xml
     <com.google.android.material.appbar.AppBarLayout
         android:layout_width="match_parent"
         android:layout_height="wrap_content">
         <com.google.android.material.appbar.MaterialToolbar
             android:id="@+id/toolbar"
             android:layout_width="match_parent"
             android:layout_height="wrap_content"
             app:title="Material Design Demo" />
     </com.google.android.material.appbar.AppBarLayout>
     ```
   - **Floating Action Button (FAB):** Botón para la acción principal de la pantalla.
     ```xml
     <com.google.android.material.floatingactionbutton.FloatingActionButton
         android:id="@+id/fab"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:src="@drawable/ic_add"
         app:layout_anchor="@id/app_bar"
         app:layout_anchorGravity="bottom|end" />
     ```
   - **Snackbars:** Mensajes breves e informativos.
     ```java
     Snackbar.make(findViewById(android.R.id.content), "Acción realizada correctamente", Snackbar.LENGTH_SHORT).show();
     ```

3. **Colores y Tipografías:**
   - Define los colores en el archivo `colors.xml` para garantizar consistencia.
     ```xml
     <color name="primary">#6200EE</color>
     <color name="primaryDark">#3700B3</color>
     <color name="accent">#03DAC6</color>
     ```
   - Usa la tipografía recomendada por Material Design.
     ```xml
     <style name="TextAppearance.MaterialComponents.Body1">
         <item name="fontFamily">sans-serif</item>
         <item name="android:textSize">16sp</item>
     </style>
     ```

4. **Diseño Adaptativo:**
   - Utiliza **ConstraintLayout** para garantizar que los elementos de la interfaz se adapten a diferentes tamaños de pantalla.

5. **Iconografía:**
   - Usa íconos estándar de Google Material Icons o define los tuyos propios.
     ```xml
     <ImageView
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:src="@drawable/ic_launcher" />
     ```
#### Cómo usar temas generados con Material Theme Builder:
Accede a la página de [Material Theme Builder](https://material-foundation.github.io/material-theme-builder/) y edita tu propio tema. Una vez terminado, exportalo como `Android Views (XML)`.
PAra usar el tema exportado desde **Material Theme Builder**, sigue estos pasos para integrarlos en tu proyecto en Android Studio:

---

### **1. Coloca los archivos exportados en tu proyecto**
1. En **Android Studio**, navega hasta la carpeta `res/values` de tu proyecto:
   - Si no tienes la carpeta `values` (poco probable), créala en `res/`.

2. Copia los archivos exportados (`colors.xml`, `theme_overlays.xml` y `themes.xml`) dentro de la carpeta `res/values`.

---

### **2. Estructura de los archivos**
Después de copiarlos, deberían verse más o menos así:

#### **colors.xml**
Define los colores base y las variantes generadas por **Material Theme Builder**:

```xml
<resources>
    <color name="md_theme_light_primary">#6200EE</color>
    <color name="md_theme_light_onPrimary">#FFFFFF</color>
    <color name="md_theme_light_secondary">#03DAC6</color>
    <!-- Otros colores -->
</resources>
```

#### **themes.xml**
Define el tema base y los atributos asociados:

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <style name="Theme.MyApp" parent="Theme.Material3.Light.NoActionBar">
        <item name="colorPrimary">@color/md_theme_light_primary</item>
        <item name="colorOnPrimary">@color/md_theme_light_onPrimary</item>
        <item name="colorSecondary">@color/md_theme_light_secondary</item>
        <!-- Otros atributos -->
    </style>
</resources>
```

#### **theme_overlays.xml**
Proporciona configuraciones adicionales como temas oscuros (opcional).

```xml
<resources>
    <style name="ThemeOverlay.MyApp.Dark" parent="ThemeOverlay.Material3.Dark">
        <item name="colorPrimary">@color/md_theme_dark_primary</item>
        <item name="colorOnPrimary">@color/md_theme_dark_onPrimary</item>
    </style>
</resources>
```

---

### **3. Configura el tema en `AndroidManifest.xml`**
1. Abre el archivo `AndroidManifest.xml`.
2. Busca el atributo `android:theme` dentro de la etiqueta `<application>` y cámbialo para que apunte al tema que acabas de importar (`Theme.MyApp`):

```xml
<application
    android:theme="@style/Theme.MyApp"
    ... >
```

---

### **4. Ajusta el tema en tus layouts**
En los archivos de diseño XML (`activity_main.xml`, etc.), asegúrate de que los colores definidos se aplican automáticamente. Si necesitas especificar colores en algún componente, usa los valores definidos en `colors.xml`. Por ejemplo:

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hola"
    android:backgroundTint="?attr/colorPrimary"
    android:textColor="?attr/colorOnPrimary" />
```

---

### **5. Prueba el tema**
1. Ejecuta la aplicación para comprobar cómo se aplican los colores en tu UI.
2. Si quieres probar el tema oscuro:
   - Entra en las **opciones de desarrollador** del emulador/dispositivo y activa el modo oscuro.
   - El tema definido en `theme_overlays.xml` debería aplicarse automáticamente.

---

### **6. Personalizaciones adicionales**
Si necesitas realizar cambios en los colores o temas después de la integración:
- Modifica directamente los valores en `colors.xml`, `themes.xml` o `theme_overlays.xml`.
- Usa el **Theme Editor** de Android Studio:
  - Haz clic en `res/values/themes.xml`.
  - Selecciona **Open in Theme Editor** en la esquina superior derecha.

---


#### Buenas Prácticas con Material Design
- **Feedback Visual:** Proporciona respuestas inmediatas a las acciones del usuario mediante cambios visuales como animaciones o cambios de color.
- **Consistencia en el Diseño:** Usa componentes de Material Design en todas las pantallas para unificar la experiencia.
- **Pruebas de Usabilidad:** Asegúrate de que tu diseño sea claro, intuitivo y accesible para todo tipo de usuarios.

#### Recursos Útiles
- [Guía oficial de Material Design](https://material.io/design)
- [Componentes de Material Design para Android](https://developer.android.com/jetpack/androidx/releases/material)
- [Herramienta de comprobación de accesibilidad](https://accessibilityinsights.io/)


### 7. Buenas Prácticas
- **Diseño modular:** Facilita cambios y mantenimiento.
- **Validación:** Usa herramientas como las de Android Studio para verificar accesibilidad y consistencia visual.
- **Pruebas:** Asegúrate de probar en múltiples dispositivos y resoluciones.
- **Organización:** Mantén el código y recursos estructurados y limpios.


# Semana 4: Implementación de Splash Screen y Migración a Fragments con Navigation Drawer

---

#### 1. **Splash Screen**

Una Splash Screen es una pantalla de bienvenida que se muestra al iniciar una aplicación, normalmente con un logo, animación o mensaje de carga. Su propósito es mejorar la transición entre el arranque de la app y la pantalla principal.

##### 1.1. **Crear un Tema para la Splash Screen**

En `res/values/themes.xml`, agrega un nuevo tema para la Splash Screen:

```xml
<style name="Theme.SplashScreen" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_background</item>
</style>
```

En `res/drawable/splash_background.xml`, define el fondo:

```xml
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/primary" />
    <item>
        <bitmap
            android:gravity="center"
            android:src="@drawable/logo" />
    </item>
</layer-list>
```

> **Nota**: Sustituye `@drawable/logo` con el logo de tu aplicación.

##### 1.2. **Crear la Activity para la Splash Screen**

Crea una nueva `SplashActivity`:

```java
public class SplashActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        startActivity(new Intent(this, MainActivity.class));
        finish();
    }
}
```

Modifica `AndroidManifest.xml` para establecer `SplashActivity` como la pantalla de inicio:

```xml
<activity
    android:name=".SplashActivity"
    android:theme="@style/Theme.SplashScreen">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

##### 1.3. **Agregar un Retraso Opcional**

Para que la Splash Screen dure unos segundos antes de abrir `MainActivity`, usa un `Handler`:

```java
new Handler(Looper.getMainLooper()).postDelayed(() -> {
    startActivity(new Intent(SplashActivity.this, MainActivity.class));
    finish();
}, 2000); // 2000 milisegundos (2 segundos)
```

> **Recomendación**: No abuses del retraso; si la aplicación carga rápido, evita hacerlo innecesario.

---

#### 2. **Migración a Fragments, Navigation Drawer y Configuración de Usuario**

##### 2.1. **Introducción**

En las semanas anteriores, se crearon varias `Activities` como `DashboardActivity`, `FavouritesActivity`, y `DetailActivity`. Ahora se migrarán a `Fragments` para mejorar la arquitectura de la aplicación, reducir código duplicado y permitir una navegación más fluida con un `Navigation Drawer`.

##### 2.2. **Migración de Activities a Fragments**

Cada `Activity` será reemplazada por un `Fragment`. Los `Fragments` incluyen:

- `DashboardFragment` en lugar de `DashboardActivity`
- `FavouritesFragment` en lugar de `FavouritesActivity`
- `DetailFragment` en lugar de `DetailActivity`
- `ProfileFragment` para configuración de usuario (cambio de contraseña y modo claro/oscuro)

###### 2.2.1. **Creación de Fragments**

Ejemplo para `DashboardFragment`:

```java
public class DashboardFragment extends Fragment {
    public DashboardFragment() { }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);
        // Configura RecyclerView, adaptadores, etc.
        return view;
    }
}
```

#### 2.3. **Layouts para los Fragments**

Cada `Fragment` tendrá su propio archivo de layout. Ejemplo para `fragment_dashboard.xml`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/dashboardRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

---

#### 3. **Creación de MainActivity con Navigation Drawer**

##### 3.1. **Estructura XML del Navigation Drawer**

Edita el layout de `MainActivity` para incluir un `DrawerLayout` y un `NavigationView`:

```xml
<androidx.drawerlayout.widget.DrawerLayout
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        
    <FrameLayout
        android:id="@+id/fragmentContainer"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/drawer_menu" />
</androidx.drawerlayout.widget.DrawerLayout>
```

##### 3.2. **Menú del Drawer**

Define las opciones en `res/menu/drawer_menu.xml`:

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/nav_dashboard" android:title="Dashboard" />
    <item android:id="@+id/nav_favourites" android:title="Favoritos" />
    <item android:id="@+id/nav_profile" android:title="Perfil" />
    <item android:id="@+id/nav_logout" android:title="Cerrar Sesión" />
</menu>
```

##### 3.3. **Lógica en MainActivity**

En `MainActivity`, gestiona la selección de los ítems del menú:

```java
public class MainActivity extends AppCompatActivity {
    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        
        binding.navigationView.setNavigationItemSelectedListener(item -> {
            if (id == R.id.nav_dashboard) {
                openFragment(new DashboardFragment());
            } else if (id == R.id.nav_favourites) {
                openFragment(new FavouritesFragment());
            } else if (id == R.id.nav_profile) {
                openFragment(new ProfileFragment());
            } else if (id == R.id.nav_logout) {
                logoutUser();
            }
    
            binding.drawerLayout.closeDrawers();
            return true;
        });

        if (savedInstanceState == null) {
            openFragment(new DashboardFragment());
        }
    }

    private void openFragment(Fragment fragment) {
        getSupportFragmentManager().beginTransaction()
            .replace(R.id.fragmentContainer, fragment)
            .commit();
    }

    private void logoutUser() {
        FirebaseAuth.getInstance().signOut();
        Intent intent = new Intent(this, LoginActivity.class);
        startActivity(intent);
        finish();
    }
}
```

---

#### 4. **Implementación de la Pantalla de Perfil (ProfileFragment)**

##### 4.1. **Layout de ProfileFragment**

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText android:id="@+id/currentPasswordEditText" android:hint="Contraseña actual" android:inputType="textPassword"/>
    <EditText android:id="@+id/newPasswordEditText" android:hint="Nueva contraseña" android:inputType="textPassword"/>
    <Button android:id="@+id/changePasswordButton" android:text="Cambiar contraseña"/>
    <Switch android:id="@+id/darkModeSwitch" android:text="Modo oscuro"/>
</LinearLayout>
```

##### 4.2. **Lógica de cambio de contraseña y modo oscuro**

```java
public class ProfileFragment extends Fragment {
    private EditText currentPasswordEditText, newPasswordEditText;
    private Switch darkModeSwitch;

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_profile, container, false);

        currentPasswordEditText = view.findViewById(R.id.currentPasswordEditText);
        newPasswordEditText = view.findViewById(R.id.newPasswordEditText);
        darkModeSwitch = view.findViewById(R.id.darkModeSwitch);

        SharedPreferences prefs = requireActivity().getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
        darkModeSwitch.setChecked(prefs.getBoolean("darkMode", false));

        darkModeSwitch.setOnCheckedChangeListener((compoundButton, checked) -> toggleDarkMode(checked));
        view.findViewById(R.id.changePasswordButton).setOnClickListener(v -> changePassword());

        return view;
    }

    private void changePassword() {
        String currentPass = currentPasswordEditText.getText().toString();
        String newPass = newPasswordEditText.getText().toString();
        // Código para cambiar la contraseña con Firebase
    }

    private void toggleDarkMode(boolean enableDarkMode) {
        SharedPreferences prefs = requireActivity().getSharedPreferences("AppConfig", Context.MODE_PRIVATE);
        prefs.edit().putBoolean("darkMode", enableDarkMode).apply();
        AppCompatDelegate.setDefaultNightMode(enableDarkMode ? AppCompatDelegate.MODE_NIGHT_YES : AppCompatDelegate.MODE_NIGHT_NO);
        requireActivity().recreate();
    }
}
```

---

#### 5. **Navegación al DetailFragment desde el RecyclerView (DashboardFragment)**

##### 5.1. **Pasar datos al DetailFragment**

```java
public class DashboardFragment extends Fragment {
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);

        RecyclerView recyclerView = view.findViewById(R.id.dashboardRecyclerView);
        MyAdapter adapter = new MyAdapter(item -> {
            DetailFragment detailFragment = new DetailFragment();
            Bundle bundle = new Bundle();
            bundle.putString("ITEM_ID", item.getId());
            detailFragment.setArguments(bundle);
            requireActivity().getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragmentContainer, detailFragment)
                .addToBackStack(null)
                .commit();
        });

        recyclerView.setAdapter(adapter);
        return view;
    }
}
```

##### 5.2. **Recibir datos en DetailFragment**

```java
public class DetailFragment extends Fragment {
    private String itemId;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_detail, container, false);

        if (getArguments() != null) {
            itemId = getArguments().getString("ITEM_ID");
        }
        // Cargar datos con itemId
        return view;
    }
}
```

---

#### 6. **Logout desde el Drawer**

```java
private void logoutUser() {
    FirebaseAuth.getInstance().signOut();
    Intent intent = new Intent(this, LoginActivity.class);
    startActivity(intent);
    finish();
}
```

---

#### 7. **Consolidación de MVVM**

Mantén la arquitectura MVVM:

- **ViewModels**: Manejan la lógica de cada `Fragment`.
- **Repositorios**: Se comunican con Firebase.
- **Models**: Representan los datos.
- **Views (Fragments)**: Interactúan con los `ViewModels` usando `LiveData`.

Ejemplo de `DashboardViewModel`:

```java
public class DashboardFragment extends Fragment {
    private DashboardViewModel viewModel;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);
        viewModel = new ViewModelProvider(this).get(DashboardViewModel.class);
        viewModel.getItemsLiveData().observe(getViewLifecycleOwner(), items -> {
            // Actualizar RecyclerView
        });
        return view;
    }
}
```




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
   - Habilitar Data Binding en el archivo `build.gradle` del módulo:
     ```gradle
     android {
         buildFeatures {
             dataBinding = true
         }
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





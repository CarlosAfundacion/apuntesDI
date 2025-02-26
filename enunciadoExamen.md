
# **EXAMEN  - DESARROLLO DE INTERFACES MÓVILES CON FIREBASE Y MVVM**

En este examen, implementarás una nueva funcionalidad en el proyecto de la aplicación que has desarrollado durante el trimestre. 
Como base puedes usar el proyecto que adjunto en classroom o el proyecto que has desarrollado tú.
Si eliges mi proyecto, un usuario válido es **j@j.com** con contraseña **123456**
La práctica consistirá en:

1. **Añadir un nuevo Fragment** accesible desde el **Navigation Drawer**.
2. **Mostrar un elemento aleatorio de Firebase** (título, imagen y descripción) dentro de este Fragment.
3. **Permitir cambiar dinámicamente el contenido** mediante un botón.
4. **Mantener buenas prácticas de arquitectura MVVM** y **uso de Data Binding**.
5. **Evitar el uso de texto en crudo** en los archivos XML de diseño.
6. **Uso de constraintlayout para el diseño del fragment**
7. **Hacer la aplicación accesible** 

---

## **Descripción de la funcionalidad a implementar**
Deberás **crear un nuevo Fragment**, accesible desde el menú de navegación de la aplicación. Este fragmento mostrará:

- Un **título**, una **imagen** y una **descripción** obtenidos de **Firebase Realtime Database**.
- Un **botón** en la parte inferior que permitirá **cambiar el contenido mostrado** por otro elemento aleatorio de la base de datos.

Al iniciar, el Fragment deberá cargar **un elemento aleatorio de la base de datos** y al pulsar el botón, se actualizará con otro elemento aleatorio.

---

## **Requisitos técnicos**
### **1. Creación y navegación del Fragment**
- Agrega una nueva entrada en el **Navigation Drawer** para acceder a este nuevo Fragment.
- Usa el **patrón MVVM** correctamente para gestionar los datos.
- Implementa **Data Binding** para enlazar los datos del modelo con la vista.

### **2. Carga de datos desde Firebase**
- Implementa la lógica para seleccionar un **elemento aleatorio** de la base de datos
- Recupera un **elemento aleatorio** de la base de datos al abrir el Fragment.
- Cambia el **elemento aleatorio** al pulsar el botón.
- Puedes recoger un elemento aleatorio de una lista con el siguiente código:
  
  ```
  import java.util.Random;
  Random random = new Random();
  int randomIndex = random.nextInt(recipeList.size());
  Recipe reicpe = RecipeList.get(randomIndex)
  ```

### **3. Organización del código y arquitectura**
- Sigue el patrón **MVVM**, separando correctamente **Model**, **ViewModel** y **Repository**.
- Usa **LiveData** para actualizar la UI al recibir datos de Firebase.
- Evita el uso de **texto en crudo** en el XML.
- Usa constraintLayout para organizar los elementos

### **4. Accesibilidad**
- Añade compatibilidad con lectores de pantalla. 

---

## **Rúbrica de Evaluación**

| **Criterio** | **Descripción** | **Puntuación** |
|-------------|---------------|---------------|
| **1. Creación del Fragment y su navegación desde el Navigation Drawer** | Se ha añadido correctamente un nuevo Fragment y es accesible desde el menú lateral. | **2 punto** |
| **2. Implementación del diseño con ConstraintLayout** | Se ha utilizado **ConstraintLayout** correctamente para organizar los elementos en el Fragment. | **0,5 punto** |
| **3. Uso de Data Binding** | Se ha utilizado **Data Binding** para enlazar los datos de Firebase con la vista sin `findViewById`. | **2 punto** |
| **4. Recuperación del elemento aleatorio desde Firebase al abrir el Fragment** | Se obtiene correctamente un **elemento aleatorio** almacenado en Firebase al iniciar el Fragment. | **1 punto** |
| **5. Cambio de contenido al pulsar el botón** | Al pulsar el botón, se selecciona y muestra **otro elemento aleatorio** de la base de datos. | **1 punto** |
| **6. Uso correcto de MVVM y LiveData** | Se ha implementado correctamente la arquitectura **MVVM**, separando **Model, ViewModel y Repository**, y utilizando **LiveData**. | **2 punto** |
| **7. XML sin texto en crudo** | **No** se han usado strings en crudo dentro de los archivos XML, sino que se han enlazado en el sitio adecuado. | **0,5 punto** |
| **8. Accesibilidad** | Se han implementado prácticas compatibles con lectores de pantalla. | **0,5 punto** |
| **9. Código limpio y organizado** | El código está bien estructurado, con nombres de variables y métodos adecuados, siguiendo las buenas prácticas de desarrollo. | **0,5 punto** |

### **Total: 10 puntos**
 **Hace media:** **4,5 puntos o más.**  


---

## **Notas adicionales**
**Si no hay vídeo demostrativo de la aplicación la nota será un 0**
**Si no está el txt con el resultado de la monitorización de red la nota será un 0**
**Cualquier uso de IA o de búsquedas en internet no permitidas supondrá una nota de 0**

---

## **Entrega**
- **Classroom**:
  - Sube a Classroom tu código, el monitoreo de conexiones de red y un vídeo en el que se muestren todas las funcionalidades pedidas, todo en único archivo con el formato **NombreApellido.zip**.

---

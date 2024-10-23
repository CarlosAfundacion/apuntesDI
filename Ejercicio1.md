
# **Aplicación de gestión de notas**

Vas a desarrollar una aplicación de gestión de notas con Tkinter, que permita agregar, eliminar y visualizar notas. Además, esta aplicación descargará una imagen desde GitHub de manera asíncrona usando hilos y gestionará las notas guardándolas y cargándolas desde un archivo de texto. Implementarás esta aplicación usando el patrón de diseño Modelo-Vista-Controlador (MVC). También, deberás detectar eventos de clic del ratón fuera de los botones utilizando `bind`.

#### **Requisitos generales:**

1. **Vista (Interfaz gráfica)**:
   - Utilizarás diferentes widgets de Tkinter para crear la interfaz gráfica:
     - **Label**: Para mostrar el título de la aplicación y las coordenadas donde se haga clic.
     - **Listbox**: Para mostrar las notas que el usuario añade.
     - **Entry**: Para que el usuario escriba una nueva nota.
     - **Button**: Para agregar notas, eliminar notas, guardar notas en un archivo, cargar notas desde un archivo y descargar una imagen desde GitHub.
     - **Label**: Para mostrar la imagen descargada desde GitHub.

2. **Modelo (Manejo de datos)**:
   - Crearás una clase `NotasModel` que gestionará la lógica de las notas:
     - **Método `agregar_nota(nueva_nota)`**: Añade una nueva nota a la lista de notas.
     - **Método `eliminar_nota(indice)`**: Elimina la nota en el índice especificado.
     - **Método `obtener_notas()`**: Devuelve la lista de notas.
     - **Método `guardar_notas()`**: Guarda las notas en un archivo de texto (`notas.txt`).
     - **Método `cargar_notas()`**: Carga las notas desde el archivo de texto (`notas.txt`) al iniciar la aplicación.

3. **Controlador (Manejo de lógica de interacción)**:
   - Crearás una clase `ControladorNotas` que conectará la vista con el modelo y manejará la interacción del usuario:
     - **Método `agregar_nota()`**: Obtiene el texto del `Entry` de la vista y llama al modelo para agregar la nota, luego actualiza la vista.
     - **Método `eliminar_nota()`**: Elimina la nota seleccionada en el `Listbox` y actualiza la vista.
     - **Método `guardar_notas()`**: Guarda las notas en el archivo de texto llamando al modelo y muestra un mensaje de confirmación.
     - **Método `cargar_notas()`**: Carga las notas desde el archivo de texto y actualiza la vista.
     - **Método `descargar_imagen()`**: Descarga la imagen desde GitHub en un hilo separado para no congelar la interfaz gráfica.
     - **Método `actualizar_coordenadas(event)`**: Captura las coordenadas del clic del ratón fuera de los botones y actualiza un `Label` en la vista.
     - **Método `actualizar_listbox(event)`**: Refresca la visualización del listbox de la vista con las notas del modelo en el momento actual.

4. **Descarga de imagen desde GitHub utilizando hilos**:
   - Debes usar la biblioteca `threading` para realizar la descarga de la imagen en un hilo separado y así evitar que la interfaz gráfica se congele durante la descarga.
   - Utilizarás `Pillow` para mostrar la imagen descargada desde GitHub en un `Label`.

5. **Evento de clic fuera de los botones**:
   - Usa el método `bind()` de Tkinter para enlazar el evento `<Button-1>` (clic izquierdo) a una función que actualice las coordenadas del clic en un `Label` específico.

---

### **Instrucciones detalladas para la implementación:**

#### 1. **Modelo: `NotasModel`**

- **Clase `NotasModel`**:
  - Esta clase gestionará la lista de notas y proporcionará métodos para agregar, eliminar y recuperar las notas.
  - **Atributo**:
    - `self.notas`: Lista que almacena las notas.

- **Métodos del modelo**:
  - **`agregar_nota(nueva_nota)`**: Añade una nueva nota a la lista de notas. Utiliza el método `append()` para agregar la nueva nota.
  - **`eliminar_nota(indice)`**: Elimina una nota en un índice específico. Usa `del self.notas[indice]` para eliminarla.
  - **`obtener_notas()`**: Devuelve la lista completa de notas. Este método retorna el valor de `self.notas`.
  - **`guardar_notas()`**: Abre el archivo `notas.txt` en modo escritura (`'w'`), y escribe cada nota en una nueva línea utilizando `write()`.
  - **`cargar_notas()`**: Abre el archivo `notas.txt` en modo lectura (`'r'`) y lee cada línea, eliminando los saltos de línea usando `strip()`.

#### 2. **Vista: `VistaNotas`**

- **Clase `VistaNotas`**:
  - Esta clase gestionará la interfaz gráfica de la aplicación, definiendo los widgets y su disposición.

- **Widgets utilizados**:
  - **`Label`**:
    - Se utiliza para mostrar el título de la aplicación.
    - También se usa para mostrar las coordenadas del clic del ratón y la imagen descargada.
  - **`Listbox`**:
    - Muestra la lista de notas agregadas. Se usará el método `insert()` para añadir elementos y `delete()` para eliminarlos.
  - **`Entry`**:
    - Entrada de texto donde el usuario puede escribir una nueva nota.
  - **`Button`**:
    - Botones para agregar, eliminar, guardar, cargar notas, y descargar una imagen.
    - Cada botón estará asociado a una función del controlador mediante la opción `command`.
  - **`Label`** (para la imagen):
    - Este `Label` será donde se mostrará la imagen descargada desde GitHub.

#### 3. **Controlador: `ControladorNotas`**

- **Clase `ControladorNotas`**:
  - Esta clase gestionará la lógica de la interacción entre el usuario, la vista y el modelo.

- **Métodos del controlador**:
  - **`agregar_nota()`**:
    - Obtiene el texto del widget `Entry` de la vista utilizando `self.vista.entry_nota.get()`.
    - Llama al método `agregar_nota()` del modelo para agregar la nueva nota.
    - Actualiza el `Listbox` llamando a `actualizar_listbox()`.
  - **`eliminar_nota()`**:
    - Obtiene el índice seleccionado del `Listbox` usando `self.vista.listbox.curselection()`.
    - Llama al método `eliminar_nota()` del modelo para eliminar la nota en el índice seleccionado.
    - Actualiza el `Listbox` llamando a `actualizar_listbox()`.
  - **`guardar_notas()`**:
    - Llama al método `guardar_notas()` del modelo para guardar la lista de notas en un archivo.
    - Muestra un mensaje de confirmación con `messagebox.showinfo()`.
  - **`cargar_notas()`**:
    - Llama al método `cargar_notas()` del modelo para cargar las notas desde un archivo.
    - Actualiza el `Listbox` llamando a `actualizar_listbox()`.
  - **`descargar_imagen()`**:
    - Llama a un método que descarga una imagen desde GitHub utilizando la biblioteca `requests`. La descarga se realiza en un hilo separado usando `threading.Thread()`.
    - Usa `after()` para actualizar la interfaz gráfica con la imagen descargada.
  - **`actualizar_coordenadas(event)`**:
    - Captura las coordenadas del clic y actualiza el `Label` de coordenadas usando `event.x` y `event.y`.
  - **`actualizar_listbox()`**:
    - Eliminar los elementos actuales del `Listbox` usando `delete(0, tk.END)`.
    - Obtener las notas actuales desde el modelo llamando a `obtener_notas()`.
    - Insertar cada nota en el ´Listbox´ usando `insert(tk.END, nota)` en un bucle para asegurarse de que todas las notas se reflejan en la interfaz gráfica.



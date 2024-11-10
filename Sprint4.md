# Juego de memoria
Esto es una guía de implementación, no tienes por qué seguirla al pie de la letra siempre y cuando las funcionalidades de la app no varíen
## `main.py`
### Importaciones y dependencias

1. **Librerías importadas**: 
   - `tkinter as tk` proporciona acceso a la biblioteca Tkinter para construir la interfaz gráfica.
   - `GameController` (de `controlador.py`) es el controlador principal del juego, manejando la lógica entre el modelo y la vista.
   - `GameModel` (de `modelo.py`) representa el modelo de datos del juego.
   

2. **Función principal**: 
   - La lógica principal se ejecuta dentro de un bloque `if __name__ == "__main__"` para evitar que el código se ejecute al importar el archivo en otro módulo.

### Variables y componentes

1. **root (variable)**:
   - Tipo: instancia de `Tk`, proporcionada por `tkinter`
   - Uso: `root` es la ventana raíz de la aplicación Tkinter y actúa como el contenedor de todos los widgets.
   - Configuración: `root` se inicializa.

2. **model (variable)**:
   - Tipo: instancia de `GameModel`
   - Uso: `model` define el modelo de datos del juego, inicializándolo con una dificultad predefinida y un nombre de jugador.
   - Configuración: `model` se configura para establecer una dificultad inicial y un nombre para el jugador. Estos valores pueden ser predeterminados o ajustados según las preferencias del usuario al iniciar el juego.

3. **controller (variable)**:
   - Tipo: instancia de `GameController`
   - Uso: `controller` maneja la lógica de la aplicación, actuando como intermediario entre la interfaz de usuario y el modelo de datos.
   - Configuración: `controller` se inicializa y se conecta a la ventana raíz, permitiendo la integración de la ventana principal con el controlador y sus vistas.

### Guía de implementación por partes

1. **Implementación del controlador del juego**:
   - `GameController` es instanciado y sirve como controlador principal. Este componente centraliza la lógica del juego y funciona como un intermediario entre el modelo (`GameModel`) y la vista (`GameView`). El controlador se encarga de iniciar el menú principal, gestionar el flujo de eventos (como iniciar el juego, mostrar estadísticas o salir) y actualizar la vista de acuerdo con el estado del modelo.

2. **Inicialización del bucle principal**:
   - `root.mainloop()` es el bucle de eventos principal de Tkinter, que mantiene la interfaz activa y escuchando eventos de usuario, como clics o interacciones con botones. Este bucle es necesario para que la interfaz gráfica permanezca operativa una vez inicializada.

3. **Elementos interactivos (dentro de los archivos importados)**:
   - Aunque `main.py` actúa como el punto de entrada del programa, los componentes interactivos principales del juego se encuentran en los módulos `controlador`, `vista` y `modelo`. La interacción y comunicación entre ellos se realiza de la siguiente manera:
   
     - **Menú principal (MainMenu)** en `vista.py` define el menú inicial del juego, donde el jugador puede seleccionar opciones como jugar, ver estadísticas y salir. Este menú está conectado al controlador (`GameController`) para que gestione estas opciones.
     - **Vista del juego (GameView)**, también en `vista.py`, representa la ventana principal del juego, incluyendo el tablero, el contador de movimientos y el temporizador.
     - **Modelo del juego (GameModel)** en `modelo.py` administra la lógica central del juego, como la generación del tablero, la carga de imágenes, y el cálculo del tiempo y de los movimientos realizados.

## `recursos.py`

### Importaciones y dependencias

1. **requests**: permite realizar solicitudes HTTP para descargar imágenes desde URLs especificadas. Es esencial para obtener las imágenes externas usadas en el juego.
   
2. **PIL (Python Imaging Library)**: `Image` y `ImageTk` pertenecen a la biblioteca PIL (específicamente, la versión moderna conocida como `Pillow`), que proporciona funcionalidades avanzadas para manipular y mostrar imágenes. `Image` se usa para abrir y modificar imágenes, mientras que `ImageTk` convierte estas imágenes en un formato que Tkinter puede utilizar.

3. **io**: permite gestionar el flujo de datos en bytes. Aquí se utiliza para convertir los datos descargados (almacenados en un objeto `BytesIO`) en una imagen que `PIL` puede procesar.

### Función principal

**descargar_imagen(url, size)**

- **Descripción**: `descargar_imagen` es la única función del archivo y su propósito es descargar una imagen desde una URL, redimensionarla y convertirla en un formato compatible con Tkinter para ser mostrada en la interfaz gráfica.
- **Parámetros**:
  - `url`: dirección web desde la cual se descargará la imagen.
  - `size`: tamaño al que se redimensionará la imagen en píxeles.
- **Proceso**:
  1. La función realiza una solicitud GET a la URL proporcionada utilizando `requests.get(url)`.
  2. `response.raise_for_status()` verifica si la solicitud fue exitosa. Si no, se lanza una excepción que puede ser manejada en el bloque `except`.
  3. Si la descarga es exitosa, `Image.open` abre la imagen desde el flujo de bytes, y `resize` la ajusta al tamaño especificado, usando el filtro `LANCZOS` para una redimensionada de alta calidad.
  4. La imagen redimensionada se convierte en un formato compatible con Tkinter (`ImageTk.PhotoImage`) y es devuelta por la función para su uso en otros componentes del juego.
- **Manejo de excepciones**:
  - Si se produce algún error en el proceso de descarga, se captura la excepción `RequestException`, se muestra un mensaje de error indicando la URL fallida y se devuelve `None` como resultado, indicando que la imagen no pudo ser descargada.

Esta función es fundamental en la aplicación para obtener las imágenes del tablero de juego, asegurando su disponibilidad en el tamaño y formato necesarios para integrarse adecuadamente en la interfaz de usuario de Tkinter.


## `modelo.py`

### Importaciones y dependencias

1. **threading**: permite la creación y gestión de hilos, que se utiliza en este archivo para cargar las imágenes en segundo plano, optimizando la carga inicial del juego. 
   
2. **time**: proporciona acceso a funciones relacionadas con el tiempo, como `time()` para capturar el tiempo en segundos. Aquí, se usa para calcular la duración de la partida.
   
3. **random**: se utiliza para mezclar las posiciones de las imágenes en el tablero y así variar la disposición de las cartas en cada partida.
   
4. **datetime**: importa funciones de fecha y hora. En este contexto, se usa para guardar la fecha en que el jugador ha completado una partida en el archivo de puntuaciones.
   
5. **descargar_imagen (de recursos.py)**: función externa que permite descargar y redimensionar las imágenes desde una URL. Las imágenes representan las cartas del juego y una versión oculta que se utiliza en el tablero hasta que se descubren.

### Clase principal: GameModel

**GameModel** representa el modelo de datos del juego, que incluye la lógica para manejar el tablero, la carga de imágenes, el cálculo de tiempo, la validación de coincidencias y la gestión de puntuaciones.

### Atributos y métodos clave

1. **`__init__(self, difficulty, player_name, cell_size=100)`**:
   - Inicializa el modelo del juego.
   - Establece el tamaño del tablero basado en la dificultad (4x4, 6x6 u 8x8).
   - Llama a `_generate_board()` para crear la estructura del tablero y a `_load_images()` para cargar las imágenes en segundo plano.
   - Inicializa variables clave como `start_time` para el temporizador y `moves` para contar los movimientos.

2. **Métodos privados**:
   - **`_generate_board(self)`**: genera el tablero del juego creando un conjunto de pares de identificadores de imágenes y los mezcla aleatoriamente. Esto asegura que cada partida tenga una distribución distinta de las cartas.
   - **`_load_images(self)`**: inicia un hilo separado para descargar y cargar las imágenes necesarias desde una URL base. La imagen oculta se asigna a `hidden_image` y cada identificador de carta se asigna a una imagen descargada específica. Se utiliza un hilo para evitar demoras en la interfaz del usuario durante la carga. Puedes consultar más información sobre su implementación en este [enlace:](hilosSprint%234.md) 

3. **Métodos públicos**:
   - **`images_are_loaded(self)`**: verifica si todas las imágenes del juego han sido cargadas. Devuelve un valor booleano que indica si el evento `images_loaded` ha sido activado, indicando que las imágenes están listas.
   - **`start_timer(self)`**: reinicia el tiempo de inicio del juego para el temporizador, permitiendo el registro del tiempo transcurrido.
   - **`get_time(self)`**: calcula y devuelve el tiempo en segundos desde que se inició el temporizador.
   - **`check_match(self, pos1, pos2)`**: incrementa el contador de movimientos y verifica si dos posiciones del tablero contienen la misma imagen, indicando una coincidencia. Si se encuentran las imágenes coincidentes, se incrementa el contador `pairs_found`.
   - **`is_game_complete(self)`**: verifica si se han encontrado todas las parejas del tablero. Si el número de parejas encontradas equivale a la mitad del tamaño total del tablero, el juego se considera completado.
   - **`save_score(self)`**: guarda la puntuación del jugador en un archivo `ranking.txt`. Los datos incluyen el nombre del jugador, la dificultad, el número de movimientos y la fecha. Solo se guardan las tres mejores puntuaciones para cada nivel de dificultad, basándose en el menor número de movimientos.
   - **`load_scores(self)`**: carga y devuelve las puntuaciones desde el archivo `ranking.txt`. Si el archivo no existe, el método devuelve un diccionario vacío con listas para cada nivel de dificultad. Esta información es útil para mostrar el ranking de los mejores jugadores en la interfaz.

### Flujo de funcionamiento

El `GameModel` se encarga de la lógica de negocio del juego de memoria, asegurando una experiencia de juego fluida al gestionar el estado del tablero, la carga de imágenes en segundo plano, el cálculo del tiempo, el conteo de movimientos y el almacenamiento de las mejores puntuaciones. Su diseño modular permite una fácil interacción con otros componentes, como el controlador del juego y la interfaz de usuario.

---

## `vista.py`

### Importaciones y dependencias

1. **tkinter**: biblioteca principal para la construcción de la interfaz gráfica. Importa tanto `tkinter` como `simpledialog` y `Toplevel`, componentes necesarios para gestionar ventanas y diálogos secundarios en la aplicación.
   
2. **GameView y MainMenu**: clases principales de la interfaz, encargadas de mostrar el tablero del juego y el menú principal respectivamente, así como de gestionar la interacción del usuario.

### Clase GameView

**GameView**  es una ventaan **TopLevel** y representa la vista del juego y gestiona la interfaz del tablero, el contador de movimientos y el temporizador. Esta clase se comunica con el controlador mediante callbacks para responder a los eventos del usuario.  Las callbacks se utilizan para enlazar funciones que se deberían ejecutar en la parte de la vista, pero como estamos tratando de seguir un modelo vista controlador, lo que hacemos es crear una función que sólo hará una llamada de vuelta a la implementación de la misma, esta se lleva a cabo en el controlador, cuando instanciemos la clase de la vista, asociaremos esos callbacks a las funciones correctas que tenemos definidas en la clase controlador. En las siguientes imágenes podéis ver cómo sería el constructor de las clases de la vista utilizando callbacks:
![imagen](https://github.com/user-attachments/assets/80104444-8061-4276-8f26-689941623133)

![imagen](https://github.com/user-attachments/assets/a789d2bf-0b09-40df-b955-64705e43537b)


#### Atributos y métodos clave

1. **`__init__(self, on_card_click_callback, update_move_count_callback, update_time_callback)`**
   - Inicializa los elementos necesarios para la vista del juego.
   - **`self.labels`**: almacena las etiquetas de las cartas, representadas por `Label`, asociando cada posición del tablero a una etiqueta específica.
   - **`on_card_click_callback`, `update_move_count_callback`, `update_time_callback`**: funciones de callback que permiten que la vista se comunique con el controlador cuando se realizan acciones como hacer clic en una carta, actualizar el contador de movimientos o el temporizador.

2. **`create_board(self, model)`**
   - Crea la ventana de juego como una instancia de `Toplevel`, estableciendo su título y usando el modelo (`model`) para definir el tamaño y el contenido del tablero.
   - Genera un tablero de `Label` en función del tamaño, cada una mostrando una imagen oculta. Las etiquetas se organizan en una cuadrícula y están vinculadas a eventos de clic que llaman al callback `on_card_click_callback`.
   - Añade etiquetas para el contador de movimientos y el temporizador, posicionándolas debajo del tablero.

3. **`update_board(self, pos, image_id)`**
   - Actualiza la imagen de una carta en una posición específica (`pos`), configurando la imagen correspondiente según el `image_id` proporcionado.
   
4. **`reset_cards(self, pos1, pos2)`**
   - Restaura las imágenes de dos cartas a su estado oculto, útil cuando el jugador no encuentra una coincidencia entre dos cartas seleccionadas.

5. **`update_move_count(self, moves)`**
   - Actualiza el contador de movimientos en la interfaz, modificando el texto de la etiqueta que muestra los movimientos actuales.

6. **`update_time(self, time)`**
   - Actualiza el temporizador en la interfaz para reflejar el tiempo transcurrido.

7. **`destroy(self)`**
   - Cierra la ventana del tablero y limpia los elementos almacenados en `labels`. Esto es útil para restablecer la interfaz al terminar el juego o al regresar al menú principal.

### Clase MainMenu

**MainMenu** es la interfaz principal que permite al jugador navegar por las opciones de juego, iniciar una nueva partida, ver las estadísticas y salir de la aplicación.

#### Atributos y métodos clave

1. **`__init__(self,root, start_game_callback, show_stats_callback, quit_callback)`**
   - Inicializa la ventana principal del menú y establece su título.
   - Crea tres botones: **Jugar**, **Estadísticas** y **Salir**, cada uno enlazado a un callback específico para iniciar el juego, mostrar las estadísticas y cerrar la aplicación.

2. **`ask_player_name(self)`**
   - Abre un diálogo para solicitar el nombre del jugador. Esta función se utiliza al iniciar una partida para personalizar la experiencia del usuario.

3. **`show_stats(self, stats)`**
   - Abre una ventana `Toplevel` para mostrar las estadísticas de los jugadores. Las puntuaciones se organizan por nivel de dificultad, y cada entrada muestra el nombre y el número de movimientos realizados.
   - Cada nivel de dificultad tiene su propia sección con una lista de las mejores puntuaciones.

### Flujo de funcionamiento

`vista.py` define la interfaz gráfica del usuario, proporcionando una estructura de navegación y visualización del juego. `MainMenu` gestiona el acceso al juego y a las estadísticas, mientras que `GameView` ofrece una experiencia visual interactiva durante la partida, mostrando el tablero, las cartas y los contadores de tiempo y movimientos. La comunicación entre la vista y el controlador se facilita a través de funciones de callback, permitiendo una actualización dinámica y una experiencia de usuario fluida.

---
## `controlador.py`

### Importaciones y dependencias

1. **tkinter**: se importan `messagebox`, `simpledialog`, `Toplevel`, `Label` y `Tk` para manejar ventanas emergentes, diálogos, y la estructura general de la interfaz.
   
2. **GameModel (de modelo.py)**: representa el modelo de datos del juego, incluyendo la lógica central para la generación del tablero, la carga de imágenes y la verificación de coincidencias.
   
3. **MainMenu y GameView (de vista.py)**: componentes de la interfaz que permiten la interacción del usuario. `MainMenu` muestra el menú principal, mientras que `GameView` maneja la vista del juego y el tablero.

4. **time**: módulo utilizado para gestionar los tiempos de espera y la sincronización en la interfaz del juego.

### Clase GameController

**GameController** es el controlador principal del juego y coordina el flujo de la aplicación, gestionando la comunicación entre el modelo (`GameModel`) y la vista (`MainMenu` y `GameView`). Maneja eventos de usuario como clics en el tablero, y actualiza el estado del juego y la interfaz en consecuencia.

#### Atributos y métodos clave

1. **`__init__(self, root)`**
   - Inicializa el controlador del juego y configura los atributos clave:
     - **`self.root`**: referencia a la ventana principal de Tkinter.
     - **`self.model`**: instancia del modelo del juego, creada al iniciar una partida.
     - **`self.selected`**: lista que almacena las posiciones de las cartas seleccionadas por el jugador.
     - **`self.timer_started`**: booleano que indica si el temporizador ha comenzado.
   - Crea el menú principal (`MainMenu`) y establece sus callbacks para iniciar el juego, mostrar estadísticas y salir de la aplicación.

2. **`show_difficulty_selection(self)`**
   - Abre un diálogo para que el jugador seleccione la dificultad del juego (fácil, medio o difícil). Si elige una opción válida, se solicita el nombre del jugador y, si se proporciona, se inicia el juego.

3. **`start_game(self, difficulty)`**
   - Muestra una ventana de carga y crea una instancia de `GameModel` con la dificultad y el nombre del jugador proporcionados. Llama a `check_images_loaded` para verificar que las imágenes del juego se han descargado antes de mostrar el tablero.

4. **`show_loading_window(self, message)`**
   - Crea una ventana de carga temporal para indicar que el tablero está siendo preparado. Esta ventana impide que el jugador interactúe con otras partes de la interfaz hasta que se complete la carga.

5. **`check_images_loaded(self)`**
   - Comprueba si las imágenes se han cargado completamente en el modelo. Una vez listas, destruye la ventana de carga y crea una instancia de `GameView` para mostrar el tablero y los controles del juego. Si las imágenes no están listas, vuelve a comprobar después de un breve intervalo.

6. **`on_card_click(self, pos)`**
   - Maneja el evento de clic en una carta del tablero. Si el temporizador no ha comenzado, lo inicia y actualiza el temporizador en la interfaz.
   - Almacena la posición de la carta seleccionada y, si hay dos cartas en `self.selected`, llama a `handle_card_selection` para verificar si coinciden.

7. **`handle_card_selection(self)`**
   - Verifica si las dos cartas seleccionadas forman una pareja utilizando el método `check_match` del modelo. Si coinciden, mantiene la visualización de las cartas; si no, las oculta después de un breve retraso.
   - Actualiza el contador de movimientos en la interfaz y llama a `check_game_complete` para ver si el jugador ha completado el juego.

8. **`update_move_count(self, moves)`**
   - Actualiza el contador de movimientos en `GameView` para reflejar el número actual de movimientos realizados.

9. **`check_game_complete(self)`**
   - Verifica si el juego está completo llamando a `is_game_complete` del modelo. Si el jugador ha encontrado todas las parejas, muestra un mensaje de victoria y regresa al menú principal.

10. **`return_to_main_menu(self)`**
    - Cierra la vista de juego actual y vuelve al menú principal, permitiendo que el jugador inicie una nueva partida o salga del juego.

11. **`show_stats(self)`**
    - Obtiene las estadísticas de puntuaciones desde el modelo y las muestra en el menú principal.

12. **`update_time(self)`**
    - Actualiza el temporizador en la vista del juego. Llama a sí misma cada segundo mientras el juego esté activo, manteniendo el tiempo en la interfaz sincronizado con el tiempo transcurrido.

### Flujo de funcionamiento

`GameController` orquesta la interacción entre la interfaz de usuario y el modelo de datos, manejando eventos de usuario y actualizando el estado del juego en tiempo real. Gestiona la carga de imágenes, el temporizador, el contador de movimientos y la lógica de coincidencias, permitiendo una experiencia de juego interactiva y dinámica. A través de los métodos de callback y las ventanas auxiliares, el controlador asegura una navegación fluida entre el menú principal y el tablero de juego.

--- 

## Fases de codificación del programa 

### Fase 1: Configuración del menú principal

1. **Objetivo**: Crear el menú principal con opciones para iniciar el juego, ver estadísticas y salir.
2. **Pasos**:
   - Implementa la clase `MainMenu` en `vista.py` para mostrar las opciones de “Jugar”, “Estadísticas” y “Salir”.
   - Asegúrate de que cada botón está correctamente enlazado a los métodos `start_game_callback`, `show_stats_callback`, y `quit_callback` definidos en `GameController`. Estas funciones pueden mostrar una ventana emergente indicando que se ha pulsado cada botón, o pueden enlazar con una función "tonta" con un pass cómo único código de ejecución
   - **ESTO NO HAY QUE HACERLO** ~~Oculta la ventana principal de Tkinter (`root.withdraw()`) en `main.py` tras inicializar el menú.~~
   - **Prueba**: Ejecuta `main.py` y verifica que el menú principal aparece con las opciones indicadas, y que al seleccionar "Salir" la aplicación se cierra correctamente.

---

### Fase 2: Selección de dificultad y nombre del jugador

1. **Objetivo**: Implementar la funcionalidad para pedir la dificultad y el nombre del jugador antes de empezar la partida.
2. **Pasos**:
   - En `GameController`, implementa el método `show_difficulty_selection`, que solicita al jugador la dificultad mediante un cuadro de diálogo (Puedes usar `simpledialog.askstring(Título, texto, parent = widgetPadre)`). Si ya lo has implementado de otra forma, no hay problema. Asegúrate de que solo se aceptan opciones válidas: “facil”, “medio” y “dificil”.
   - Si la dificultad es válida, solicita el nombre del jugador usando el método `ask_player_name` de `MainMenu` y almacénalo en `self.player_name`. También puedes usar `simpledialog.askstring`.
   - **Prueba**: Ejecuta la aplicación, selecciona “Jugar” en el menú principal y verifica que solicita la dificultad y el nombre. El menú principal debe permanecer abierto tras realizar estos pasos.

---

### Fase 3: Creación del tablero y carga de imágenes

1. **Objetivo**: Crear el tablero de juego y descargar las imágenes necesarias para las cartas, asegurando que solo se muestra el tablero una vez que todas las imágenes están disponibles.
2. **Pasos**:
   - En el modelo (`GameModel` en `modelo.py`), utiliza `_generate_board` para generar el tablero con posiciones aleatorias de cartas, basadas en la dificultad seleccionada.
   - Implementa el método `_load_images`, que usa `descargar_imagen` de `recursos.py` para descargar la imagen oculta y las cartas desde GitHub.
     - Usa el hilo de ejecución en `load_images_thread` para descargar las imágenes de forma asíncrona.
     - Asegúrate de que el evento `self.images_loaded` se establece una vez que todas las imágenes están descargadas.
   - En `GameController`, crea el método `show_loading_window` para mostrar una ventana de carga mientras las imágenes se descargan.
   - Una vez descargadas las imágenes (`images_are_loaded` es verdadero), destruye la ventana de carga y llama a `GameView.create_board` para mostrar el tablero.
     Puedes consultar más información sobre su implementación en este [enlace:](hilosSprint%234.md) 
   - **Prueba**: Ejecuta la aplicación, selecciona la dificultad y el nombre. Verifica que se muestra la ventana de carga y que, después, se abre el tablero con las cartas ocultas.

---

### Fase 4: Interacción del jugador (selección de cartas)

1. **Objetivo**: Implementar la funcionalidad para que el jugador pueda hacer clic en dos cartas, mostrarlas temporalmente y verificar si son iguales.
2. **Pasos**:
   - En `GameController`, implementa `on_card_click`, que maneja la selección de cartas:
     - Si el temporizador aún no ha comenzado, inicia el temporizador llamando a `model.start_timer()` y usa `update_time` para actualizar el tiempo.
     - Almacena la posición de cada carta seleccionada en `self.selected` y usa `update_board` de `GameView` para mostrar temporalmente la imagen de la carta.
   - Implementa `handle_card_selection`, que verifica si ambas cartas seleccionadas son iguales:
     - Si coinciden, actualiza las cartas permanentemente en el tablero.
     - Si no coinciden, usa `reset_cards` de `GameView` para ocultarlas tras un breve retraso.
   - Llama a `update_move_count` para incrementar el contador de movimientos y actualiza la etiqueta de movimientos en la vista.
   - Usa `update_time` en `GameController` para actualizar el temporizador cada segundo con `self.root.after(1000, self.update_time)`.
   - **Prueba**: Ejecuta la aplicación, selecciona dos cartas y verifica que las cartas se muestran y se ocultan correctamente según coincidan o no. La etiqueta de movimientos y el temporizador deben actualizarse conforme al juego.

---

### Fase 5: Completitud del juego y guardado de estadísticas

1. **Objetivo**: Detectar la finalización del juego y guardar el resultado del jugador, mostrando estadísticas.
2. **Pasos**:
   - En `check_game_complete` de `GameController`, verifica si el jugador ha encontrado todos los pares de cartas.
     - Si el juego está completo, muestra un mensaje de felicitación con el número de movimientos utilizados y llama a `return_to_main_menu` para volver al menú principal.
   - Implementa el método `save_score` en `GameModel` para guardar la puntuación del jugador:
     - Registra el nombre, dificultad, número de movimientos y fecha en `ranking.txt`.
     - Ordena las puntuaciones de cada dificultad y almacena solo las tres mejores en el archivo.
   - En `load_scores`, carga las puntuaciones desde `ranking.txt` y devuelve un diccionario con las puntuaciones por dificultad. Para que las estadísticas se puedan cargar al iniciar la aplicación, tenemos que modificar el constructor del controlador en `main.py` pasándole el modelo.
   - En `MainMenu`, implementa `show_stats`, que crea una ventana emergente con el ranking de puntuaciones por dificultad.
     - Asegúrate de que las estadísticas se ordenan y muestran con el formato correcto.
   - **Prueba**: Completa el juego y verifica que la puntuación se guarda correctamente en `ranking.txt`. Abre la sección de “Estadísticas” en el menú principal y asegúrate de que las puntuaciones se muestran en orden.

---

## Rúbrica



### **Fase 1: Configuración del menú principal**

| Criterio                        | Excelente (10)                                                                                          | Bueno (7)                                                                                  | Adecuado (5)                                                                                | Insuficiente (2)                                                                          |
|---------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Menú principal funcional**    | El menú principal muestra correctamente las opciones de “Jugar”, “Estadísticas” y “Salir”.             | El menú principal se muestra y las opciones están presentes, pero con pequeños errores visuales. | El menú principal se muestra, pero falta al menos una opción o tiene errores en el diseño.     | El menú principal no se muestra o es completamente disfuncional.                           |
| **Función de los botones**      | Todos los botones responden y activan la función correspondiente en `GameController`.                  | Todos los botones responden, aunque algunos presentan ligeros errores de activación.       | Algunos botones no funcionan o presentan errores importantes en el comportamiento.            | Los botones no funcionan en absoluto o muestran fallos importantes en el código.           |

---

### **Fase 2: Selección de dificultad y nombre del jugador**

| Criterio                        | Excelente (10)                                                                                          | Bueno (7)                                                                                  | Adecuado (5)                                                                                | Insuficiente (2)                                                                          |
|---------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Solicitud de dificultad**     | Solicita la dificultad y acepta únicamente “facil”, “medio” y “dificil”.                              | Solicita la dificultad y funciona correctamente, pero acepta alguna entrada incorrecta.    | Solicita la dificultad, pero no siempre funciona según lo esperado o permite cualquier entrada. | No solicita la dificultad o no realiza ninguna verificación de entrada.                   |
| **Solicitud de nombre**         | Solicita el nombre del jugador y lo almacena correctamente en `self.player_name`.                      | Solicita el nombre del jugador y lo almacena, aunque presenta ligeros errores.             | Solicita el nombre, pero no siempre se almacena correctamente o presenta problemas de flujo.  | No solicita el nombre del jugador o no lo almacena, afectando el flujo del juego.         |

---

### **Fase 3: Creación del tablero y carga de imágenes**

| Criterio                        | Excelente (10)                                                                                          | Bueno (7)                                                                                  | Adecuado (5)                                                                                | Insuficiente (2)                                                                          |
|---------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Generación del tablero**      | El tablero se genera correctamente con cartas ocultas en posiciones aleatorias según la dificultad.    | El tablero se genera y las posiciones son aleatorias, aunque con errores menores.          | El tablero se genera pero no cumple con la aleatoriedad o dificultad especificada.            | El tablero no se genera correctamente o está completamente disfuncional.                  |
| **Carga de imágenes**           | Las imágenes se descargan correctamente, se redimensionan y se muestran en el tablero.                 | Las imágenes se descargan y se muestran, aunque hay ligeros problemas de tamaño o calidad. | Las imágenes se descargan, pero presentan errores significativos en el tamaño o no todas se muestran. | Las imágenes no se descargan o no se muestran, afectando la funcionalidad del tablero.     |
| **Ventana de carga**            | La ventana de carga se muestra mientras se descargan las imágenes y se cierra automáticamente al finalizar. | La ventana de carga aparece y se cierra, aunque con algunos retrasos en la sincronización. | La ventana de carga se muestra, pero no se cierra automáticamente tras la descarga.           | La ventana de carga no aparece o no se cierra, afectando la experiencia de usuario.       |

---

### **Fase 4: Interacción del jugador (selección de cartas)**

| Criterio                          | Excelente (10)                                                                                          | Bueno (7)                                                                                  | Adecuado (5)                                                                                | Insuficiente (2)                                                                          |
|-----------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Selección de cartas**           | Permite al jugador seleccionar dos cartas y mostrarlas temporalmente.                                  | Permite seleccionar cartas y las muestra, pero presenta ligeros errores en la visibilidad. | Permite seleccionar cartas, pero algunas veces no muestra correctamente las cartas seleccionadas. | No permite seleccionar cartas o no muestra las cartas en absoluto.                         |
| **Verificación de coincidencia**  | Las cartas iguales permanecen reveladas, mientras que las que no coinciden se ocultan tras un tiempo.  | La verificación de coincidencia funciona, aunque presenta ligeros problemas de consistencia. | La verificación es inconsistente y en algunos casos no oculta correctamente las cartas que no coinciden. | La verificación de coincidencia no funciona en absoluto, afectando la jugabilidad.         |
| **Actualización de movimientos**  | La etiqueta de movimientos se actualiza correctamente tras cada par de cartas seleccionadas.           | La etiqueta de movimientos se actualiza, aunque con ligeros errores en el conteo.          | La etiqueta de movimientos solo se actualiza en algunos casos, mostrando datos incorrectos.    | La etiqueta de movimientos no se actualiza o muestra siempre el mismo valor.              |
| **Temporizador en la interfaz**   | El temporizador se inicia al primer clic y se actualiza en tiempo real cada segundo en la interfaz.    | El temporizador se inicia y actualiza, pero presenta retrasos menores.                     | El temporizador se inicia, pero su actualización es inconsistente o irregular.                | El temporizador no se inicia ni se actualiza en la interfaz de usuario.                   |

---

### **Fase 5: Completitud del juego y guardado de estadísticas**

| Criterio                          | Excelente (10)                                                                                          | Bueno (7)                                                                                  | Adecuado (5)                                                                                | Insuficiente (2)                                                                          |
|-----------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Verificación de juego completo** | Detecta correctamente cuando el juego se completa y muestra un mensaje de felicitación.               | Detecta la finalización del juego, pero con pequeños errores en el mensaje.               | Detecta la finalización del juego solo en algunos casos, sin siempre mostrar el mensaje.      | No detecta la finalización del juego o no muestra ningún mensaje al completarlo.          |
| **Guardado de puntuación**         | La puntuación (nombre, dificultad, movimientos) se guarda correctamente en `ranking.txt`.             | La puntuación se guarda en `ranking.txt`, aunque presenta errores menores en el formato.   | La puntuación se guarda, pero con errores significativos en el formato o los datos.           | No guarda la puntuación, o los datos guardados son incorrectos o están incompletos.       |
| **Carga y visualización de estadísticas** | Las estadísticas se cargan de `ranking.txt` y se muestran correctamente en el menú.        | Las estadísticas se cargan y muestran, aunque con pequeños errores de formato.             | Las estadísticas se cargan pero se muestran de manera desorganizada o con datos incorrectos. | No carga ni muestra las estadísticas, o presenta errores significativos que afectan su visualización. |

---



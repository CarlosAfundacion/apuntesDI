
### Guía detallada de implementación del juego de memoria en Tkinter

#### **1. `main.py` - punto de entrada del juego**
   - **Propósito**: Actúa como el punto de inicio de la aplicación, creando la instancia de `GameController` y lanzando el juego.
   - **Implementación**:
     - Crea una instancia de `GameController` y ejecuta `mainloop()` para mantener abierta la ventana.
     - **Sugerencia**: asegúrate de que `GameController` controla correctamente las vistas y modelos.
   - **Estructura**:
     - No requiere clases ni atributos; solo es el inicializador de `GameController`.

---

#### **2. `controlador.py` - controlador del juego**
   - **Clase `GameController`**: Conecta el modelo y la vista, gestionando el flujo del juego, los eventos de usuario y la lógica de juego.
   
   - **Atributos**:
     - `root: MainMenu`: instancia del menú principal, creada cuando se inicia el juego.
     - `model: GameModel | None`: referencia al modelo de datos del juego, inicializada solo al comenzar una partida.
     - `view: GameView | None`: referencia a la vista del tablero de juego, creada al cargar el tablero.
     - `player_name: str`: almacena el nombre del jugador.
     - `loading_window: Toplevel | None`: ventana de carga temporal mientras las imágenes se cargan.

   - **Métodos**:
     - `show_difficulty_selection()`: abre un cuadro de diálogo para seleccionar la dificultad (`str`). Si es válida, pide el nombre del jugador (`ask_player_name()` en `MainMenu`) y llama a `start_game(difficulty)`.
       - **Pista**: utiliza una lista de dificultades válidas y condicionales para verificar la entrada.
     - `start_game(difficulty: str)`: inicializa `GameModel` y `loading_window`, que es una ventana emergente mientras se cargan las imágenes. Después, llama periódicamente a `check_images_loaded()` para verificar si el tablero está listo.
       - **Pista**: muestra un mensaje en `loading_window` para indicar que el tablero se está cargando.
     - `check_images_loaded()`: comprueba el atributo `images_loaded` de `GameModel`. Si está listo, destruye `loading_window` y crea `GameView`. A continuación, llama a `update_time()` para mostrar el tiempo de juego.
       - **Pista**: usa `after()` de Tkinter para reintentar en intervalos.
     - `reveal_card(pos: tuple[int, int])`: gestiona la selección de cartas. Si hay dos seleccionadas, llama a `check_match()` en `GameModel`. Si coinciden, las mantiene descubiertas; si no, usa `reset_cards()` en `GameView`.
       - **Pista**: controla la lógica de coincidencias con una lista `selected` que guarda dos posiciones y se reinicia tras cada intento.
     - `check_game_complete()`: revisa si todos los pares de cartas están descubiertos, y si es así, llama a `save_score()` en `GameModel` y muestra un mensaje de victoria. Retorna al menú principal llamando a `return_to_main_menu()`.
       - **Pista**: usa el atributo `pairs_found` del modelo para controlar el progreso.
     - `show_stats()`: llama a `load_scores()` en `GameModel` y a `show_stats()` en `MainMenu` para mostrar una ventana con las estadísticas.
       - **Pista**: este método solo se activa desde el menú principal y abre una ventana secundaria para mostrar los resultados.

---

#### **3. `modelo.py` - modelo de datos del juego**
   - **Clase `GameModel`**: gestiona la lógica de datos del juego, incluyendo el tablero, la carga de imágenes y el sistema de puntuación.
   
   - **Atributos**:
     - `difficulty: str`: nivel de dificultad elegido (`fácil`, `medio` o `difícil`).
     - `player_name: str`: nombre del jugador.
     - `board_size: int`: calcula el tamaño del tablero en función de la dificultad (`4`, `6` u `8`).
     - `board: list[list[int]]`: matriz bidimensional con identificadores de imagen organizados en pares.
     - `start_time: float`: tiempo de inicio del juego, almacenado con `time.time()`.
     - `moves: int`: contador de movimientos del jugador.
     - `pairs_found: int`: número de pares encontrados.
     - `images: dict[int, PhotoImage]`: diccionario con las imágenes de cartas, con el identificador de imagen como clave.
     - `hidden_image: PhotoImage | None`: imagen oculta de las cartas.
     - `images_loaded: threading.Event`: se usa para indicar que las imágenes se han cargado.

   - **Métodos**:
     - `_generate_board() -> list[list[int]]`: genera el tablero en forma de matriz bidimensional. Dado el número de pares necesarios, organiza los identificadores de imagen en pares aleatorios.
       - **Pista**: usa `random.shuffle` para mezclar los identificadores de imagen en la matriz.
     - `_download_image(image_id: str) -> PhotoImage`: descarga y ajusta el tamaño de cada imagen desde un repositorio. Llamado por `_load_images()` para gestionar la carga.
       - **Pista**: usa `requests` y `PIL` para obtener y redimensionar imágenes.
     - `_load_images()`: inicia la carga de imágenes en un hilo separado. Descarga tanto la imagen oculta como las imágenes individuales de cada par.
       - **Pista**: usa `threading.Thread` y establece `images_loaded` cuando todas las imágenes estén disponibles.
     - `check_match(pos1: tuple[int, int], pos2: tuple[int, int]) -> bool`: compara dos posiciones en el tablero y verifica si las imágenes son iguales. Incrementa `pairs_found` si coinciden.
       - **Pista**: devuelve `True` si las posiciones coinciden y actualiza el contador `moves`.
     - `get_time() -> int`: calcula el tiempo de juego actual restando `start_time` del tiempo actual.
       - **Pista**: `get_time` se llama en intervalos regulares para actualizar el tiempo en la interfaz.
     - `save_score()`: guarda la puntuación en `ranking.txt`, ordenando los mejores resultados para cada nivel de dificultad.
       - **Pista**: carga las puntuaciones actuales, agrega la nueva y vuelve a guardar, manteniendo solo las tres mejores.
     - `load_scores() -> dict[str, list[dict[str, Union[str, int]]]]`: carga y organiza las puntuaciones de `ranking.txt` en un diccionario con tres niveles de dificultad.
       - **Pista**: usa un `try-except` para gestionar la ausencia de archivo y cargar un formato de puntuación simple con `nombre`, `movimientos` y `fecha`.

---

#### **4. `vista.py` - vista del juego**
   - **Clases**:
     - `MainMenu`: gestiona el menú principal con botones para iniciar el juego, ver estadísticas o salir.
     - `GameView`: muestra el tablero de juego y permite la interacción del usuario para descubrir cartas y ver el progreso del juego.
   
   - **Atributos** (GameView):
     - `controller: GameController`: referencia al controlador para gestionar los eventos de usuario.
     - `model: GameModel`: referencia al modelo para acceder al estado del tablero y las imágenes.
     - `board_size: int`: tamaño del tablero, basado en el nivel de dificultad.
     - `labels: dict[tuple[int, int], Label]`: diccionario de etiquetas en la interfaz, con cada posición del tablero representada como una etiqueta.
     - `move_label: Label`: etiqueta que muestra el número de movimientos realizados.
     - `time_label: Label`: etiqueta que muestra el tiempo de juego transcurrido.

   - **Métodos** (GameView):
     - `update_board(pos: tuple[int, int], image_id: int)`: actualiza una etiqueta específica para mostrar la imagen de la carta en `pos`.
       - **Pista**: accede al diccionario `labels` y configura la imagen de la etiqueta con el `image_id`.
     - `reset_cards(pos1: tuple[int, int], pos2: tuple[int, int])`: restablece dos cartas al estado oculto si no coinciden.
       - **Pista**: usa `labels[pos1]` y `labels[pos2]` para ocultar ambas cartas.
     - `update_move_count(moves: int)`: actualiza `move_label` con el número actual de movimientos.
       - **Pista**: actualiza el texto de `move_label` con el valor de `moves`.
     - `update_time(time: int)`: actualiza `time_label` con el tiempo de juego actual.
       - **Pista**: usa `time_label.config(text=...)` para mostrar el tiempo en segundos.
   - **Métodos** (MainMenu):
     - `ask_player_name() -> str`: abre un cuadro de diálogo para que el jugador ingrese su nombre.
       - **Pista**: usa `simpledialog.askstring` para capturar la entrada de texto del jugador.
     - `show_stats(stats: dict

[str, list[dict[str, Union[str, int]]]])`: muestra una ventana con las puntuaciones y mejores resultados.
       - **Pista**: para cada nivel de dificultad en `stats`, usa etiquetas (`Label`) para mostrar los nombres y movimientos.

---

### Desarrollo incremental y fases de codificación

1. **Fase 1: configuración del proyecto y creación del menú principal**
   - Crear archivos base `main.py`, `controlador.py`, `modelo.py` y `vista.py`.
   - Implementar `MainMenu` con botones y el método `show_difficulty_selection()` en el controlador, que pedirá el nombre y la dificultad del jugador.
   - **Commit**: "Configuración inicial del proyecto y creación del menú principal."

2. **Fase 2: inicialización del tablero y carga de imágenes**
   - Configurar `GameModel` para generar el tablero con pares de identificadores de imágenes.
   - Desarrollar el método `_load_images()` para cargar las imágenes en segundo plano. `start_game()` en el controlador debe llamar a `check_images_loaded()` para verificar si la carga ha finalizado y mostrar el tablero.
   - **Commit**: "Configuración de tablero y carga de imágenes."

3. **Fase 3: lógica de juego y coincidencias**
   - Implementar `reveal_card()` y `check_match()` en el controlador y modelo para verificar coincidencias.
   - Crear `GameView` con el método `update_board()` para mostrar cartas y `reset_cards()` para ocultarlas si no coinciden.
   - **Commit**: "Implementación de lógica de coincidencias y revelación de cartas."

4. **Fase 4: finalización del juego y estadísticas**
   - Completar `check_game_complete()` para finalizar el juego y mostrar un mensaje de victoria. Este método debe llamar a `save_score()` para guardar la puntuación.
   - Implementar `show_stats()` en el controlador, que debe llamar a `load_scores()` en el modelo y `show_stats(stats)` en `MainMenu` para visualizar los resultados.
   - **Commit**: "Finalización del juego, registro de puntuaciones y visualización de estadísticas."

---

### Rúbrica de evaluación para cada fase

| **Criterios de evaluación**         | **Fase 1** (10 puntos)                                       | **Fase 2** (15 puntos)                                            | **Fase 3** (20 puntos)                                            | **Fase 4** (15 puntos)                                             |
|-------------------------------------|--------------------------------------------------------------|-------------------------------------------------------------------|-------------------------------------------------------------------|-------------------------------------------------------------------|
| **Estructura del proyecto**         | **2 puntos** <br> Archivos organizados correctamente         | **3 puntos** <br> Lógica de carga de imágenes correctamente en hilos | **5 puntos** <br> Configuración completa de tablero en `GameView` | **3 puntos** <br> `ranking.txt` implementado correctamente        |
| **Interfaz de menú principal**      | **3 puntos** <br> Botones de `MainMenu` funcionales          | **4 puntos** <br> Ventana de carga visualizada                    | **5 puntos** <br> Eventos de clic en cartas funcionales           | **4 puntos** <br> Ventana de estadísticas muestra resultados       |
| **Controlador**                     | **5 puntos** <br> `show_difficulty_selection()` y `start_game()` correctamente implementados | **8 puntos** <br> `check_images_loaded()` inicia tablero | **5 puntos** <br> `reveal_card()` correctamente para verificar pares | **8 puntos** <br> `check_game_complete()` y mensaje de finalización |
| **Modelo de datos y lógica**        |                                                              | **10 puntos** <br> `GameModel` con generación y carga de tablero | **5 puntos** <br> `check_match()` verifica pares correctamente     |

Con esta guía, podrás implementar el juego paso a paso, entendiendo cómo se relacionan los métodos y clases entre sí.

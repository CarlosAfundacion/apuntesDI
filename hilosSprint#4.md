### Pasos de un ejemplo de código para la descagra de imágenes usando hilos

1. **Definir la función `_load_images` y su hilo interno**:
   - `_load_images` contiene una función interna llamada `load_images_thread`, que será ejecutada en un hilo independiente. Esto permite que la interfaz gráfica de usuario siga funcionando mientras las imágenes se descargan en segundo plano.
   

2. **Definir la URL base**:
   - La URL base (`url_base`) indica la ubicación en línea de las imágenes. En este caso, todas las imágenes del juego están almacenadas en un repositorio en GitHub.


3. **Cargar la imagen oculta (cara trasera de la imagen)**:
   - La primera imagen que se descarga es la "imagen oculta" (`hidden.png`). Esta imagen se mostrará en cada carta al inicio del juego y se almacena en el atributo `self.hidden_image`.


4. **Obtener identificadores únicos de imágenes**:
   - Para evitar la descarga de imágenes duplicadas, el código recorre cada fila del tablero (`self.board`). Si el identificador (`image_id`) de una imagen no está ya en `unique_image_ids`, se añade a la lista. Esto garantiza que solo se descarguen una vez las imágenes que se van a utilizar en el tablero.

    ```python
    if image_id not in unique_image_ids:
                unique_image_ids.append(image_id)
    ```

5. **Descargar cada imagen con los identificadores únicos**:
   - Con la lista `unique_image_ids`, el código itera sobre cada `image_id` y utiliza la función `descargar_imagen` para descargar la imagen correspondiente desde la URL base. Cada imagen descargada se almacena en el diccionario `self.images` usando su `image_id` como clave.


6. **Indicar que las imágenes se han cargado**:
   - Una vez que todas las imágenes se han descargado y almacenado, `self.images_loaded` se establece en `True`. Esto permite que otras partes del programa verifiquen si las imágenes ya están disponibles.


7. **Iniciar el hilo para cargar imágenes**:
   - Al final de `_load_images`, un nuevo hilo se inicia usando `threading.Thread` y ejecuta `load_images_thread`. Este hilo es `daemon`, lo que significa que se cerrará automáticamente cuando la aplicación principal se cierre.

    ```python
    threading.Thread(target=load_images_thread, daemon=True).start()
    ```



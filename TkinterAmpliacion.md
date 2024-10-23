# Ampliación Python y Tkinter

## Índice

1. [Manipulación de archivos en Python](#1-manipulación-de-archivos-en-python)
2. [Uso de excepciones en Python](#2-uso-de-excepciones-en-python)
3. [Tkinter: Ampliación de conceptos](#3-tkinter-ampliación-de-conceptos)
    - [Eventos y Binding (Teclado y ratón) en Tkinter](#eventos-y-binding-teclado-y-ratón-en-tkinter)
    - [Manejo de ventanas emergentes (Toplevel) en Tkinter](#manejo-de-ventanas-emergentes-toplevel-en-tkinter)
        - [grab_set()](#grab_set)
        - [winfo_screenwidth y winfo_screenheight](#winfo_screenwidth-y-winfo_screenheight)
        - [winfo_width y winfo_height](#winfo_width-y-winfo_height)
        - [update_idletasks](#update_idletasks)
        - [winfo_reqwidth y winfo_reqheight](#winfo_reqwidth-y-winfo_reqheight)
    - [Uso de `quit` y `destroy` en Tkinter](#uso-de-quit-y-destroy-en-tkinter)
4. [Funciones lambda en Python y su aplicación en Tkinter](#4-funciones-lambda-en-python-y-su-aplicación-en-tkinter)
5. [Gestión de imágenes en Tkinter](#5-gestión-de-imágenes-en-tkinter)
    - [Uso de Pillow](#uso-de-pillow)
    - [Carga de imágenes desde GitHub](#carga-de-imágenes-desde-github)
    - [Uso de hilos](#uso-de-hilos)
6. [Modelo Vista Controlador (MVC) en Tkinter](#6-modelo-vista-controlador-mvc-en-tkinter)

---


## 1. Manipulación de archivos en Python

Python ofrece múltiples formas de manejar archivos, permitiendo leer, escribir y manipular datos de manera eficiente.

### Apertura y cierre de archivos

Para trabajar con archivos, primero se deben abrir utilizando la función `open()`, y es una buena práctica cerrarlos después de su uso con `close()` o utilizando el contexto `with`.

```python
# Abrir un archivo en modo lectura
archivo = open('datos.txt', 'r')

# Leer el contenido
contenido = archivo.read()
print(contenido)

# Cerrar el archivo
archivo.close()
```

**Usando contexto `with`:**

```python
with open('datos.txt', 'r') as archivo:
    contenido = archivo.read()
    print(contenido)
# El archivo se cierra automáticamente al salir del bloque
```

### Lectura de archivos

Existen varias formas de leer archivos:

- `read()`: Lee todo el contenido.
- `readline()`: Lee una línea a la vez.
- `readlines()`: Lee todas las líneas y las devuelve como una lista.

**Ejemplo: Leer línea por línea**

```python
with open('datos.txt', 'r') as archivo:
    for linea in archivo:
        print(linea.strip())
```

### Escritura de archivos

Para escribir en un archivo, se puede abrir en modo escritura `'w'` (sobrescribe) o modo append `'a'` (añade al final).

**Ejemplo: Escribir en un archivo**

```python
with open('salida.txt', 'w') as archivo:
    archivo.write("Primera línea\n")
    archivo.write("Segunda línea\n")
```

### Modos de Apertura

| Modo | Descripción |quit
|------|-------------|
| `'r'` | Lectura (por defecto). El archivo debe existir. |
| `'w'` | Escritura. Crea un archivo nuevo o sobrescribe uno existente. |
| `'a'` | Añadir. Escribe al final del archivo si existe, sino lo crea. |
| `'b'` | Modo binario. Se puede combinar con otros modos, e.g., `'rb'`. |
| `'+'` | Lectura y escritura. |

### Manejo de archivos CSV

Python tiene un módulo dedicado para manejar archivos CSV.

**Ejemplo: Leer un archivo CSV**

```python
import csv

with open('datos.csv', 'r', newline='') as archivo:
    lector = csv.reader(archivo)
    for fila in lector:
        print(fila)
```

**Ejemplo: Escribir en un archivo CSV**

```python
import csv

datos = [
    ['Nombre', 'Edad', 'Ciudad'],
    ['Ana', '28', 'Madrid'],
    ['Luis', '34', 'Barcelona']
]

with open('salida.csv', 'w', newline='') as archivo:
    escritor = csv.writer(archivo)
    escritor.writerows(datos)
```

---

## 2. Uso de excepciones en Python

Las excepciones permiten manejar errores de manera controlada, evitando que el programa se detenga abruptamente.

### Estructura básica de manejo de excepciones

```python
try:
    # Código que puede causar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    print("Error: División por cero.")
finally:
    print("Este bloque se ejecuta siempre.")
```

### Tipos comunes de excepciones

- `ZeroDivisionError`: División por cero.
- `FileNotFoundError`: Archivo no encontrado.
- `ValueError`: Valor incorrecto.
- `TypeError`: Tipo de dato incorrecto.

### Capturar múltiples excepciones

```python
try:
    # Código que puede causar múltiples excepciones
    numero = int(input("Introduce un número: "))
    resultado = 10 / numero
except ZeroDivisionError:
    print("Error: División por cero.")
except ValueError:
    print("Error: No has introducido un número válido.")
```

### Usar `else` en manejo de excepciones

El bloque `else` se ejecuta si no se produjo ninguna excepción.

```python
try:
    numero = int(input("Introduce un número: "))
    resultado = 10 / numero
except ZeroDivisionError:
    print("Error: División por cero.")
except ValueError:
    print("Error: No has introducido un número válido.")
else:
    print(f"El resultado es {resultado}")
```

### Crear excepciones personalizadas

Se pueden definir excepciones propias heredando de `Exception`.

```python
class MiError(Exception):
    pass

def verificar_valor(x):
    if x < 0:
        raise MiError("El valor no puede ser negativo.")

try:
    verificar_valor(-5)
except MiError as e:
    print(f"Se ha producido un error: {e}")
```

---

## 3. Tkinter: Ampliación de conceptos

Tkinter es la biblioteca estándar de Python para crear interfaces gráficas de usuario (GUI).

### Eventos y Binding (Teclado y ratón) en Tkinter

Los eventos permiten que la aplicación responda a acciones del usuario, como clics de ratón o pulsaciones de teclas.

#### Binding de eventos

Se usa el método `.bind()` para asociar un evento con una función (callback).

**Sintaxis:**

```python
widget.bind(evento, función_callback)
```

**Eventos comunes:**

- `<Button-1>`: Clic izquierdo del ratón.
- `<Button-3>`: Clic derecho del ratón.
- `<Key>`: Cualquier tecla.
- `<Return>`: Tecla Enter.

**Ejemplo: Detectar clics de ratón y pulsaciones de teclas**

```python
import tkinter as tk

def clic_izquierdo(event):
    print("Clic izquierdo en:", event.x, event.y)

def tecla_presionada(event):
    print("Tecla presionada:", event.char)

ventana = tk.Tk()
ventana.geometry("300x200")

# Asociar eventos al widget principal
ventana.bind("<Button-1>", clic_izquierdo)
ventana.bind("<Key>", tecla_presionada)

ventana.mainloop()
```

#### Eventos de ratón

Se pueden capturar movimientos, doble clic, etc.

**Ejemplo: Doble clic**

```python
def doble_clic(event):
    print("Doble clic en:", event.x, event.y)

ventana.bind("<Double-Button-1>", doble_clic)
```

#### Eventos de teclado

Capturar combinaciones de teclas o teclas específicas.

**Ejemplo: Capturar tecla Enter**

```python
def enter_presionado(event):
    print("Se ha presionado Enter.")

ventana.bind("<Return>", enter_presionado)
```

### Manejo de ventanas emergentes (Toplevel) en Tkinter

Las ventanas emergentes se crean usando el widget `Toplevel`, permitiendo crear ventanas adicionales aparte de la principal.

**Crear una ventana Toplevel:**

```python
def abrir_ventana():
    ventana_emergente = tk.Toplevel(ventana)
    ventana_emergente.title("Ventana Emergente")
    ventana_emergente.geometry("200x100")
    etiqueta = tk.Label(ventana_emergente, text="Hola desde la ventana emergente!")
    etiqueta.pack()

boton = tk.Button(ventana, text="Abrir Ventana", command=abrir_ventana)
boton.pack()
```

#### grab_set()

`grab_set()` captura todos los eventos para la ventana actual, evitando que el usuario interactúe con otras ventanas hasta que se cierre la actual. Es útil para diálogos modales.

**Ejemplo: Uso de `grab_set()`**

```python
def abrir_ventana_modal():
    ventana_modal = tk.Toplevel(ventana)
    ventana_modal.title("Modal")
    ventana_modal.geometry("200x100")
    etiqueta = tk.Label(ventana_modal, text="Esta es una ventana modal.")
    etiqueta.pack()
    boton_cerrar = tk.Button(ventana_modal, text="Cerrar", command=ventana_modal.destroy)
    boton_cerrar.pack()
    ventana_modal.grab_set()

boton_modal = tk.Button(ventana, text="Abrir Modal", command=abrir_ventana_modal)
boton_modal.pack()
```

#### winfo_screenwidth y winfo_screenheight

Estos métodos obtienen el ancho y alto de la pantalla en píxeles.

**Ejemplo: Obtener dimensiones de la pantalla**

```python
ancho_pantalla = ventana.winfo_screenwidth()
alto_pantalla = ventana.winfo_screenheight()
print(f"Resolución de pantalla: {ancho_pantalla}x{alto_pantalla}")
```

#### winfo_width y winfo_height

Obtienen el ancho y alto actual del widget.

**Ejemplo: Obtener dimensiones de la ventana**

```python
def mostrar_dimensiones():
    ancho = ventana.winfo_width()
    alto = ventana.winfo_height()
    print(f"Dimensiones de la ventana: {ancho}x{alto}")

boton_dimensiones = tk.Button(ventana, text="Mostrar Dimensiones", command=mostrar_dimensiones)
boton_dimensiones.pack()
```

#### update_idletasks

Actualiza todas las tareas pendientes del evento, asegurando que las dimensiones sean correctas antes de obtenerlas.

**Ejemplo: Uso de `update_idletasks`**

```python
def obtener_dimensiones_reales():
    ventana.update_idletasks()
    ancho = ventana.winfo_width()
    alto = ventana.winfo_height()
    print(f"Dimensiones reales: {ancho}x{alto}")

boton_real = tk.Button(ventana, text="Obtener Dimensiones Reales", command=obtener_dimensiones_reales)
boton_real.pack()
```

#### winfo_reqwidth y winfo_reqheight

Obtienen el ancho y alto requeridos del widget, es decir, el tamaño que necesita para mostrar su contenido.

**Ejemplo: Obtener tamaño requerido por un botón**

```python
boton = tk.Button(ventana, text="Botón")
boton.pack()

def mostrar_tamano_requerido():
    req_ancho = boton.winfo_reqwidth()
    req_alto = boton.winfo_reqheight()
    print(f"Tamaño requerido del botón: {req_ancho}x{req_alto}")

boton_size = tk.Button(ventana, text="Mostrar Tamaño Requerido", command=mostrar_tamano_requerido)
boton_size.pack()
```

---
### Uso de `quit` y `destroy` en Tkinter

En aplicaciones Tkinter, es importante gestionar correctamente el cierre de ventanas y la terminación del bucle principal. Los métodos `quit()` y `destroy()` son fundamentales para este propósito, pero tienen comportamientos distintos que es crucial entender para utilizarlos de manera efectiva.

#### Método `quit()`

- **Descripción:** `quit()` detiene el bucle principal de Tkinter (`mainloop()`), la ventana deja devisualizarse pero no elimina la ventana y sus widgets de memoria. Esto implica que se podría seguir ejecutando código una vez que hayamos usado este método. Por ejemplo podríamos programar el guardado de información automático a continuación de que usemos el método `quit()`.
- **Uso Común:** Se utiliza para detener la ejecución de la aplicación sin cerrar la ventana. Es útil cuando se desea pausar la interfaz o realizar operaciones adicionales después de detener el bucle.

**Ejemplo: Uso de `quit()` para detener el bucle principal**

```python
import tkinter as tk

def detener_bucle():
    print("Deteniendo el bucle principal.")
    ventana.quit()

ventana = tk.Tk()
ventana.title("Uso de quit()")

boton_quit = tk.Button(ventana, text="Detener Bucle", command=detener_bucle)
boton_quit.pack(pady=20)

ventana.mainloop()
print("El bucle principal ha sido detenido.")
```

**Explicación:**

- Al hacer clic en el botón "Detener Bucle", la función `detener_bucle()` se ejecuta, llamando a `ventana.quit()`.
- Esto detiene el `mainloop()`, pero la ventana permanece abierta hasta que se cierre manualmente.

#### Método `destroy()`

- **Descripción:** `destroy()` cierra la ventana y elimina todos los widgets asociados, liberando los recursos utilizados por Tkinter.
- **Uso Común:** Se utiliza para cerrar completamente la aplicación o una ventana específica, como una ventana emergente.

**Ejemplo: Uso de `destroy()` para cerrar la aplicación**

```python
import tkinter as tk

def cerrar_aplicacion():
    print("Cerrando la aplicación.")
    ventana.destroy()

ventana = tk.Tk()
ventana.title("Uso de destroy()")

boton_cerrar = tk.Button(ventana, text="Cerrar Aplicación", command=cerrar_aplicacion)
boton_cerrar.pack(pady=20)

ventana.mainloop()
print("La aplicación ha sido cerrada.")
```

**Explicación:**

- Al hacer clic en el botón "Cerrar Aplicación", la función `cerrar_aplicacion()` se ejecuta, llamando a `ventana.destroy()`.
- Esto cierra completamente la ventana y finaliza la aplicación.

#### Diferencias entre `quit()` y `destroy()`

| Método   | Acción Principal                                         | Persistencia de la Ventana        |
|----------|----------------------------------------------------------|------------------------------------|
| `quit()` | Detiene el bucle principal (`mainloop()`)                | La ventana sigue existiendo pero no se visualiza      |
| `destroy()` | Cierra la ventana y elimina todos sus widgets          | La ventana deja de existir         |

#### Ejemplo Combinado: Uso de `quit()` y `destroy()`

En algunos casos, puede ser necesario utilizar ambos métodos para asegurarse de que la aplicación se cierre correctamente.

```python
import tkinter as tk

def cerrar_completamente():
    print("Cerrando la aplicación y deteniendo el bucle.")
    ventana.destroy()
    ventana.quit()

ventana = tk.Tk()
ventana.title("Combinación de quit() y destroy()")

boton_cerrar = tk.Button(ventana, text="Cerrar y Detener", command=cerrar_completamente)
boton_cerrar.pack(pady=20)

ventana.mainloop()
print("La aplicación ha sido cerrada y el bucle detenido.")
```

**Explicación:**

- Al hacer clic en el botón "Cerrar y Detener", la función `cerrar_completamente()` se ejecuta, llamando primero a `ventana.destroy()` para cerrar la ventana y luego a `ventana.quit()` para detener el bucle principal.
- Esto asegura que la aplicación se cierre completamente y que no queden procesos en ejecución.

#### Consideraciones al usar `quit()` y `destroy()`

- **Evitar uso Redundante:** No es necesario llamar a ambos métodos en la mayoría de los casos. `destroy()` ya cierra la ventana y finaliza la aplicación, por lo que `quit()` puede no ser necesario.
- **Ventanas emergentes:** Cuando se manejan ventanas emergentes (`Toplevel`), es preferible usar `destroy()` para cerrar la ventana específica sin afectar al resto de la aplicación.
- **Eventos de cierre:** Es posible enlazar eventos de cierre de la ventana (como el clic en la "X") para ejecutar funciones personalizadas que utilicen `destroy()` o `quit()`.

**Ejemplo: Enlazar el evento de cierre de la ventana**

```python
import tkinter as tk
from tkinter import messagebox

def on_cerrar():
    if messagebox.askokcancel("Salir", "¿Deseas salir de la aplicación?"):
        ventana.destroy()

ventana = tk.Tk()
ventana.title("Evento de Cierre")

ventana.protocol("WM_DELETE_WINDOW", on_cerrar)

boton = tk.Button(ventana, text="Cerrar Aplicación", command=on_cerrar)
boton.pack(pady=20)

ventana.mainloop()
```

**Explicación:**

- Se utiliza `ventana.protocol("WM_DELETE_WINDOW", on_cerrar)` para enlazar el evento de cierre de la ventana con la función `on_cerrar()`.
- Al intentar cerrar la ventana (ya sea haciendo clic en el botón o en la "X"), se muestra un cuadro de diálogo de confirmación.
- Si el usuario confirma, se llama a `ventana.destroy()` para cerrar la aplicación.

---
## 4. Funciones lambda en Python y su aplicación en Tkinter

Las funciones lambda son una característica poderosa de Python que permiten definir funciones pequeñas y anónimas en una sola línea. En Tkinter, las funciones lambda son especialmente útiles para manejar callbacks y pasar argumentos a las funciones asociadas a eventos o comandos de widgets.

### Funciones lambda en Python

#### Definición y sintaxis

- **Definición:** Una función lambda es una función anónima que se define utilizando la palabra clave `lambda`. Se puede utilizar para crear funciones pequeñas y concisas sin la necesidad de definir una función completa con `def`.
- **Sintaxis:**

  ```python
  lambda argumentos: expresión
  ```

- **Características:**
  - Solo puede contener una expresión.
  - Devuelve el valor de la expresión.
  - No puede contener declaraciones ni anotaciones complejas.

**Ejemplo básico:**

```python
# Función normal
def suma(a, b):
    return a + b

# Función lambda equivalente
suma_lambda = lambda a, b: a + b

print(suma(3, 5))        # Salida: 8
print(suma_lambda(3, 5)) # Salida: 8
```

#### Ventajas de usar funciones Lambda

- **Concisión:** Permite escribir funciones pequeñas en una sola línea.
- **Flexibilidad:** Útil para operaciones rápidas y temporales.
- **Integración con funciones de orden superior:** Funciona bien con funciones como `map()`, `filter()`, y `sorted()`.

**Ejemplo con `map()`:**

```python
numeros = [1, 2, 3, 4, 5]
cuadrados = list(map(lambda x: x**2, numeros))
print(cuadrados) # Salida: [1, 4, 9, 16, 25]
```

### Aplicación de funciones lambda en Tkinter

En Tkinter, las funciones lambda son comúnmente utilizadas para:

- **Pasar Argumentos a callbacks:** Cuando un widget requiere una función de callback que acepta argumentos.
- **Simplificar el código:** Reducir la necesidad de definir múltiples funciones pequeñas para manejar eventos.

#### Pasar argumentos a callbacks

Cuando se asocia una función a un evento o comando de un widget, a menudo es necesario pasar argumentos a esa función. Sin las funciones lambda, esto podría requerir la definición de múltiples funciones. Las lambdas simplifican este proceso.

**Ejemplo: Botones que imprimen diferentes mensajes**

```python
import tkinter as tk

def mostrar_mensaje(mensaje):
    print(mensaje)

ventana = tk.Tk()
ventana.title("Funciones Lambda en Tkinter")

# Crear múltiples botones que muestran diferentes mensajes
boton1 = tk.Button(ventana, text="Saludo",
                   command=lambda: mostrar_mensaje("¡Hola!"))
boton1.pack(pady=5)

boton2 = tk.Button(ventana, text="Despedida",
                   command=lambda: mostrar_mensaje("¡Adiós!"))
boton2.pack(pady=5)

boton3 = tk.Button(ventana, text="Pregunta",
                   command=lambda: mostrar_mensaje("¿Cómo estás?"))
boton3.pack(pady=5)

ventana.mainloop()
```

**Explicación:**

- Cada botón utiliza una función lambda para pasar un mensaje diferente a la función `mostrar_mensaje()`.
- Esto evita la necesidad de definir múltiples funciones separadas para cada mensaje.

#### Asociar eventos con funciones lambda

Las lambdas también son útiles para manejar eventos donde se requieren argumentos adicionales.

**Ejemplo: Clic en botones con identificadores**

```python
import tkinter as tk

def accion_boton(id_boton):
    print(f"Se ha presionado el botón {id_boton}")

ventana = tk.Tk()
ventana.title("Eventos con Lambda")

# Crear múltiples botones con identificadores
for i in range(5):
    boton = tk.Button(ventana, text=f"Botón {i+1}",
                      command=lambda i=i: accion_boton(i+1))
    boton.pack(pady=2)

ventana.mainloop()
```

**Explicación:**

- Se crean cinco botones numerados del 1 al 5.
- Cada botón utiliza una lambda que captura el valor actual de `i` para pasar el identificador correcto a `accion_boton()`.
- La sintaxis `lambda i=i: ...` asegura que cada lambda capture el valor de `i` en cada iteración del bucle.

#### Uso de Lambdas con `bind()`

Además de los comandos de botones, las funciones lambda pueden utilizarse con métodos de binding para manejar eventos.

**Ejemplo: Capturar movimiento del ratón con coordenadas**

```python
import tkinter as tk

def mostrar_coordenadas(x, y):
    print(f"Coordenadas: ({x}, {y})")

ventana = tk.Tk()
ventana.title("Binding con Lambda")

# Asociar el evento de movimiento del ratón con una lambda
ventana.bind("<Motion>", lambda event: mostrar_coordenadas(event.x, event.y))

ventana.mainloop()
```

**Explicación:**

- Se enlaza el evento `<Motion>` (movimiento del ratón) con una función lambda que extrae las coordenadas `event.x` y `event.y` y las pasa a `mostrar_coordenadas()`.
- Esto permite manejar el evento sin definir una función separada para el callback.

#### Ventajas y desventajas de usar lambdas en Tkinter

**Ventajas:**

- **Código más limpio y conciso:** Evita la necesidad de definir múltiples funciones pequeñas.
- **Flexibilidad en callbacks:** Permite pasar argumentos fácilmente a las funciones de callback.
- **Mejora la legibilidad:** Al mantener las funciones de manejo de eventos cerca de la definición del widget.

**Desventajas:**

- **Complejidad en lambdas grandes:** Las lambdas deben mantenerse simples; funciones complejas deben definirse con `def`.
- **Depuración difícil:** Las funciones lambda pueden ser más difíciles de depurar debido a su naturaleza anónima.

**Buenas prácticas:**

- **Mantener lambdas simples:** Usar lambdas para operaciones pequeñas y definir funciones completas para lógica más compleja.
- **Evitar exceso de uso:** No abusar de las lambdas; usarlas donde aporten claridad y simplicidad.
- **Documentación adecuada:** Aunque las lambdas son concisas, asegurar que el propósito de cada lambda sea claro para otros desarrolladores.

#### Ejemplo avanzado: Uso de lambda para actualizar widgets

Supongamos que queremos actualizar múltiples etiquetas con diferentes textos al presionar diferentes botones. Usar lambdas simplifica la asignación de comandos a cada botón.

```python
import tkinter as tk

def actualizar_etiqueta(etiqueta, texto):
    etiqueta.config(text=texto)

ventana = tk.Tk()
ventana.title("Actualizar Etiquetas con Lambda")

# Crear etiquetas
etiqueta1 = tk.Label(ventana, text="Etiqueta 1")
etiqueta1.pack(pady=5)

etiqueta2 = tk.Label(ventana, text="Etiqueta 2")
etiqueta2.pack(pady=5)

etiqueta3 = tk.Label(ventana, text="Etiqueta 3")
etiqueta3.pack(pady=5)

# Crear botones para actualizar etiquetas
boton1 = tk.Button(ventana, text="Actualizar Etiqueta 1",
                   command=lambda: actualizar_etiqueta(etiqueta1, "Actualizada 1"))
boton1.pack(pady=2)

boton2 = tk.Button(ventana, text="Actualizar Etiqueta 2",
                   command=lambda: actualizar_etiqueta(etiqueta2, "Actualizada 2"))
boton2.pack(pady=2)

boton3 = tk.Button(ventana, text="Actualizar Etiqueta 3",
                   command=lambda: actualizar_etiqueta(etiqueta3, "Actualizada 3"))
boton3.pack(pady=2)

ventana.mainloop()
```

**Explicación:**

- Se crean tres etiquetas y tres botones correspondientes.
- Cada botón utiliza una lambda para pasar la etiqueta específica y el texto actualizado a la función `actualizar_etiqueta()`.
- Esto permite reutilizar la misma función para actualizar diferentes etiquetas sin necesidad de definir múltiples funciones separadas.

---

## 5. Gestión de imágenes en Tkinter

Tkinter proporciona funcionalidades básicas para manejar y mostrar imágenes en las interfaces gráficas. Sin embargo, para trabajar con formatos más avanzados y realizar operaciones de procesamiento de imágenes, es común utilizar la biblioteca Pillow (PIL Fork). Además, cargar imágenes desde fuentes externas, como GitHub, y manejar tareas que consumen tiempo, como la descarga de imágenes, puede requerir el uso de hilos para mantener la interfaz responsiva.

### Uso de Pillow

#### Instalación de Pillow

Pillow es una biblioteca de procesamiento de imágenes en Python que extiende las capacidades de Tkinter para manejar más formatos y realizar operaciones sobre las imágenes.

```bash
pip install Pillow
```

#### Cargar y mostrar una imagen con Pillow

**Ejemplo básico: mostrar una imagen en una ventana Tkinter**

```python
import tkinter as tk
from PIL import Image, ImageTk

ventana = tk.Tk()
ventana.title("Mostrar Imagen con Pillow")

# Cargar la imagen desde el sistema de archivos
imagen = Image.open('ruta/a/tu/imagen.jpg')

# Convertir la imagen para que sea compatible con Tkinter
imagen_tk = ImageTk.PhotoImage(imagen)

# Crear un widget Label para mostrar la imagen
etiqueta = tk.Label(ventana, image=imagen_tk)
etiqueta.pack()

ventana.mainloop()
```

**Explicación:**

- **Cargar la imagen:** `Image.open('ruta/a/tu/imagen.jpg')` abre la imagen utilizando Pillow.
- **Convertir para Tkinter:** `ImageTk.PhotoImage(imagen)` convierte la imagen al formato compatible con Tkinter.
- **Mostrar la imagen:** Se crea un widget `Label` que contiene la imagen y se empaqueta en la ventana.

#### Redimensionar imágenes

A menudo, es necesario ajustar el tamaño de las imágenes para que se adapten a la interfaz.

**Ejemplo: Redimensionar una imagen**

```python
import tkinter as tk
from PIL import Image, ImageTk

ventana = tk.Tk()
ventana.title("Redimensionar Imagen")

# Cargar la imagen
imagen = Image.open('ruta/a/tu/imagen.jpg')

# Redimensionar la imagen a 200x200 píxeles
imagen_redimensionada = imagen.resize((200, 200), Image.Resampling.LANCZOS)

# Convertir para Tkinter
imagen_tk = ImageTk.PhotoImage(imagen_redimensionada)

# Mostrar la imagen
etiqueta = tk.Label(ventana, image=imagen_tk)
etiqueta.pack()

ventana.mainloop()
```

**Explicación:**

- **Redimensionar:** `imagen.resize((200, 200), Image.Reasmplig.LANCZOS)` cambia el tamaño de la imagen a 200x200 píxeles con antialiasing para suavizar la imagen.
- **Mostrar:** La imagen redimensionada se muestra en un widget `Label`.

#### Rotar y girar Imágenes

Pillow permite realizar transformaciones como rotación y giro.

**Ejemplo: Rotar una imagen 90 Grados**

```python
import tkinter as tk
from PIL import Image, ImageTk

ventana = tk.Tk()
ventana.title("Rotar Imagen")

# Cargar la imagen
imagen = Image.open('ruta/a/tu/imagen.jpg')

# Rotar la imagen 90 grados
imagen_rotada = imagen.rotate(90, expand=True)

# Convertir para Tkinter
imagen_tk = ImageTk.PhotoImage(imagen_rotada)

# Mostrar la imagen
etiqueta = tk.Label(ventana, image=imagen_tk)
etiqueta.pack()

ventana.mainloop()
```

**Explicación:**

- **Rotar:** `imagen.rotate(90, expand=True)` rota la imagen 90 grados. El parámetro `expand=True` ajusta el tamaño de la imagen para evitar recortes.
- **Mostrar:** La imagen rotada se muestra en un widget `Label`.

### Carga de imágenes desde gitHub

Cargar imágenes directamente desde GitHub o cualquier URL implica descargar la imagen y luego procesarla para mostrarla en Tkinter. Para esto, se utilizan las bibliotecas `requests` para la descarga y `io.BytesIO` para manejar los datos de la imagen en memoria.

#### Descargar y mostrar una imagen desde una URL

**Ejemplo: Cargar una imagen desde GitHub**

```python
import tkinter as tk
from PIL import Image, ImageTk
import requests
from io import BytesIO

ventana = tk.Tk()
ventana.title("Cargar Imagen desde URL")

# URL de la imagen en GitHub
url = 'https://github.com/usuario/repositorio/raw/main/imagen.jpg'

# Descargar la imagen
respuesta = requests.get(url)

# Verificar si la descarga fue exitosa
if respuesta.status_code == 200:
    # Abrir la imagen desde los bytes descargados
    imagen = Image.open(BytesIO(respuesta.content))
    imagen_tk = ImageTk.PhotoImage(imagen)
    
    # Mostrar la imagen
    etiqueta = tk.Label(ventana, image=imagen_tk)
    etiqueta.pack()
else:
    etiqueta = tk.Label(ventana, text="No se pudo descargar la imagen.")
    etiqueta.pack()

ventana.mainloop()
```

**Explicación:**

- **Descargar la imagen:** Se utiliza `requests.get(url)` para descargar la imagen desde la URL proporcionada.
- **Verificar descarga:** Se comprueba si la respuesta tiene un código de estado `200` (éxito).
- **Abrir y mostrar:** Si la descarga es exitosa, se abre la imagen desde los bytes descargados utilizando `BytesIO` y se muestra en un widget `Label`.
- **Manejo de errores:** Si la descarga falla, se muestra un mensaje de error en la ventana.

#### Consideraciones de seguridad y acceso

- **URLs correctas:** Asegúrate de que la URL apunte directamente al archivo de la imagen y que sea accesible públicamente.
- **Manejo de errores:** Implementa manejo de excepciones para gestionar posibles fallos en la descarga o en la apertura de la imagen.
- **Autenticación:** Si la imagen está en un repositorio privado, se necesitarán métodos de autenticación adecuados para acceder a ella.

### Uso de hilos

Cuando se realizan operaciones que consumen tiempo, como descargar imágenes desde Internet, la interfaz gráfica puede volverse no responsiva si se ejecutan en el hilo principal. Para evitar esto, se puede utilizar la biblioteca `threading` para ejecutar estas tareas en hilos separados.

#### Descarga y muestra de una imagen en un hilo separado

**Ejemplo: Descargar y mostrar una imagen sin congelar la interfaz**

```python
import tkinter as tk
from PIL import Image, ImageTk
import requests
from io import BytesIO
import threading

def descargar_imagen(url, etiqueta):
    try:
        respuesta = requests.get(url)
        respuesta.raise_for_status()  # Lanza una excepción si la descarga falla
        imagen = Image.open(BytesIO(respuesta.content))
        imagen_tk = ImageTk.PhotoImage(imagen)
        
        # Actualizar la interfaz en el hilo principal
        etiqueta.config(image=imagen_tk)
        etiqueta.image = imagen_tk  # Mantener una referencia
    except requests.exceptions.RequestException as e:
        print(f"Error al descargar la imagen: {e}")
        etiqueta.config(text="Error al descargar la imagen.")

def iniciar_descarga():
    url = 'https://github.com/usuario/repositorio/raw/main/imagen.jpg'
    hilo = threading.Thread(target=descargar_imagen, args=(url, etiqueta))
    hilo.start()

ventana = tk.Tk()
ventana.title("Descargar Imagen con Hilos")

etiqueta = tk.Label(ventana, text="Descargando imagen...")
etiqueta.pack(pady=10)

boton_descargar = tk.Button(ventana, text="Descargar Imagen", command=iniciar_descarga)
boton_descargar.pack(pady=5)

ventana.mainloop()
```

**Explicación:**

- **Función `descargar_imagen()`:** Descarga la imagen desde la URL, la procesa y actualiza el widget `Label`. Se maneja cualquier excepción que pueda ocurrir durante la descarga.
- **Función `iniciar_descarga()`:** Crea y inicia un hilo que ejecuta `descargar_imagen()`. Esto asegura que la descarga no bloquea el hilo principal de la interfaz.
- **Actualizar la Interfaz:** Aunque Tkinter no es seguro para hilos, en este caso, `etiqueta.config()` se ejecuta desde el hilo secundario. Para mayor seguridad, es preferible utilizar métodos como `after()` para programar actualizaciones en el hilo principal.

#### Mejorando la seguridad de hilos con `after()`

Para asegurar que las actualizaciones de la interfaz se realicen en el hilo principal, se puede utilizar el método `after()` de Tkinter.

**Ejemplo mejorado: Uso de `after()` para actualizar la interfaz**

```python
import tkinter as tk
from PIL import Image, ImageTk
import requests
from io import BytesIO
import threading

def descargar_imagen(url, callback):
    try:
        respuesta = requests.get(url)
        respuesta.raise_for_status()
        imagen = Image.open(BytesIO(respuesta.content))
        imagen_tk = ImageTk.PhotoImage(imagen)
        # Programar la actualización de la interfaz en el hilo principal
        ventana.after(0, callback, imagen_tk)
    except requests.exceptions.RequestException as e:
        print(f"Error al descargar la imagen: {e}")
        ventana.after(0, callback, None)

def actualizar_etiqueta(imagen_tk):
    if imagen_tk:
        etiqueta.config(image=imagen_tk)
        etiqueta.image = imagen_tk  # Mantener una referencia
    else:
        etiqueta.config(text="Error al descargar la imagen.")

def iniciar_descarga():
    url = 'https://github.com/usuario/repositorio/raw/main/imagen.jpg'
    hilo = threading.Thread(target=descargar_imagen, args=(url, actualizar_etiqueta))
    hilo.start()

ventana = tk.Tk()
ventana.title("Descargar Imagen con Hilos y after()")

etiqueta = tk.Label(ventana, text="Descargando imagen...")
etiqueta.pack(pady=10)

boton_descargar = tk.Button(ventana, text="Descargar Imagen", command=iniciar_descarga)
boton_descargar.pack(pady=5)

ventana.mainloop()
```
---

## 5. Modelo Vista Controlador (MVC) en Tkinter

El patrón de diseño MVC separa la aplicación en tres componentes principales:

- **Modelo:** Gestiona los datos y la lógica de negocio.
- **Vista:** Se encarga de la presentación de la información.
- **Controlador:** Maneja la interacción del usuario y actualiza el modelo y la vista.

### Implementación básica de MVC en Tkinter

**Estructura de archivos:**

```
mi_app/
│
├── modelo.py
├── vista.py
├── controlador.py
└── main.py
```

**modelo.py:**

```python
class Modelo:
    def __init__(self):
        self.datos = "Hola, Mundo!"

    def obtener_datos(self):
        return self.datos

    def actualizar_datos(self, nuevo_texto):
        self.datos = nuevo_texto
```

**vista.py:**

```python
import tkinter as tk

class Vista:
    def __init__(self, root):
        self.root = root
        self.root.title("Aplicación MVC con Tkinter")
        
        self.etiqueta = tk.Label(root, text="")
        self.etiqueta.pack()
        
        self.entrada = tk.Entry(root)
        self.entrada.pack()
        
        self.boton = tk.Button(root, text="Actualizar")
        self.boton.pack()

    def actualizar_etiqueta(self, texto):
        self.etiqueta.config(text=texto)
```

**controlador.py:**

```python
class Controlador:
    def __init__(self, modelo, vista):
        self.modelo = modelo
        self.vista = vista

        # Inicializar la vista con datos del modelo
        self.vista.actualizar_etiqueta(self.modelo.obtener_datos())

        # Asociar eventos
        self.vista.boton.config(command=self.actualizar_modelo)

    def actualizar_modelo(self):
        nuevo_texto = self.vista.entrada.get()
        self.modelo.actualizar_datos(nuevo_texto)
        self.vista.actualizar_etiqueta(self.modelo.obtener_datos())
```

**main.py:**

```python
import tkinter as tk
from modelo import Modelo
from vista import Vista
from controlador import Controlador

def main():
    root = tk.Tk()
    modelo = Modelo()
    vista = Vista(root)
    controlador = Controlador(modelo, vista)
    root.mainloop()

if __name__ == "__main__":
    main()
```

### Explicación del código

1. **Modelo (`modelo.py`):** Contiene la lógica de negocio y los datos. En este ejemplo, maneja una cadena de texto.

2. **Vista (`vista.py`):** Define la interfaz gráfica. Tiene una etiqueta para mostrar texto, una entrada para que el usuario ingrese nuevo texto y un botón para actualizar.

3. **Controlador (`controlador.py`):** Interactúa entre el modelo y la vista. Inicializa la vista con datos del modelo y actualiza el modelo cuando el usuario interactúa con la interfaz.

4. **Main (`main.py`):** Inicializa los componentes y ejecuta la aplicación.

### Ventajas de usar MVC

- **Separación de responsabilidades:** Facilita el mantenimiento y la escalabilidad.
- **Reutilización de código:** Componentes independientes pueden ser reutilizados en diferentes contextos.
- **Facilita las pruebas:** Cada componente puede ser probado de manera aislada.

### Consideraciones al implementar MVC en Tkinter

- **Comunicación entre componentes:** El controlador debe gestionar la comunicación entre el modelo y la vista.
- **Actualización de la vista:** Cuando el modelo cambia, la vista debe actualizarse para reflejar los nuevos datos.
- **Eventos en la vista:** La vista no debe contener lógica de negocio; solo debe manejar la presentación y capturar eventos.

---



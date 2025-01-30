
##  **Pasos para instalar TalkBack en un emulador de Android Studio a través de Google Play Store**

### **1. Configurar un emulador con acceso a Google Play**
1. Abre **Android Studio** y accede al **AVD Manager**:
   - Ve a **Tools** → **Device Manager**.
   - Si no tienes un emulador creado con Google Play Store, debes crear uno nuevo.
2. Crea o edita un emulador compatible:
   - Pulsa **Create Virtual Device** o edita uno existente.
   - Selecciona un dispositivo con soporte para **Google Play Store** (aparece un icono de Play Store en la columna "Play Store").
   - Usa una imagen de sistema que tenga **Google APIs + Play Store** (ejemplo: **Android 12.0 Google Play**).
   - Finaliza la creación y **ejecuta el emulador**.

---

### **2. Acceder a Google Play Store**
1. Abre el emulador y espera a que cargue el sistema.
2. **Accede a la Google Play Store** desde el cajón de aplicaciones.
3. Inicia sesión con una cuenta de **Google** (si no lo has hecho antes).

---

### **3. Instalar TalkBack**
1. En la Google Play Store, busca **TalkBack**.
2. Descarga e instala la aplicación **Android Accessibility Suite** (TalkBack está incluido en esta suite).
3. Espera a que termine la instalación.

---

### **4. Activar TalkBack**
1. Ve a **Configuración** (**Settings**).
2. Accede a **Accesibilidad** (**Accessibility**).
3. Busca y selecciona **TalkBack**.
4. Accede a Settings -> Text-to-speech settings y elige el idioma que usas en tus contentDescription.
5. Actívalo usando el interruptor correspondiente.

---

### **5. Probar TalkBack**
- Si TalkBack está activado, deberías escuchar un mensaje de voz indicando que la función está en uso.
- Puedes explorar la pantalla deslizando un dedo y TalkBack te leerá los elementos en voz alta.
- Ten paciencia, hay que hacer un click en el elemento que queremos activar para seleccionarlo. Y luego doble click muy rápido con el ratón para activarlo (Botón, elemento del Recycler, configuraciones de Android...) Usa las flechas del teclado para desplazarte por menús cuyos componentes no sean visibles de un vistazo.

---

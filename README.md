# 📡 Internet Archive Uploader — Guía Termux

Sube archivos a Internet Archive desde tu Android usando rclone + Flask.

---

## 🧰 Requisitos previos

- Termux instalado (desde F-Droid, **no** desde Play Store)
- Conexión a Internet
- Una cuenta en [archive.org](https://archive.org)

---

## 📦 Paso 1 — Instalar dependencias en Termux

Abre Termux y ejecuta estos comandos **uno por uno**:

```bash
# Actualizar paquetes
pkg update && pkg upgrade -y

# Instalar Python
pkg install python -y

# Instalar pip (normalmente ya viene con Python)
python -m ensurepip --upgrade

# Instalar Flask y Werkzeug
pip install flask werkzeug

# Instalar rclone
pkg install rclone -y
```

---

## 🔑 Paso 2 — Configurar rclone con Internet Archive

```bash
rclone config
```

Sigue estos pasos dentro del asistente interactivo:

```
n) New remote
name> internetarchive          ← escribe este nombre exacto
Storage> internetarchive       ← busca la opción de Internet Archive
access_key_id>                 ← tu Access Key de archive.org
secret_access_key>             ← tu Secret Key de archive.org
endpoint>                      ← deja vacío (Enter)
front_endpoint>                ← deja vacío (Enter)
disable_checksum>              ← deja vacío (Enter)
```

### ¿Dónde obtengo las claves de Internet Archive?

1. Inicia sesión en [archive.org](https://archive.org)
2. Ve a: https://archive.org/account/s3.php
3. Copia el **Access Key** y el **Secret Key**

### Verificar que funciona:

```bash
rclone lsd internetarchive:
```

Si no muestra error, ¡está configurado! ✅

---

## 📁 Paso 3 — Copiar el proyecto a Termux

Tienes dos opciones:

### Opción A — Descomprimir el ZIP (recomendado)

```bash
# Ir a la carpeta de descargas (ajusta según tu dispositivo)
cd /sdcard/Download

# Descomprimir
unzip ia-uploader.zip -d ~/ia-uploader

# Entrar al proyecto
cd ~/ia-uploader
```

### Opción B — Crear los archivos manualmente

```bash
mkdir -p ~/ia-uploader/static
# Pega el contenido de app.py y static/index.html manualmente
```

---

## 🚀 Paso 4 — Iniciar el servidor

```bash
cd ~/ia-uploader
python app.py
```

Deberías ver:

```
==================================================
🌐 Internet Archive Uploader
==================================================
📂 Carpeta temporal: /data/data/com.termux/files/home/ia_uploads_tmp
🔗 Abre en tu navegador:
   http://localhost:5000
   http://127.0.0.1:5000
==================================================
```

---

## 🌐 Paso 5 — Abrir en el navegador

1. Abre el navegador de tu Android (Chrome, Firefox, etc.)
2. Escribe en la barra de direcciones:

```
http://localhost:5000
```

o

```
http://127.0.0.1:5000
```

¡La app debería cargar! 🎉

---

## 📤 Paso 6 — Subir un archivo

1. Toca **"Toca para seleccionar"** y elige uno o más archivos
2. En el campo de texto, escribe el **nombre del item** en Internet Archive
   - Ejemplo: `mi-coleccion-de-musica-2024`
   - Solo letras, números, guiones y puntos
3. Toca **"🚀 Subir a Internet Archive"**
4. Observa los logs en tiempo real mientras rclone trabaja

---

## 💡 Tips y solución de problemas

### rclone no se encuentra
```bash
which rclone
# Si no aparece nada:
pkg install rclone -y
```

### El servidor no inicia (puerto en uso)
```bash
# Cambiar puerto en app.py, última línea:
app.run(host="0.0.0.0", port=8080, ...)
# Luego accede en: http://localhost:8080
```

### Dar permisos de almacenamiento a Termux
```bash
termux-setup-storage
```
Esto te dará acceso a `/sdcard` para subir archivos del almacenamiento interno.

### Ver items subidos en Internet Archive
```bash
rclone ls internetarchive:nombre-del-item
```

### Mantener el servidor activo con la pantalla apagada
```bash
# Instalar termux-wake-lock
pkg install termux-api -y
termux-wake-lock
python app.py
```

---

## 📂 Estructura del proyecto

```
ia-uploader/
├── app.py              ← Backend Flask (servidor)
├── static/
│   └── index.html      ← Frontend (interfaz web)
└── README.md           ← Este archivo
```

---

## 🔒 Seguridad

- El servidor solo escucha en `localhost` (no accesible desde otros dispositivos de la red por defecto)
- Los archivos temporales se eliminan automáticamente tras la subida
- Las claves de Internet Archive las gestiona rclone de forma segura

---

## 📖 Referencias

- [rclone + Internet Archive](https://rclone.org/internetarchive/)
- [Claves S3 de Internet Archive](https://archive.org/account/s3.php)
- [Termux Wiki](https://wiki.termux.com)

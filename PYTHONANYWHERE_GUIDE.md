# Guía de Despliegue en PythonAnywhere - EasyBank

Esta guía detalla los pasos para poner tu proyecto EasyBank en línea.

## 1. Subir el código a PythonAnywhere
La forma más sencilla es usar Git. Abre una consola en PythonAnywhere y ejecuta:
```bash
git clone https://github.com/tu-usuario/EasyBank.git
cd EasyBank
```

## 2. Configurar el Entorno Virtual
En la consola de PythonAnywhere:
```bash
mkvirtualenv --python=/usr/bin/python3.10 easybank-venv
pip install -r requirements.txt
```

## 3. Crear el archivo de configuración (.env)
Copia el archivo de ejemplo y edítalo con tus datos:
```bash
cp .env.example .env
nano .env
```
**Dentro del archivo `.env`:**
- Cambia `DEBUG=False`.
- Pon una `SECRET_KEY` segura (puedes generar una en [djecrety.ir](https://djecrety.ir/)).
- En `ALLOWED_HOSTS`, pon tu dominio: `tuusuario.pythonanywhere.com`.

## 4. Configurar la pestaña "Web"
En el panel de PythonAnywhere, ve a la pestaña **Web** y haz clic en **Add a new web app**.
- Elige **Manual Configuration** y selecciona **Python 3.10** (o la versión que usaste).

### Directorios (Pestaña Web):
- **Source code**: `/home/tuusuario/EasyBank`
- **Working directory**: `/home/tuusuario/EasyBank`
- **Virtualenv**: `/home/tuusuario/.virtualenvs/easybank-venv`

## 5. Configurar el archivo WSGI
En la pestaña **Web**, busca la sección **Code** y haz clic en el enlace del archivo **WSGI configuration file**.
Borra todo y pega lo siguiente (ajusta `tuusuario`):

```python
import os
import sys

# Ruta al proyecto
path = '/home/tuusuario/EasyBank'
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'easybank.settings'

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

## 6. Archivos Estáticos (Static Files)
En la misma pestaña **Web**, busca la sección **Static files**. Agrega estas dos entradas:

1. **URL**: `/static/`
   **Path**: `/home/tuusuario/EasyBank/staticfiles`

2. **URL**: `/media/`
   **Path**: `/home/tuusuario/EasyBank/media`

**IMPORTANTE**: Después de configurar esto, ejecuta este comando en la consola para recolectar los archivos:
```bash
python manage.py collectstatic
```

## 7. Base de Datos
Finalmente, ejecuta las migraciones para crear las tablas:
```bash
python manage.py migrate
```

¡Listo! Haz clic en el botón **Reload** en la pestaña Web y tu app debería estar en línea.

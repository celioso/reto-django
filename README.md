# RETO DJANGO

1. ver la version de `python --version`

2. selecionar la version de python se usa Ctrl + shift + P  y se selecciona Python: Select Interprete y se escoje la version que se desea trabajar.

3. crear el entorno virtual: `python -m venv venv`

4. pactivar el entorno virtual en windows `.\venv\Scripts\activate` y en unix o MAcOS es `source tutorial-env/bin/activate` 

5. instalar Django es `pip install django`

6.  ver las dependencias instaladas `pip freeze` si se desea crear el requirements.txt que espara cargar la dependencias se utiliza `pip freeze > requirements.txt` y para cargarlo se utiliza `pip install -r requirements.txt`.

7. la construccion del proyecto `django-admin startproject <nombre-proyecto>`

8. crear la migracion de la base de datos se dirige a la carpeta mysite y luego se el codigo `python manage.py migrate`

9. se usa el servidor web de python `python manage.py runserver`

10. se crea la app con `python manage.py startapp blog`

11. se aplica la migración `python manage.py makemigrations blog`

12. cuando se quiere saber que sentencia va a crear django para actuar con la base de datos `python manage.py sqlmigrate blog 0001` 0001 es el numero de la migración

13. para migrarlo `python manage.py migrate` 

[Django 4 by example (4th Edition) github](https://github.com/PacktPublishing/Django-4-by-example)

[Django 4 by example (4th Edition) book](https://books.google.es/books?id=GLaEEAAAQBAJ&pg=PA171&hl=es&source=gbs_selected_pages&cad=1#v=onepage&q&f=false)

### Creacion de superusuario

crear el super usuario `python manage.py createsuperuser`
se crea el usuario: **root** o el que desee, luego pide el **correo**  y un **password**

### crear un nuevo registro ORM

crea la clase `python manage.py shell`

### enviar correo 

se debe instalar `pip install python-dotenv`

**enviar un correo de prueba**

```shell
(venv) PS C:\Users\OneDrive\Escritorio\programación\django\mysite> python manage.py shell
Python 3.12.2 (tags/v3.12.2:6abddd9, Feb  6 2024, 21:26:36) [MSC v.1937 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.       
(InteractiveConsole)
>>> from django.core.mail import send_mail
>>> send_mail('Prueba Django','Este es el body de mi mensaje','dedondeseenvia@gmail.com',['prueba1@gmail.com','prueba@hotmail.com'],fail_silently=False)
1
>>>
```

crea el modelo : `python manage.py makemigrations blog`
Aplicar la migracion: `python manage.py migrate`

## 3. Extending Your Blog Aplication

Agragamos funciones tagging al proyecto

se Instala django-taggit: `pip install django-taggit`

Agregar tags

```shell
(venv) PS C:\Users\celio\OneDrive\Escritorio\programación\django\mysite> python manage.py shell
Python 3.12.2 (tags/v3.12.2:6abddd9, Feb  6 2024, 21:26:36) [MSC v.1937 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.       
(InteractiveConsole)
>>> from blog.models import Post
>>> post = Post.objects.get(id=1)
>>> print(post)
Aprendiendo Django
>>> post.tags.add('music', 'jazz', 'django')  
>>> post.tags.all()
<QuerySet [<Tag: django>, <Tag: music>, <Tag: jazz>]>
>>>
```

eliminar tags `post.tags.remove('django')`

[Documentacion de agregacion](https://docs.djangoproject.com/en/5.1/topics/db/aggregation/)

instalar markdown `pip install markdown`
se realiza la coneccion con una base de datos se utiliza psycopg2-binary `pip install psycopg2-binary`

la exportar los dato de db.sqlite3 a postgres `python manage.py dumpdata --indent=2 --output=mysite_data.json`, pero si hay algun problema por los acentos se utiliza `python -xutf8 manage.py dumpdata --indent=2 --output=mysite_data.json`

cargamos los datos `python manage.py loaddata mysite_data.json`

### Python Decouple: separación estricta de la configuración del código

Usar un archivo .env para gestionar las variables de entorno es una práctica común y segura. Aquí te muestro cómo hacerlo en un proyecto Django.

### 1. Instalar python-decouple

Para manejar las variables de entorno desde un archivo .env, puedes usar la biblioteca python-decouple. Primero, instala python-decouple:

`pip install python-decouple`

[python-decouple 3.8](https://pypi.org/project/python-decouple/)

### 2. Crear un Archivo `.env`

Crea un archivo llamado .env en la raíz de tu proyecto (al mismo nivel que manage.py). Añade las siguientes líneas con tus configuraciones de base de datos:

```makefile
DB_NAME=nombre_de_tu_base_de_datos
DB_USER=tu_usuario_de_base_de_datos
DB_PASSWORD=tu_contraseña_de_base_de_datos
DB_HOST=localhost
DB_PORT=5432
```
### 3. Modificar `settings.py` para Usar `python-decouple`

Modifica tu archivo `settings.py` para usar `python-decouple` para cargar las variables de entorno:

```python
from decouple import config
from pathlib import Path

# Construir rutas dentro del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST'),
        'PORT': config('DB_PORT'),
    }
}
```
### 4. Verificar y Aplicar Migraciones

Después de configurar el archivo `.env` y modificar `settings.py`, verifica si puedes conectarte a la base de datos y aplicar migraciones:

```bash
python manage.py migrate
```

### Ejemplo Completo

Archivo .env:

```makefile
DB_NAME=mi_base_de_datos
DB_USER=mi_usuario
DB_PASSWORD=mi_contraseña
DB_HOST=localhost
DB_PORT=5432
```
`settings.py:`

```python
from decouple import config
from pathlib import Path

# Construir rutas dentro del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST'),
        'PORT': config('DB_PORT'),
    }
}
```
### Ejecutar migraciones:

`python manage.py migrate`

### Asegurar el Archivo `.env`

Es importante que el archivo `.env` no se suba al control de versiones (por ejemplo, a Git). Para evitarlo, añade la línea `.env` a tu archivo `.gitignore`:

bash
Copy code
```bash
# .gitignore

# Python
__pycache__/
*.py[cod]
*$py.class

# Django
*.log
*.pot
*.pyc
__pycache__/
local_settings.py

# Environ
.env
```
Con esto, tu proyecto Django debería poder conectarse a la base de datos utilizando las credenciales almacenadas en el archivo `.env`.

## buscando en múltiples campos

```shell
(venv) PS C:\Users\celio\OneDrive\Escritorio\programación\django\mysite> python manage.py shell
Python 3.12.2 (tags/v3.12.2:6abddd9, Feb  6 2024, 21:26:36) [MSC v.1937 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.       
(InteractiveConsole)
>>> from blog.models import Post
>>> Post.objects.filter(title__search='django')
<QuerySet [<Post: Aprendiendo Django>]>
>>> Post.objects.filter(body__search='django')  
<QuerySet []>
>>> Post.objects.filter(body__search='java')   
<QuerySet []>
>>> from django.contrib.postgres.search import SearchVector
>>> Post.objects.annotate(search=SearchVector('title', 'body'),).filter(search='django')
<QuerySet [<Post: Aprendiendo Django>]>
>>>
```

Zxx
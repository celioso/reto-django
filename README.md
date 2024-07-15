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

para eliminar tags `post.tags.remove('django')`

[Documentacion de agregacion](https://docs.djangoproject.com/en/5.1/topics/db/aggregation/)

instalar markdown `pip install markdown`
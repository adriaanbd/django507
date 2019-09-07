# Django 507

Este projecto nace en las vísperas de un taller Django REST API, a realizarse un 7 de septiembre de 2019 en la Universidad de Panamá, Facultad de Informática, Electrónica y Comunicación, a fin de fomentar la educación sobre:

* [Django](https://www.djangoproject.com/)
* [Django Rest Framework](https://www.django-rest-framework.org/)

Se presenta un temario de la información que se desarrolla a continuación, con el objetivo de sentar las bases necesarias para poder realizar los mini-proyectos que ponen en práctica el conocimiento adquirido.

## Instalación de Python y Django

### Windows

#### Python

1. Visitar la pagina oficial de Python ([aqui](https://www.python.org/downloads/windows/)).
2. Descargar la última versión (a la fecha 3.7.4).
3. Instalar Python y agregar el programa a `PATH`. Repito, agregar a `PATH`.
4. Verificar que Python esté instalado correctamente, abriendo Powershell e ingresar `> python` para entrar al interpretador de Python, luego ingresar `exit()` para salir.
5. Si el paso previo no ingreso a Python, intente `python.exe`. Si esto funciona, salga del interpretador con `exit`.
6. . Instalar el sistema de administrador librerias / modulos `pip`, ingresando `python -m pip install -U pip`.


#### Virtualenv
1. Instalar el administrador de ambiente virtual de Python: `pip install virtualenv`.
2. Crear un directorio (folder): `django507`
3. Desde su directorio, crear un ambiente virtual: `C:\Users\...>python virtualenv djangovenv`
4. Activar el ambiente virtual: `C:\Users\...\django507>scripts\activate`

#### Django y Django Rest Framework

1. Ver que el command prompt ahora tenga `(djangovenv) C:\Users\...\django507>`
2. Si le sale algún error de permiso, debe entrar al PowerShell como administrador y ejecutar lo siguiente: `Set-ExecutionPolicy remoteSigned`
3. Instale Django: `$ pip install django`
4. Instale Django Rest Framework (DRF) `$ pip install djangorestframework`
5. Instalar dependencias opcionales de DRF: `$ pip install markdown django-filter`

##### Fuentes:

https://virtualenv.pypa.io/en/stable/installation/
https://www.djangoproject.com/download/
https://www.django-rest-framework.org/#installation

### Mac y Linux

#### Python

Lo más probable es que ya tenga Python pre-instalado, pero revise con `$ which python` y `$ python --version`. No obstante, lo anterior, recomiendo instalar Python con `pyenv`, un sistema de administracion de versiones de Python, siguiendo las instrucciones en: https://github.com/pyenv/pyenv

Mas información sobre pyenv y su instalación, ver https://realpython.com/intro-to-pyenv/.

#### Virtualenv (python-virtualenv)

1. Seguir las instrucciones ubicadas en: https://github.com/pyenv/pyenv-virtualenv.
2. Crear un ambiente nuevo `$ pyenv virtualenv djangovenv`
3. Activar el ambiente `$ pyenv activate djangovenv` (se desactiva `$ pyenv deactivate`)

#### Django y Django Rest Framework

Con el ambiente virtual activado: `$ pip install django djangorestframework markdown django-filter`


## Breve Historia de Django

Django es un framework web genérico de código abierto que fue desarrollado entre 2003 y 2005 por un equipo de desarrolladores del periódico Lawrence Journal-World de Kansas, EEUU, a fin de automatizar la creación de aplicaciones web. Su primera versión se publicó en 2008 y la más reciente versión (2.2) se publicó en abril de 2019 ([ver aqui](https://docs.djangoproject.com/en/2.2/releases/2.2/)).



## Perspectiva de Alto Nivel

La arquitectura de Django consiste en tres (3) componentes principales, más una interfaz de administración: 

1. Django Models. Un conjunto de herramientos para trabajar con data y bases de datos.
2. Django Templates. Un sistema de plantillas.
3. Django Views. Un sistema que maneja la comunicación entre el usuario y la base de datos.
4. Django Admin. Una interfaz para la administración de un website.

Un diagrama de una aplicación web de Django:
![Fuente: MDN](https://mdn.mozillademos.org/files/13931/basic-django.png)

Un ejemplo práctico de lo anterior:

```python
# models.py
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=10)

# urls.py
urlpatterns = [
	path('', views.index),
]

# views.py
from django.shortcuts import render

def index(request):
	productos = Producto.objects.all()
    return render(request, 'index.html', {'productos': productos})
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<body>
 <ul>
 {% for producto in productos %}
 	<li>{{ producto.nombre }}</li>
 {% endfor %}
 </ul>
{% else %}
    <p>No hay productos disponibles.</p>
{% endif %}
</body>
</html>
```

Para ver un diagrama general de una petición y respuesta HTTP dinámica, ver [aquí](https://developer.mozilla.org/es/docs/Learn/Server-side/Primeros_pasos/Vision_General_Cliente_Servidor#Anatom%C3%ADa_de_una_petici%C3%B3n_din%C3%A1mica). 



## Estructura de un Proyecto Django

Django no requiere de una estructura en particular, no obstante lo anterior, si viene con una manera estandar de hacer las cosas, lo cual es necesario entender para desarrollar profesionalmente  en Django. Es importante destacar que la unidad fundamental de una aplicacion Django es el denominado *Django Project* o *Projecto Django*, que es compuesto por una (1) o más aplicaciones Django. 

Estructura jerarquica:

```
proyecto-django/
	app-1
	app-2
	django-apps
```

Una app Django es una aplicación como un Blog o Calendario de Eventos y las Django apps son un conjunto de aplicaciones proporcionadas automaticamente por Django, a fin de facilitar la creación y administración de un website. Estas aplicaciones no se aprecian en la estructura de directorios de un proyecto Django, pero se pueden observar en un archivo denominado `settings.py`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

## Creación de un proyecto

Para crear un proyecto, se utiliza una herramienta de linea de comando denominada `django-admin` ([mas info aqui](https://docs.djangoproject.com/en/2.2/ref/django-admin/)), ejecutando lo siguiente: `$ django-admin startproject django507 .` 

Esto crea una serie de carpetas / directorios:

```
django507/				<- El contenedor del proyecto
	manage.py			<- Script para administrar el proyecto
	django507/			<- Este es su proyecto (el nombre no importa)
		settings.py		<- Configuraciones del website
		urls.py			<- Las rutas de urls a vistas
		wsgi.py			<- Para comunicarse con el servidor en produccion
```

El archivo `manage.py` se utiliza para crear aplicaciones, administrar la base de datos e iniciar el servidor.

## Creación de aplicación

Para crear una aplicación, se utiliza el script de administración `manage.py`: 
`$ python manage.py startapp eventos` 

En Windows, el comando debe ser: `$ py -3 manage.py startapp eventos`

Esto crea una nueva carpeta denominada `eventos` que contiene una serie de sub-carpetas y archivos con código base para empezar a trabajar:

```
django507/
	manage.py
	django507/
	eventos/
		__init__.py	<- Indicacion que los archivos son modulos Python
		admin.py	<- Configuracion del sitio de administracion
		apps.py		<- Registro de aplicaciones
		models.py	<- Los modelos de tu base de datos
		tests.py	<- Las pruebas
		views.py	<- Las vistas
		migrations/	<- Carpeta de actualizaciones a la base de datos
```

## Registro de la aplicacion

Dentro del archivo `settings.py`, en `INSTALLED_APPS`, agregue la aplicación:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',	# no se olvide la coma
    'eventos.apps.EventosConfig'	# registrar aplicación
]	
```

## Configuración de base de datos

Dentro del archivo `settings.py`, en `DATABASES`, podrá observar que la base de datos pre-configurada es `SQLite3` cuya documentación de interfaz de Python se encuentra [aquí](https://docs.python.org/3/library/sqlite3.html).

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

[SQLite](https://sqlite.org/index.html) es una base de datos utilizada generalmente en un ambiente de desarrollo, sin embargo, carece de la funcionalidad requerida en un ambiente de producción. La base de datos ideal para aplicaciones Django es PostgreSQL, seguida por MySQL y Oracle. Para configurar su base de datos con estas plataformas, se necesitan configuraciones adicionales [ver documentación](https://docs.djangoproject.com/en/2.2/ref/settings/#databases):

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

Por lo general, esta información es configurada mediante variables de ambiente, a fin de no se expuestas al público no deseado y poder cambiar de configuraciones con facilidad. Para la administración de variables de ambiente, existen varias herramientas en el ecosistema Python, como `django-environ` [repositorio](https://github.com/joke2k/django-environ).

Al utilizar este tipo de herramientas, las variables de ambiente se configuran en un archivo denominado `.env`, y la configuración anterior podrá verse así:

```
DATABASES = {
    'default': {
        'ENGINE': env('DB_ENGINE'),
        'NAME': env("DB_NAME"),
        'USER': env("DB_USER"),
        'PASSWORD': env("DB_PASSWORD"),
    }
}
```

Aunado a lo anterior, importante destacar que Django guarda una variable denominada `SECRET_KEY` que se utiliza para proteger el website que dicho también debe ser protegida utilizando la herramienta en referencia. 

*También puede cambiar la zona de tiempo de tu aplicación, ajustando la variable `TIME_ZONE`, lo cual es importante ya que su base de datos y aplicación en general usa fecha y tiempo como referencia de un hecho*.

## Su primera vista (view)

Abra el archivo `eventos/views.py` y coloque lo siguiente:

```python
from django.shortcuts import render  	# agregado por Django startapp
from django.http import HttpResponse	# protocolo de comunicación

# función para la vista index
def index(request):
	return HttpResponse("<h1>Hola Django507!</h1>")
```

## Activar el servidor

Para activar el servidor utilicemos nuevamente `manage.py`, ejecutando el siguiente comando: `$ python manage.py runserver` (en Windows lo anterior podrá ser diferente, sustituyendo `python` por `py -3`.

Ahora abra su navegador, navegue a `http://127.0.0.1:8000` o `localhost:8000/` y podrá observar  la página de presentación de Django, toda vez que todavía no hemos configurado / mapeado la ruta a nuestra vista `index`. 

*Nota: Observará en su terminal un aviso de "migraciones sin aplicar", lo cual abordaremos luego.*

## Configución URLs

El archivo `django507/urls.py` contiene las rutas (URLs) de su proyecto:

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

En este momento, solamente esta configurada la ruta de acceso a la plataforma de administración, que por cierto no funciona todavía porque no hemos aplicado la migración inicial a nivel de proyecto. 

Para agregar nuestra ruta debemos modificar la variable anterior a fin de incluid las rutas que serán utilizadas por el app `eventos`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('eventos.urls')),
]
```

Con esta configuración, el string vacío `''` se encarga de mapear todo después del nombre de dominio, es decir `localhost:8000/` será mapeado a las rutas correspondientes al archivo `urls.py` de la app `eventos`. Por lo tanto, debemos crear un archivo denominado `urls.py` dentro de nuestra aplicación de eventos y configurar la vista que será utilizada para manejar esta ruta:

`eventos/urls.py`:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index')
]
```

Si tiene el servidor desactivado, activelo nuevamente y navege a `localhost:8000/eventos`.

Lo que sucedió fue lo siguiente:
1. El navegador le solicita al servidor de desarrollo de Django que le responda con el contenido para la ruta en referencia.
2. Django hace una busqueda en `django507/urls.py`.
3. Django revisa los patrones de `urlpatterns` en orden, empezando en este caso con `admin/`, para luego avanzar a la siguiente linea.
4. Django encuentra el patrón ruta, que incluye las rutas de `eventos/urls.py` y realiza el mismo ejercicio nuevamente.
5. Django encuentra la ruta y le envia la petición a la función de vista `index`, la cual responde con un mensaje HTML via `HttpResponse`.

## Aplique las migraciones

Cuando iniciamos el servidor de desarrollo nos percatamos que habían unas migraciones pendientes, hagamos eso ahora.

Ejecute los siguientes comandos:
* `$ python manage.py makemigrations`, para generar el archivo de actualización
* `$ python manage.py migrate`, para aplicar las actualizacion iniciales

Output generado por `migrate`:

```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
```

Leyendo el output podemos observar que hay una especie de tablas de usuarios y grupos, aunque no lo hemos hecho nosotros. 

Vamos acceder a la plataforma de administración para ver de que se trata.

## Plataforma de Administación

Para acceder a la misma, hay que crear un superusuario:

`$ python manage.py createsuperuser`
`	Username: admin`
`	Password: admin123`

Ahora naveguemos a `localhost:8000/admin` e ingresemos las credenciales.

## Usuario

Desde la plataforma de administración, tenemos la facilidad de crear usuarios e incluso ingresar ciertos campos como nombre de usuario, primer nombre, apellido, email, entre otros campos. Esto es posible por que el modelo de Usuario viene pre-instalado en el modulo `django.contrib.auth`. 

Se acuerda de:

```python
INSTALLED_APPS = [
    ...
    'django.contrib.auth',
	...
]	
```

La documentación correspondiente se encuentra [aqui](https://docs.djangoproject.com/en/2.2/ref/contrib/auth/).

## Modelos

Los modelos se colocan en el archivo `eventos/models.py` y son creados utilizando clases que heredan el modulo `db.models` de Django, lo cual le da accesso a una variedad de métodos, atributos y opciones. La documentación es vasta y puede ser consultada [aquí](https://docs.djangoproject.com/en/2.2/topics/db/models/). Para crear un modelo hay que definirlo y especificar sus campos.

Empecemos con algo simple:

```python
from django.db import models

class Evento(models.Model):
    nombre = models.CharField(max_length=30)
    ubicacion = models.CharField(max_length=60)
    fecha = models.DateField()
    hora = models.TimeField()
    descripcion = models.TextField(blank=True)
```

Importante destacar que el campo `id` que es el identificador único de un record u instancia del modelo y se crea automáticamente, toda vez que Django ejecuta lo siguiente `id = models.AutoField(primary_key=True)`; 

Como se puede apreciar, los modelos de Django se crean con tipos de campos predefinidos, que le indican a la base de datos que tipo de data va a guardar en cada columna: numero, texto, numero y texo, fecha, etc. Las distinas opciones de tipos de campos pueden ser consultadas [aqui](https://docs.djangoproject.com/en/2.2/ref/models/fields/#model-field-types).

### Migracion

`$ python manage.py makemigrations eventos`
`$ python manage.py migrate`

### Registrar modelo a Admin

Para poder adminisitrar el modelo en el panel de administracion, es necesario registrarlo en `eventos/admin.py`:

```python
from django.contrib import admin
from .models import Evento

admin.site.register(Evento)
```
### Operaciones de creacion, lectura, actualizacion y eliminacion de objetos

#### Desde Admin

Accedamos a la plataforma de administracion para realizar estas operaciones: 
http://localhost:8000/admin/

#### Desde la consola

Django viene con una consola interactiva donde podemos importar los modulos de nuestra aplicacion. Se accede a la misma con `manage.py`, utilizando el siguiente comando:
`$ python manage.py shell`

#### Creacion
```
>>> from eventos.models import Evento
>>> Evento.objects.create(nombre="Django 507", ubicacion="Universidad de Panama", fecha="2019-9-7", hora="08:00")
<Evento: Django 507>
>>> e = Evento(nombre="Django Rest Framework", ubicacion="Universidad de Panama", fecha="2019-9-7", hora="08:00")
>>> e.save()
```

Por el momento, importante destacar que la base de datos permite la creación de dos (2) objetos con el mismo nombre, toda vez que no hemos indicado lo contrario. Esto lo pudieramos configurar, estableciendo el parametro `unique=True` como argumento en alguno de nuestros campos.

#### Lectura
```
>>> eventos = Evento.objects.all()
>>> eventos
<QuerySet [<Evento: Django 507>, <Evento: Django Rest Framework]
```

Para obtener un record, por lo general se utiliza `Modelo.objects.get`, para obtener mas de un record, se utiliza `Modelo.objects.filter`.

#### Actualizacion

```
>>> evento = Evento.objects.get(nombre="Django Rest Framework")
>>> evento.hora
datetime.time(8, 0)
>>> evento.hora = "10:00"
>>> event.save()
>>> evento = Evento.objects.get(nombre="Django Rest Framework")
datetime.time(10, 0)
```

Para asegurar que el record se actualize de manera eficiente, es mejor utilizar `update(campo=nuevo_valor)`.

#### Eliminacion

```
>>> evento = Evento.objects.get(nombre="Django Rest Framework")
>>> evento.delete()
>>> Evento.objects.all()
<QuerySet [<Evento: Django 507>]
```

### Relaciones / Asociaciones 

#### Tipos de Asociaciones

Los tipos de relaciones / asociaciones que se pueden crear son:
* Many-to-One (Muchos-a-Uno), ejemplos [aqui](https://docs.djangoproject.com/en/2.2/topics/db/examples/many_to_one/).
* Many-to-Many (Muchos-a-Muchos), ejemplos [aqui](https://docs.djangoproject.com/en/2.2/topics/db/examples/many_to_many/).
* One-toOne (Uno-a-Uno), ejemplos[aqui](https://docs.djangoproject.com/en/2.2/topics/db/examples/one_to_one/).


#### Evento y Usuario

Un evento tiene un creador, un facilitador, coordinador e incluso asistentes al mismo. Lo primero es determinar que tipo de relacion tiene un modelo a otro. Si queremos agregar un creador y asociarlo a un usuario, necesitamos un modelo de Usuario. Como ya vimos en el panel de administración, Django nos proporciona un modelo de Usuario.

Como un evento tiene un creador, y un creador tiene muchos eventos, definamos la relación en nuestro modelo y apliquemos la migración correspondiente:

`eventos/models.py`
```python
from django.db import models
from django.contrib.auth.models import User

class Evento(models.Model):
    nombre = models.CharField(max_length=30)
    ubicacion = models.CharField(max_length=60)
    fecha = models.DateField()
    hora = models.TimeField()
    descripcion = models.TextField(blank=True)
    creador = models.ForeignKey(User, on_delete=models.CASCADE, related_name='events')

    def __str__(self):
        return self.nombre
```

El argumento adicional `related_name` es para poder referirnos al modelo de Evento desde la perspectiva de un usuario, es decir, queremos poder ejecutar `usuario.eventos.all()` y obtener todos los eventos creador por este usuario. De lo contrario, tenemos que obtener los mismos utilizando `usuario.evento_set.all()`, lo cual es menos intuitivo. La documentación para este tipo de busquedas se puede consultar [aqui](https://docs.djangoproject.com/en/2.2/topics/db/queries/#backwards-related-objects) y [aqui](https://docs.djangoproject.com/en/2.2/ref/models/relations/).

Apliquemos la migración:
`$ python manage.py makemigrations eventos`
`$ python manage.py migrate`

*Nota*: si tenemos algun usuario en nuestra base de datos, la migración nos advertirá sobre la existencia de otros objetos que no tienen un creador y debemos instruirle que hacer con los mismos, toda vez que este es un campo obligatorio. Idealmente, este campo lo hubiesemos creado antes de crear un evento, cuando ideamos el modelo, pero por ahora, simplemente podemos indicarle a Django que aplique un valor predefinido: `2`, que corresponde al id del segundo usuario, el primero siendo el administrador. 

## Plantillas (Templates)

Django ubica las plantillas a base de lista de carpetas contenidas en una llave denominada `'DIRS'`,  dentro de `TEMPLATES`, ubicado en el archivo de configuraciones del proyecto `django507/settings.py`:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],		# Aqui
        'APP_DIRS': True,
        'OPTIONS': {
        # ...
```

Si `APP_DIRS` es `True`, Django buscará un archivo denominado `templates` en cada una de las apps del proyecto, incluyendo cualquier ruta de archivo incluida en `DIRS`.  Por lo general, un proyecto tiene una plantilla general, a fin de que otras plantillas puedan heredarlas y extenderlas. Veremos lo que esto significa a continuación.

Agreguemos lo siguiente:`'DIRS': [os.path.join(BASE_DIR, 'django507/templates')],`

En vista de lo anterior, tenemos que agregar una carpeta denominada `templates` a la altura de `settings.py`. Agreguemos tambien una especificacion para la carpeta de archivos estaticos en `settings.py`:

```python
STATIC_URL = '/static/'	# esto le indica a Django que busque esta carpeta

STATICFILES_DIRS = [
	os.path.join(BASE_DIR, 'django507/static')
]
```

### Plantilla Base

Dentro de la carpeta `templates`, creamos un archivo denominado `base.html`, que contendrá la estructura general HTML de nuestro sitio. Para efectos prácticos, estaremos utilizando [MaterializeCSS](https://materializecss.com/).

`django507/base.html`
```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Django 507</title>
</head>
<body>
  <nav>
    <div class="nav-wrapper">
      <a href="" class="brand-logo right">Django 507</a>
      <ul id="nav-mobile" class="left">
        <li><a href="">Eventos</a></li>
        <li><a href="#">Crear Evento</a></li>
        <li><a href="#">Acceder</a></li>
        <li><a href="#">Salir</a></li>
        <li><a href="#">Registrarse</a></li>
      </ul>
    </div>
  </nav>
  <div class="content" id="main">
    <div class="row">
      <div class="col s3"></div>
      <div class="col s6">
        {% block content %}
        <h1 class="center">Django 507</h1>
        {% endblock content %}
      </div>
      <div class="col s3"></div>
    </div>
  </div>
</body>
</html>
```

Las plantillas Django consisten en HTML, etiquetas de plantilla y variables de plantilla. Las etiquetas de plantillas son identificadas como `{% %}` y se utilizan para visualizar logica `{% if %}`, control de loops `{% for evento en eventos %}`, declaraciones de bloque `{% block content %}`, importar contenido `{% include "navbar.html"`, extender plantilla `{% extends "base.html" %}`, entre otras. En cambio, las variables de plantillas se colocan entre `{{ }}` y transfieren data desde la vista a la plantilla. Estas variables pueden ser variables simples, atributos de objeto e incluso métodos.

En cuanto al archivo en referencia `base.html`, la primera linea `{% load static %}` se utiliza para hacer el enlace entre la plantilla y `STATIC_ROOT` del proyecto, que viene a ser lo mismo que `STATIC_URL`.

La declaración de bloque `{% block content %}` es una indicación del lugar en donde se extenderá la plantilla.

Las etiquetas y variables de plantilla harán más sentido cuando se ponen en práctica.

Por ejemplo, cuando las rutas estan propiamente configuradas, uno puede utilizar el valor especificado en `name`, a fin de generar una ruta de manera dinamica. Por ejemplo, si la ruta tiene el valor `name=index`, uno puede generar la ruta en la plantilla con `{% url 'index' %}`. 

### Actualice su vista 

`eventos/views.py`
```python
from django.shortcuts import render

def index(request):
    return render(request, 'base.html')
```

### Actualice el enlace de Eventos

En la plantilla `base.html`, puede actualizar el enlace:
`<li><a href="{% url 'index' %}">Eventos</a></li>`

### Extensión de la plantilla base

La app de eventos necesita una carpeta de `templates`, que contendrá las plantillas específicas a este sitio. Vamos a crear una carpeta denominada `templates` y otra carpeta denominada `eventos`, dentro de esta carpeta. La estructura será la siguiente:
```
eventos/
	templates/
		eventos/
			index.html
```

`index.html`
```html
{% extends "base.html" %}

{% block content %}
<h3 class="center">Eventos</h3>
<table class="striped">
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Ubicacion</th>
        <th>Fecha</th>
      </tr>
    </thead>
    <tbody>
      {% for evento in eventos %}
        <tr>
          <td><a href="{{ event.get_absolute_url }}">{{ evento.nombre }}</a></td>
          <td>{{ evento.nombre }}</td>
          <td>{{ evento.ubicacion }}</td>
          <td>{{ evento.fecha }}</td>
        </tr>
      {% endfor %}
    </tbody>
</table>
{% endblock content %}
```

## Vistas (Views)

Con la plantilla `index.html` ya estamos en posición para configurar nuestras vistas. 

Necesitamos pasarle a la plantilla todos los eventos de la base de datos:

```python
from django.shortcuts import render
from .models import Evento

def index(request):
	eventos = Evento.objects.all()  # aqui obtenemos todos los eventos
	return render(request, 'eventos/index.html', {'eventos': eventos})
```

Si recarga el sitio y navega nuevamente a `http://localhost:8000`, podrá observar la tabla de eventos generados uno por uno, en el modo especificado en la plantilla. Si se fija en el enlace de cada evento, observará que cada enlace le corresponde a cada item, por ejemplo `eventos/1` para el evento uno (1). 

Configuremos la ruta, vista y plantilla de detalle de un evento.

```python
# eventos/urls.py
urlpatterns = [
    # ...
    path('eventos/<int:pk>/', views.show)
]

# eventos/views.py
def show(request, pk):
	evento = Evento.objects.get(id=pk)
	return render(request, 'eventos/index.html', {'evento': evento})
```

`eventos/templates/eventos/show.html`

```html
{% extends "base.html" %}

{% block content %}
<div class="center">
  <h3>{{ evento.nombre }}</h3>
    <a href="#">Editar</a>
    <a href="#">Borrar</a>
</div>
<table class="striped">
  <tr><td class="center">Ubicación: {{ evento.ubicacion }}</td></tr>
  <tr><td class="center">Fecha: {{ evento.time_start }}</td></tr>
  <tr><td class="center">Hora: {{ evento.hora }}</td></tr>
  <tr><td class="center">Descripción: {{ evento.descripcion }}</td></tr>
  <tr><td class="center">Creador: {{ evento.creador }}</td></tr>
</table>
{% endblock content %}
```

Si recarga el sitio y navega nuevamente a `http://localhost:8000`, podrá observar la tabla de detalle de un evento.

## Formularios (Forms)

Django viene con una clase denominada `Forms` que facilita la creación de formularios, validación de data y widgets asociados (documentación [aquí](https://docs.djangoproject.com/en/2.2/topics/forms/#the-django-form-class). Esta clase es muy parecida a los modelos. 

Por ejemplo:

```
from django import forms

class MiFormulario(forms.Form):
	nombre = forms.CharField(max_length=10)
```

Generaría:

```html
<label for="nombre">Your name: </label>
<input id="nombre" type="text" name="your_name" maxlength="100" required>
```

Aunado a esto, esta clase nos da la posibilidad de generar el formulario a base de [ver](https://docs.djangoproject.com/en/2.2/topics/forms/#form-rendering-options):
* Tabla: 	{{ form.as_table }}
* Parrafo: 	{{ form.as_p }}
* Lista: 	{{ form.as_ul }}

Importante observar que la clase no genera las etiquetas de `<form>` ni el botón. 

Para crear un formulario heredando de `Form`, vamos a tener que replicar practicamente el mismo código que tenemos en nuestro modelo de Evento. Por tanto, Django nos tiene  unos formularios denominados `ModelForms`, que permiten crear un formulario a base de un modelo con poco código [ver documentación](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/#django.forms.ModelForm):

`eventos/forms.py`

```python
from django import forms
from django.forms import ModelForm
from .models import Evento

class EventForm(ModelForm):
    class Meta:
      model = Evento
      fields = '__all__'
```

Para configuración / selección de campos para usar en el formulario, ver [documentación](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/#selecting-the-fields-to-use).

### Creacion de plantilla `new.html`
`eventos/templates/eventos/new.html`

```html
{% extends "base.html" %}

{% block content %}
<h3 class="center">New Event</h3>
<form method="post" novalidate>
  {{ form.as_p }}
  <input type="submit" value="Submit">
{% csrf_token %}
</form>
{% endblock content %}
```


### Agregar vista para crear evento

```python
from django.shortcuts import render, redirect
from .models import Evento

# ...

def new(request):
    if request.method == 'POST':
        form = EventForm(request.POST)
        if form.is_valid():
        	form.save()
            return redirect('index')
    else:
        form = EventForm()
        if request.method == 'GET':
            return render(request, 'eventos/new.html', {'form': form})
```

### Probar el formulario

Navegar a http://127.0.0.1:8000/eventos/new/ y pruebe el nuevo formulario. Notará que está el campo de `creador` en el formulario. 

Que piensa que debe hacer para que no aparezca?
Que archivo deberá modificar?

## Autenticacion

Hemos creado usuarios a través de la plataforma de administración, y nos hemos autenticado como administrador a través de la misma, sin embargo, debemos poder autenticar usuario del sitio mediante un formulario. La documentación al respecto se encuentra [aquí](https://docs.djangoproject.com/en/2.2/topics/auth/default/), perp no es tan clara al respecto.

El objeto Usuario es pieza fundamental del sistema de autenticación, toda vez que el mismo se autentica proporcionando su nombre de usuario y contraseña, utilizando el método `authenticate`:

```python
from django.contrib.auth import authenticate
user = authenticate(username='john', password='secret')
if user is not None:
    # A backend authenticated the credentials
else:
    # No backend authenticated the credentials
```

Lo anterior es parte de la respuesta, pero lo cierto es que Django hace todo esto automaticamente, puesto que `django.contrib.auth.urls` esta preconfigurada a utilizar una plantilla ubicada en `registration/login.html`, a nivel de proyecto (no de app). Por lo tanto, la configuración básica consiste en crear una carpeta denominda `registration`, crear un archivo denominado `login.html`, e incluir una ruta nueva con el modulo `django.contrib.auth.urls`.

```html
{% extends "base.html" %}

{% block content %}
<h3 class="center">Login</h3>
<form method="post" action="{% url 'login'%}">
  {{ form.as_p }}
  <input type="submit" value="Login">
  <input type="hidden" name="next" value="{{ next }}" />
{% csrf_token %}
</form>
{% endblock content %}
```

No tenemos que crear / configurar una vista para esto, porque Django ya viene con una. Lo único que hay que hacer es configurar la ruta, proporcionar el archivo e indicar la ruta de suceso y fracaso.

`django507/urls.py`
```python
# ...
urlpatterns = [
	# ...
	path('accounts/', include('django.contrib.auth.urls')),
]
```

`django507/settings.py`
```
LOGIN_REDIRECT_URL = '/eventos'  # sin especificar va a accounts/profile/
LOGOUT_REDIRECT_URL = '/eventos'
```

### Actualice el enlace de Acceder en plantilla base
```html
<!-- django507/templates/base.html -->

<li><a href="{% url 'login' %}">Acceder</a></li>
<li><a href="{% url 'logout' %}">Salir</a></li>
```

### Autenticarse

Navegar a http://127.0.0.1:8000/accounts/login/ y acceder con el usuario creado a través de la plataforma de administración. Intente acceder y salir.



# Django Rest Framework



## Registar la aplicación

`django507/settings.py`
```
INSTALLED_APPS = [
	'rest_framework',
	#...
]
```



## Configurar el Serializador

Un serializador transforma la data de nuestra base de datos para que puedan ser reproducidas en formato `JSON` o algun otro. Para ello, hay que crear un archivo `serializers.py` o `serializadores.py`

```
# eventos/serializador.py

from .models import Event
from rest_framework import serializers

class EventosSerializador(serializers.Serializer):
    nombre = serializers.CharField(max_length=60)
    ubicacion = serializers.CharField(max_length=240)
    fecha = serializers.CharField(max_length=60)
    hora = serializers.TimeField()
    descripcion = serializers.CharField(max_length=120)
```



## Configurar la vista

```
# eventos/views.py

from rest_framework import serializers
from .serializadores import EventosSerializador

class EventosApiView(APIView):
    def get(self, request):
        eventos = Evento.objects.all()
        serializer = EventosSerializador(eventos, many=True)
        return Response({'eventos': serializer.data})
```



## Configurar la ruta

`django507/eventos/urls.py`
```
path('api/events/', views.EventsApiView.as_view())
```



## Obtener respuesta del API

Navegar a http://localhost:8000/api/eventos/ para obtener una respuesta JSON del servidor de desarrollo y visualizarla en el navegador.


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
	path('show/<int:producto_id>/', views.show)
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



## Vistas (Views)



- Vistas (Views)
  - Web app vs. API
  - Vistas a base de clase y funciones
  - Configuracion de URLs
- Plantillas (Templates)
  - Etiquetas y variables de plantilla
  - Busqueda de plantillas
  - Plantilla general y archivos estaticos
  - Plantilla especial y herencia de plantillas
- Formularios (Forms)
  - Los formularios de Django
  - Creación de un formulario
  - La vista de un formulario

**Django Rest Framework**

- REST
- Instalación
- Preparación del proyecto
- Serializadores (Serializers)
- Vistas (Views)
- URLs / Paths
- Autenticación (opcional)

# Proyectos

*Django*

* Tema: 
  - Web app de talleres de desarrollo de software
* Requerimentos: 
  - Modelos: 
    - Taller (tema, facilitador, descripción, sede, fecha, horario)
    - Participante (primer nombre, apellido, email)
  - Relaciones:
    - Un taller, muchos participantes
    - Un participante, muchos talleres
* Requerimentos adicionales (opcional):
  - Modelos (adicionales):
    - Facilitador (primer nombre, apellido, email)
    - Sede (nombre, dirección)
  - Relaciones (adicionales):
    - Un taller, un facilitador
    - Un facilitador, muchos talleres
    - Un taller, una sede
    - Una sede, muchos talleres

*Django REST API*

* Tema:
  - Api del web app de talleres de desarrollo de software
* Requerimentos:
  - Lectura de talleres (`GET`)
  - Creación de un taller (`POST`)
  - Actualización parcial de un taller (`PATCH`)
  - Actualización total de un taller (`PUT`)
  - Eliminación de un taller (`DELETE`)
* Requerimentos adicionales (`opcional`):
  - Autenticación para realizar `POST`, `PATCH` / `PUT`, y/o `DELETE`.
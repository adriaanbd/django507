# Django 507

Este projecto nace en las vísperas de un taller Django REST API, a realizarse un 7 de septiembre de 2019 en la Universidad de Panamá, Facultad de Informática, Electrónica y Comunicación, a fin de fomentar la educación sobre:

* [Django](https://www.djangoproject.com/)
* [Django Rest Framework](https://www.django-rest-framework.org/)

Se presenta un temario de la información que se desarrolla a continuación, con el objetivo de sentar las bases necesarias para poder realizar los mini-proyectos que ponen en práctica el conocimiento adquirido.

## Temario

**Python**

* Instalación

**Django**

* Breve historia
* Perspectiva de alto nivel
  - MVC / MTV
  - Modelos, Plantillas y Vistas
  - Estructura de un proyecto
  - URLConf
* Instalación
* Modelos (Models)
  - Creación
  - Migración
  - Consola
  - Crear, Leer, Actualizar y Borrar
  - Relaciones
* Vistas (Views)
  - Web app vs. API
  - Vistas a base de clase y funciones
  - Configuracion de URLs
* Django Admin
  - Acceso
  - Registro de modelos
  - Modificaciones al panel de administración
  - Administración de modelos (lectura, creación, actualización, eliminación)
* Plantillas (Templates)
  - Etiquetas y variables de plantilla
  - Busqueda de plantillas
  - Plantilla general y archivos estaticos
  - Plantilla especial y herencia de plantillas
* Formularios (Forms)
  - Los formularios de Django
  - Creación de un formulario
  - La vista de un formulario

**Django Rest Framework**

* REST
* Instalación
* Preparación del proyecto
* Serializadores (Serializers)
* Vistas (Views)
* URLs / Paths
* Autenticación (opcional)

**Proyectos**

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
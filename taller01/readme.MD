# Taller 01 #
## Infraestructura como código ##

## Objetivos: ##

- Comprender las motivaciones de la automatización de infraestructura.
- Realizar de forma autónoma el aprovisionamiento automático de infraestructura.
- Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables.

# Escenario 01 (Sesión en clase)

Con el fin de probar una aplicación web, se requiere montar un servidor con CentOS 8.1, donde se ejecute el servidor web NGINX.

## Pre Requisitos
- [virtualBox](https://www.virtualbox.org/)

## Actividades.
1. descargue la imagen [ISO de CentOS](https://wiki.centos.org/Download)
2. cree una máquina virtual en virtualbox e Instale CentOS en ella. ([Instalación estándar de centOS](https://docs.centos.org/en-US/8-docs/standard-install/))
3. Acceda a la máquina CentOS e [instale NGINX](https://www.cyberciti.biz/faq/how-to-install-and-use-nginx-on-centos-8/)
4. configure en VirtualBox una [red solo anfitrión](https://condor.depaul.edu/glancast/443class/docs/vbox_host-only_setup.html) que le permita acceder a Nginx desde la máquina local.

## Entregable
un documento en formato [MarkDown](https://markdown.es/sintaxis-markdown/) (se suguiere el nombre Escenario01.MD) que contenga:
1. Nombres completos y códigos de los estudiantes.
2. Documentación y evidencias de la realización de cada una de las actividades propuestas.
3. Dificultades encontradas en el proceso.
4. Escriba en máximo 3 párrafos por qué esta forma de crear ambiente de pruebas puede ser apropiado (o inapropiado) para desplegar réplicas de este ambiente.

# Escenario 02 (sesión en clase)

Con el fin de probar una aplicación web, se requieren montar dos servidores con CentOS 8.1: en al primero se ejecutará el servidor web NGINX y en el segundo un servidor de PostgreSQL

## Pre Requisitos
- [virtualBox](https://www.virtualbox.org/)
- [vagrant](http://vagrantup.com)
- [Saltstack](https://www.saltstack.com/)


## Actividades.
1. Con acompañamiento del profesor elabore la creación y provisionamiento de la máquina virtual para la base de datos.
2. Realice la conexión via SSH a la máquina virtual (con acompañamiento del profesor)
3. Destruya y reconstruya la máquina virtual de la BD (con acompañamiento del profesor). 
4. A partir del conocimiento anterior cree y provisione la máquina virtual para el servidor web, recuerde que el servidor web sea acessible desde la máquina local.


## Entregable
un documento en formato MarkDown (se suguiere el nombre Escenario02.MD) que contenga:
1. Nombres completos y códigos de los estudiantes.
2. Documentación y evidencias de la realización de cada una de las actividades propuestas.
3. Dificultades encontradas en el proceso.
4. Escriba en máximo 3 párrafos por qué esta forma de crear ambiente de pruebas puede ser apropiado (o inapropiado) para desplegar réplicas de este ambiente.

# Escenario 03. (Para hacer en casa y entregar en 15 días)

En este escenario se hará uso de lo aprendido anteriormente para implementar una aplicación web que haga uso de una base de datos, el sistema debe permitir insertar y listar datos de una tabla en la base de datos. La aplicación debe ser desplegada en un servidor web y la base de datos en un servidor aparte.

## Pre Requisitos
- [virtualBox](https://www.virtualbox.org/)
- [vagrant](http://vagrantup.com)
- [Saltstack](https://www.saltstack.com/)

## Entregable
un documento en formato MarkDown (se suguiere el nombre Escenario03.MD) que contenga:
1. Nombres completos y códigos de los estudiantes.
2. Documentación y evidencias del proceso de creación, provisionamiento y despliegue de la aplicación.
3. Dificultades encontradas en el proceso.
4. Conclusiones acerca de los hallazgos y aprendizajes obtenidos en el desarrollo del laboratorio.
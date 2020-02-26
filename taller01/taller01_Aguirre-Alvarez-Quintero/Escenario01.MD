# Taller 01

### Car Lewis Álvarez Cossio - A00310516
### Andrés Felipe Aguirre Aguilar - A00329792
### Juan Sebastián Quintero Yoshioka - A00310448

## Infraestructura como código
## Objetivos:
- Comprender las motivaciones de la automatización de infraestructura.
- Realizar de forma autónoma el aprovisionamiento automático de infraestructura.
- Diagnosticar y ejecutar de forma autónoma las acciones necesarias para lograr infraestructuras estables.

## Escenario 01 (Sesión en clase)

Con el fin de probar una aplicación web, se requiere montar un servidor con CentOS 8.1, donde se ejecute el servidor web Nginx.

## Pre Requisitos

- VirtualBox

## Actividades

 1. Descargue la imagen ISO de CentOS.
 
 El proyecto CentOS es un esfuerzo de software libre impulsado por la comunidad centrado en entregar un ecosistema robusto de código abierto alrededor de una plataforma Linux. Es una plataforma consistente y manejable que se adapta a una amplia variedad de implementaciones. Para algunas comunidades de código abierto, es una base sólida y predecible sobre la cual construir. [1]
 
 En este taller, se descarga la ISO versión 7 de CentOS Linux, disponible en: https://wiki.centos.org/Download
 
 ![ISO CentOS](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/ISOCentOS.png)

 2. Cree una máquina virtual en VirtualBox e Instale CentOS en ella. (Instalación estándar de centOS)
 
 VirtualBox es un potente producto de virtualización x86 y AMD64/Intel64 para uso empresarial y doméstico. [2]
 
 Una vez descargado (disponible en: https://www.virtualbox.org/) e iniciado VirtualBox, se da click en "Nueva", en lo que se debe colocar un nombre descriptivo de la nueva máquina virtual, tamaño de disco duro y tamaño en memoria:
 
 ![Crear máquina](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/CrearMaquina.png)
 
 ![Disco duro](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/DiscoDuro.png)
 
 ![Tamaño en memoria](/taller01/taller01_Aguirre-Alvarez-Quintero_/imagenes/TamahnioMemoria.png)
 
 La configuración es la siguiente:
 
 ![Resumen configuración](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/ResumenConfiguracion.png)
 
 Al final del proceso de instalación, se debe especificar un usuario y contraseña:
 
 ![User and password](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/InstalacionCentOS7.png)
 
 3. Acceda a la máquina CentOS e instale Nginx.
 
 Una vez accedemos a la máquina CentOS, procedemos a la instalación de Nginx abriendo la consola y ejecutando los siguientes comandos: [3]
 
  a. Agregamos el repositorio CentEL 7 EPEL: 
  
  <code> sudo yum install epel-release </code>
  
  b. Instalamos Nginx:
  
  <code> sudo yum install nginx </code>
  
  c. Ejecutamos Nginx:
  
  <code> sudo systemctl start nginx </code>
  
  d. Si se está ejecutando un firewall, se debe ejecutar los siguientes comandos para permitir el tráfico HTTP y HTTPS:
  
  <code> sudo firewall-cmd --permanent --zone=public --add-service=http </code> <br>
  <code> sudo firewall-cmd --permanent --zone=public --add-service=https </code> <br>
  <code>sudo firewall-cmd --reload </code>
 
  e. Para finalizar, podemos verificar que Nginx está ejecutándose:
  
  <code> sudo systemctl status nginx </code>
  
  ![Verificación Nginx](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/VerificarNginx.png)
  
 4. Configure en VirtualBox una red solo anfitrión que le permita acceder a Nginx desde la máquina local.
 
 Para configurar VirtualBox en red solo anfitrión, se debe realizar lo siguiente: [4]
 
  a. Seleccionar el adaptador de red en "Adapatador solo anfitrión":
  
  ![Configuración red servidor](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/ConfiguracionRedervidor.png)
  
  b. A continuación, se debe crear o editar el adaptador solo anfitrión, para ellos, se debe ir a VritualBox -> Preferencias -> Red
  
  ![VirtualBox preferencias](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/VirtualBoxPreferencias.png)
  
  c. Se debe agregar una red solo anfitrión:
  
  ![Configurar red anfritión](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/ConfigurarRedAnfitrion.png)
  
  d. Por último, verificamos que funciona, esto, accediendo desde el navegador de nuestra máquina a la dirección IP donde se encuentra el servidor Nginx:
  
  ![Verificar funcionamiento](/taller01/taller01_Aguirre-Alvarez-Quintero/imagenes/VerificarFuncionamiento.png)

### Dificultades encontradas en el proceso

- La principal dificultad se presentó en la instalación de CentOS7, primero, no teniamos la ISO de instalación, por lo que se tuvo que esperar a que esta se descargara. Luego, el proceso de configuración es lento, lo que significó un mayor tiempo de espera. Aproximadamente, entre la descarga y configuración nos demoró una hora.
- No sabiamos configurar VirtualBox en red solo anfitrión, por lo que al principio no podiamos acceder al servidor Nginx desde la máquina local. 

### ¿Es apropiado o no para desplegar réplicas en un ambiente de pruebas?

Esta forma de crear un ambiente de pruebas no nos parece apropiada, puesto que es un poco tediosa, ya que, si en un caso necesitamos instalar servicios en diferentes máquinas virtuales, tendríamos que descargar la ISO de dicha máquina e instalar paso a paso cuántas máquinas virtuales necesitemos. Además, requería que todo el tiempo estuvieramos pendientes para poder definir los parámetros de configuración y seguir el proceso de instalación.

Por lo tanto, para realizar nuestra reflexión, consideramos que debemos de crear o usar sistemas que nos permita crear instrucciones y procesos para disminuir la interración que tenemos en este tipo de tareas, es decir, infraestructura como código. La infraestructura como código nos permite gestionar las máquinas virtuales, eliminando esa necesidad de configuración manual, permitiendo implementar una máquina o muchas con el mismo código. Esto se traduce, por ejemplo, en ahorrarnos el tiempo que le tuvimos que dedicar a la instalación y configuración de CentOS7 con Nginx, y, en un futuro, si se desea desplegar una réplica, no tener que perder más tiempo.

### Bibliografía

[1] CentOS Project. Retrieved 2 February 2020, from https://www.centos.org/ <br>
[2] Oracle VM VirtualBox. Retrieved 2 February 2020, from https://www.virtualbox.org/ <br>
[3] Digital Ocean. Retrieved 23 February 2020, from https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7 <br>
[4] Condor.depaul.edu. Retrieved 23 February 2020, from https://condor.depaul.edu/glancast/443class/docs/vbox_host-only_setup.html <br>

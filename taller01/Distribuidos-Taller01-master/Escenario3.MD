# Distribuidos

### Manuel Alejandro Coral Lozano - A00301480
### Jose Camilo Moctezuma Ruiz - A0024104
### Cristian Alexander Duque Salazar - A00301448

## Escenario 03

Para la realización de este escenario, se nos pedia que crearamos dos máquinas virtuales (Una que haria las veces de un servidor web y la otra que haria las veces de una base de datos). Es importante recalcar que quedaba a nuestra decision que servidor web podiamos usar y que motor de base de datos utilizar.
![Imagen 1](/taller01/Distribuidos-Taller01-master/images/20200205_194850.jpg)  


### Actividades

#### 1. Para la realización de este escenario se procedio primero a realizar la aplicación en local, esto con el fin de poder conocer:
- Cuales sistemas operativos usariamos en las máquinas virtuales
- Que servidor web usar
- Que motor de base de datos utilizar

Al analizar todas las opciones que y al realizar una investigación sobre cada tecnologia que podiamos usar se decidieron tomar las siguientes decisiones:  
- Se usarian dos sistemas operativos (CentOS 7 y Ubuntu 18.04) este principalmente con el objetivo de amplir nuestros conocimientos sobre la creacion y futuro aprovisonamiento en diferentes sistemas operativos.
- Se usaria Node.js como servidor web porque en nuestro grupo ya había integrantes que han trabajo con él.
- Se usaria Postgresql como motor de base de datos porque en nuestro grupo ya había integrantes que han trabajo con él.

#### 2. Una vez tomadas estas decisiones se procedio a crear el Vagrantfile 

A continuación se muestran las capturas del Vagrantfile que se uso.
![Imagen 2](/taller01/Distribuidos-Taller01-master/images/vagrantfile-escenario3.png) 

En esta captura se puede evidenciar:
- Como primero se crea la máquina virtual que hará las veces de Backend y que tendra el sistema operativo CentOS 7.
- Como se crea la máquina virtual que hará las veces de Fronend y que tendra el sistema operativo Ubuntu 18.04.
- Las configuraciones básicas para cada máquina (Su dirección IP, su hostname, su port fordwarding)
- Además se puede apreciar el fragmento de codigo ansible para realizar el aprovisionamiento.

#### 3. Se realizó el archivo de **hosts** y un playbook llamado **servers.yml** para realizar el aprovisionamiento.

El archivo hosts utilizado fue el siguiente:  
![Imagen 3](/taller01/Distribuidos-Taller01-master/images/hosts-escenario3.png) 

**NOTA:** Es importante recalcar que dentro del playbook **servers.yml** se tienen los dos aprovisionamientos para cada máquina virtual, haciendo uso de los grupos que se crearon el archivo de **hosts**

Es por eso que para mostrar el archivo **servers.yml** lo haremos en dos apartados, el de la base de datos (backend) y el del servidor web (frontend)

- Aprovisionamiento para la máquina virtual Backend
![Imagen 4](/taller01/Distribuidos-Taller01-master/images/serverBack-Escenario3-captura1.png) 
 En esta primera captura se puede apreciar como a la máquina virtual que hará las veces del backend (la que tiene CentOS 7) se aprovisiona con:
  + Una libreria llamada python-psycopg2 (que permitira luego la creación de base de datos, usuarios y tablas desde este archivo ansible)
  + Se descarga e instalaga postgresql 11
  + Se inicia el servicio de postgresql 11
  + Se modifican los archivos postgresql.conf y pg_hba.conf con el fin de que el servidor de la base de datos este escuchando **solicitudes** por el puerto en el que trabaja y desde cualquier dirección de la red 192.168.0.0/16 
  + Por último se inicia el servicio de postgresql 11  
 
  ![Imagen 5](/taller01/Distribuidos-Taller01-master/images/serverBack-Escenario3-captura2.png)  
  En esta segundo captura (que sigue siendo la continuación del aprovisionamiento para la máquina virtual Backend) podemos apreciar como:
  + Se crea la base de datos llamada escenario23
  + Se crea una tabla dentro de esa base de datos que fue creada previamente
  + Se crea un usuario  
  
  ![Imagen 6](/taller01/Distribuidos-Taller01-master/images/serverBack-Escenario3-captura3.png)  
  En esta ultima captura se puede ver como:
  + Se le otorga al usuario que se creo, todos los permisos para la base de datos. 
  + Se le otorga al usuario que se creo, todos los permisos sobre la secuencia de los identificador del usuario  

- Aprovisionamiento para la máquina virtual Frontend 
![Imagen 7](/taller01/Distribuidos-Taller01-master/images/serverfront-captura1.png)  
En esta captura podemos ver el aprovisionamiento que se realiza para la máquina vitual que hará las veces del frontend.
  + Primero, se intala nodejs
  + Se hace uso del comando **forever** para ejecutar la aplicación que se encuentra en la ruta /vagrant/src/index.js
 
#### 4. Ahora consideramos importante mostrar el codigo fuente de la aplicación 

Para mostrar el codigo fuente consideramos imporantes 3 archivos:  
- index.js
- index.js (que se encuentra en la carpeta /routes)
- controller.js (que es quien se encarga de la conexión con la base de datos y de satisfacer las necesidades, es decir, guardar infomarción en la base de datos y extrarla cuando se solicita)  

- En la siguiente captura se puede apreciar el archivo **index.js**  
![Imagen 8](/taller01/Distribuidos-Taller01-master/images/index1.png)  
De esta captura se puede evidenciar que la aplicación estará utilizando un archivo que se encuentra bajo la ruta **"/routes/index.js"** y que estara escuchando por el puerto 3000

- En la siguiente captura se puede ver el archivo **/routes/index.js**  
![Imagen 9](/taller01/Distribuidos-Taller01-master/images/index2.png)  
De esta captura se puede evidenciar que la aplicación estará utilizando un archivo (donde se encuentra toda la logica para resolver los requerimientos) que se encuentra bajo la ruta **"/controllers/controller.js"**. Además de que se encuentran los métodos Get y Post que serviran para traer o guardar información.  
- En la siguiente captura se puede ver el archivo **controller.js**
![Imagen 10](/taller01/Distribuidos-Taller01-master/images/controller.png)  
En esta captura se puede ver la conexión con la base de datos, que ocurre por el puerto 5432, y los métodos que se encargan de seleccionar o insertar elementos a la base de datos.  

#### 5. Se realiza el vagrant up y se muestra el correcto funcionamiento de la aplicación.

- Se utiliza el comando vagrant up para empezar la creación y aprovisionamiento de las máquinas virtuales  
![Imagen 11](/taller01/Distribuidos-Taller01-master/images/vagrantup-escenario3.png)  

- Creación y aprovisionamiento correcto de la máquina virtual ServerBack  
![Imagen 12](/taller01/Distribuidos-Taller01-master/images/funcionamientoServerBack.png)  

- Creación y aprovisionamiento correcto de la máquina virtual ServerFront  
![Imagen 13](/taller01/Distribuidos-Taller01-master/images/funcionamientoServerFront.png)  

- Funcionamiento de la aplicación
![Imagen 14](/taller01/Distribuidos-Taller01-master/images/funcionamientoApp.png)  

#### Dificultades  
Para este escenario se encontro con la dificultad de "alcanzar" la base de datos cuando se encontraba en otra máquina, es decir, cuando no estaba en localhost. Al investigar, nos dimos cuenta que postgresql tiene dos archivos de configuración que permiten que la base de datos sea alcanzada por un puerto en especifico y desde una dirección de red especifica, al modificar estos archivos se logro acceder a la base de datos y realizar toda la actividad sin ningun problema.

#### Conclusiones  
Entrar en contacto con las tecnologias de nodejs y de postgresql fue muy enriquecedora debido a que la mayoria del grupo no habia tenido tal contacto y se aprovecho bastante para entender como funciona un APIRouter, un API REST y como realizar la conexión a una base de datos.
En cuanto a las herramientas como vagrant y ansible, se logro una profundización (que había empezado desde el escenario 2) que permitió un mejor entendimiento sobre la utilidad y la importancia que tienen estas herramientas en el mercado actual.

# Distribuidos

### Manuel Alejandro Coral Lozano - A00301480
### Jose Camilo Moctezuma Ruiz - A00024104
### Cristian Alexander Duque Salazar - A00301448

## Escenario 02  

### Actividades

##### 1. Con acompañamiento del profesor elabore la creación y provisionamiento de la máquina virtual para la base de datos.  

+ Para la creación de la máquina virtual para la BD se hizo uso del siguiente archivo Vagrantfile  
![Imagen 1](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/creacion-vm-DB-vagrantfile.PNG)  
+ Para el aprovisionamiento de la máquina virtual de la BD se hizo uso de lo siguiente: 
  * Un fragmento de codigo en el Vagrantfile:  
  ![Imagen 2](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/fragmento-ansible-vagrantfile.PNG)
  * Se realizo lo siguiente en el archivo/directorio **hosts**:  
  ![Imagen 3](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/hosts.PNG)
  * Se hizo uso del siguiente archivo ***"db.yml"*** que se encuentra en el playbook:  
  ![Imagen 4](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/ansible-yml-instalarDB.PNG)  
  * Cuando se realizo el provisionamiento, se puede observar que fue exitoso.  
  ![Imagen 5](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/instalacion-db-completa.PNG)  
    
##### 2. Realice la conexión via SSH a la máquina virtual
 
+ Para lograr la conexión SSH se utilizó el comando ***"vagrant ssh"***:  
![Imagen 6](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/prueba-ssh-db-instalada.PNG)

##### 3. Destruya y reconstruya la máquina virtual de la BD.

+ Para destruir la máquina virtual se utilizó el comando ***"vagrant destroy"***:  
![Imagen 7](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/destroy-vm-db.PNG)
+ Para reconstruir la máquina virtual se utilizó el comando ***"vagrant up"***:
![Imagen 8](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/up-vm-db.PNG)

##### 4. A partir del conocimiento anterior cree y provisione la máquina virtual para el servidor web, recuerde que el servidor web sea acessible desde la máquina local.

+ Para la creación de la máquina virtual del servidor web se hizo uso del siguiente archivo Vagrantfile  
![Imagen 9](/images/vagrantfile-vm-ws.PNG)
+ Para el aprovisionamiento de la máquina virtual del servidor web se hizo uso de lo siguiente: 
  * Un fragmento de codigo Ansible en el Vagrantfile:  
  ![Imagen 10](/images/ansible-fragmento-vm-ws.PNG)
  * Se realizo lo siguiente en el archivo/directorio **hosts**:  
  ![Imagen 11](https://raw.githubusercontent.com/ManuelCoral1998/Distribuidos-Taller01/master/images/hosts-ws.PNG)
  * Se hizo uso del siguiente archivo ***"ws.yml"*** que se encuentra en el playbook:
  ![Imagen 12](/images/ansible-ws.PNG)
  * Se ejecuto el comando ***"vagrant up"*** y se obtuvo el siguiente resultado:  
  ![Funcionamiento Apache](/images/prueba-ws.PNG)
#### (ACTIVIDAD EXTRA) Con el fin de estudiar y practicar, el grupo decidio realizar una activiad extra, en la cual se pudieran:
**1. Crear 2 máquinas virtuales, una para un servidor web y la otra para una base de datos.**
**2. Aprovisionar el servidor web con Apache**
**3. Aprovisionar el servidor de base de datos con Postgresql-11**

+ Para ello se elaboró el siguiente archivo Vagrantfile:  
![Imagen 13](/images/actividad-extra.PNG	)  
![Imagen 13](/images/actividad-extra-ansible.PNG)  

+ El archivo de ***hosts*** se creo de la siguiente manera:  
![Imagen 14](/images/hosts-actividad-extra.PNG)  

+ Se creo un playbook llamado ***"servers-ws-db.yml"*** el cual contenia la siguiente información:  
![Imagen 15](/images/yml-actividad-extra.PNG)

+ Resultado
![status](/images/status.PNG)

#### Dificultades  
En este caso la dificultad fue principalmente el hecho de que por culpa de una linea "ansible_limit = all", se estaba intentado aprovisionar una máquina que aun no habia sido creada, es decir, se creaba la máquina A, se aprovisionaba la máquina A pero en lugar de crear la máquina B, intentaba aprovisionar la máquina B que no ha sido creada y por tal motivo, ocurrian errores y no se lograba la completitud del proceso.

#### Conclusiones  
Esta forma de crear ambientes de pruebas y desplegar réplicas resulta muy apropiada debido a que no se pierde mucho tiempo realizando la configuración de cada máquina virtual, lo que usualmente quita mucho tiempo de lo que realmente importa, es decir, se pierde tiempo realizando configuración que despues no se volveran a tocar en lugar de estar ocupando ese tiempo probando el servicio o trabajando directamente sobre la aplicación.
Tanto la creación, despliegue e inclusto algo del monitoreo resulta ser muy rápido y beneficioso para el negocio, ya que permite avanzar (en caso de exitos) o retroceder (en caso de errores) de una manera muy facil e intuitiva.
El uso de las herramientas como vagrant y ansible demostró ser una habilidad muy importante si se mira desde una perspectiva "DevOps" ya que son herramientas que nos permite automatizar los procesos que, de manera tradicional, ocupan mucho tiempo y esfuerzo.

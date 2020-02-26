# Escenario 2
## Estudiantes
Jorge Moreno - Andres Varela - Leonardo Zambrano

Para este escenario se identifican 3 etapas principales:

En la primera de este escenario, se realiza la creación de una única máquina utilizando Vagrant en la carpeta 00-Single-Machine. Posteriormente, por medio de Vagrant se genera un nuevo ambiente con multiples servidores en 01-Multiple-Machine. Por último, se genera un ambiente con multiples máquinas con vagrant, aprovisionados como servidores web y de base de datos por medio de Ansible.

### Procedimiento
# 00-Single-machine
1. Para definir la configuración de Vagrant, se establece el archivo VagrantFile, incluyendo en esta el sistema operativo centos 7, su hostname, y una dirección ip de su red.

![Vagrant File](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-VagrantFile.PNG)

Se verifica el estado de la máquina de vagrant, que se encuentran por ahora inactivas.

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-vagrantstatusapagada.PNG)

2. Para encender la máquina, se utiliza el comando vagrant up, ubicados desde la carpeta en la cual se encuentra el archivo VagrantFile

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-vagrantup.PNG)

Una vez se ha ejecutado este procedimiento correctamente, se puede verifica que el estaod en que se encuentra la máquina ahora es "en ejecución".

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-vagrantstatusprendida.PNG)

3. Ahora, es posible acceder a la máquina mediante ssh desde el terminal de la máquina real en visualstudiocode, creando previamente un par de llaves de autenticación mediante el comando ssh-keygen:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-vagrantssh.PNG)

4. Finalmente, la máquina puede ser destruida para dejar de utilizarla y no dejar rastro de ella, o recursos de vagrant utilizandose sin necesidad mediante el comando vagrant destroy.

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/00-vagrantdestroy.PNG)

# 01-Multiple-machines

Este es el archivo vagrantfile, mediante el cual se crean las máquinas con sistema operativo Centos7, asignando su dirección ip, memoria RAM, y CPUs.

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantfile.PNG)

Se verifica que las maquinas se encuentran creadas más no activas.

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantstatusapagadas.PNG)

 Ahora pueden encenderse ambas maquinas, y puede observarse todo el proceso que realiza vagrant para esto:
 
![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantupa.PNG)
![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantupb.PNG)

Las máquinas se encuentran ahora ejecutandose:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantstatusprendidas.PNG)

Tambien puede accederse a ambas mediante ssh:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantssh.PNG)

AL igual que en el caso anterior, se destruyen las maquinas en ejecución:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/01-vagrantdestroy.PNG)

# 02-Ansible-Provision

Para esta ultima parte, se plantea un escenario que parte de los dos anteriores, pero en este caso se requiere que las máquinas creadas sean: Un servidor web y un servidor database, para esto se debe utilizar aprovisionamiento sobre los mismas, y en este caso se realiza mediante Ansible.

Esta configuración en VagrantFile, tambien con sistema operativo centos7, incluye además asignación de diferentes puertos ssh, y lo más importante, el aprovisionamiento con ansible haciendo llamado a archivos adicionales: hosts,y un playbook llamado servers.yml:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/02-vagrantfilepartea.PNG)
![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/02-vagrantfileparteb.PNG)

Este es el archivo hosts, donde se definen las direcciones ip que Ansible debe identificar para cada servidor:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/02-hosts.PNG)

Este es el script especificado en el VagrantFile para aprovisionar con ansible:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/02-playbookservers.PNG)

Las maquinas son creadas con vagran up y se verifica que se estan ejecutando normalmente:

![Vagrant Status](https://github.com/dvlopez9811/SD20201-Taller01/blob/master/Escenario02/images/02-vagrantstatusprendidas.PNG)

## Dificultades encontradas en el proceso

A lo largo del desarrollo de este taller se presentaron algunos problemas, principalmente por el hecho de ser la primera vez que se utilizan estas herramientas para La creación y aprovisionamiento de máquinas virtuales. Aunque se partió de un repositorio que ya contenía gran parte de la implementación, el aprovisionamiento con ansible no funcionó correctamente y se debió proceder a entender cómo funcionaba la ejecución del archivo yml que ejecuta esta tarea para corregir algunas partes del código. La primera fue definir las máquinas en el archivo hosts como ansible_host, y la segunda eliminar una acción que hacia que se intentara hacer el aprovisionamiento en paralelo a la creación de las máquinas, pues ansible estaba intentando aprovisionar la segunda máquina antes de ser creada.

## Conclusiones y reflexión

El uso de herramientas que permiten automatizar los procesos de creación de infraestuctura mediante ejecución de código resulta muy útil hoy en día, pues se realiza un despliegue mucho más rápido, además de que permite su condiguración y administración y utilizar solo los recursos de hardware necesarios para implementar una solución. En este taller se pudo tener un acercamiento a esta tecnología, y empezar a comprender la forma en que se utilizan estas herramientas para cumplir estas funciones dentro del desarrollo y despliegue de una solución de TI.

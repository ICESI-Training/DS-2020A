
### Equipo de trabajo:
* Jorge Andres Moreno - A00328088
* Andres Varela - A00049016
* Leonardo Cifuentes - A00027552

---

### Escenario 03

En este escenario se hará uso de lo aprendido anteriormente para implementar una aplicación web que haga uso de una base de datos, el sistema debe permitir insertar y listar datos de una tabla en la base de datos. La aplicación debe ser desplegada en un servidor web y la base de datos en un servidor aparte.

### Pre Requisitos
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](http://vagrantup.com)
- [Saltstack](https://www.saltstack.com/)

### Entregable

un documento en formato MarkDown (se suguiere el nombre Escenario03.MD) que contenga:

1. Nombres completos y códigos de los estudiantes.
2. Documentación y evidencias del proceso de creación, provisionamiento y despliegue de la aplicación.
3. Dificultades encontradas en el proceso.
4. Conclusiones acerca de los hallazgos y aprendizajes obtenidos en el desarrollo del laboratorio.

---

### Instalación

#### **1. Vagrant.**
Ingresmos al siguiente link:

https://www.vagrantup.com/docs/installation/

En la esquina superior derecha, seleccionamos la opción Download y después seleccionamos el instalador o paquete apropiado para nuestro sistema operativo (en este caso es Lubuntu 19.04).

Para validar la correcta instalación utilizamos el comando dentro de la consola de la maquina que instalamos Vagrant:

~~~
vagrant -v
~~~

#### **2. VirtualBox.**

Ingresamos al siguiente link:

https://vitux.com/how-to-install-virtualbox-on-ubuntu/

Agregamos un repositorio fuente y actualizamos el sistema.
~~~
sudo add-apt-repository multiverse && sudo apt-get update
~~~

Ahora instalamos VirtualBox con el comando.
~~~
sudo apt install virtualbox
~~~

Para abrirlo directamente desde la terminal se escribe:
~~~
virtualbox
~~~

#### **3. Ansible**

Ingresamos al siguiente link:

https://github.com/Jorge-Andres-Moreno/Ansible_examples

y ejecutamos los siguientes comandos para la instalación en lubuntu:

~~~
sudo apt install software-properties-common -y
sudo apt-add-repository ppa:ansible/ansible -y
sudo apt install ansible -y
~~~

Por ultimo validamos si quedo instalado:

~~~
ansible -v
~~~

---

### Base de datos - MySQL

Utilizamos el motor de base de datos de MySQL. Para la instalación creamos el playbook para aprovisionar con ansible y luego integrarlo con el proyecto en Vagrant.

Primero configuramos el nombre del host que lo nombramos **database** .Además, habilitamos el usuario root y metodo sudo para la instalación y no tener problemas de permisos.

~~~
  # web server installation
- hosts: database
  become: yes
  become_user: root
  become_method: sudo
  gather_facts: no
  ~~~

En las tareas de ejecución tenemos primero la instalación de dependencias y librerias necesarias para utilizar MySQL. Además, se verifican si algunas si el servicio de MySQL esta corriendo, para verificar que las librerias y el servicio ha sido instalado correctamente.

~~~
 - name: "updating server"
    apt:
      update_cache: yes
 
  - name: "Installing apt dependencies"
    apt:
      name: "{{ item }}"
      update_cache: yes
    with_items:
      - libmysqlclient-dev
      - mysql-client
      - python-dev
      - python-pip
      - python-mysqldb
      - python3-pip

  - name: Make sure pymysql is present
    become: true # needed if the other tasks are not played as root
    pip:
      name: pymysql
      state: present

  - name: "Installing  mysql server"
    apt:
      name: mysql-server

  - name: Ensure mysql is running and starts on boot
    service:
      name: mysql
      state: started
      enabled: yes
    become: yes
~~~

Ahora configuramos MySQL para que pueda ser visible ante otros usuarios. Ya que, de momento solo esta corriendo en localhost y necesitaremos más adelante conectar el **Web server**.

**NOTA** : la linea **bind-address** por defecto esta en **127.0.0.1** en el archivo de configuración de MySQL. Esto  con lleva a que sólo permita que desde la maquina **host** acceder al servicio de base de datos. Remplazando esta linea en el archivo de configuración por **0.0.0.0** pudimos se habilita el servicio para todos los usuarios y no solo para el **localhost**.

~~~
- name: Configure mysql to listen to external interface
    lineinfile:
      dest: /etc/mysql/mysql.conf.d/mysqld.cnf
      regexp: '^bind-address'
      line: bind-address = 0.0.0.0
~~~

Seguiremos con la creación de una base de datos llamada **mydb** que podra ser accedida desde cualquier parte. Y creamos las credenciales para un **root** con la contrasela **password**. Este usuario podra acceder a la base de datos.

**NOTA**: Fue importante en este paso hallar el comando adecuado para que este usuario pudiera acceder desde cualquier lugar.

~~~
 - name: Create mysql database
    mysql_db:
      login_unix_socket: /var/run/mysqld/mysqld.sock
      name: mydb
      state: present 

  - name: "Creating mysql user"
    mysql_user:
      name: root
      password : password
      priv: '*.*:ALL'
      host: '%'
      state: present
      login_unix_socket: /var/run/mysqld/mysqld.sock
    notify: restart mysql
~~~

Por ultimo se reinicia el servicio para que la configuración sean efectivos en el servicio.

**NOTA**: Para esto se implemento un handle por buenas practicas en Ansible pero este servicio tambien puede ser incluido perfectamente en las **task**.
~~~
handlers:
    - name: restart mysql
      service:
        name: mysql
        state: restarted
~~~

___


### Web server - NodeJS

Utilizamos la herramienta de NodeJS que nos permitio desarrollar en el mismo framework el Back-end y Front-End de la aplicación.

Codigo fuente del servidor web se encuentra en el siguiente repositorio:

[Servidor web](https://github.com/Jorge-Andres-Moreno/Web_server.git)

Para la instalación del servidor web utilizamos la herramienta de **Ansible** para aprovisionar y luego anexarlo más adelante al proyecto en **Vagrant**.

Comenzamos en el playbook, escribiendo el nombre del host que ser **webserver** 

~~~
- hosts: webserver
  gather_facts: yes
  become: yes
~~~

Luego configuraremos la version que utilizaremos de Node, para este caso utilizaremos la version 8x que es la ultima.

~~~
vars:
    NODEJS_VERSION: "8"
~~~

**NOTA**: Lo siguiente se encuentra dentro de **task** del playbook.

Ahora seguiremos a instalar NodeJS, descargando su llave e instalando su repositorio que lleva los archivos y dependencias de instalación.

~~~
  - name: Install the gpg key for nodejs LTS
    apt_key:
      url: "https://deb.nodesource.com/gpgkey/nodesource.gpg.key"
      state: present

  - name: Install the nodejs LTS repos
    apt_repository:
      repo: "deb https://deb.nodesource.com/node_{{ NODEJS_VERSION }}.x {{ ansible_distribution_release }} main"
      state: present
      update_cache: yes

  - name: Install the nodejs
    apt:
      name: nodejs
      state: present
~~~

Ahora lo que haremos es instalar la herramienta **Git** para posteriormente descargar el repositorio del servidor web.

~~~
  - name: Install git
    apt:
      name: git
      state: present

  - name: Git Clone Repo
    git: 
      repo: https://github.com/Jorge-Andres-Moreno/Web_server.git
      dest: '~/app'
      update: yes
      force: yes
      accept_hostkey: no
~~~

Ahora lo que tenemos que hacer es instalar las dependencias que trae el proyecto de NodeJS, para este caso instalaremos las librerias de **express**, **body-paser** y **mysql**.

~~~
 - name: Install modules node
    npm:
      name: "{{item}}"
      path: ~/app
    with_items:
      - express
      - body-parser
      - mysql
~~~

Por ultimo, instalamos **PM2**. Esta herramienta nos permite correr en segundo plano un **Demon**, en ubuntu para que el servidor web corra cada vez que se inicie el host.

~~~

  - name: Install PM2
    npm:
      name: pm2
      global: yes
  
  - name: To Start a App
    become: yes
    become_method: sudo
    become_user: root
    command: sudo pm2 start ~/app/index.js
~~~
___

### Vagrant + Ansible

Ya en este punto con los **playbooks** listos podemos **virtualizar** dos maquinas las cuales aprovisionaremos más adelante con los **playbooks** que hemos realizado anteriormen.

Empezaremos definiendo en un archivo llamado **hosts** las direcciónes IPs que llevaran las maquinas virtuales que crearemos.

En primer lugar definiremos la IP y el usuario de la maquina donde se encuentra la base de datos. El nombre de la maquina utilizara es **ServerB** y tendra la **IP 192.168.56.3**.

**NOTA**: Notese que el mismo nombre que llevara es igual a el que habiamos definido en el playbook,**database** , al igual que pasa con el nombre del servidor web, **webserver**. Esto se debe en gran medida ya que cuando Vagrant lee la información de este archivo define que hosts del playbook le corresponde aprovisionarlo.

En segundo lugar definiremos la IP y el usuario de la maquina donde se encuentra en servidor web. El nombre de la maquina que utilizara es **ServerA** y tendra la **IP 192.168.56.2**.

A continuación el contenido del archivo **hosts** .

~~~
server_provision

[database]
ServerB ansible_host=192.168.56.3 

[webserver]
ServerA ansible_host=192.168.56.2 
~~~

Luego con el comando se creara un archivo llamado Vagrantfile que editaremos y el entorno que necesita un proyecto en vagrant:
~~~
vagrant init
~~~

En el archivo de configuración **ansible.cfg** pondremos por defecto que los host que utilizara Vagrant seran los que estan en el archivo **hosts**.

La configuración se hace de la siguiente manera:

~~~
[defaults]
hostfile = hosts
host_key_checking = False
~~~

Ahora en el archivo **Vagrantfile** proceder a definir las cualidades de la maquina virtual, con los nombres que les hemos asignado, tales como: CPU, Memoria RAM, puerto ssh y dirección IP.
~~~
servers = {
  "ServerB" => { :ip => "192.168.56.3", :cpus => 1, :mem => 512, :ssh_port=>2200 },
  "ServerA" => { :ip => "192.168.56.2", :cpus => 1, :mem => 512, :ssh_port=>2222 }
}
~~~

Seguirmos con la configuración **ssh** de las maquinas virtuales. Decidimos utilizar la **box** de vagrant **ubuntu/bionic64** ya que es la distro de linux **Ubuntu 18.04**, la cual es compatible con los playbooks desarrollados.

~~~

Vagrant.configure("2") do |config|
  servers.each_with_index do |(hostname, info), index|
     config.vm.define hostname do |cfg|
       cfg.vm.provider :virtualbox do |vb, override| 
         config.vm.box = "ubuntu/bionic64"
         override.vm.network "private_network", ip: "#{info[:ip]}"
         #Fix SSH forwarded port
         override.vm.network "forwarded_port", guest: 22, host:"#{info[:ssh_port]}", id: "ssh", auto_correct: true
         override.vm.hostname = hostname
         vb.name = hostname
         override.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/public_key"
         override.vm.provision "shell", inline: "cat /home/vagrant/.ssh/public_key >> /home/vagrant/.ssh/authorized_keys"
         override.vm.provision "shell", inline: "rm /home/vagrant/.ssh/public_key"
         vb.name = hostname
       end
     end
  end
~~~

por ultimo aprovisionamos a las maquinas virtuales con el archivo server.yml en el cual se encuetra los aprovisionamientos correspondientes.

**NOTA**: Es importante saber la gerarquia de las carpetas y como se ha estructurado el archivo server.yml ya que en el se encuentran los dos aprovisionamientos que funcionan independientes y tienen el nombre de **roles** en **Ansible**.

~~~
#provision for servers
    config.vm.provision "ansible" do |ansible|
            ansible.inventory_path = 'hosts'
            ansible.verbose = 'vvv'
            ansible.playbook = 'playbooks/servers.yml'
           # ansible.limit = "all"
    end
end
~~~
___
### Resultados

En la siguiente imagen se puede apreciar la pagina web principal que responde el servidor.

**NOTA**: Esta petición es **/** mediante una petición GET de http. Esta petición responde con el archivo **principal.html** en el servidor.

![Web server - HTML](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-29-59.png)

A continuación, registraremos las credenciales de tres personas. Para lo cual llameremos el metodo POST  en el servidor llamado **/add**.

Ingresamos el usuario **jorge** con el id **1**

![POST - Jorge](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-30-17.png)

Ingresamos el usuario **Varela** con el id **2**

![POST - Varela](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-30-30.png)

Ingresamos el usuario **Leonardo** con el id **3**

![POST - Leonardo](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-30-46.png)

Para comprobar que el servidor se haya conectado exitosamente a la base de datos, y haya guardado exitosamente los usuarios ingresados. Utilizaremos una petición **POST** llamada **/list** para pedir la lista de usuarios registrados.

![Customers](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-31-12.png)

##### Comprobación de los servicios

Por ultimo ingresaremos manualmente por **SSH** a las maquinas virtuales para comprobar el funcionamiento de los servicios de los servidores.

Verificamos antes que los las maquinas virtuales se encuentren activas.

![servers running](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-32-52.png)

Ingresamos por **SSH** con el usuario vagrant al servidor web que lo habiamos llamado **ServerA**.

![SSH - ServerA](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-33-19.png)

Ahora proccediremos a ver el **status** de **PM2** que nos dira di el servicio está activado.

![PM2 status](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-33-48.png)

Ahora, ingresamos por **SSH** con el usuario vagrant a la base datos que lo habiamos llamado **ServerB**.

![SSH - ServerB](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-35-37.png)


Ahora ingresaremos a la base de datos con el usuario **root** y la contraseña **password**.

![MySQL - SignIn](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-36-18.png)

Ahora ingresamos a la base de datos, y luego ahi imprimiremos en consola la tabla **customers** que es en la cual se guardan todos los usuarios que registramos anteriormente.

![MySQL - Database](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-38-01.png) 

![MySQL - print all](https://raw.githubusercontent.com/Jorge-Andres-Moreno/DS-2020A/master/3-Escenario/Screenshots/Captura%20de%20pantalla%20de%202020-02-19%2016-38-00.png) 

___
### Conclusiones


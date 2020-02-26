# Instalando CentOS

### Desargue la imagen ISO de CentOS: [ISO DE CentOS](https://wiki.centos.org/Download)  
Después de descargar Centos en la carpeta se verá un archivo iso así:  
![**Imagen de la ISO de CentOS**:](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/pantalazo%20de%20la%20iso%20de%20Centos.PNG).

Luego abrimos virtual box y le damos click en **Nueva**, escribimos el nombre que queremos darle a la nueva máquina virtual y seleccionamos el sistema operativo (Windows, Linux, Otros) y luego si es de 32 o 64 bits como aparece en la pantalla. Para este taller seleccionamos: **Linux** y **Other/Unknown (64-bit)]** ![Creando máquina nueva en VBx:](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/creando%20m%C3%A1quina%20nueva%20en%20VBX.PNG) Después escogemos la cantidad de memoria RAM nosotros seleccionamos **2048MB**, click en *Next* y seleccionamos **crear disco duro virtual ahora**. 

![Seleccionando memoria RAM](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/seleccionando%20la%20cantidad%20de%20memoria%20RAM.png) 

![**Creando diso duro virtual**](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/crear%20disco%20duro%20virtual.PNG).  

Luego seleccionamos reservado dinámicamente ![reservado dinámicamente](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/reservado%20din%C3%A1micamente.PNG) Seleccionamos una cantidad de disco duro mayor a 8,20GB porque la máquina CENTOS pesa poco más de 7GB y los archivos básicos con los que virtualbox instala Centos pesan más de 1 GB para un aproximado de 8,12GB. ![Tamaño de disco](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/seleccionando%20cantidad%20de%20disco%20duro.PNG) Entonces con un total de 9GB será suficiente. Ahora se crea la máquina pero no se ha instalado Centos, esta máquina, aparece apagada, le damos click en iniciar (flecha verde) ![Botón Iniciar](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/bot%C3%B3n%20verde%20de%20inicio.PNG) la máquina inicia y procedemos a seleccionar la (imagen) ISO de Centos le damos abrir: ![Selccionando CentOS](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/seleccionando%20la%20imagen%20de%20Centos.PNG). 

Le damos iniciar y seleccionamos Instalar CENTOS 8. A partir de acá se selecciona las características que queremos tener, Idioma, se debe crear el usuario root con su respectiva contraseña. ![Seleccionamos Idioma](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/seleccionamos%20idioma.PNG). También nos pedirá crear un usuario con su respectiva contraseña. Es posible que termine la instalación y una vez se reinicie vuelva a iniciar la instalación, para evitar esto y que el Sistema CentOS arranque correctamente se debe eliminar el disco de la unidad virtual, en la parte superior de virtual box, damos click en la pestaña ***Máquina***, luego en ***configuración***, después en ***Almacenamiento*** y finalmente como aparece en la imagen. ![**Así**](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/finalizar%20la%20instalaci%C3%B3n.PNG). 
### Acceda a la máquina CentOS e instale NGINX: ###
Primero actualizamos el sistema con el comando ***sudo yum update*** ![yum update](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/Instalando%20Nginx.PNG)

luego debemos instalar **NGINX** con el siguiente comando ***sudo yum install Nginx*** ![install nginx](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/NGINX.PNG), ![Instalado y listo nginx](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/NGINX%202.PNG)


Ahora procedemos a ejecutar los siguientes comandos que aparecen en las imágenes: ![systemctl enable nginx](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/systemctl%20nginx%20service.PNG)

luego ![systemctl start nginx, systemctl status nginx](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/start%20and%20status.PNG)

Ahora procedemos a habilitar (abrimos) los puertos 80 y 443 usando firewall-cmd:
![comandos de firewall](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/firewall%20cmd%20service%20http%20completo%201.PNG).
![para mirar los puertos que escucha nginx](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/escuchando%20por%20los%20puertos.PNG).  
El estado de nginx también lo podemos verificar vía web usando la ip correspondiente: ![nginx vía web](https://github.com/leonardoZambranoCifuentes/Images-Distribuidos/blob/master/good%20nginx.PNG).

## Dificultades ##
Una de las cosas más difíciles fue la instalación de la imagen de centOS debido a que generó problemas al finalizar la instalación, el sistema se reiniciaba una y otra vez y fue necesario buscar en internet como solucionar ese problema. Además la demora en instalar el sistema operativo y la cantidad de pasos y configuraciones por comandos. También aprender el formato de Markdown representa una pequeña dificultad, pero en especial los espacios y la distancia entre parráfos fue una dificultad entenderlos en Markdown.

## ¿Es apropiado? ##
No es del todo apropiado, es bastante complejo querer hacer réplicas de este manera porque si falla la instalación toca volver a iniciar desde el principio todo el proceso. Además, el proceso también puede fallar si la imagen del S.O se corrompe y tocaría descargar nuevamente la imagen del S.O.

Andrés Varela López, Jorge Andrés Moreno Y Leonardo Zambrano Cifuentes.

Primer punto: se selecciona la imagen del SO a instalar
 <img src="/taller01/Distribuidos-Taller01-master/images/img1.jpg">

Luego se selecciona la opción de instalar centos8
<img src="/taller01/Distribuidos-Taller01-master/images/img2.jpg">
 
Luego mestra una interfaz grafica que nos permite configurar nuestra maquina virtual.
<img src="/taller01/Distribuidos-Taller01-master/images/img3.jpg">
 
 De manera predeterminada lo que falta es asignar el disco a lo cual lo seleccionamos mediante la interfaz grafica. 
 <img src="/taller01/Distribuidos-Taller01-master/images/img4.jpg">
 
Seguido a este paso procedemos a la instalación
<img src="/taller01/Distribuidos-Taller01-master/images/img5.jpg">
 
En la siguiente pantalla se puede configurar parámetros de usuarios como la contraseña del super usuario y usuarios
<img src="/taller01/Distribuidos-Taller01-master/images/img6.jpg">
 
Finalmente se reinicia la maquina virtual
<img src="/taller01/Distribuidos-Taller01-master/images/img7.jpg">
 
En caso de no iniciar con la imagen de centos al momento de inicar la maquina presional F12 y seleccionar la opción Hard Disk 
 <img src="/taller01/Distribuidos-Taller01-master/images/img8.jpg">
 
Y aceptar los términos y condiciones
<img src="/taller01/Distribuidos-Taller01-master/images/img9.jpg">
 
Ya en centos se procede a actualizar e instalar Nginx
<img src="/taller01/Distribuidos-Taller01-master/images/img10.jpg">
 
Y levantar su servicios
<img src="/taller01/Distribuidos-Taller01-master/images/img11.jpg">

Luego para acceder a los servicios se deben cambiar unos parámetros en la aplicación de vrtual box en la configuración de red conectarlo a “solo anfrition”
 <img src="/taller01/Distribuidos-Taller01-master/images/img12.jpg">

Para finalmente acceder a la pagina mediante otro equipo
 <img src="/taller01/Distribuidos-Taller01-master/images/img13.jpg">

#### Conclusiones
Como se pudo observar el método de montar un servicio es bastante tedioso debido a que se tiene que configurar toda la máquina virtual además que en alguno de estos pasos pueden surgir errores por tanto no es la manera mas optima para montar un servicio distribuido. Lo anterior debido a que, de ser asi, se gastarían recursos innecesarios de tiempo y dinero montando servidores de esta manera además de estas virtualizaciones pueden ser objetivo para otros fines por fuera de los creados por tanto son mas inseguros.

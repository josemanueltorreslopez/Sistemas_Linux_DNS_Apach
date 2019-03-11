# Sistemas Linux DNS Apach
Lo primero seria instalar el Bind9 con el comando -> apt-get install bind9. 
<br/>
Una vez instalado debemos introducir las zonas de las paginas webs(Lo mas recomendable es una a una y comprobar que funciona cada una).
![imagen de las zonas](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/zonas.JPG)
<br/>
**La zona inversa no funciona.** <br/>

Ahora debemos cambian el archivo named.conf.options y cambiar el forwarders a los siguientes<br/>
### Forwarders
![imagen de los forwarders](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/forwarders.JPG)
<br/>

Ahora debemos crear los archivos rd para cada una de las paginas que queremos implementar. Lo que haremos sera copiar el archivo db.local con un nuevo nombre para que no tengamos ningun problema.

Esta es el rd de gato.com <br/>
### rd.gato.com
![imagen del rd de gato.com](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/gato.JPG)
<br/>

Esta es el rd de mosquito.com 
### rd.mosquito.com
![imagen del rd de mosquito.com](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/mosquito.JPG)
<br/>

Esta es el rd de escherichiacoli.es
###rd.escherichiacoli.es
![imagen del rd de escherichiacoli.es](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/escherichiacoli.JPG)
<br/>

Esta es el rd de chip555.org
###rd.chip555.org
![imagen del rd de chip555.org](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/chip555.JPG)
<br/>

Y ahora seria crear la zona inversa de cada una pero, aactualmente no sabemos crear una zona inversa para 4 paginas que apunta al mismo servidor 
<br/>

Ahora debemos comprobar una a una de que no hay ningun problema en la zona y en los archivos rd con los comandos: 
<br/>
named-checkconf named-checkzone gato.com rd.gato.com 
named-checkzone mosquito.com rd.mosquito.com 
named-checkzone escherichiacoli.es rd.escherichiacoli.es 
named-checkzone chip555.org rd.chip555.org
<br/>

Ahora cambiamos el archivo resolv.conf para que apunte a nuestro servidor
###Resolv.conf
![imagen del archivo resolv.conf](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/resolv.JPG)
<br/>

Una vez modificado el archivo debemos reiniciar el servicio o demonio del bind9 con: etc/init.d/bind9 restart
<br/>

Y a continuacion hacemos una comprobacion con el comando host y la pagina a la que intentemos acceder.
###Host a todas las Paginas
![imagen del comando hosts](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/comprobacion.JPG)

<br/>

Bien ahora vamos a instalar el servicio de apache2 para poder hacer una gestion a nuestras paginas webs. Lo primero instalarlo con: <br/>
apt-get install apache2
<br/>

Ahora deberemos crear una estructura donde guardaremos los datos del sitio. Con el comando: 
<br/>
mkdir -p /var/www/gato.com/html
mkdir -p /var/www/mosquito.com/html
mkdir -p /var/www/escherichiacoli.es/html
mkdir -p /var/www/chip555.org/html
<br/>

Les damos permisos a los usuarios para que puedan modificarlo con el comando: 
<br/>
chmod -R 777 /var/www
<br/>
**Cuidado esto hace que la carpeta tenga todos los permisos**
<br/>

Ahora pondremos algo en nuestras paginas web para poder comprobar que todo va bien, como por ejemplo: 
<br/>
echo "Esta es la pagina de gato" > /var/www/gato.com/html/index.html
echo "Esta es la pagina de mosquito" > /var/www/mosquito.com/html/index.html
echo "Esta es la pagina de escherichiacoli" > /var/www/escherichiacoli.es/html/index.html
echo "Esta es la pagina de chip555" > /var/www/chip555.org/html/index.html
<br/>

Ahora vamos a crear un archivo virtual host para cada uno de nuestros dominios copiando el que trae por defecto apache2: 
<br/>
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/gato.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mosquito.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/escherichiacoli.es.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/chip555.org.conf
<br/>
Y configuramos sus archivos con las siguientes lineas de codigo.
###Configuracion de los Ficheros .conf
<br/>
![imagen del archivo gato.com.conf](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/gatoconf.JPG)
![imagen del archivo mosquito.com.conf](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/mosquitoconf.JPG)
![imagen del archivo escherichiacoli.es.conf](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/escherichiacoliconf.JPG)
![imagen del archivo chip555.org.conf](https://github.com/josemanueltorreslopez/Sistemas_Linux_DNS_Apach/blob/master/chip555conf.JPG)
<br/>
**Las dos ultimas capturas tienen un directory creado para las contraseñas, ahora mismo se deben de obviar.**
<br/>

Ahora debemos habilitarlas con un comando de apache2, en cada una de las paginas debemos deshabilitar el arhivo 000-default.conf:
<br/>
sudo a2ensite gato.com.conf
sudo a2dissite 000-default.conf
sudo a2ensite gato.com.conf

sudo a2ensite mosquito.com.conf
sudo a2dissite 000-default.confsudo 
sudo a2ensite mosquito.com.conf

sudo a2ensite escherichiacoli.es.conf
a2dissite 000-default.confsudo
sudo a2ensite escherichiacoli.es.conf

sudo a2ensite chip555.org.conf
a2dissite 000-default.conf
sudo a2ensite chip555.org.conf

<br/>

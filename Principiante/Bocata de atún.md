ESCANEO Y ENUMENRACIÓN.
-----------------------------------------------------


**Objetivo** -> Descubrir servicios abiertos y vulnerabilidades. 


![](Pasted%20image%2020250717175246.png)
Esta sería nuestra ip. 

Ahora buscaremos con arp-scan cuál es la ip de la víctima. 

```
arp-scan -I eth0 --localnet
```

![](Pasted%20image%2020250717175650.png)

La ip Víctima es 
```
10.0.2.5
```

A continuación haremos un escaneo de puertos.: 

Esta vez he usado este comando: 

```
nmap -p- -n -Pn -vvv 10.0.2.5
```

-p-  Para que aplique a todos los puertos.
-n   Para que no haga resolución DNS. 
-Pn Para que no haga ping. 
-vvv Para que cuando encuentre puerto nos lo muestre por pantalla. 

![](Pasted%20image%2020250717180305.png)

Ahí tenemos los diferentes puertos. 

Como tenemos el puerto 80, vamos a ver que hay ahí. 

![](Pasted%20image%2020250717180911.png)


Nos metemos en el directorio /IMAGES y vemos algo que no sale en las imágenes: 

![](Pasted%20image%2020250717181437.png)


Nos bajamos la imagen. 

![](Pasted%20image%2020250717181550.png)

Nos podemos meter en cualquier convertidor de hexadecimal a texto y metemos el código en hexadecimal que viene en ese .jpg 

y saldría "Borrar despues". 

Es curioso que salga eso, vamos a usar ahora steghide para ver los metadatos de la imagen. 

```
steghide info horse_426f7272617220646573707565730a.jpg
```

![](Pasted%20image%2020250717182355.png)

Nos pide una clave, así que vamos hacer fuerza bruta. 

```
stegseek --crack horse_426f7272617220646573707565730a.jpg /usr/share/wordlists/rockyou.txt 
```

![](Pasted%20image%2020250717182626.png)

Nos da la clave. 

Así que haremos lo de antes pero metiendo la clave. 


```
steghide info horse_426f7272617220646573707565730a.jpg
```

![](Pasted%20image%2020250717182940.png)


![](Pasted%20image%2020250717182919.png)

Vamos a este directorio: 

![](Pasted%20image%2020250717183054.png)

Y pondremos el nombre del agente Melissa. 

![](Pasted%20image%2020250717183208.png)

Tenemos acceso al historial de la conversación del agente. 

Ahora vamos a hacer con wfuzz para ver si hay más usuarios. 

```
wfuzz -u "http://10.0.2.5/secr3t_message_app.php/p?name=FUZZ" -w /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt --hw 104
```

![](Pasted%20image%2020250717184121.png)

Nos metemos en "ross"

![](Pasted%20image%2020250717184200.png)

Nos dice que Tienes toda la información en la ruta "m1si0n.php". Así que vamos a ello. 

![](Pasted%20image%2020250717184357.png)

Nos pide el código secreto. 

No lo tenemos. 
Pero si seguimos leyendo recuerda al compañero la primera vez que se vieron y le deja una foto. 

![](Pasted%20image%2020250717184818.png)


Nos descargamos la imagen 

![](Pasted%20image%2020250717185010.png)

Y con la herramienta exiftool la bicheamos. 


![](Pasted%20image%2020250717185102.png)

Ahí tendríamos el flag. 

Ahora vamos a hacer google dork. 

![](Pasted%20image%2020250717185617.png)


Que es el coment de la foto anterior. 

Nos metemos en cada una y parece que el lugar es Almería. Probamos esa contraseña. 

![](Pasted%20image%2020250717190153.png)

Y ahí tenemos el flag de validación. 
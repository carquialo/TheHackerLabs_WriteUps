![](Doculabs/Principiante/images/Pasted%20image%2020251218112342.png)


# Reconomiento 
------------------

Para averiguar cuál es la ip víctima usaremos arp-scan.

```
arp-scan -I eth0 --localnet
```

La ip víctima es : 

```
10.0.2.17
```

A continuación, haremos un escaneo de puertos: 

```
nmap -sVC -p- -n --min-rate 5000 10.0.2.17
```


![](images/Pasted%20image%2020251218113357.png)


Tenemos los puertos 21, 22 y 80.
# Exploración
---------------

Nos metemos en la web, ponniendo la ip en el buscador. 

![](images/Pasted%20image%2020251218113529.png)

Buscamos en subdirectorios. 


```
gobuster dir -u http://10.0.2.17/ -w /usr/share/wordlists/rockyou.txt -x txt,py,sh,php
```

![](images/Pasted%20image%2020251218113658.png)

Y encontramos un secret.txt

![](images/Pasted%20image%2020251218113821.png)

Como teníamos el puerto ftp podemos entrar con el usuario anonymous. 

![](images/Pasted%20image%2020251218114019.png)

# Explotación. 
---------------
Como podemos descargar los archivos y son los mismo de la páguina web, lo que podemos hacer es subir una revershell en php. 

Lo subimos con put la reveshell .

![](images/Pasted%20image%2020251218121329.png)


Ahora nos vamos a la escucha con netcat. 

Nos sale un BrainFuck 

![](images/Pasted%20image%2020251218122223.png)

Y ya tenemos la contraseña de un usuario que será arasaka.

![](images/Pasted%20image%2020251218122442.png)


Y ahí lo tenemos. 

# Privilegios.
-------------

Como no somos root aún, vamos a ello. 

```
sudo -l 
```

![](images/Pasted%20image%2020251218122621.png)


Listamos permisos ocultos con : 

```
ls -la
```

![](images/Pasted%20image%2020251218122730.png)


![](images/Pasted%20image%2020251218122827.png)



Como observamos que el archivo utiliza la biblioteca `base64` de manera relativa, procederemos a crear un archivo llamado `base64.py` . Luego, le otorgaremos permisos de ejecución para que podamos aprovecharlo cuando se ejecute con privilegios de `sudo`.


Ejecutamos el script con permisos de root. 
```
sudo -u root /usr/bin/python3.11 /home/arasaka/randombase64.py
```

Y luego ejecutamos el siguiente comando : 

```
ls -la /bin/bash
bash -p
```


![](images/Pasted%20image%2020251218123314.png)

Somos root


![](images/Pasted%20image%2020251218123546.png)

Conseguido!!!
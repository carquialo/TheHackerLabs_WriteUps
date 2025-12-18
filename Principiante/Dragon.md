
![](Pasted%20image%2020250822170649.png)

Escaneo

Primero tenemos que saber cuál es nuestra ip. 

```
ip addr
```

![](Pasted%20image%2020250822160711.png)

Ahora continuaremos con el escaneo de red 

```
arp-scan -I eth0 --localnet
```

![](Pasted%20image%2020250822161039.png)

Esta sería la ip víctima 

```
10.0.2.5
```

Podríamos hacer un ping hacia la máquina víctima y poder ver los saltos. 

```
ping -c1 10.0.2.5 -R
```

![](Pasted%20image%2020250822161555.png)

La anterior imagen muestra como pega el salto desde la atacante a la víctima y desde la víctima a la atacante. 
Podemos ver también que el tiempo de vida del TTL es de 64 por lo que sería un linux. 


Escaneo de puertos. 

Escanearemos los puertos abiertos con nmap. 

```
nmap -p- --open -sS -T4 -n -Pn -vvv 10.0.2.5
```

![](Pasted%20image%2020250822162149.png)

Nos muestra que hay dos puertos abiertos. 

Nos vamos a la página de esa IP. 

Nos sale el siguiente mensaje. 
![](Pasted%20image%2020250822162359.png)

Con gobuster haremos para detectar algúntipo de directorio. 

```
gobuster dir -u http://10.0.2.5/ -w /usr/share/wordlists/rockyou.txt
```

![](Pasted%20image%2020250822162929.png)


![](Pasted%20image%2020250822163237.png)

Podríamos probar fuerza bruta con el usuario Dragon. 

![](Pasted%20image%2020250822163924.png)

Ahí lo tenemos. 


![](Pasted%20image%2020250822164109.png)

Y ya estaríamos adentro. 

![](Pasted%20image%2020250822164330.png)
Ahí tenemos la flag del ususario. 

Ahora para elevar permiso, 
nos vamos a ``sudo -l`` 

![](Pasted%20image%2020250822164544.png)

![](Pasted%20image%2020250822164625.png)

Podemos echar un vistazo para ver ese binario para levantar una shell como root. 

![](Pasted%20image%2020250822165418.png)

Una vez que hayamos sido root, solo nos falta encontrar el flag. 

![](./images/![](Pasted%20image%2020250822170642.png)Pasted%20image%2020250822165352.png)



![](Pasted%20image%2020251112101058.png)


Averiguaremos la IP víctima e IP atacante. 
Para saber cual es la ip que tenemos en nuestro rango de red, usaríamos arp-scan.

```
arp-scan -I eth0 --localnet
```

IP víctima: 
```
10.0.2.15
```

Y para saber cuál es la ip nuestra. 

```
ip a
```

IP atacante: 
```
10.0.2.14
```


ENUMERACIÓN
--------------------------

Usamos nmap. Esta vez uso estos parámetros. 

```
nmap -p- --open -sS -n -Pn -vvv 10.0.2.15
```

![](Pasted%20image%2020251112102719.png)

Tenemos estos dos puertos. 
Ahora vamos a mirar la página web. 

![](Pasted%20image%2020251112103019.png)

Tenemos un apache. 

Miramos el código fuente y nos encontramos un código Brainfuck, así que lo desencriptaremos. 

![](Pasted%20image%2020251112104222.png)

Lo desencriptamos. 

![](Pasted%20image%2020251112104258.png)


"abuelacalientalasopa"

Pues tenemos el código, puede ser que abuela sea un usuario. así que probaremos con ello. 
También podemos hacer un diccionario con diferentes usuarios. 

Pero esta vez probamos con : 


```
ssh abuela@10.0.2.15
```

y con la contraseña : abuelacalientalasopa

![](Pasted%20image%2020251112105136.png)


PRIVILEGIOS
--------------------

No somos root aún así que vamos ...

```
sudo -l
```

![](Pasted%20image%2020251112105357.png)

Ahora nos vamos a la web de GTFOBins. 


![](Pasted%20image%2020251112105558.png)

![](Pasted%20image%2020251112105627.png)

Ya somos root. 

Ahora a encontrar los flags. 

![](Pasted%20image%2020251112105833.png)


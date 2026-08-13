========================================
# Resolucion de problemas basicos de red
========================================

--> Leccion 1

### Ejercicios Guiados 
1. ¿Qué comandos se pueden utilizar para listar las interfaces de red?

```
ip link

ifconfig 
```

2. ¿Cómo se desactiva temporalmente una interfaz? ¿Cómo se vuelve a habilitar?

```
ip link set wlan up/down


ifconfig wlan0 up/down
```


3. ¿Cuál de las siguientes es una máscara de subred razonable para IPv4?

0.0.0.255   

255.0.255.0

255.252.0.0  Esta es una mascara de red razonable

/24 Esta es una mascara de red razonable

4. ¿Qué comandos puede utilizar para verificar su ruta por defecto?

```
ip route

route 

```

5. ¿Cómo se añade una segunda dirección IP a una interfaz?

```
ip addr add 192.168.1.55/24 dev wlan0

ifconfig wlan0 192.168.1.55 netmask 255.255.255.0 
```

### Ejercicios Exploratorios

1. ¿Qué subcomando de ip se puede utilizar para configurar el etiquetado vlan?

```
ip link add link enp0s9 name enp0s9.20 type vlan id 20
```


2. ¿Cómo se configura una ruta por defecto?

```
Formato basico

ip route default [gw-router] dev [interface]  --> Agregar una ruta

ip route del default [gw-router] --> Eliminar una ruta

```

```
route add default gw 192.168.1.1
```


Nota:Para que estos cambios funcionen en un entorno real hay que configurar el firewall (iptables) si es que linux este como router

3. ¿Cómo se puede obtener información detallada sobre el comando ip neighbour? ¿Qué sucede si lo ejecuta por sí mismo?

```
man ip neighbour 

ip neighbour --> Muestra la cache arp 
```

4. ¿Cómo se hace una copia de seguridad de la tabla de enrutamiento? ¿Cómo se restaura desde ella?

```
# ip route save > /root/routes/route_backup
# ip route restore < /root/routes/route_backup
```

5. ¿Qué subcomando ip puede utilizarse para configurar las opciones del árbol de expansión?

Arbol de expansion o Spanning Tree Protocol (STP), es un protocolo de capa 2 que previene un bucle de datos, detiene las tormentas de difusión y proporciona rutas de respaldo

De forma similar a la gestión de la configuración de las vlan, ip link puede configurar el árbol de expansión utilizando el tipo bridge. El ejemplo muestra la adición de una interfaz virtual con una prioridad STP de 50


```
ip link add link enp0s9 name enp0s9.50 type bridge priority 50
```


--> Leccion 2

### Ejercicios Guiados 

1.¿Qué comando(s) utilizaría para enviar un eco ICMP a learning.lpi.org?

```
ping learning.lpi.org

traceroute -I learning.lpi.org
tracepath learning.lpi.org

```

2. ¿Cómo podría determinar la ruta a 8.8.8.8?

Hay dos comandos, traceroute o tracepath
```
traceroute 8.8.8.8

tracepath 8.8.8.8

```

3.¿Qué comando le mostraría si algún proceso está escuchando en el puerto TCP 80?

```
netstat -tulnp

ss -tulnp

```

4.¿Cómo se puede saber qué proceso está escuchando en un puerto?

El mismo ss o netstat se puede ver el proceso que esta escuchando el puerto, por ejemplo:

en la salida:

```
Netid  State   Recv-Q   Send-Q                          Local Address:Port     Peer Address:Port  Process
udp    UNCONN  0        0                                 224.0.0.251:5353          0.0.0.0:*      users:(("brave",pid=2404,fd=90))
udp    UNCONN  0        0                                 224.0.0.251:5353          0.0.0.0:*      users:(("brave",pid=2595,fd=55))
udp    UNCONN  0        0                                 224.0.0.251:5353          0.0.0.0:*      users:(("brave",pid=2595,fd=95))
udp    UNCONN  0        0          [fe80::593f:eaaa:d758:cd81]%wlp2s0:546              [::]:*
tcp    LISTEN  0        128                                   0.0.0.0:22            0.0.0.0:*
tcp    LISTEN  0        128                                      [::]:22               [::]:*
```

Podemos ver que el proceso en la columna, o podemos hacer

```
ss -ln | grep ":80"

netstat -ln | grep ":80"
```

```
lsof -Pi:80
```



5.¿Cómo se puede determinar la MTU máxima de una ruta de red?

Se puede usar el comando tracepath, con este comando podemos rastrear los tamaños de las unidades maximas de trasmision a lo largo de la ruta

```
tracepath somehost.example.com 
```

### Ejercicios Exploratorios

1. ¿Cómo podría utilizar netcat para enviar una petición HTTP a un servidor web?

```
 echo -n "GET / HTTP/1.0\r\n\r\n" | nc host.example.com 80
```

[nc doc](https://linux.die.net/man/1/nc)


2. ¿Cuáles son algunas de las razones por las que puede fallar el ping a un host?

Algunos firewalls no aceptan este tipo de trafico o simplemente evitar una escalada hacia un posible ataque o simplemente el host no esta activo 


- El host remoto no funciona.

- Un ACL del router está bloqueando el ping.

- El firewall del host remoto está bloqueando el ping.

- Puede que esté utilizando un nombre o una dirección de host incorrecta.

- Su resolución de nombres está devolviendo una dirección incorrecta.

- La configuración de red de su máquina es incorrecta.

- El cortafuegos de su máquina lo está bloqueando.

- La configuración de red del host remoto es incorrecta.

- La(s) interfaz(es) de su máquina está(n) desconectada(s).

- La(s) interfaz(es) de la máquina remota está(n) desconectada(s).

- Un componente de la red, como un conmutador, un cable o un router entre su máquina y la remota, ya no funciona.


3. Nombre una herramienta que pueda utilizar para ver los paquetes de red que llegan o salen de un host Linux.

```
tcpdump 
```

[Documentation of tcpdump command](https://www.digitalocean.com/community/tutorials/how-to-use-netcat-to-establish-and-test-tcp-and-udp-connections)

4. ¿Cómo se puede forzar a traceroute a utilizar una interfaz diferente?

Usando la opcion -i del comando, como:

```
traceroute 8.8.8.8 -i wlan1
```


5. ¿Es posible que traceroute informe de las MTU?

```
traceroute gob.mx -i wlp2s0 --mtu
```





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

### Ejercicios Exploratorios




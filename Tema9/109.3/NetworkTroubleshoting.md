# Resolucion de problemas basicos de red

-> Leccion 1

### Sobre el comando ip

Para ver la pagina man del comando:

```
man ip
```

o para ver el subcomando  a ip, por ejemplo,

```
ip-route
```

Otra fuentes es la funcion de ayuda, por ejemplo

```
ip address help
```

### Revision de mascaras de enrutamiento
- IPv4 e IPv6 son lo que se conoce como protocolos enrutados, Esto significa que estan diseñados de tal manera que los diseñadores de redes pueden controlar el flujo de trafico

- Los protocolos enturables permiten a los diseñadores de redes segmentarlas para reducir los requisitos de procesamiento de los dispositivos de conectividad, proporcionar redundancia y gestionar el  trafico

- Las direcciones 4 y 6 tienen dos segmentos uno de red y otro de host 

- El primer conjunto de bits constituye la seccion de red, mientras que el segundo conjunto constituye la parte de host. El numero de bits que componen la parte de red viene determinado por la mascara de red (tambien llamada mascara de subred). A veces tambien se denomina longitud del prefijo

- La parte de red de una direccion es utilizada por las maquinas IPv4 o IPv6 para buscar en su tabla de enrutamiento la interfaz por la que se debe enviarse el paquete

- Cuando un host ipv4 o ipv6 con el enrutamiento activado recibe un paquete que no es para el propio host, intenta hacer coincidir la parte de red del destino con una red en la tabla de enrutamiento. Si se ubica una entrada que coincida, envia el paquete al destino especificado en la tabla de enrutamiento. 

- Si no se encuentra ninguna entrada y se ha configurado una ruta por defecto, se envia a la ruta por defecto. Si no se localiza ninguna entrada y no se ha configurado ninguna ruta por defecto, el paquete se descarta 


### Configurar una interfaz 
Hay dos herramientas que cubriremos que se puede usar para configurar una interfaz de red:

1. ifconfig (en las nuevas distros, este comando no esta heredado, pero se puede usar instalando el paquete: net-tools)
2. ip

### Ver interfaces estan disponibles en el sistema

```
ifconfig -a

ip address (ip a) (ip addr) (ip add)...
```

si queremos ver la pagina de manual del subcomando ip, hariamos lo siguiente:

```
man ip-address
```

### Enlaces de interfaces disponibles

```
ip link
```

Mostrara y listara los enlaces de interfaz disponibles para su configuracion

Asumiendo que el sistema de archivos sys esta nontado, tambien puede listar el contenido de /sys/class/net/

```
ls /sys/class/net
```

### Configurar una interfaz de red 

- Deben de iniciar sesion como root

```
ifconfig enp1s0 192.168.50.50/24
```

La version de linux de ifconfgi es flexible con la forma de especificar la mascara de subred

```
ifconfig eth2 192.168.50.50 netmask 255.255.255.0

ifconfig eth2 192.168.50.50 netmask 0xffffff00

ifconfig enp0s8 add 2001:db8::10/64
```

- Revise que con ipv6 se ha usado la palabra clave ad, si no precede una direccion ipv6 con add, recibira un mensaje de error


```
ip addr add 192.168.5.5/24 dev enp0s8

ip addr add 2001:db8::10/64 dev enp0s8

```

### Configuracion de opciones de bajo nivel

Una tarea comun para ip link es desactivar o activar una interfaz y con ifconfig tambien se hace como:

```
ip link set dev enp0s8 down 

ip link show dev enp0s8  
```

```
ifconfig enp0s8 up

ip link show dev enp0s8
```

A veces puede ser necesario ajustar la MTU de una interfaz. Al igual que con la habilitacion/deshabilitacion de interfaces, esto puede hacerse con `ifconfig` o `ip link`

```
ip link set enp0s8 mtu 2000
```

```
ifconfig enp0s3 mtu 1500
```

### La tabla de enrutamiento 

Para ver la tabla de rutas, podemos usar:

```
route

netstat -r

ip route
```

Para modificar las tablas debemos usar 

```
route 

ip route
```

Para ver la tabla de enrutamiento para ipv6, debe usar uno de los siguientes comandos:

```
route -6 

netstat -6r 

ip -6 route

```

```
$ route -6
Kernel IPv6 routing table
Destination                    Next Hop                   Flag Met Ref Use If
2001:db8::/64                  [::]                       U    256 0      0 enp0s8
fe80::/64                      [::]                       U    100 0      0 enp0s3
2002:a00::/24                  [::]                       !n   1024 0      0 lo
[::]/0                         2001:db8::1                UG   1   0      0 enp0s8
localhost/128                  [::]                       Un   0   2     84 lo
2001:db8::10/128               [::]                       Un   0   1      0 lo
fe80::a00:27ff:fe54:5359/128   [::]                       Un   0   1      0 lo
ff00::/8                       [::]                       U    256 1      3 enp0s3
ff00::/8                       [::]                       U    256 1      6 enp0s8
```

**Flag**: proporciona informacion sobre la ruta
- U indica que la ruta esta activa
- ! significa que la rutta ha sido rechazada, es decir, que no sera utilizada
- n significa que la ruta no ha sido cacheada (el kernel mantienen una cache de rutas para busquedas mas rapidas por separado de todas las rutas conocidas)
- Indica una puerta de enlace 
- Metric o Met no es utilizada por el kernel. Se refiere a la distancia administrativa al objetivo, esta distancia administrativa es utilizada por los protocolos de enrutamiento para determinar las rutas dinamicas
- Ref es el recuento de referencias, o el numero de usos de una ruta, al igual que metric, no es utilizada por el kernel
- Use muestra el numero de busquedas de una ruta

```
[root@archlinux ~]# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         _gateway        0.0.0.0         UG        0 0          0 enp0s3
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 enp0s3
```
- MSS indica el tamaño maximo de segmento para las conexiones TCP sobre esa ruta
- La columna window muestra el tamaño predeterminado de la ventana TCP
- La columna irtt muestra el tiempo de ida y vuelta de los paqyuetes 

```
default via 192.168.1.254 dev enp0s3 proto static 
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.78 
```

- Destino
- Direccion opcional seguida de interfaz
- El protocolo de enrutamiento utilizado para añadir la ruta
- El ambito de la ruta. Si se omite, se trata de un ambito global o de una puerta de enlace
- La metrica de la ruta. Esta es utilizada por los protocolos de enrutamiento dinamico para determinar el coste de la ruta. La mayoria de los sistemas no la utilizan
- Si es una ruta IPv6, la preferencia de ruta RFC4191

Otros ejemplos:

ipv4

```
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
```

1. El destino es la ruta por defecto.

2. La dirección de la puerta de enlace es 10.0.2.2 alcanzable a través de la interfaz enp0s3.

3. Ha sido añadida a la tabla de enrutamiento por DHCP.

4. Se ha omitido el ámbito, por lo que es global.

5. La ruta tiene un valor de coste de 100.

6. No hay preferencia de ruta IPv6


ipv6

```
fc0::/64 dev enp0s8 proto kernel metric 256 pref medium
```


1. El destino es fc0::/64.

2. Es alcanzable a través de la interfaz enp0s8.

3. Ha sido añadida automáticamente por el kernel.

4. Se ha omitido el ámbito, por lo que es global.

5. La ruta tiene un valor de coste de 256.

6. Tiene una preferencia IPv6 de media.


## Gestion de rutas

Las rutas pueden ser gestionadas usando 

- route 
- ip route 

Para agregar y eliminar 

```
route -6 add 2001:db8:1::/64 gw 2001:db8::3   -> Agregar con route

route -6 del 2001:db8:1::/64 gw 2001:db8::3   -> Eliminar con route

```

```
ip route add 2001:db8:1::/64 via 2001:db8::3

ip route del 2001:db8:1::/64 via 2001:db8::3
```




-> Leccion 2

Para probar conexion, usaremos algunas herramientas que nos pueden ayudar a diagnosticar el estado, si hay conexion o no o algun otro problema

## Probar conexion con `ping`

Los comandos ping y ping6 son usados para enviar una solicitud de eco ICMP a una direccion IPv4 o IPv6, respectivamente.

1. Una peticion de eco de ICMP envia una pequeña cantidad de datos a la direccion de destino
2. Si la direccion de destino es alcanzable, enviara un mensaje de respuesta de eco ICMP de vuelta al remitente con los mismos datos que le fueron enviados 


**Nota:** Se puede usar la opcion -c para especificar el numero de paquetes a enviar, si se omite, tanto ping y ping6 seguiran enviando paquetes hasta la señal de salida `ctrl+c`

Que no se pueda hacer ping a un host no significa que no pueda conectarse a el. Muchas organizaciones tienen cortafuegos o listas de control de acceso al router, que bloquean todo lo que no sea el minimo necesario para que sus sistemas funcionen

## Traceroute

Los programas `traceroute` y  `traceroute6` pueden usarse para mostrar la ruta que sigue un paquete para llegar a su destino. Lo hacen enviando multiples paquetes al destino, incrementando el campo Time-To-Live (TTL) de la cabecera IP con cada paquete subsiguiente






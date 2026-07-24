#########################################
# Resolucion de problemas basicos de red
########################################


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






-> Leccion 2



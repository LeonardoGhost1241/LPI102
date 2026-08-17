# Configuracion DNS en el lado del cliente

Los programas que resuelven nombres a numeros casi siempre utilizan funciones proporcionadas por la biblioteca estandar de C 

Lo primero que hacen estas funciones es:

1. Leer el archivo `/etc/nsswitch.conf` para ibtener instrucciones sobre como resolver ese tipo de nombre
2. Una vez que el proceso lee el archivo, busca el nombre de manera especificada


## Clases de DNS
El DNS tiene 3 clases de registro

- IN
- HS
- CH

En esta leccion todas las consultas DNS seran de tipo IN, esta clase es para las direcciones de internet que utilizan la pila TCP/IP

CH es para ChaosNet que es una tecnologia de red que tuvo una vida corta y ya no esta en uso

HS es para Hesiod, Hesios es una forma de almacenar cosas como passwd y entradas de grupo en DNS

## Comprendiendo el archivo `/etc/nsswitch.conf`

Para leer su pagina de manual, podemos hacer: 

```
man nsswitch.conf
```

Ejemplo del archivo

```
   passwd:         compat
           group:          compat
           shadow:         compat

           hosts:          dns [!UNAVAIL=return] files
           networks:       nis [NOTFOUND=return] files
           ethers:         nis [NOTFOUND=return] files
           protocols:      nis [NOTFOUND=return] files
           rpc:            nis [NOTFOUND=return] files
           services:       nis [NOTFOUND=return] files
           # This is a comment. It is ignored by the resolution functions.
```
En reumen:

Este archivo, aloja el origen para obtener informacion de cualquier base de datos, por ejemplo:

```
    hosts:  files dns
```

El sistema primero buscara el nombre de dominio en los archivos del sistema, como lo es /etc/host, si no lo encuentra, seguira al resolvedor de nombres dns

[Ejemplos nsswitch.conf](https://bytelearning.blogspot.com/2016/04/fichero-nsswitchconf-que-es-y-para-que.html)
[Pagina man](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html)


El archivo esta organizado en columnas

- La del extremo izquierdo es el tipo de base de datos de nombres
- El resto de las columnas son los metodos que las funciones de resolucion deben utilizar para buscar un nombre 
- Los metodos van seguidos de las funciones de izquierda a derecha. Las columnas con [] se utilizan para proporcionar alguna logica condicional limitada a la columna inmediatamente a la izquierda de la misma

Ejemplo:
Supongamos que queremos resolver el nombre de host learing.lpi.org
1. Haria una llamada a la biblioteca C apropiada (probablemente gethostbyname), esta funcion leera encontces `/etc/nsswitch.conf`
2. Cuando lea el archivo de nsswitch, lo que encontrara es la linea que comienza con `host`
3. Ubicada esta linea, comenzara a buscar, primero usara el dns para resolver el nombre
4. La siguiente columna que nos dice `[!UNAVAIL=return]` significa que si el servicio no esta disponible, entonces no intente la siguiente fuente, es decir, si el DNS esta disponible deje de resolver el nombre del host aunque los servidores de nombres no puedan. Si el dns no esta disponible, entonces continue con la siguiente fuente, en este caso la sigueinte fuente es `files`


## El archivo `/etc/resolv.conf`

Este archivo es usado para configurar la resolucion de hots mediante DNS

- Algunas distribuciones de inicio, demonios y otras herramientas que escriben en este archivo

**Nota:** Algunas herramientas, como Network Manager dejaran un comentatio en el archivo para infomarle que los cambios manuales se sobreescribiran

El formato del archivo es sencillo
- En la Columna de la izquierda se encuentra la opcion `name`
- El resto de las columnas en la misma linea son el valor de la opcion

La opcion mas comun es la del nameserver, se utiliza para especificar la direccion ipv4 o ipv6 de un servidor DNS

- Si su archivo de /etc/resolv.conf no tiene  la opcion nameserver, su sistema utilizara por defecto el servidor de nombres de la maquina local

Ejemplo:

```
search lpi.org
nameserver 10.0.0.53
nameserver fd00:ffff::2:53

```


La opción search se utiliza para permitir las búsquedas de forma corta. En el ejemplo, se ha configurado un único dominio de búsqueda de lpi.org. Esto significa que cualquier intento de resolver un nombre de host sin una porción de dominio tendrá .lpi.org añadido antes de la búsqueda. Por ejemplo, si intentara buscar un host llamado aprendizaje, el resolvedor buscaría aprendizaje.lpi.org. Puede tener configurados hasta seis dominios de búsqueda.

Otra opción común es la opción domain. Se utiliza para establecer el nombre de dominio local. Si esta opción no está presente, se utiliza por defecto todo lo que sigue al primer . en el nombre de host de la máquina. Si el nombre de host no contiene un ., se asume que la máquina es parte del dominio raíz. Al igual que search, domain puede utilizarse para búsquedas de nombres cortos.

Tenga en cuenta que domain y search son mutuamente excluyentes. Si ambos están presentes, se utiliza la última instancia del archivo.

Hay varias opciones que se pueden establecer para afectar el comportamiento del resolver. Para establecerlas, utilice la palabra clave options, seguida del nombre de la opción a establecer, y si es aplicable, un : seguido del valor. A continuación se muestra un ejemplo de configuración de la opción de tiempo de espera, que es el tiempo en segundos que el resolvedor esperará a un servidor de nombres antes de rendirse:

option timeout:3
Hay otras opciones en resolv.conf, pero estas son las más comunes



## El archivo `/etc/hosts`

Este arhcivo se usa para resolver nombres a direcciones IP y veceversa.

Tanto ipv4 como ipv6 son compatibles

- La columna de la izquierda es la direccion IP
- El resto son nombres asociados a esa direccion

El usmo mas comun de /etc/hosts es para hosts y direcciones donde el DNS no es posible, como las direcciones de loopback

Ejemplo:

```
127.0.0.1       localhost
127.0.1.1       proxy
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters

10.0.0.1        gateway.lpi.org gateway gw
fd00:ffff::1    gateway.lpi.org gateway gw

10.0.1.53       dns1.lpi.org
fd00:ffff::1:53 dns1.lpi.org
10.0.2.53       dns2.lpi.org
fd00:ffff::2:53 dns2.lpi.org
```

## Systemd-resolved

Systemd proporciona un servicio llamado systemd-resolved. Proporciona mDNS, DNS y LLMNR. Cuando se ejecuta, escucha las peticiones DNS en 127.0.0.53. No proporciona un servidor DNS completo. Las peticiones DNS que recibe se buscan consultando los servidores configurados en /etc/systemd/resolv.conf o /etc/resolv.conf. Si desea utilizar esto, use resolve para hosts en /etc/nsswitch.conf. Tenga en cuenta que el paquete del sistema operativo que tiene la biblioteca systemd-resolved puede no estar instalado por defecto.


## Herramientas de resolucion de nombres 
- getent, es util para ver como se resuelven las peticiones del mundo real
- host, es util para consultas DNS simples
- dig, es util para operaciones DNS complejas que pueden ayudar a solucionar problemas del servidor DNS


### GETENT

* Se usa para mostrar entradas de las bases de datos del servicio de nombres
* Puede recuperar registros de cualquier fuente configurable por `/etc/nsswitch.conf`

Para usarlo, pasaremos como primer parametro el tipo de nombre que deseamos devolver y podemos pasar opcionalmente un segundo argumento para especificar para buscar

```
getent host 
```

A partir de la version 2.2.5 de glibc, puede forzar a getent a utilizar una fuente de datos especifica con la opcion -s, por ejemplo:

```
$ getent -s files hosts learning.lpi.org
::1             learning.lpi.org
$ getent -s dns hosts learning.lpi.org
208.94.166.198  learning.lpi.org
```

### HOST
Es un programa sencillo para buscar entradas DNS. Sin opciones, si a host se le da un nombre, devuelve los conjuntos de registros A, AAAA y MX. Si se le da una direccion IPv4 o IPv6, devuelve el registro PTR si hay uno disponible

```
$ host wikipedia.org
wikipedia.org has address 208.80.154.224
wikipedia.org has IPv6 address 2620:0:861:ed1a::1
wikipedia.org mail is handled by 10 mx1001.wikimedia.org.
wikipedia.org mail is handled by 50 mx2001.wikimedia.org.
$ host 208.80.154.224
224.154.80.208.in-addr.arpa domain name pointer text-lb.eqiad.wikimedia.org.
```

Si busca un tipo de registro específico, puede utilizar host -t:

```
$ host -t NS lpi.org
lpi.org name server dns1.easydns.com.
lpi.org name server dns3.easydns.ca.
lpi.org name server dns2.easydns.net.
$ host -t SOA lpi.org
lpi.org has SOA record dns1.easydns.com. zone.easydns.com. 1593109612 3600 600 1209600 300
```

host también puede utilizarse para consultar un servidor de nombres específico si no desea utilizar los que se encuentran en /etc/resolv.conf. Simplemente añada la dirección IP o el nombre del servidor que desea utilizar como último argumento:


```
$ host -t MX lpi.org dns1.easydns.com
Using domain server:
Name: dns1.easydns.com
Address: 64.68.192.10#53
Aliases:

lpi.org mail is handled by 10 aspmx4.googlemail.com.
lpi.org mail is handled by 10 aspmx2.googlemail.com.
```

## DIG
Este comando es mas detallado que host

- Por defecto dig consulta los registros A
- Funciona para busquedas sencillas, pero es mas adecuado para solucionar problemas de configuracion del servidor 

```
dig learning.lpi.org


; <<>> DiG 9.11.5-P4-5.1+deb10u1-Debian <<>> learning.lpi.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63004
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 3, ADDITIONAL: 5

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: ca7a415be1cec45592b082665ef87f3483b81ddd61063c30 (good)
;; QUESTION SECTION:
;learning.lpi.org.		IN	A

;; ANSWER SECTION:
learning.lpi.org.	600	IN	A	208.94.166.198

;; AUTHORITY SECTION:
lpi.org.		86400	IN	NS	dns2.easydns.net.
lpi.org.		86400	IN	NS	dns1.easydns.com.
lpi.org.		86400	IN	NS	dns3.easydns.ca.

;; ADDITIONAL SECTION:
dns1.easydns.com.	172682	IN	A	64.68.192.10
dns2.easydns.net.	170226	IN	A	198.41.222.254
dns1.easydns.com.	172682	IN	AAAA	2400:cb00:2049:1::a29f:1835
dns2.easydns.net.	170226	IN	AAAA	2400:cb00:2049:1::c629:defe

;; Query time: 135 msec
;; SERVER: 192.168.1.20#53(192.168.1.20)
;; WHEN: Sun Jun 28 07:29:56 EDT 2020
;; MSG SIZE  rcvd: 266
```


La salida esta dividida en secciones
1. Muestra informacion sobre la version de dig instalada y la consulta enviada, junto con las opciones utilizadas para el comadno
2. A continuacion muestra informacion sobre la consulta y respuesta
3. La siguiente seccion muestra informacion sobre las extensiones EDNS utilizadas y la consulta. En el ejemplo se utiliza la extension cookie
4. La siguiente seccion muestra el resultado de la consulta. El numero de la segunda columna es el TTL del recurso en segundos
5. El resto de la salida proporciona informacion sobre los servidores de nombres del dominio, incluyendo los registros NS del servidor junto con los registros A y AAAA de los servidores en el registro NS del dominio


Al igual que host, puede especificar un tipo de registro con la opcion -t:

```
$ dig -t SOA lpi.org

; <<>> DiG 9.11.5-P4-5.1+deb10u1-Debian <<>> -t SOA lpi.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16695
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 3, ADDITIONAL: 6

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 185c67140a63baf46c4493215ef8906f7bfbe15bdca3b01a (good)
;; QUESTION SECTION:
;lpi.org.			IN	SOA

;; ANSWER SECTION:
lpi.org.		600	IN	SOA	dns1.easydns.com. zone.easydns.com. 1593109612 3600 600 1209600 300

;; AUTHORITY SECTION:
lpi.org.		81989	IN	NS	dns1.easydns.com.
lpi.org.		81989	IN	NS	dns2.easydns.net.
lpi.org.		81989	IN	NS	dns3.easydns.ca.

;; ADDITIONAL SECTION:
dns1.easydns.com.	168271	IN	A	64.68.192.10
dns2.easydns.net.	165815	IN	A	198.41.222.254
dns3.easydns.ca.	107	IN	A	64.68.196.10
```

El comando dig tiene muchas oopciones para afinar tanto la salida como la consulta enviada al servidor. Estas opciones comienzan con +, una de ellas es la opcion `short` que suprime toda la salida excepto el resultado

```
$ dig +short lpi.org
65.39.134.165
$ dig +short -t SOA lpi.org
dns1.easydns.com. zone.easydns.com. 1593109612 3600 600 1209600 300

```

Ejemplo de como desactivar la extension de la cookie EDNS:

```
$ dig +nocookie -t MX lpi.org

; <<>> DiG 9.11.5-P4-5.1+deb10u1-Debian <<>> +nocookie -t MX lpi.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 47774
;; flags: qr rd ra; QUERY: 1, ANSWER: 7, AUTHORITY: 3, ADDITIONAL: 5

;; OPT PSEUDOSECTION:
```





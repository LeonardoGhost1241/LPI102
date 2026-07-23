================================
# Configuracion de red persistente
=================================

-> Leccion 1 

### Interfaz de red   
La interfaz de red es el canal de comunicacion configurado para trabajar con el ardware de red conectado a un sistema, la excepcion a esto es la interfaz loopack, que el sistema operativo utiliza cuando necesita establecer conexion consifo mismo 


El comando completo para mostrar las interfaces es:
```
ip link show 
```

si esta disponible 

```
nmcli device
```

Estos comandos pueden ser usados por cualuqier usurio, ya que no modifica nada del sistema


### Nombres de interfaces
En las distribuciones de linux que utilizan el esquema de nomenglatura systemd, todos los nombres de interfaz comienzan con un prefijo de dos carcateres que significa el tipo de interfaz

- en: Ethernet
- ib: InfiniBand
- sl: Serial line IP (slip)
- wl: Wireless local area network (WLAN)
- ww: Wireless wide area network (WWAN)


De mayor a menor prioridad, el sistema operativo utiliza las siguientes reglas para nombrar y numerar las interfaces de red:

1. Nombra la interfaz según el índice proporcionado por la BIOS o por el firmware de los dispositivos integrados, por ejemplo, eno1.

2. Designa la interfaz según el índice de la ranura PCI express, tal y como lo indica la BIOS o el firmware, por ejemplo ens1.

3. Nombra la interfaz según su dirección en el bus correspondiente, por ejemplo enp3s5.

4. Designa la interfaz con la dirección MAC de la misma, por ejemplo enx78e7d1ea46da.

5.  Nombra la interfaz utilizando la convención heredada, por ejemplo eth0.


Ejemplo:
```
enp3s5
```
Podemos suponer que se denomino asi por que no se ajustaba a los dos primeros metodos de denominacion, por lo que se uso el bus y la renura correspondiente 


### Gestion de interfaces 
Hay comandos auxiliares para la activacion y configuracion de las interfaces en lugar de usar ip

- ifup
- ifdown 

Pueden usarse para configurar las interfaces de red basandose en las definiciones de las interfaces que se encuentran en el fichero `/etc/network/interfaces`

**Aunque pueden ser invocados manualmente, estos comandos se ejecutan normalmente de forma automatica durante el arranque del sistema**



Todas las interfaces de red gestionadas por `ifup` y `ifdown` deben de estar listadas en el fichero `/etc/network/interfaces`

El formato del archivo `/etc/network/interfaces` es:

- Inicia con la palabra `auto` que se usa para identificar las interfaces fisicas que se van a activar cuando se ejecute ifup con la opcion -a

- La configurcion real de la interfaz se escribe empezando por la palabra `iface`, seguido del nombre de la interfaz, el nombre de la familia de direcciones que utiliza la interfaz y el nombre del metodo utilizado para configurar la interfaz

**Todas las interfaces marcadas como auto se activan en el momento del arranque, en el orden en que aparecen en la lista**

Ejemplo de archivo:
```
auto lo
iface lo inet loopback

auto enp3s5
iface enp3s5 inet dhcp
```

La familia de direcciones debe ser:
- `inet` para redes TCP/IP
- `ipx` para redes IPX
- `inet6` para redes IPv6
- `loopback` para interfaces Loopback
- `dhcp` la interfaz usara la configuracion IP proporcionada por el servidor DHCP de la red


Podemos ejecutar el comando `ifup` con argumentos, como:
```
ifup enp3s5
```

En el ejemplo anterior, esa interfaz tenia dhcp, por lo que le comando `ifup` llamo a un programa cliente DHCP, de la misma manera se puede usar `ifdown enp3s5` para apagar la interfaz 


En redes sin servidor DHCP, se puede usar el metodo `static` en su lugar y proporcionar la configuracion IP manualmente en `/etc/network/interfaces`, por ejemplo:

```
iface enp3s5 inet static
    address 192.168.1.2/24
    gateway 192.168.1.1
```

**Las interfaces que utilizan el metodo static  no necesitan una directiva `auto`, ya que se activan siempre que se detecta el hardware de la red**

- Si la misma interfaz tiene mas de una entrada, `iface`, entonces todas las direcciones y opciones configuradas se aplicaran al abrir esa interfaz. Util para configurar direcciones ipv4 como ipv6 en la misma interfaz o para confgiruar multiples direcciones del mismo tipo en una sola interfaz



##### Archivo /etc/network/interfaces

Este archivo es el corazon de la configuracion de red tradicional en sistemas operativos basados en Debian 


Estructura basica del archivo

El arhcivo se compone de dos directivas clave:

-  auto / allow-hotplug: Le dice al sistema que active la interfaz al arrancar (auto) o cuando se detecte el cable/hardware (allow-hotplug)

- iface: Define la interfaz y el método de configuración (ej. inet dhcp o inet static)



Configuracion por IP dinamica (DHCP)

```
auto eth0
iface eth0 inet dhcp

```

Configuracion por IP estatica (Fija)

```
auto eth0
iface eth0 inet static
    address 192.168.1.150
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4

```


### Nombres locales y remotos 

El nombre local suele coincidir con el nombre de red de la maquina, auqnue algunas veces no es asi, por lo que es bueno cambiarlo aunque no se conecte a la red

El fichero `/etc/hostname` puede editarse directamente, pero el nombre de la maquina puede definirse con el comando hostnamectl cuando se suministra el subcomando `set-hostname`, seguido de un argumento como:

```
# hostnamectl set-hostname storage
# cat /etc/hostname
storage
```



- Nombre estatico:  El nombre de host definido en `/etc/hostname/` es el nombre de host del sistema en el arranque, este nombre puede ser una cadena de forma libre de hasta 64 carcateres. Sin embargo se recomienda que conste solo de carcateres ASCII en minusculas y sin espacios ni puntos

```
hostnamectl set-hostname storage

```


- Pretty hostname: A diferencia del nombre estatico, este nombre puede incluir todo tipo de caracteres especiales, puede usarse para ser mas descriptivo ejemplo: "LAN Shared Storege"

```
hostnamectl --pretty set-hostname "LAN Shared Storage"

o

hostnamectl set-hostname "LAN Shared Storage" --pretty
```

- Transiest hostname: Se utiliza cuando el nombre de host estatico no esta establecido o cuando es el nombre `localhost` por defecto. 

```
hostnamectl --transient set-hostname generic-host
```


Resumen:
- Static hostname: Nombre principal del sistema utilizado por la red, ssh, logs y servicios, es permamante

- Pretty hostname: Nombres descriptivo para mostrar interfaces graficas, es permanente

- Transiest hostname: Nombre temporal asignado dinamicamente (normalmente por DHCP o servicios de red), no es permanente


**Si no se utiliza la opción --pretty ni --transient, los tres tipos de nombres de host se establecerán con el nombre dado.**

Para establecer SOLO el nombre statico sin afectar a los demas, debemos usar el argumento de --static, en todos los casos, solo el nombre de host se almacena en el fichero `/etc/hostname`


Hay dos formas que tiene el sistema hacer coincidir nombres y numeros IP
- Utilizar una fuente local
- O usar un servidor remoto para traducir los nombres en numeros IP y visceversa

Los metodos pueden complementarse entre si y su orden de prioridad se define en el archivo de configuracion Name Service Switch: `/etc/nsswitch.conf`. Este fichero es utilizado por el sistema y las aplicaciones para determinar no sólo las fuentes de coincidencias nombre-IP, sino también las fuentes de las que puede obtener información de servicios de nombres en una serie de categorías, llamadas bases de datos



El archivo `/etc/nsswitch.conf` nos muestra la base de datos de cada informacion administrativa, como el archivo paswd, shadow, groups, etc, este archivo le dice al sistema donde buscar cada cosa. La linea encargada de definir la busqueda del archivo host es `/etc/hosts` y para definirlo en el archivo nsswitch.conf hacemos:

```
hosts: files dns
```

Esto definira el proceso de busqueda de nombres de host, en primer lugar, el sistema buscara coincidencias en los arhcivos locales, y luego preguntara al servicio DNS por las coincidencias



El archivo `/etc/hosts`, asocia direcciones IP con nombres de host, una linea se vera como:

```
127.0.0.1 localhost

```

El numero Ip 127.0.0.1 es la direccion por defecto de la interfaz loopback, de ahi su asociacion con el nombre localhost 



También es posible vincular alias opcionales a la misma IP. Los alias pueden proporcionar ortografías alternativas, nombres de host más cortos y deben añadirse al final de la línea, por ejemplo:

```
192.168.1.10 foo.mydomain.org foo
```



Las reglas de formato para el archivo /etc/hosts son:

- Los campos de la entrada están separados por cualquier número de espacios en blanco y/o caracteres de tabulación.

- El texto desde un carácter # hasta el final de la línea es un comentario y se ignora.

- Los nombres de host sólo pueden contener caracteres alfanuméricos, signos menos y puntos.

-  Los nombres de host deben comenzar con un carácter alfabético y terminar con un carácter alfanumérico.




Las direcciones ipv6 tambien pueden agregarse a `/etc/hosts`, la siguiente entrada es la direcicon ipv6 de loopback


```
::1 localhost ip6-localhost ip6-loopback
```




Tras la especificación del servicio files, la especificación dns indica al sistema que solicite a un servicio DNS la asociación nombre/IP deseada. El conjunto de rutinas responsables de este método se llama resolver y su fichero de configuración es /etc/resolv.conf. El siguiente ejemplo muestra un /etc/resolv.conf genérico que contiene entradas para los servidores DNS públicos de Google:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

la palabra clave `nameserver` indica la direccion IP del servidor DNS

Si no hay entradas de servidor de nombres, el comportamiento por defecto es utilizar el servidor de nombres de la maquina local

El resolvedor puede configurarse para agregar automaticamente el dominio a los nombres antes de consultarlos en el servidor de nombres, ejemplo:

```
nameserver 8.8.4.4
nameserver 8.8.8.8
domain mydomain.org
search mydomain.net mydomain.com
```

La entrada dominio establece midominio.org como nombre de dominio local, por lo que las consultas de nombres dentro de este dominio podrán utilizar nombres cortos relativos al dominio local. La entrada search tiene un propósito similar, pero acepta una lista de dominios para probar cuando se proporciona un nombre corto. Por defecto, sólo contiene el nombre del dominio local.






-> Leccion 2

Las aplicaciones cliente para NetworkManager se pueden dividir en dos, graficas y de comandos 

- Graficas: nm-tray, network-manager-gnome, nm-applet o plasma-mn
- Comandos: nmcli y nmtui siendo esta ultima algo pseudografico en la temrinal


El comando separa todas las propiedades relacionadas con la red controladas por NetworkManager en categorias llamadas objects:

- `general`: El estado y las operaciones generales de NetworkManager
- `networking`: Control general de la red
- `radio`: Conmutadores de radio NetworkManager
- `Connection`: Las coneciones del NetworkManager
- `device`: Dispositivos gestionados por NetworkManager
- `agent`: Agente secreto NetworkManager o agente polkit
- `monitor`: Supervisa los cambios del NetworkManager


* Mostrar el estado general de conectividad

```
nmcli general

STATE      CONNECTIVITY  WIFI-HW  WIFI     WWAN-HW  WWAN
connected  full          enabled  enabled  enabled  enabled

```

La columna `STATE` indica al sistema si esta conectado a una red o no, etc

La columna `CONNECTIVITY` nos informara del estado de la conexion "completa"

Si aparece Portal en la columna `CONECTIVITY`, significa que requiere pasos adicionales de autenticacion (normalmente a travez del navegador) para completar el proceso de zonexion

Las columnas restantes informaran del estado de las conexiones inalambricas (si las hay), ya sean WIFI o WAN

El sufijo `HW` indica que el estado corresponde al dispositivo de red y no a la conexion de red del sistema, es decir, indica si el hardware esta activado o desactivado para ahorrar energia 




**nmcli necesita un argumento de comando en este caso un argumento para nmcli general [argument], en este caso si no hay alguno se usa status por defecto**



* Escanear redes disponibles con el comando 

```
nmcli dev wifi list
```

como ejemplo:

nmcli dev wifi connect Hypnotoad

Si esto se ejecuta en un emulador grafico, aparecera un cuadro de dialogo solicitando la frase de acceso a la red

o bien, podemos conectarnos directamente como

```
nmcli device wifi connect Hypnotoad password MyPassword
```

Si la red wifi oculta su nombre SSID, nmcli aun puede conectarse a ella con agumento extra de `hidden yes`

```
nmcli device wifi connect Hypnotoad password MyPassword hidden yes
```

En dado caso de tener mas un un adaptador wifi, podemos indicarlo de la siguiente manera

```
nmcli device wifi connect Hypnotoad password MyPassword ifname wlo1
```

Depues conectarse, NetworkManager le dara el nombre de SSID correspondiente (si es una conexion wi-fi) y lo conservara para futuras conexiones


* Los nombres de las conexiones y sus UUIDs son listados por el siguiente comando 

```
nmcli connection show 
```


* Desactivar una conexion 

Para realizar acciones sobre una conexion concreta, hay que proporcionar su nombre o UUID, por ejemplo, para desactivar la conexion `Hypnotoad`

```
nmcli connection down Hypnotoad
```

o para conectarla 

```
nmcli connection up Hypnotoad
```
 
esto por que ya esta guardada por NetworkManager


El nombre de la interfaz tambien puede utilizar para reconectar, pero en este caso se debe utilizar el objeto `device` en su lugar

```
nmcli device disconnect wlo2
```

al igual que se puede establecer conexion con el nombre de la misma interfaz


```
nmcli device connect wlo2

Device 'wlo1' successfully activated with '833692de-377e-4f91-a3dc-d9a2b1fcf6cb'.
```


Tenga en cuenta que el UUID de la conexion cambia cada vez que se abre la conexion, por lo que es preferible utilizar su nombre para mantener la coherencia


* apagar adaptadores

Si el adaptador inalambrico esta disponible pero no se esta utilizando, se puede apagar para ahorrar energia, esta vez usaremos el objeto radio



Una vez establecida la conexion, no sera necesario hacer mas, ya que NetworkManager identifica las redes conocidas disponibles y se conecta automaticamente. Si es necesario NetworkManager tiene plugins que pueden ampliar sus funcionalidades, como el pugin para soportar conexiones VPN


### systemd-networkd

Los sistemas que ejecutan systemd puede usar demonios para gestionar la conectividad de red: 

- systemd-networkd para controlar las interfaces de red 
- systemd-resolved para gestionar la resolucion de nombres locales



Los archivos de configuracion utilizados por systemd-networkd para configurar las interfaces de red pueden encontrarse en cualquiera de los tres directorios siguientes:

- `/lib/systemd/network`
El directorio de la red del sistema.

- `/run/systemd/network`
El directorio de red volátil en tiempo de ejecución.

- `/etc/systemd/network`
El directorio de red de la administración local

**Se recomienda comenzar sus nombres con numeros para facilitar la lectura y el ordenamiento ya que los archivos se procesas en orden lexicografico **


En orden de prioridad de mayor a menor, serian:

1. `/etc`
2. `/run`
3. `/lib`

Lo que significa que si los archivos de configuracion en diferentes directorios tienen el mismo nombre, entonces systemd-networkd ignorara los archivos con menor prioridad


- Los archivos que terminana en `.netdev` son utilizados por systemd-networkd para crear dispositivos de red virtuales, como los dispositivos bridge o tun
- Los archivos que terminan en `.link` establecen configuraciones de bajo nivel para la interfaz de red correspondiente

Systemd-network detecta y configura los dispositivos de red automaticamente a medida que aparecen, ademas de ignorar los dispositivos de red automaticamente a medida que aparecen 

El sufijo mas importante es .network, los archivos que tienen este sufijo pueden utilizarse para configurar direcciones y rutas de red.

**Al igual que los otros tipos de archivos de configuracion, el nombre del archivo define el orden en que el que se procesara el archivo** 


1. La interfaz de red a la que se refiere el fichero de confiuracion se define en la seccion [Match] dentro del mismo


ejemplo: En el archivo `/etc/systemd/network/30-lan.network` puede especificarse la interfaz de red como

```
[Match]
Name=enp3s5
```

Tambien podemos especificar varias formas como como:

Varias interfaces
```
[Match]
Name=enp3s5 wlan0 wlo2
```

Conjunto de interfaces

```
[Match]
Name=esp0*
```

Seleccion por MAC

```
[Match]
MACAdress=00:c0:ca:b1:b4:04
```

2. La configuracion del dispositivo se encuentra en la seccion [Network] del archivo

- Una configuracion siempre estatica requiere de las entradas `Address` y `Gateway`

```
[Match]
MACAddress=00:c0:ca:b1:b4:04

[Network]
Address=192.168.0.100/24
Gateway=192.168.0.1
```

- Para utilizar el protocolo DHCP en lugar de direcciones IP estaticas, se debe de usar la entrada DHCP como:

```
[Match]
MACAddress=00:16:3e:8d:2b:5b

[Network]
DHCP=yes
```

### Redes inalambricas protegidas por contrasenia 

Las redes inalambricas protegidas por clave pueden ser configuradas por systemd-networkd, aunque  el **Adaptador de red debe de estar ya configurado o autenticado en la red antes de que systemd-networkd pueda configurarlo**

La autenticacion la realiza WPA supplicant, un programa dedicado a configurar adaptadores de red protegidas por contrasenia

1. Creamos el archivo de credenciales con el comando `wpa_passphrase`

como:

```
wpa_passphrase MyWifi > /etc/wpa_supplicant/wpa_supplicant-wlo1.conf
```

Este comando tomará la frase de contraseña para la red inalámbrica MyWifi de la entrada estándar y almacenará su hash en el archivo /etc/wpa_supplicant/wpa_supplicant-wlo1.conf. 


Tenga en cuenta que el nombre del archivo debe contener el nombre apropiado de la interfaz inalámbrica, de ahí el wlo1 en el nombre del archivo

2. El gestor systemd lee los archivos de frases de paso WPA en `/etc/wpa_supplicant/` y crea el servicio correspondiente para ejecutar WPA supplicant y poner en marcha la interfaz


- El archivo de frases de paso creado tendra una unidad de servicio llamado: `wpa_supplicant@wlo1.service`, el cual asociara el adaptador inalambrico con el punto de acceso remoto

- El comando `systemctl estart wpa_supplicant@wlo1.service` asociara el adaptador inalambrico con el punto de acceso remoto

- El comando `systemctl enable wpa_supplicant@wlo1.service` hace que la asociacion sea automatica durante el aranque

3. Finalmente, un archivo `.network` que coincida con la interfaz wlo1 debe estar presente en /etc/systemd/network/, ya que systemd-networkd lo utilizará para configurar la interfaz tan pronto como el suplicante WPA finalice la asociación con el punto de acceso.


#### Ejemplo con `wpa_supplicant`

- `wpa_supplicant` = Es el programa principal que gestiona las redes 
- `wpa_phrasse` = Es una herramienta auxiliar para generar de forma segura claves cifradas


1. Debemos de crear el hash con la constrasenia, esto lo hacemos con el siguiente comando:

```
wpa_phrasse "SSID" "password" > /etc/wpa_supplicant/wpa_supplicant-[NOMBRE DE LA RED].conf

ejemplo:

wpa_passphrase INFINITUMBC38 "jm5cRL6qDe" > /etc/wpa_supplicant/wpa_supplicant-wlp4s0.conf
```

**Nota: Se recomienda borrar la password en texto plano**


2. Encenderemos manualmente el servicio con systemctl o con wpa_supplicant de la siguiente manera

- Con `wpa_supplicant`

```
wpa_supplicant -B -i wlp4s0 -c /etc/wpa_supplicant/wpa_supplicant-wlp4s0.conf
```

Esto iniciara automaticamente el servicio con systemctl

- Con systemctl

```
systemctl enable/start wpa_supplicant@wlp0s20f0u1.service

```
Identificador dinámico: Todo lo que escribes después del @ (en este caso, wlp0s20f0u1) se conoce como el nombre de la instancia.Reutilización de código: En lugar de crear un archivo de servicio diferente para cada tarjeta WiFi (wlan0, wlan1, etc.), systemd usa la misma plantilla y le pasa el nombre de la interfaz como un argumento.


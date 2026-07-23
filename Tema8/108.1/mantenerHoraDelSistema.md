# Mantener la hora del sistema 

# Leccion 1

- En la mayoria de los sistemas modernos de Linux, la hora del sistema y del hardware estan sincronizados con el tiempo de la red, que se implementa mediante el protocolo de Tiempo de Red (NTP)

- Local vs TIempo Universal El reloj del sistema está ajustado al Tiempo Universal Coordinado (UTC), que es la hora local de Greenwich, Reino Unido. Normalmente un usuario quiere saber su hora local. La hora local se calcula tomando la hora UTC y aplicando un offset basado en la zona horaria y en un "Horario de verano" De esta manera, se puede evitar mucha complejidad. 

- El reloj del sistema puede ajustarse a la hora UTC o a la hora local, pero se recomienda que se ajuste a la hora UTC


date - Es una utilidad basica que simplemente imprime la hora local 
    -u  Ver hora UTC actual
    
    Otras opciones que se adhiere a un modelo RFC aceptado:
    -I Fecha/hora en formato ISO 8601. Si se añade date (-Idate) se limitará la salida a la fecha solamente. Otros formatos son hours para horas, minutes para minutos, seconds para segundos y ns para nanosegundos
    -R Devuelve la fecha y la hora en formato RFC 5322
    --rfc-3339 Devuelve la fecha y la hora en formato RFC 3339


Se puede personalizar la salida del comando, como:

date +%s    ->  1574014515, Este formato se refiere al tiempo UNIX (El tiempo Unix se utiliza internamente en la mayoría de los sistemas tipo Unix. Almacena la hora UTC como el número de segundos desde Epoch, que ha sido definido como el 1 de enero de 1970)

Además, date --date puede usarse para dar formato a una hora que no es la actual, como:

```
date --date="next monday" +"%A %d-%m-%Y"    -> Monday 28-04-2026

date --date="2024-12-25 18:30" +"%Y/%m/%d - %H:%M"   -> 2024/12/25 - 18:30

```

Ademas podemos usar --debug para asegurar que una fecha pueda ser analizada con exito

La siguiente fecha es valida:

```
$ date --debug --date="Fri, 03 Jan 2020 14:00:17 -0500"
date: parsed day part: Fri (day ordinal=0 number=5)
date: parsed date part: (Y-M-D) 2020-01-03
date: parsed time part: 14:00:17 UTC-05
date: input timezone: parsed date/time string (-05)
date: using specified time as starting value: '14:00:17'
date: warning: day (Fri) ignored when explicit dates are given
date: starting date/time: '(Y-M-D) 2020-01-03 14:00:17 TZ=-05'
date: '(Y-M-D) 2020-01-03 14:00:17 TZ=-05' = 1578078017 epoch-seconds
date: timezone: system default
date: final: 1578078017.000000000 (epoch-seconds)
date: final: (Y-M-D) 2020-01-03 19:00:17 (UTC)
date: final: (Y-M-D) 2020-01-03 14:00:17 (UTC-05)
```


### Reloj de Hardware 
hclock - Ver como la hora se mantiene en el reloj en tiempo real.
    --verbos Dara mas detalles que puedne ser utiles 

- Necesita privilegios 

```
$ sudo hwclock --verbose
hwclock from util-linux 2.34
System Time: 1578079387.976029
Trying to open: /dev/rtc0
Using the rtc interface to the clock.
Assuming hardware clock is kept in UTC time.
Waiting for clock tick...
...got clock tick
Time read from Hardware Clock: 2020/01/03 19:23:08
Hw clock time : 2020/01/03 19:23:08 = 1578079388 seconds since 1969
Time since last adjustment is 1578079388 seconds
Calculated Hardware Clock drift is 0.000000 seconds
2020-01-03 14:23:07.948436-05:00

Fíjese en la línea que contiene Calculated Hardware Clock drift. Esta salida puede decirle si la hora del sistema y la del hardware se desvían una de otra.

```

### timedatectl 

timedatectl - comprobar el estado general de la hora y la fecha, incluyendo si la hora de red se ha sincronizado o no

Por defecto timedatectl devuelve información similar a date, pero con la adición de la hora RTC (hardware) así como el estado del servicio NTP:

```
$ timedatectl
Local time: Thu 2019-12-05 11:08:05 EST -> Hora local
            Universal time: Thu 2019-12-05 16:08:05 UTC -> Hora UTC
                  RTC time: Thu 2019-12-05 16:08:05 -> Hora del reloj de hardware (RTC), normalmente en UTC
                 Time zone: America/Toronto (EST, -0500) -> Zona horaria configurada en el sistema
 System clock synchronized: yes -> Indica si el reloj del sistema está sincronizado correctamente con NTP
               NTP service: active -> Estado del servicio NTP (si está activo o no)
           RTC in local TZ: no -> Indica que el RTC está en UTC (no en hora local)
```


Podemos setear la fecha con timedatectl, como:

```
# timedatectl set-time '2011-11-25 14:00:00'
```

Podemos cambiar la zona horaria (cuando no ahay una interfaz grafica, se hace con este comando)

```
$ timedatectl set-timezone Africa/Cairo

```

Podemos establecer la zona horaria sin usar ` timedatectl `, esto se hace:

Primero buscando la zona horaria de nuestro lugar en el directorio /usr/share/zoneinfo

Estos archivos que hay dentro, tienen reglas necesarias para calcular el desfase de la hora local en relacion con UTC y tambien son importantes si su region hace uso del horario de verano 

Despues crearemos un enlace simbolico llamado localtime en el directorio /etc/ como:

```
ln -s /usr/share/zoneinfo/Canada/Eastern /etc/localtime 
```

Este sera leido cuando linux necesite determinar la zona horaria local

Despues de hacer esto se recomienda ajustar el reloj del hardware desde el reloj del sistema. Esto ajustara el reloj de hardware desde el reloj de sistema (el reloj de tiempo real se ajustara a la misma hora que date )

```
# hwclock --systohc
```


/etc/timezone y  /etc/localtime son similares, es una representacion de datos de la zona horaria local, sin embargo el archivo /etc/timezone puede no existir en todas las distros 


### Desactivar NTP usando timedatectl 

Este servicio se puede desactivar usando systemctl, como es un servicio, sin embargo, lo haremos con timedatectl 

```
$ timedatectl set-ntp no

Para volverlo a activar usamos:

$ timedatectl set-ntp yes

```


### Establecer la fecha y hora sin timedatectl 

Se suele usar systemd para hacer esto, la herramienta es timdatectl, con date o hwclock no se recomienda usar para hacer esta accion, sin embargo es importante conocerlos 

####  Usando date 

se usa la opcion --set o -s para fijar la fecha

```
# date --set="11 Nov 2011 11:11:11"
```

Tambien podemos cambiar la hora o la fecha de forma independiente:

```
Aquí debemos especificar las secuencias para que nuestra cadena sea interpretada correctamente. Por ejemplo, %Y se refiere al año, y así los primeros cuatro dígitos 2011 se interpretarán como el año 2011

# date +%Y%m%d -s "20111125"
```

```
De forma similar %T es la secuencia para el tiempo y se demuestra como:

# date +%T -s "13:11:00"
```



Después de cambiar la hora del sistema, se recomienda también ajustar el reloj del hardware para que ambos relojes, el del sistema y el del hardware, estén sincronizados:

```
# hwclock --systohc

systohc significa “system clock to hardware clock”.
```


#### Usando hwclock 

Podemos cambiarlo como:

```
# hwclock --set --date "4/12/2019 11:15:19"
# hwclock
Fri 12 Apr 2019 6:15:19 AM EST -0.562862 seconds
```

Note que por defecto el hwclock espera la hora UTC, pero devuelve la hora local por defecto.

Después de ajustar el reloj del hardware, tendremos que actualizar el reloj del sistema a partir de este. hctosys significa “hardware clock to system clock”.

```
# hwclock --hctosys
```


# Leccion 2 
- La hora mas precisa se mide con relojes atomicos 
- El protocolo para mantener a todos los sistemas sincronizados con la hora es el protocolo NTP (Protocolo de tiempo de red )

NTP usa estrcutura jerarquica para distribuir la hora. Los relojes de referencia estan conectados a servidores superiores de la jerarquia, Estos serviores son maquinas de estrato 1 (Que no son accesibles al publico, sin embargo las maquinas del estrato 1 son accesibles para el estrato 2, las del estrato 2 son accesibles por las maquinas del estrato 3 y asi sucesivamente)

Buena practica: Cuando se configura NTP para una red grande, es bueno que tengamos un numero pequeño de ordenadores conectados a los servidores de estrado 2+ y hacer que estas maquinas provean a las demas maquinas de la red, de esta manera se minimiza el trafico hacia estos servidores

Conceptos basicos de NTP:

| concepto  | Descripcion | 
|-----------|-------------|
|offset | Se refiere a la diferencia absoluta entre la hora del sistema y la hora NTP. Por ejemplo, si el reloj del sistema marca las 12:00:02 y la hora NTP marca las 11:59:58, el desfase entre los dos relojes es de cuatro segundos. | 
| step | Si el desfase de horario entre el proveedor NTP y un consumidor es superior a 128ms, entonces NTP realizará un único cambio significativo en la hora del sistema, en lugar de atrasar o adelantar la hora del sistema. Esto se llama stepping. | 
| Slew  | Si la diferencia de tiempo entre tu máquina y el servidor NTP es menor a 128 ms, el sistema ajusta el reloj poco a poco (esto se llama slewing)            (Se refiere a los cambios realizados en la hora del sistema cuando el offset entre la hora del sistema y la NTP es inferior a 128 ms. Si este es el caso, los cambios se harán gradualmente. Esto se conoce como slewing) | 
| Insane Time | Si el offset entre la hora del sistema y la hora NTP es superior a 17 minutos, la hora del sistema se considera insane y el demonio NTP no introducirá ningún cambio en la hora del sistema. Habrá que tomar medidas especiales para que la hora del sistema esté dentro de los 17 minutos de la hora correcta  |
| Drift   | Se refiere al fenómeno por el que dos relojes se desincronizan con el tiempo. Esencialmente, si dos relojes están inicialmente sincronizados pero luego se desincronizan con el tiempo, entonces se está produciendo una deriva del reloj |
| Jitter  | La fluctuación se refiere a la cantidad de desviación desde la última vez que se consultó un reloj. Así, si la última sincronización NTP se produjo hace 17 minutos, y el desfase entre el proveedor y el consumidor NTP es de 3 milisegundos, entonces 3 milisegundos es el jitter  |


Si su maquina trabaja con systemd, por defecto implementa un servicio NTP, pero ojo, esto es solo para la maquina, no puede proveer el servicio o servidor de NTP a otros ordenadores

Podemos comprobarlo con:

```
Con systemd

$ systemctl status systemd-timesyncd

O con timedatectl 

$ systemctl status systemd-timesyncd
```

#### Instalar servidor ntp

Para usarlo, instalaremos el paquete ntp 

Comprobar que esta corriendo el servicio 

```
$ systemctl status ntpd

```

Las consultas NTP se realizan en el puerto TCP 123. Si NTP falla, asegurarse de que este puerto esta abierto a la escucha 

**Configuracion NTP**

NTP es capaz de sondear varias fuentes y seleccionar las mejores candidatas para utilizarlas en el ajuste de la hora del sistema. Si se pierde una conexión de red, NTP utiliza los ajustes anteriores de su historial para estimar los ajustes futuros

- /etc/ntp.conf -> Contiene informacion de configuracion sobre como su sistema se sincroniza con la hora de la red 

Los servidores se especifican en una seccion como:

```


# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst

La sintaxis para añadir servidores NTP es la siguiente:

server (IP Address)
server server.url.localhost

```

pool.ntp.org -> Los servidores NTP utilizados por defecto son un proyecto de codigo abierto. Puede encontrar mas informacion en [ntppool.org](https://www.ntppool.org/en/)


**ntpdate**

Durante la configuracion del ntp, puede hacer offset entre la hora del sistema y del NTP, si esto supera los 17 minutos, el demonio NTP no realizara cambios, por lo que debemos de hacerlo de forma manual 

En estos casos, hay que detener el servicio y despues ejecutar: 

```
ntpdate pool.ntp.org
```

Para realizar una unica sincronizacion inicial, donde pool.ntp.org se refiere a la direccion IP o URL de un servidor NTP. (Puede ser necesaria mas de una sincronizacion)


**ntpq**

Una vez realizado lo anterior, podemos ver el estado de los servidores con:

```
ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+37.44.185.42    91.189.94.4      3 u   86  128  377  126.509  -20.398   6.838
+ntp2.0x00.lv    193.204.114.233  2 u   82  128  377  143.885   -8.105   8.478
*inspektor-vlan1 121.131.112.137  2 u   17  128  377  112.878  -23.619   7.959
 b1-66er.matrix. 18.26.4.105      2 u  484  128   10   34.907   -0.811  16.123

```

Donde:
- p imprimira un resumen de los pares
-n Devolvera las direcciones IP

- remote -> Nombre del host del provedor NTP
- refid -> ID de referencia del provedor NTP
- st -> Estrato del provedor
- when -> Numero de segundos desde la utima consulta 
- poll -> Numero de segundos entre consultas 
- reach -> ID de estado para indicar si se ha alcanzado un servidor. Las conexiones exitosas aumentaran este numero en 1 
- delay ->     Tiempo en ms entre la consulta y la respuesta del servidor.
- offset ->  Tiempo en ms entre la hora del sistema y la hora NTP.
- jitter ->    Offset en ms entre la hora del sistema y la NTP en la última consulta

el comando chrony si se ejecuta, entraremos al modo interactivo, para ver los comandos disponibles usamos ? 


#### Instalar servidor Chrony

Es otra forma de implementar NTP (disponible en nuevos sistemas, sin embargo es posible que en algunos sistemas like old school, no esten disponibles, podemos instalarlo con el paquete chrony1)

Despues de instalarlo, podemos ver la informacion sobre el NTP y la hora del sistema con:

```
$ chronyc tracking
Reference ID    : 3265FB3D (bras-vprn-toroon2638w-lp130-11-50-101-251-61.dsl.)
Stratum         : 3
Ref time (UTC)  : Thu Jan 09 19:18:35 2020
System time     : 0.000134029 seconds fast of NTP time
Last offset     : +0.000166506 seconds
RMS offset      : 0.000470712 seconds
Frequency       : 919.818 ppm slow
Residual freq   : +0.078 ppm
Skew            : 0.555 ppm
Root delay      : 0.006151616 seconds
Root dispersion : 0.010947504 seconds
Update interval : 129.8 seconds
Leap status     : Normal
```

- Reference ID::  -> El ID de referencia y el nombre con el que el ordenador está actualmente sincronizado
- Stratum    -> Número de saltos a un ordenador con un reloj de referencia conectado.
- Ref time    -> Es la hora UTC a la que se realizó la última medición de la fuente de referencia.
- System time     -> Retraso del reloj del sistema desde el servidor sincronizado.
- Last offset    -> Offset estimado de la última actualización del reloj.
- RMS offset    -> Promedio a largo plazo del valor de offset.
- Frequency    -> Se trata de la tasa en la que el reloj del sistema pudiera estar incorrecto si el cronyd no lo corrigiera. Se proporciona en ppm (partes por millón).
- Residual freq   ->  Frecuencia residual que indica la diferencia entre las mediciones de la fuente de referencia y la frecuencia que se utiliza actualmente.
- Skew    -> Límite de error estimado de la frecuencia.
- Root delay ->   Total de los retrasos de la ruta de red hacia el ordenador del estrato, desde el que se está sincronizando el ordenador.
- Leap status  ->   Es el estado de salto que puede tener uno de los siguientes valores: normal, insertar segundo, borrar segundo o no sincronizado.

Podemos ver la informacion de tallada sobre la ultima actualizacion NTP valida con:

```
# chrony ntpdata
Remote address  : 172.105.97.111 (AC69616F)
Remote port     : 123
Local address   : 192.168.122.81 (C0A87A51)
Leap status     : Normal
Version         : 4
Mode            : Server
Stratum         : 2
Poll interval   : 6 (64 seconds)
Precision       : -25 (0.000000030 seconds)
Root delay      : 0.000381 seconds
Root dispersion : 0.000092 seconds
Reference ID    : 61B7CE58 ()
Reference time  : Mon Jan 13 21:50:03 2020
Offset          : +0.000491960 seconds
Peer delay      : 0.004312567 seconds
Peer dispersion : 0.000000068 seconds
Response time   : 0.000037078 seconds
Jitter asymmetry: +0.00
NTP tests       : 111 111 1111
Interleaved     : No
Authenticated   : No
TX timestamping : Daemon
RX timestamping : Kernel
Total TX        : 15
Total RX        : 15
Total valid RX  : 15
```


De igual manera podemos ver la info de los servidores NTP usados para sincronizar la hora:

```
$ chronyc sources
210 Number of sources = 0
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
```

La salida anterior refleja que no hay alguna funente configurada, para eso, agregamos fuentes desde pool.ntp.org, abriendo el fichero de configuracion de chromy, el fichero es: /etc/chrony.conf


```
210 Number of sources = 0
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
# Most computers using chrony will send measurement requests to one or
# more 'NTP servers'.  You will probably find that your Internet Service
# Provider or company have one or more NTP servers that you can specify.
# Failing that, there are a lot of public NTP servers.  There is a list
# you can access at http://support.ntp.org/bin/view/Servers/WebHome or
# you can use servers from the 3.arch.pool.ntp.org project.

! server 0.arch.pool.ntp.org iburst iburst
! server 1.arch.pool.ntp.org iburst iburst
! server 2.arch.pool.ntp.org iburst iburst

! pool 3.arch.pool.ntp.org iburst
```

(hay que eliminar los signos `!` para que surja efecto 

El archivo varia segun la distro, pero solo basta con agregarlo al final de este especificando que estos son los servidores



Además, en este archivo podemos elegir cambiar la configuración por defecto en cuanto a skew y drift, así como la ubicación del driftfile y keyfile.

En esta máquina, necesitamos hacer una gran corrección inicial del reloj. Optaremos por descomentar la siguiente línea:

! makestep 1.0 3

Después de realizar los cambios en el archivo de configuración, reinicie el servicio chronyd y utilice chronyc makestep para escalar manualmente el reloj del sistema:

```
# chronyc makestep
200 OK
```

Y a continuación, utilice el chronyc tracking como antes para verificar que los cambios se han producido.
















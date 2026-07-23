# Registro del sistema 

# Leccion 1 

Ademas de systemd-journald, hay 3 servicios principales que manejan registros, estos son:

- Syslog
- syslog-ng (syslog new generation) y 
-rsyslog (the rocket-fast system)

**El rsyslog aporto importantes mejoras como (RELP - Reliable Event Logging Protocol y amplia la funcionalidad del protocolo syslog para proporcionar una entrega de los mensajes) y se ha convertido en la opcion mas popular hoy en dia**

**Dado a que rsyslog esta en las distros principales, se trabajara con este servicio**

Cada uno recoge mensajes de cada servicio y los va alojando en `/var/log`, sin embargo, algunos servicios se ocupan de sus logs como (cups, apache,etc) y estos se encuentran en la carpeta `/var`


#### RSYSLOG

- Usa un modelo cliente-servidor, el cliente y el servidor pueden estar en el mismo host o en diferentes maquinas
- El demonio de rsyslog llamado `rsyslogd`, trabaja junto con klogd (que gestiona los mensajes del kernel) 

Tipos de registro

Se puede clasificar los registros como:
- Registros del sistema 
- Registros de servicio o programa 


- `/var/log/auth.log` contiene todos los registros de autenticacion, como logins, acceso ssh, intentos fallidos, el uso de sudo, etc.

- `/var/log/syslog` -> Archivo centralizado para practicamente todos los registros por rsyslogd, ya que incluye mucha informacion,  los otros servicios se distribuyen de otros archivos de acuerdo a la configuracion suministrada en /etc/rsyslog.conf 

- `/var/log/debug` -> Informacion de depuracion de programas 

- `/var/log/kern.log` -> Mensajes del kernel 

- `/var/log/messages` -> Mensajes ajenos o no relacionados con el kernel. También es el destino por defecto del registro del cliente remoto en una implementación de servidor de registro centralizado

- `/var/log/daemon.log` -> Informacion sobre demonios o servicios ejecutados en segundo plano 

- `/var/log/mail.log` -> Informacion relacionada con el servidor de correo electronico, como postfix

- `/var/log/Xorg.0.log` -> Informacion relacionada con la tarjeta grafica

- `/var/run/utmp and /var/log/wtmp` -> Registros de accesos exitosos

- `/var/log/btmp` -> Intentos fallidos de inicio de sesion, por ejemplo, ataque de fuerza bruta a travez de ssh

- `/var/log/faillog` -> Intentos de autenticacion fallidos 

- `/var/log/lastlog` -> Fecha y hora de los ultimos inicios de sesion de los usuarios 

Algunos ejemplos logs de servicios como:

- `/var/log/cups/` -> Directorio para los registros del Sistema de impresión (Common Unix Printing System). Normalmente incluye los siguientes archivos de registro por defecto: error_log, page_log y access_log

- `/var/log/apache2/ or /var/log/httpd` -> Directorio para los registros del Servidor Web Apache. Normalmente incluye los siguientes archivos de registro por defecto: access.log, error_log, y other_vhosts_access.log

- `/var/log/mysql` -> Directorio para los registros del Sistema de Gestión de Bases de Datos Relacionales MySQL. Suele incluir los siguientes archivos de registro por defecto: error_log, mysql.log y mysql-slow.log

- `/var/log/samba/` -> Directorio para los registros del protocolo Session Message Block (SMB). Suele incluir los siguientes archivos de registro por defecto: log., log.nmbd and log.smbd

**El nombre de los servicios puede variar de cada distribucion. Tambien hay un archivo en /var/packageManager, el cual tiene los logs de instalacion de los paquetes, como pacman, apt, zypper, etc**


#### Leyendo registros 

Se pueden leer con:
- less
- more 
- zless (mismo que less, pero usado para los registros que se comprimen con gzip)
- zmore (mismo que more, pero usado para registros que se comprimen con gzip )
- tail (muestra las ultimas lineas de un arhcivo, por defecto 10 lineas, la opcion -f usada para mostrar dinamicamente las nuevas lineas )
- head (primeras 10 lineas por defecto)
- grep (usado para filtrar lineas )

Las salida de muchos logs se ven como:
- Marca de tiempo
- Nombre del host desde el que se originó el mensaje
- Nombre del programa/servicio que ha generado el mensaje
- El PID del programa que generó el mensaje
- Descripción de la acción realizada

Hay algunos ejemplos en los que los registros no son de TEXTO, sino son archivos BINARIOS, por lo que hay que usar comandos para analiszarlos, como:
- `/var/log/wtmp` -> Usamos who o w
- `/var/log/btmp` -> Usamos utmpdump o last -f 
- `/var/log/faillog` -> Usamos faillog 
- `/var/log/lastlog` -> Usamos lastlog 

De igual manera tambien hay herramientas graficas para la lectura de logs, como `gnome-logs` y `KSystemLog`


#### Como se convierten los mensajes en registros 

1. Las aplicaciones, los servicios y el kernel escriben mensajes en archivos especiales (sockets y buffers de memoria), por ejemplo `/dev/log` o `/dev/kmsg`
2. rsyslogd obtiene la información de los sockets o buffers de memoria.
3. Dependiendo de las reglas encontradas en `/etc/rsyslog.conf` y/o de los archivos en `/etc/ryslog.d/`, rsyslogd mueve la información al archivo de registro correspondiente (típicamente encontrado en /var/log).

**Los sockets son un tipo de archivo especial para trasnferiri informacion entre diferentes procesos, los podemos listar con systemctl list-sockets --all **


#### Facilidad, prioridades y acciones
- El archivo de configuracion de rsyslog es /etc/rsyslog.conf (En algunas otras distribuciones tambien pueden encontrar archivos de configuracion  en `/etc/rsyslog.d/`)
- El archivo de confiruacion se divide en 3 secciones, MODULES, GLOBAL DIRECTIVES y RULES
    - MODULES: Incluye el soporte de modulos para el registro , la capacidad de mensaje y la recepcion de registros UDP/TCP
    - GLOBAL DIRECTIVES: Permite configurar una seria de cosas como los registros y los permisos del directorio de registros 
    - RULES: Es donde entran las facilidades, las prioridades y las acciones. Estas configuraciones le dice al demonio de registro que filtre los mensajes de acuerdo con ciertas reglas y registre o envie vuando sea necesario 


Cada una tiene un propocito y un mensaje que se asocia con un numero y palabra, como:

| Número  | Palabra clave   | Descripción                                  |
| ------- | --------------- | -------------------------------------------- |
| 0       | kern            | Mensajes del kernel de Linux                 |
| 1       | user            | Mensajes a nivel de usuario                  |
| 2       | mail            | Mensajes del sistema de correo               |
| 3       | daemon          | Demonios del sistema                         |
| 4       | auth, authpriv  | Mensajes de seguridad/autorización           |
| 5       | syslog          | Mensajes de syslogd                          |
| 6       | lpr             | Subsistema de impresión de línea             |
| 7       | news            | Mensajes del subsistema de red               |
| 8       | uucp            | Subsistema UUCP (Unix-to-Unix Copy Protocol) |
| 9       | cron            | Demonio del reloj                            |
| 10      | auth, authpriv  | Mensajes de seguridad/autorización           |
| 11      | ftp             | Demonio del FTP                              |
| 12      | ntp             | Demonio del NTP                              |
| 13      | security        | Registros de auditoría                       |
| 14      | console         | Registros de alertas                         |
| 15      | cron            | Demonio del cron                             |
| 16 - 23 | local0 - local7 | Uso local 0 - 7                              |


Ademas, a cada mensaje se le asigna un nivel de prioridad: 

| Código | Severidad     | Palabra clave | Descripción                         |
| ------ | ------------- | ------------- | ----------------------------------- |
| 0      | Emergency     | emerg, panic  | El sistema es inutilizable          |
| 1      | Alert         | alert         | Hay que actuar inmediatamente       |
| 2      | Critical      | crit          | Condiciones críticas                |
| 3      | Error         | err, error    | Condiciones de error                |
| 4      | Warning       | warn, warning | Condiciones de advertencia          |
| 5      | Notice        | notice        | Condición normal pero significativa |
| 6      | Informational | info          | Mensajes informativos               |
| 7      | Debug         | debug         | Mensajes de nivel de depuración     |


Este es un extracto del archivo rsyslog.conf 


```
###############
#### RULES ####
###############

# Primero algunos archivos de registro estándar.  Registro por instalación.
#
auth,authpriv.*                 /var/log/auth.log
*.*;auth,authpriv.none          -/var/log/syslog
#cron.*                         /var/log/cron.log
daemon.*                        -/var/log/daemon.log
kern.*                          -/var/log/kern.log
lpr.*                           -/var/log/lpr.log
mail.*                          -/var/log/mail.log
user.*                          -/var/log/user.log

#
# Registro para el sistema de correo.  Dividirlo para que
# sea fácil escribir scripts para analizar estos archivos.
#
mail.info                       -/var/log/mail.info
mail.warn                       -/var/log/mail.warn
mail.err                        /var/log/mail.err

#
# Algunos archivos de registro "catch-all"
#
*.=debug;\
        auth,authpriv.none;\
	news.none;mail.none     -/var/log/debug
*.=info;*.=notice;*.=warn;\
	auth,authpriv.none;\
	cron,daemon.none;\
	mail,news.none          -/var/log/messages
```


El formato de la regla es el sigueinte: 


El formato de la regla es el siguiente:

```
`<facilidad>.<prioridad>` <acción>
```


Los selectores de `<facilidad>.<prioridad>` filtran los mensajes que deben coincidir 

**Los niveles de prioridad son jerárquicamente inclusivos, lo que significa que rsyslog coincidirá con los mensajes de la prioridad especificada y superiores**

El selector `<accion>` muestra la accion a realizar (donde se envia el mensaje de registro)

Ejemplos: 

```
auth,authpriv.*                     /var/log/auth.log

Los mensajes de seguridad/autorizacion seran enviados a /var/log/auth.log independientemente de su prioridad que esta expresada con un (*)
```

```
*.*;auth,authpriv.none              -/var/log/syslog

Estas son dos sentencias en una, la primera parte:

Nos dice que para cualquier facilidad (*) y de cualquier prioridad (*) las envie al archivo -/var/log/syslog, (el signo menos, significa que evitara excesivamente escrituras en disco )

La segunda parte nos dice:

Que para las facilidades de auth y authpriv se excluiran, esto es lo que significa el sufijo (.none)


Otra explicacion: 

Todos los mensajes — independientemente de su prioridad (*) — de todas las facilidades (*) — descartando los de auth o authpriv (de ahí el sufijo .none) — se escribirán en /var/log/syslog (el signo menos (-) antes de la ruta evita excesivas escrituras en disco). Tenga en cuenta el punto y coma (;) para dividir el selector y la coma (,) para concatenar dos instalaciones en la misma regla (auth,authpriv)

```

```
mail.err                             /var/log/mail.err

Nos dice que la facilidad de correo, con una prioridad de error o superior como (critico, alerta o emergencia), se enviaran al archivo /var/log/mail.err

cuando vemos solo err, o cualquier otro solo, significa que el sistema tomara desde ese 

```

```
*.=debug;\
        auth,authpriv.none;\
	news.none;mail.none     -/var/log/debug

Para evitar recibir mensajes como en el anterior caso, que abarca desde err hasta el primer nivel de prioridad usamos (=).

En este caso, todas las facilidades que tengan, de prioridad IGUAL y solamente igual a debug, seran enviadas a -/var/log/debug, evitando la sobrecarga de escritura del disco 

;\ es igual a ;, sin embargo la primera nos permite usar mas lineas 

los demas mensajes seran evitados escribirse en el archivo, ya que tienen el profijo de .none 


otra explicacion: 

Los mensajes de todas las instalaciones con la prioridad debug y ninguna otra (=) se escribirán en /var/log/debug — excluyendo cualquier mensaje procedente de las instalaciones auth, authpriv, news y mail (nótese la sintaxis: ;\).

```

#### Entradas manuales en el registro del sistema: logger 

logger - es un comando que nos permite agregar registros de mensajes que reciba /var/log/syslog

Ejemplo: 

```
carol@debian:~$ logger this comment goes into "/var/log/syslog"
```

#### rsyslog como servidor central de registros 









# Leccion 2

















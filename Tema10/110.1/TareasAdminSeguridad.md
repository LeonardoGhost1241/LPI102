# Tareas de administracion de seguridad

## Comprobacion de archivos con el conjunto SUID y SGID

El bit SUID permite que el fichero se ejecute con los privilegios del propietario del archivo. Se representa numericamente por 4000 y simbolicamente por s o S en el bit de permiso execute del propietario

Ejemplo:

```
carol@debian:~$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 63736 jul 27  2018 /usr/bin/passwd
```

La s minúscula en rws indica la presencia del SUID en el archivo, junto con el permiso de ejecución. Una S mayúscula en su lugar (rwS) significaría que el permiso execute subyacente no está establecido.


Por otro lado el bit SGID puede establecerse tanto en archivos como en directorios 

En el caso de los archivos, su comportamiento es equivalente al de SUID, pero los privilegios son los del propietarios del grupo

Sin embargo cuando se establece en un directorio, permitira que todos los archivos creados en el hereden la propiedad del grupo del directorio

Al igual que SUID, SGID se representa simbolicamente por s o S en el bit de permiso execute del grupo. Numericamente se representa por 2000. Puede establecer el SGID en un directorio utilizando chmod 


Para buscar archivos con uno o ambos conjunots de SUID y SGID puede usar el comando find y hacer uso de perm, como:


```
-perm numeric-value o -perm symbolic-value  -> Encontrar archivos que tengan el permiso especial exclusively

-perm -numeric-value o -perm -symbolic-value -> Buscar archivos que tengan el permiso especial y otros permisos

-perm /numeric-value o -perm /symbolic-value -> Encontrar archivos que tengan alguno de los permisos especiales (y otros permisos)

```

## Gestion y caducidad de contraseñas 
passwd se usa para cambiar la contraseña de un usuario normal
    -S o --status para obtener la informacion sobre el estado de su cuente 


```
carol@debian:~$ passwd -S
carol P 12/07/2019 0 99999 7 -1
```

carol: Nombre de acceso del usuario
P: Indica que el usuario tiene una contraseña válida (P); otros valores posibles son L para una contraseña bloqueada y NP para ninguna contraseña
12/07/2019: Fecha del último cambio de contraseña.
0: Edad mínima en días (el número mínimo de días entre cambios de contraseña). Un valor de 0 significa que la contraseña puede cambiarse en cualquier momento.
99999: Edad máxima en días (el número máximo de días que la contraseña es válida). Un valor de 99999 desactivará la caducidad de la contraseña
7: Período de advertencia en días (el número de días antes de la expiración de la contraseña que un usuario será advertido).
-1: Periodo de inactividad de la contraseña en días (el número de días inactivos después de la expiración de la contraseña antes de que la cuenta se bloquee). Un valor de -1 eliminará la inactividad de una cuenta

El comando passwd como root se puede usar para 

-l Bloquear cuentas de usuario
-u Desbloquear cuentas 
-e Forzar a un usuario a cambiar su contraseña en el siguiente inicio de sesion
-d Eliminar la contraseña de un usuario 

Otras opciones para bloquear o desbloquear es 

```
-- bloquear --
usermod -L carol 
usermod --lock carol

-- desbloquear --
usermod -U carol
usermod --unlock carol 

Las opciones -f o --inactive, usermod tambien puede utilizarse para establecer el numero de dias antes de que se desactive una cuenta con una contraseña expirada ejem:
usermod -f 3 carol 
```

Ademas de usermod o passwd, **EL COMANDO MAS DIRECTO** para tratar la caducidad de contraseñas y cuentas es `chage`

-l o --list seguido del nombre para ver el estado actual de la contraseña (como usuario normal puede ver solo su propia informacion)

Sin opciones, chage se comportara de forma interactiva 

Las opciones para modificar diferentes ajustes de chage son las siguientes:

```
-m days username o --mindays days username
Especifica el número mínimo de días entre cambios de contraseña (por ejemplo: chage -m 5 carol). Un valor de 0 permitirá al usuario cambiar su contraseña en cualquier momento.

-M days username o --maxdays days username
Especifica el número máximo de días que la contraseña será válida (por ejemplo: chage -M 30 carol). Para desactivar la caducidad de la contraseña, es habitual dar a esta opción un valor de 99999.

-d days username o --lastday days username
Especifica el número de días desde que la contraseña fue cambiada por última vez (por ejemplo: chage -d 10 carol). Un valor de 0 obligará al usuario a cambiar su contraseña en el siguiente inicio de sesión.

-W days username o --warndays days username
Especifica el número de días que se le recordará al usuario que su contraseña ha caducado.

-I days username o --inactive days username
Especifica el número de días inactivos después de la expiración de la contraseña (por ejemplo: chage -I 10 carol) — lo mismo que usermod -f o usermod --inactive. Una vez que haya pasado ese número de días, la cuenta se bloqueará. Sin embargo, con un valor de 0, la cuenta no se bloqueará.

-E date username o --expiredate date username
Especifica la fecha (o el número de días desde la época — el 1 de enero de 1970) en la que se bloqueará la cuenta. Normalmente se expresa en el formato YYYY-MM-DD (por ejemplo: chage -E 2050-12-13 carol)

```


## Descubrir puertos abiertos 

Cuando se trata de vigilar los puertos abiertos, hay 4 utilidades presentes
- lsof
- fuser
- netstat o ss
- nmap 

### lsof
Significa "listart archivos abiertos", esta es una herramienta de investigacion, partes de un recurso que te interesa (archivo, puerto, proceso, usuario) y descubres que proceso lo tiene abierto

Para imprimir el listado de todos los archivos de red de "internet", ejecutamos:

```
lsof -i 
```

Podemos ver los archivos que estan abiertos remotamente de una determinada ip con el siguiente comando

```
root@debian:~# lsof -i@192.168.1.7
COMMAND PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    557  root    3u  IPv4  14701      0t0  TCP 192.168.1.7:ssh->192.168.1.4:60510 (ESTABLISHED)
sshd    569 carol    3u  IPv4  14701      0t0  TCP 192.168.1.7:ssh->192.168.1.4:60510 (ESTABLISHED)
```

y para imprimir solo los archivos de red IPv4 e IPv6, usamos las opciones `-i4` y `-i6` respectivamente


Puede imprimir por ip (-i@ip-address) o por puerto, como:

```
root@debian:~# lsof -i :22
COMMAND PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    389  root    3u  IPv4  13689      0t0  TCP *:ssh (LISTEN)
sshd    389  root    4u  IPv6  13700      0t0  TCP *:ssh (LISTEN)
sshd    557  root    3u  IPv4  14701      0t0  TCP 192.168.1.7:ssh->192.168.1.4:60510 (ESTABLISHED)
sshd    569 carol    3u  IPv4  14701      0t0  TCP 192.168.1.7:ssh->192.168.1.4:60510 (ESTABLISHED)

```

Los puertos multiples se separan con comas (y los rangos se especifican con un guion)

```
root@debian:~# lsof -i@192.168.1.7:22,80
COMMAND PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    705  root    3u  IPv4  13960      0t0  TCP 192.168.1.7:ssh->192.168.1.4:44766 (ESTABLISHED)
sshd    718 carol    3u  IPv4  13960      0t0  TCP 192.168.1.7:ssh->192.168.1.4:44766 (ESTABLISHED)
```

En resumen: 

- Ver que proceso tiene abierto un documento
¿Que proceso tienen abierto `/var/log/syslog`?

```
lsof /var/log/syslog
```

- Ver los archivos abiertos de un usuario 

```
lsof -u user

si queremos matar todos sus proceso pordemos hacer

kill -9 $(lsof -u user)

```

- Archivos abiertos relacionados con conexiones de red

```
lsof -i 

Podemos buscar tambien un puerto en especifico

lsof -i :22
lsof -i :443
```

- Ver solo las conexiones de un tipo

```
lsof -iTCP

para ver su estado

lsof -i TCP -s :ESTABLISHED
```

- Buscar por proceso

```
lsof -c sshd 
```


- Buscar por PID

```
lsof -p 1234
```


**Diferencia importante entre ss y lsof**: 

`ss` pregunta --> Que sockets/conexiones existen??

mientras que 

`lsof` pregunta --> Que proceso tiene abierto este recurso/socket??


### fuser
Su proposito principal es encontrar el "usuario de un fichero", lo que implica saber que procesos estan accediendo a que ficheros, directorio o sockets

Para obtener un poco mas de informacion, conviene usar ( -v o --verbose)

```
root@debian:~# fuser .
/root:                 580c
root@debian:~# fuser -v .
                     USER        PID ACCESS COMMAND
/root:               root        580 ..c.. bash
```

Explicacion de la salida

- File: El archivo del que estamos obtenido informacion (/root)
- User: El propietario del fichero (root)
- PID: El identificador del proceso (580)
- Access: Tipo de acceso (..c..), Uno de: c Directorio Actual, e Ejecutables que se llevan acabo, f Abrir archivo (se omite en el modo de visualizacion por defecto), F Abrir archivo para escribir (se omite en el modo de visualizacion por defecto), r Directorio raiz, m archivo mmap'ed o biblioteca compartida, . Marcador de posicion (omitido en el modo de visualizacion por defecto)
- Command: El comando afiliado al archivo (bash)

Con la opcion -n (o --namespace), puede encontrar informacion sobre los puertos/sockets 


fuser también se puede utilizar para matar los procesos que acceden al archivo con las opciones -k o --kill (por ejemplo: fuser -k 80/tcp)


Resumen de fuser y lsof 

- lsof (List open files): Orientado al proceso, responde ¿Que archivos, sockets o bibliotecas tiene abiertos este proceso (el el sistema)?

- fuser (File user): Orientado al recurso, Responde a ¿Que procesos estan utilizando este archivo, directorio, puerto o punto de montaje en particular?


| Caso de uso | Comando recomendado | Razon |
|-------------|---------------------|-------|
|Desmontar un disco atascado | fuser| fuser -m /mnt/data te da directamente los PIDd que bloquean el desmontaje|
|Matar rapido procesos de un puerto | fuser| fuser -k 8080/tcp envia un SIGKILL a todos los procesos en ese puerto en un solo paso|
| Inspeccionar conexiones de red abiertas | lsof | lsof -i :80 te muestra el usuario, el protocolo, el PID y la direccion IP remota |
| Ver todo lo que un proceso especifico tiene abierto | lsof | lsof -p <PID> lista ejecutables, bibliotecas .so, sockets y archivos de configuracion en uso |





### netstat
Se utiliza para imprimir "estadisticas de red"

Sin opciones, netstat mostrara tanto las conexiones activas a internet como los sockets de Unix

Se usa:
-l o --listenig para ver los puertos y sockets en "escucha"

-t/--tcp y -u/--udp para filtrar entre tcp o udp

-e/--extend mostrara informacion adicional 

-n/--numeric Para imprimir solo los numeros de puerto y las direcciones IP

### nmap (network mapper)

Este escaner de puertos se ejecuta especificando una direccion IP o un noombre de host:

```
namp localhost

Starting Nmap 7.70 ( https://nmap.org ) at 2020-06-04 19:29 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

A parte de un solo host,nmap le permite escanear

- Múltiples hosts: Separándolos con espacios (por ejemplo: nmap localhost 192.168.1.7).
- Rangos de hosts: Utilizando un guión (por ejemplo: nmap 192.168.1.3-20).
- Subredes: Utilizando un comodín o una notación CIDR (por ejemplo: nmap 192.168.1.* o nmap 192.168.1.0/24). Puede excluir determinados hosts (por ejemplo: nmap 192.168.1.0/24 --exclude 192.168.1.7)

Para escanear un puerto, usamos -p, por lo que nmap -p 22 y nmap -p ssh daran el mismo resultado, o tambien nmap -p 22,telnet,http localhost

Otras opciones importantes son:

- -F Ejecuta un escaneo rapido en los 100 puertos mas comunes
- -v Obtiene una salida mas detallada (-vv imprimira una salida con mas informacion)

### Resumen 

lsof --> Muestra una,lista detallada de todos los archivos abiertos y los procesos que lo utilizan, responde a ¿Que arhcivos tiene abiertos ESTE proceso o el sistema?

fuser --> Identifica que procesos estan utilizando archivos, sockets o sistemas de archivos especificos, responde a ¿Que procesos estan usando ESTE recurso?


## Limites en los inicios de sesions de los usuarios, los procesos y el uso de la memoria 
Los recursos de un sistema linux  no son ilimitados, por lo que como administrador del sistema, debes asegurr un buen equilibrio entre los limites de los usuarios sobre los recursos y el correcto funcionamiento del sistema operativo. `ulimit` puede ayudar en este sentido

limite se ocupa de los limites soft y hard, especificados por las opciones -S y -H respectivamente

Si se ejecuta sin opciones, ni argumentos, mostrara los bloques de archivos con limites flexibles del usuario actual

```
carol@debian:~$ ulimit
unlimited
```

con la opcion -a mostrara todas las opciones disponibles que tenemos para cambiar

Nota:Todos los valores que cambiaremos y que se muestran con -a seran los limites soft, ya que los limites hard solo los puede cambiar(aumentar) el usuario root, por lo que concluimos que tanto -a como -Sa daran el mismo resultado. Si no se especifica si es un limite soft (-S) o hard(-H), el cambio se guardara para ambas clasificaciones. Los usuarios regulares pueden disminuir los limites estrictos y aumentar los limites flexibles hasta el valor de los limites duros 

Para que estos valores sean persistentes a travez de los reinicios, debe establecerlos en el archivo `/etc/security/limits.conf`, este archivo tambien es utilizado por el administrador para aplicar restricciones a determinados usuarios

Para este comando no hay una pagina man, ya que es una integracion de bash, asi que tenemos que consultar la pagina man de bash para aprender sobre este 


## Tratar con usuarios registrados 
Como usuario administrador, implica llevar un registro de los usuarios conectados, para ello, hay tres utilidades, como: last, who y w

### last
Esta herramienta muestra informacion sobre las ultimas sesiones de inicio de sesion de los usuarios del sistema, muy util cuando necesitas rastrear la actividad del usuario del sistema o investigar alguna posible violacion de seguridad

Ejemplo:

```
root@debian:~# last
carol    pts/0        192.168.1.4      Sat Jun  6 14:25   still logged in
reboot   system boot  4.19.0-9-amd64   Sat Jun  6 14:24   still running
mimi     pts/0        192.168.1.4      Sat Jun  6 12:07 - 14:24  (02:16)
reboot   system boot  4.19.0-9-amd64   Sat Jun  6 12:07 - 14:24  (02:17)
(...)
wtmp begins Sun May 31 14:14:58 2020
```

Considerando la anterior salida, obtenemos la informacion sobre los dos ultimos usuarios del sistema. Las dos primeras nos hablan del usuario carol, las dos siguientes del usuario mimi, la informacion es la siguiente:


1. El usuario carol en la terminal pts/0 desde el host 192.168.1.4 inicio su sesion el sabado 6 de junio a las 14:25 y todavia esta conectada.  El sistema, que utiliza el kernel 4.19.0-9-amd64, se inició (reboot system boot) el sábado 6 de junio a las 14:24 y sigue funcionando.

2. El usuario mimi en la terminal pts/0 desde el host 192.168.1.4 inició su sesión el sábado 6 de junio a las 12:07 y cerró la sesión a las 14:24 (la sesión duró un total de (02:16) horas). El sistema que utiliza el kernel 4.19.0-9-amd64, se inició (reboot system boot) el sábado 6 de junio a las 12:07 y se apagó a las 14:24 (estuvo funcionando durante (02:17) horas)


**Nota: La linea wtmp begins Sun May 31 14:14:58 2020 se refiere a /var/log/wtmp, que es el archivo de registro especial del que last obtiene la informacion**

Para ver la informacion de este arhcivo, usaremos el comando `utmpdump`

Podemos filtrar para solo a un usuario para que muestre sus entradas, porejemplo:

```
last carol

root@debian:~# last carol
carol    pts/0        192.168.1.4      Sat Jun  6 14:25   still logged in
carol    pts/0        192.168.1.4      Sat Jun  6 12:07 - 14:24  (02:16)
carol    pts/0        192.168.1.4      Fri Jun  5 00:48 - 01:28  (00:39)
(...)
```

En cuanto a la segunda columna (terminal), pts significa Pseudo Terminal Slave - en contraposición a un terminal TeleTYpewriter o tty propiamente dicho; 0 se refiere al primero (la cuenta comienza en cero)

Para ver los intentos fallidos del sistema usaremos la siguiente herraminta:

```
lastb
```


Las utilidades who y w se centran en usuarios conectado, sin embargo la primera muestra quien esta conectado, mientras que la segunda tambien muestra info de lo que estan haciendo

### who

```
root@debian:~# who
carol    pts/0        2020-06-06 17:16 (192.168.1.4)
mimi     pts/1        2020-06-06 17:28 (192.168.1.4)
```

Algunas de las opciones que podemos usar son:

-b,--boot: Muestra la hora del ultimo arranque del sistema

-r,--runlevel: Muestra el nivel de ejecucion actual

-H,--heading: Imprime los titulos de las columnas

### w
Este comando muestra una salida mas detallada

```
root@debian:~# w
 17:56:12 up 40 min,  2 users,  load average: 0.04, 0.12, 0.09
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
carol    pts/0    192.168.1.4    17:16    1.00s  0.15s  0.05s sshd: carol [priv]
mimi     pts/1    192.168.1.4    17:28   15:08   0.05s  0.05s -bash
```

La línea superior ofrece información sobre la hora actual (17:56:12), el tiempo que lleva el sistema en funcionamiento (up 40 min), el número de usuarios conectados en ese momento (2 usuarios) y los números de la media de carga (media de carga: 0,04, 0,12, 0,09). Estos valores se refieren al número de trabajos en la cola de ejecución promediados en los últimos 1, 5 y 15 minutos, respectivamente

USER: Nombre de inicio de sesión del usuario.

TTY: Nombre del terminal en el que se encuentra el usuario.

FROM: Host remoto desde el que el usuario se ha conectado.

LOGIN@: Hora de inicio de sesión.

IDLE: Tiempo de inactividad.

JCPU: Tiempo utilizado por todos los procesos conectados a la tty (incluidos los trabajos en segundo plano que se están ejecutando actualmente).

PCPU: Tiempo utilizado por el proceso actual (el que se muestra bajo WHAT).

WHAT: Línea de comandos del proceso actual

Al igual que con `who`, puede pasarle nombres de usuario `w`:

```
root@debian:~# w mimi
 18:23:15 up  1:07,  2 users,  load average: 0.00, 0.02, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
mimi     pts/1    192.168.1.4      17:28    9:23   0.06s  0.06s -bash
```

## Configuracion y uso basico de `sudo`


Con este comando, se puede ejecutar un comando como usuario root o cualquier otro usuario, Desde una perspectiva de seguridad, sugo es una opcion mucho mejor que su ya que prsenta dos ventajas:

1. Para ejecutar un comando como root, no se necesita la contraseña del usuario root, sino sólo la del usuario que lo invoca en cumplimiento de una política de seguridad. La política de seguridad por defecto es sudoers como se especifica en /etc/sudoers y /etc/sudoers.d/*.

2. sudo le permite ejecutar comandos individuales con privilegios elevados en lugar de lanzar un nuevo subshell para root como hace su.

La forma mas basica para usar el comando sudo es:

sudo -u user command, ejemplo:

```
carol@debian:~$ sudo -u mimi whoami
mimi
carol@debian:~$ sudo whoami
root
```

**Nota: sudoers utilizará una marca de tiempo por usuario (y por terminal) para el almacenamiento en caché de las credenciales, de forma que pueda utilizar sudo sin contraseña durante un período por defecto de quince minutos. Este valor por defecto puede ser modificado añadiendo la opción timestamp_timeout como un ajuste Defaults en /etc/sudoers (por ejemplo Defaults timestamp_timeout=1 establecerá el tiempo de espera de la caché de credenciales en un minuto).**


## Archivo `/etc/sudoers`

El archivo de configuración principal de sudo es /etc/sudoers (también existe el directorio /etc/sudoers.d

En este arhcivo se especifica quien puede ejecutar que comandos como que usuarios en que maquinas, asi como otras configuraciones, la sintaxis es:

```
carol@debian:~$ sudo less /etc/sudoers
(...)
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
(...)
```

La especificacion de privilegios para el usuario root es ALL=(ALL:ALL) ALL

Esto se traduce como: El usuario root puede iniciar sesion desde todas las maquinas (ALL), como todos los usuarios y todos los grupos ((ALL:ALL)) y ejecutar todos los comandos (ALL)

Lo mismo ocurre con el grupo sudo, el cual se denota como 

%sudo 

Con la sintaxis

%grupo_nombre

Ejemplo: Para que el usuario carol pueda comprobar el estado de `apache` desde cualquier host como cualquier usuario o grupo, añadira la siguiente linea en el fichero sudoers

```
carol ALL=(ALL:ALL) /usr/bin/systemctl status apache2 
```

Para que no pida contraseña al ejecutar el comando hacemos: 

```
carol ALL=(ALL:ALL) NOPASSWD: /usr/bin/systemctl status apache2
```


Si ahora queremos restringir los host a 192.168.1.7 y permitir que caro ejecute el comando anterior como usuario `mimi`, agregaremos lo siguiente

```
carol 192.168.1.7=(mimi) /usr/bin/systemctl status apache2
```

Por lo que ahora ejecutaremos el comando: 

```
carol@debian:~$ sudo -u mimi systemctl status apache2
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2020-06-09 13:12:19 CEST; 29min ago
(...)
```

En dado caso de que carol fuese promovida y quisieramos darle TODOS los privilegios, el enfoque seria mas facil con solo incluirla en el grupo especial `sudo` con:

```
sudo useradd -aG sudo carol
```

**Nota: En distribuciones como Red Hat el grupo sera wheel, que es la contrapartida del grupo administrativo especial sudo de los sistemas Debian**

En lugar de editar directamente el archivo `/etc/sudoers`, debemos de usar el comando visudo , el cual abrira el archivo con el editor de texto predefinido

**Como alternativa, puede especificar un editor de texto, a traves de la variable de entorno, EDITOR cuando usemos visudo, como: export EDITOR=$(which vim)**


Aparte de los usuarios y grupos, tambien puede hacer uso de los alias en  `/etc/sudoers`, hay tres categorias principales:

1. Alias de host (Host_Alias)
2. Alias de usuario (User_Alias)
3. Alias de comando (Cmnd_Alias)

Aqui hay un ejemplo:

```
# Especificación de alias de host

Host_Alias SERVERS = 192.168.1.7, server1, server2

# Especificación de alias de usuario

User_Alias REGULAR_USERS = john, mary, alex

User_Alias PRIVILEGED_USERS = mimi

User_Alias ADMINS = carol, %sudo, PRIVILEGED_USERS, !REGULAR_USERS

# Especificación de alias de Cmnd

Cmnd_Alias SERVICES = /usr/bin/systemctl *

# Especificación de los privilegios del usuario
root    ALL=(ALL:ALL) ALL
ADMINS  SERVERS=SERVICES

# Permitir a los miembros del grupo sudo ejecutar cualquier comando
%sudo   ALL=(ALL:ALL) ALL
```


Teniendo en cuentra este archivo, vamos a explicar los tres tipos de alias:

- `Host aliases`: Incluye una lista separada por comas de nombres de host, direcciones IP, asi como redes y netgroups (precedidos por +). Tambien se pueden especificar mascaras de red. Alias de host Servers incluye una direccion IP y dos nombres de host:

```
Host_Alias SERVERS = 192.168.1.7, server1, server2
```

- `User aliases`: Incluye una lista separa por comas de usuarios especificados como nombres de usuario, grupos (precedido por %) y netgroups (precedidos por +). Se pueden excluir usuarios concretos con !. El alias de usuario ADMINS - por ejemplo - incluye al usuario carol, los miembros del grupo sudo y aquellos miembros del alias de usuario PRIVILEGE_USERS que no pertenecen al alias de usuario REGULAR_USERS:

```
User_Alias ADMINS = carol, %sudo, PRIVILEGED_USERS, !REGULAR_USERS
```

- `Command aliases` Incluyen una lista de comandos y dirdctorios separados por comas. Si se especifica un directorio, se incluira cualquier archivo de ese directorio, aunque se ignoraran los subdirectorios. El alias de comando `Services` incluye un solo comando con todos sus subcomandos - segun lo especificado por el asterisco (*)

```
Cmnd_Alias SERVICES = /usr/bin/systemctl *
```

Como resultado de las especificaciones de alias, la línea ADMINS SERVERS=SERVICES bajo la sección Especificación de privilegios del usuario se traduce como: todos los usuarios pertenecientes a ADMINS pueden usar sudo para ejecutar cualquier comando en SERVICES en cualquier servidor en SERVERS








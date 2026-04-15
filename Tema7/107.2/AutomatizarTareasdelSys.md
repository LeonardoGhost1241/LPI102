# Automatizar tareas administrativas del sistema mediante la programacion de trabajos 

## Leccion 1
### Programar trabajos con CRON 
Cron es un demonio que se ejecute continuamente y se activa cada minuto la tabla de tareas, estas tablas se conocen como crontabs y contienen llamadas a cron jobs 

Puede ser utilizado por usuarios ordinarios, cada uno de los cuales tiene su propio crontab, cada uno de los cuales tiene su propio crontab, asi como el usuario root que gestiona los crontabs del sistema  

### Crontabs de usuario
Siempre tienen el nombre de la cuenta de usuario que los creo, pero la ubicacion de estos archivos depende de la distribucion utilizada (generalmente un subdirectorio de /var/spool/cron)

cada linea de un crontab de usuario contiene sesi campos separados por un espacio:
- El minuto de la hora (0-59)
- La hora del dia (0-23)
- El dia del mes (1-31)
- El mes del año (1-12)
- El dia de la semana (0-7 con domingo=0 o domingo=7)
- La orden a ejecutar 

Para el mes del año, el dia de la semana, puede usar las tres primeras letras del no,bre en lugar de un numero 


Los primeros 5 campos, indican el CUANDO de lo que va a ejecutar, se pueden especificar algunos de estos valores tambien:

* (asterisco)  -> Se refiere a cualquier valor 
, (coma) -> Especifica una lista de posibles valores
- (guion) -> Especifica un rango de valores posibles
/ (slash) -> Especifica valores escalonados 

Muchas distros incluyen el archivo /etc/crontab, que puede ser usado como referencia para la disposicion de un archivo cron 

ejemplo del archivoi crontab

```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

### Crontabs de sistema 

Estos archivos, son los que gestionan la programacion de los trabajos del cron del sistema y pueden ser editados por el usuario root.
El archivo /etc/crontab y todos los que se encuentran en el directorio /etc/cron.d son cronabs del sistema 

La mayoria de las distribuciones incluyen los directorios 
- /etc/cron.hourly
- /etc/cron.daily
- /etc/cron.weekly
- /etc/cron.monthly

Que son scrips para su ejecucion frecuente, si queremos ejecutar algo de forma diaria, lo colocaremos en el archivo cron.daily

La sintaxis es similar a la del cron de usuario, pero este tiene un campo extra, este sera el usuario que ejecutara el trabajo de cron

- El minuto de la hora (0-59).
- La hora del día (0-23).
- El día del mes (1-31).
- El mes del año (1-12).
- El día de la semana (0-7 con domingo=0 o domingo=7).
- El nombre de la cuenta de usuario que se utilizará al ejecutar el comando.
- El comando a ejecutar.


Al igual que el anterior formato, puede especificar con multiples valores con *,,,/,etc ademas de usar las tres primeras letras para especificar un dia 


### Esecificaciones del tiempo particulares
Tambie podemos usar atajos especiales para los 5 campos  de las primeras columnas


- @reboot
Ejecuta la tarea una vez reiniciando el equipo

- @hourly
Ejecuta la tarea especificada una vez por hora al iniciar

- @daily (o @midnight)
Ejecuta la tarea especificada una vez al dia a medianoche 

- @weekly
Ejecuta la tarea especificada una vez a la semana a medianoche del domingo

- @monthly (o @annually)
Ejecutar la tarea especificada una vez  al año de medianoche del 1 de enero

- @yearly (o @annualy)
Ejecutar la tarea especificada de una vez al año a medianoche 

### Variables crontab 
Dentro del archivo crontab hay variables definidad antes de que se declaren las tareas programadas, las variables de entorno establecidas (comunmente) son:

- HOME
El directorio donde cron invoca los comandos (por defecto el directorio principal del usuario)

- MAILTO 
EL nombre del usuario o la direccion a la que se envia la salida estandar y el error (Por defecto al propietario del crontab), Tambien se permiten multiples valores separados por comas y un valor vacio indica que no se debe enviar ningun correo

- PATH 
La ubicacion de los comandos en los sistemas de archivo 

- SHELL
El shell a usar (por defecto /bin/sh)


### Crear trabajos en un cron de usuario 
crontab  - se usa para mantener o crear archivos crontab INDIVIDUALES
    -e Permite ejecutar el comando crontab -e para editar su propiio archivo crontab o para crear uno si aun no existe 
    -l muestra el crontab actual en la salida estandar
    -r Quita el crontab actual
    -u Especifica el nombre de usuario cuyo crontab necesita ser modificado. Esta opcion requiere privilegios de root y permite que el usuario root edite los arhcivos crontab de otro usuario 

Por defecto abre el editor especificado por las variables de entorno VISUAL o EDITOR 

Algunas distribuciones le permiten elegir el editor de una lista cuando crontab se ejecuta por primera vez

ejemplo: el script foo.sh ubicado en su directorio principal todos los dias a las 10:00 am, puede agregar la siguiente linea a su arhcivo crontab:

0 10 * * * /home/frank/foo.sh

otros ejemplos:
0,15,30,45 08 * * 2 /home/frank/bar.sh
30 20 1-15 1,6 1-5 /home/frank/foobar.sh

En la primera línea, el script bar.sh se ejecuta todos los martes a las 08:00 am, a las 08:15 am, a las 08:30 am y a las 08:45 am. En la segunda, línea el script foobar.sh se ejecuta a las 08:30 pm de lunes a viernes durante los primeros quince días de enero y junio

**aunque se pueden editar de forma manual, es recomendado siempre usar el comando crontab. Los permisos de archivos crontab solo permiten su edicion mediante el comando crontab**

### Crear crones de sistema
A diferencia de los crones del usuario, estos se editan con un editor de texto, por lo que no es necesario usar el comando crontab para editar /etc/crontab y los archivos en  /etc/cron.d.

Cuando se especifica una tarea debe de especificar tambien el usuario, normalmente sera root 

ejemplo: 
30 01 * * * root /root/barfoo.sh >>/root/output.log 2>>/root/error.log

Se ejecutara a la 1:30 am, todos los dias del mes, todos los años y todos los dias de la seman el comando barfoo.sh dentro del directorio root, y se redirigira la salida al arhcivo output.log y ademas los errores ira a eror.log 


**A menos que la salida sea redirigida a un archivo como en el ejemplo anterior (o que la variable MAILTO se establezca en un valor vacío), toda la salida de un trabajo cron será enviada al usuario por correo electrónico. Una práctica común es redirigir la salida estándar a /dev/null (o a un archivo para su posterior revisión si es necesario) y no redirigir el error estándar. De esta manera el usuario será notificado inmediatamente por correo electrónico de cualquier error.**



### Configurar el acceso a la programacion de tareas 
- /etc/cron.allow 
- /etc/cron.deny

Son usados para establecer las restricciones crontab, se usan para permitir o denegar la programacion de trabajos cron para diferentes usuarios

**Si /etc/cron.allow no existe pero /etc/cron.deny existe, sólo los usuarios no root listados dentro de este archivo no pueden programar trabajos cron usando el comando crontab (en este caso un /etc/cron.deny vacío significa que a cada usuario se le permite programar trabajos cron con crontab). Si no existe ninguno de estos archivos, el acceso del usuario a la programación de trabajos cron dependerá de la distribución utilizada.**

**La lista de estos archivos, contiene los nombres de los usuarios, cada uno en una linea separada**





### Alternativa a CRON

Podemos usar systemd para programar tareas del sistema, estos son llamadaso timers, ya que su extension es .timer

Por defecto, un timer activa un servicio con el mismo nombre, exepto por el sufijo


Un temporizador incluye una seccion de [Timer] que especifica cuando debe de ejecutarse los trabajos programados 
Especificamente puede usar la opcion OnCalendar= para definir temporizadores en tiempo real que funcionan de la misma manera que los trabajos cron (basados en expresiones de eventos de calendario), esto requiere de la siguiente sintaxis 

```
DayOfWeek Year-Month-Day Hour:Minute:Second
```

Siendo DayOfWeek opcional

Los operadores *, / y , tienen el mismo significado que los usados para los trabajos de cron, mientras que puede usar .. entre dos valores para indicar un rango contiguo. En cron usabamos 1-5, aqui sera 1..5
Tambien puede usar las tres primeras letras del nombre o el nombre completo

Se pueden definir temporizadores monotonos que se activan despues de trascurrir un tiempo desde un punto de inicio especifico (Por ejemplo, cuando inicia maquina o cuando se activa el propio temporizador )

Por ejemplo, si se quiere ejecutar el sevicio /etc/systemd/system/foobar.service a las 05:30 del primer lunes de cada mes, se agrega lo siguiente al archivo /etc/systemd/system/foobar.timer

```
[Unit]
Description=Run the foobar service

[Timer]
OnCalendar=Mon *-*-1..7 05:30:00
Persistent=true

[Install]
WantedBy=timers.target
```


Una vez que haya creado el nuevo temporizador, puede activarlo e iniciarlo ejecutando los siguientes comandos como root:

```
# systemctl enable foobar.timer
# systemctl start foobar.timer
```


Puede ver la lista de temporizadores ACTIVOS con: systemctl list-timers, si quiere ver los inactivos tambien, puede agregar --all

**Recuerda que los temporizadores se registran en el diario del sistema (system journal) y puede revisar los registros de las diferentes unidades usando el comando "journalalctl". Recuerde también que si esta utilizando un usuario ordinario, necesita usar la opción "user" de los comandos "systemctl" y "journalctl"**




En lugar de la formas mencionadas anteriormente, se pueden utilizar algunas expresiones especiales que describen frecuencias particulares para la ejecución del trabajo:

hourly    Ejecutar la tarea especificada una vez por hora al comienzo de la hora.
daily    Ejecutar la tarea especificada una vez al día a medianoche.
weekly    Ejecutar la tarea especificada una vez a la semana a medianoche del lunes.
monthly    Ejecutar la tarea especificada una vez al mes a la medianoche del primer día del mes.
yearly    Ejecutar la tarea especificada una vez al año a medianoche del primer día de enero.

Puede ver las páginas de man para la lista completa de especificaciones de tiempo y fecha en systemd.timer(5)



Ejemplo practico en arch linux, el script que se ejcutara esta en /home/leonardo/sizeFile.sh

sizeFile.sh

```
#!/bin/bash

pathFile="/home/leonardo/sizesList.txt"

echo "- - - - - - - - - - - - - - - - - - - - - - - - - - -" >> "$pathFile"
date | tee -a "$pathFile" >/dev/null
find / -size +1G  -exec ls -lh {} >> "$pathFile" \; 2>/dev/null 
echo "- - - - - - - - - - - - - - - - - - - - - - - - - - -" >> "$pathFile"

```


size.unit
```
[Unit]
Description="hear the drummer"

[Service]
Type=oneshot
ExecStart=bash /root/sizeFile.sh

```

size.timer
```
[Unit]
Description="hear the drummer"

[Service]
Type=oneshot
ExecStart=bash /root/sizeFile.sh



[root@192 leonardo]# cat /etc/systemd/system/size.timer
[Unit]
Description=fvdsfvsdfvsdfvsdfsfvsfvsdfs

[Timer]
OnCalendar=*-*-* 14:50:00
Persistent=true

[Install]
WantedBy=timers.target

```


despues ejecutamos la siguiente linea:

```
systemctl daemon-reload 

systemctl start size.timer

comprobamos con:
systemctl list-timers
```










## Leccion 2
at - comando pra pgroamacion de tareas una unica vez
    ejemplo: at now +5 minutes, para salir de la pseudoshell usamos ctrl + D para guardar los cambios 
    -c imprime los comandos de una tarea especifica por medio del ID a la salida estandar
    -d Borra las tareas basadas en su ID. Es un alias para atrm
    -f Lee las tareas desde un archivo en lugar de la entrada estandar
    -l Lista las tareas pendientes del usuario. Si el usuario es root, se listan todas las tareas de todos los usuarios. Es un alias para atq
    -m  Envía un correo al usuario al final de la tarea aunque no haya mostrado salida.
    -q    Especifica una cola en forma de una sola letra de a a z y de A a Z (por defecto a para at y b para batch). Las tareas en las colas con las letras más altas se ejecutan con mayor prioridad. Los trabajos enviados a una cola con mayúsculas son tratados como tareas batch.
    -v    Muestra el tiempo en el que la tarea se ejecutará antes de leerla

El demonio atd tendra que estar ejecutandose en el sistema para que pueda usar at

### Listar tareas de at con atq

```
$ at 09:30 AM
warning: commands will be executed using /bin/sh
at> ./foo.sh
at> Ctrl+D
job 13 at Sat Sep 14 09:30:00 2019
$ at now +2 hours
warning: commands will be executed using /bin/sh
at> ./bar.sh
at> Ctrl+D
job 14 at Sat Sep 14 11:10:00 2019
```

- Recuerde que atq es un alias para at -l 

**Si ejecuta atq como root, mostrara las tareas en cola para todos los usuarios**


### Borrar tareas con atrm
Si desea borrar una tarea de at, usamos:

```
atrm 14 (ID de la tarea )
```

Puede borrar varios especificando multiples id, separados con espacios 

- Recuerde que atrm es un alias de at -d 


### Configurar el acceso a la programacion de tareas

- /etc/at.allow: Si solo existe este fichero, unicamente los usuarios que figuren en el podran ejecutar cron
- /etc/at.deny: Si solo existe este fichero, unicamente los usuarios que figuren en el tendran denegado ejecutar cron

Si no existe ninguno solo el root podra user at. Y si existen los dos (no recomendado) se aplica el .allow y se ignora el .deny

### Especificacion de tiempo

HH:MM seguido de AM/PM en formato de 12 (en dado caso de que la hora halla pasado, se agenda para el dia siguiente)

o tambien en el formato de 24 horas, como 18:00 por ejemplo (at 18:00 )

Para dias especificos, estan los siguientes formatos:
- mes dia-de-mes
-mes dia-de-mes año
- MMDDYY (MesDiaAño)
- MM/DD/YY
- DD.MM.YY
- YYYY-MM-DD

Tambien podemos usar las siguientes palabras:
midnight, noon, teatime (4 pm) y now seguido de un signo más (+) y un período de tiempo (minutos, horas, días y semanas)

Tambien puede usar este formato
Ejemplo: at 07:15 AM Jan 01

Para mas especificacion sobre la definicion exacta del tiempo veremos el archivo Timespec en /usr/share


Al igual que at, puede programar tareas unicas con el comando:
systemd-run 


Normalmente se utiliza para crear una unidad de temporizador transitoria de modo que un comando se ejcute en un momento especifico sin necesidad de crear un archivo de servicio, ejemplo:

```
# systemd-run --on-calendar='2019-10-06 11:30' date
```

Si quiere ejecutar el script foo.sh, ubicado en su directoriode trabajo actual, despues de dos minutos puede usar:
```
systemd-run --on-active="2m" ./foo.sh
```


**Recuerde que los temporizadores se registran en el diario del sistema y puede revisar los registros de las diferentes unidades usando el comando journalalctl. Recuerde también que si está actuando como un usuario ordinario, necesita usar la opción --user de los comandos systemd-run y journalctl.**









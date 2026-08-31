## Ejercicios guiados

1. Complete la siguiente tabla relativa a los permisos especiales

| Permiso especial | Representacion numerica | Representacion simbolica   |   Buscar archivos con SOLO ese permiso establecido |
|------------------|-------------------------|----------------------------|----------------------------------------------------|
|      SUID        |        4000             |          S s               |            find / -perm 4000                       |
|      SGID        |        2000             |          S s               |            find / -perm 2000                       |


2. La visualización de archivos con sólo el bit SUID o SGID activado no suele ser muy práctica. Realice las siguientes tareas para probar que sus búsquedas pueden ser más productivas:

- Encuentre todos los archivos con el SUID (y otros permisos) establecidos en /usr/bin: `find /urs/bin -perm -4000 o find /usr/bin -perm -u+s`
- Busque todos los archivos con el SGID (y otros permisos) establecidos en /usr/bin: `find /usr/bin -perm -2000 o find /usr/bin -perm -g+s`
- Encuenbtre todos los archivos con el SUID o el SGID en /usr/bin: `find /usr/bin -perm -4000 -perm -2000 o find /usr/bin -perm /6000`


3. chage permite cambiar la información de caducidad de la contraseña de un usuario. Como root, complete la siguiente tabla proporcionando los comandos correctos en el usuario mary:

| Significado                                                                   | Comando chage |
|-------------------------------------------------------------------------------|---------------| 
| Hacer que la contraseña sea valida durante 365 dias                           | chage -M 360 mary o chage --maxdays 365 mary |
| Hacer que el usuario cambie la contraseña en el proximo inicio de sesion      | chage -d 0 mary  o chage --lastday 0 mary |
| Establecer el numero minimo de dias entre cambios de contraseña a 1           | chage -m 1 mary o  chage --mindays 1 mary |
| Desactivar la caducidad de la contraseña                                      | chage -M 99999 mary  o chage --maxdays 99999 | 
| Permitir al usuario cambiar su contraseña en cualquier momento                |  chage -m 0 mary  o  chage --mindays mary |
| Establecer el periodo de advertencia en 7 dias y la fecha de caducidad de la cuenta en el 20 de Agosto de 2050 | chage -W 7  -E 2050-08-20 mary   o  chage --warndays 7 --expiredate 2050-08-20 mary | 
| Imprimir la informacion de caducidad de la contraseña del ususario            |  chage -l mary  o chage --list mary | 


4. Complete la siguiente tabla con la utilidad de red apropiada:


| Accion                                                                                                        | Comando(s) |
|---------------------------------------------------------------------------------------------------------------|------------|
| Muestra archivos de red para el host 192.168.1.55 en el puerto 22 usando lsof                                 | lsof -i@192.168.1.55:22 | 
| Muestra los  procesos que acceden al puerto por defecto del servidor web Apache en su maquina con fuser       | fuser -v -n tcp 80 o fuser --verbose --namespace tcp 80        |
| Muestra todos los sockets udp que estan escuchando en su maquina usando netstat                               | netstat -lu o ss -lu      o netstat --listening --udp     |
| Escanea los puertos del 80 al 443 en el host 192.168.1.55 usando nmap                                         |  nmap -p 80-443 192.168.1.55          |
|


5. Realice las siguientes tareas relacionadas con el tamaño del conjunto residente (RSS) y el ulimit como un usuario normal

- Mostrar límites flexibles en el máximo de RSS:

```
ulimit -m o ulimit -Sm
```

- Mostrar límites estrictos en el máximo RSS:

```
ulimit -mH
```

- Establecer los límites flexibles en el máximo RSS a 5.000 kilobytes:

Debe de ser root para poder modificar este limite

```
ulimit -Sm 5000
```

- Establecer los límites estrictos del máximo RSS a 10.000 kilobytes:

```
ulimit -Hm 10000
```
- Por último, intente aumentar el límite estricto del máximo RSS hasta 15.000 kilobytes. ¿Puede hacerlo? ¿Por qué?

No, una vez establecidos, los usuarios habituales no pueden aumentar los limites estrictos 


6. Considere la siguiente linea de salida del comando last y responda 

```
carol    pts/0        192.168.1.4      Sun May 31 14:16 - 14:22  (00:06)
```

- ¿Se conectó carol desde un host remoto? ¿Por qué?

Si, se conecto desde el host 192.168.1.4, que es la tercera columna 

- ¿Cuánto duró la sesión de carol?

Duro solamente 6 minutos, desde las 14:16, hasta las 14:22, y el tiempo total esta en la ultima columna

- ¿Se conectó carol a través de un verdadero terminal clásico basado en texto? ¿Por qué?

La conexion fue a travez de una pseudo terminal slave o pts/0, por lo que no es una verdadera terminal


7. Considere el siguiente extracto de `/etc/sudoers` y responda la siguiente pregunta

```
# Especificación de alias de host

Host_Alias SERVERS = 192.168.1.7, server1, server2

# Especificación de alias de usuario

User_Alias REGULAR_USERS = john, mary, alex

User_Alias PRIVILEGED_USERS = mimi

User_Alias ADMINS = carol, %sudo, PRIVILEGED_USERS, !REGULAR_USERS

# Especificación de alias de Cmnd

Cmnd_Alias WEB_SERVER_STATUS = /usr/bin/systemctl status apache2

# User privilege specification
root    ALL=(ALL:ALL) ALL
ADMINS  SERVERS=WEB_SERVER_STATUS

# Permitir a los miembros del grupo sudo ejecutar cualquier comando
%sudo   ALL=(ALL:ALL) ALL
```

- ¿Puede alex comprobar el estado del servidor web Apache en cualquier host? ¿Por qué?

No, no puede usar comprobar el estado del servidor apache, ya que alex es parte del alias de REGULAR_USERS, que a asu ves le es negado la ejecucion del comando a travez del alias de ADMINS con el fragmento de !REGULAR_USERS. los únicos usuarios (aparte de carol, miembros del grupo sudo y root) que pueden ejecutar systemctl status apache2 en los SERVERS.



## Ejercicios Exploratorios

1. Además del SUID y el SGID, existe un tercer permiso especial: el sticky bit. Actualmente se utiliza sobre todo en directorios como /tmp para evitar que los usuarios habituales borren o muevan archivos que no sean los suyos. Realice las siguientes tareas:


- Establecer el sticky bit en ~/tmp:

```
chmod o+t ~/tmp 

chmod 1755 ~/tmp
```

- Buscar directorios con el sticky bit (y cualquier otro permiso) establecido en su directorio principal:

```
find /home/leonardo -perm -1000 

find /home/leonardo -perm -o+t

Como dice, cualquier otro permiso, usaremos:

find /home/leonardo -perm /1000


```

- Desactivar el sticky bit en ~/tmp:

```
chmod o-t ~/tmp

chmod -t ~/tmp

chmod 0755 ~/tmp

```

2. Cuando la contraseña de un usuario está bloqueada mediante passwd -l nombre o usermod -L nombre, ¿cómo se puede saber mirando en /etc/shadow?

Mediante el signo de interrogacion que aparece despues de la contraseña, por ejemplo

```
leonardo:!$y$j9T$mAcPgM1dotHURq13S/0F8/$vez8eNaRBgRgmBRW/ZNEZAHh4er3je6h10dwjLen8.A:20696:33:33:150::20697:
```

3. ¿Cuál es la contraparte del comando usermod a chage -E date username o chage --expiredate date username?

```
usermod -e YYYY-MM-DD user

usermod --expiredate YYYY-MM-DD user
```

4. Proporcione dos comandos diferentes nmap para escanear todos los 65535 puertos en localhost:

```
nmap -p 0-65535 IP


nmap -p- localhost
```






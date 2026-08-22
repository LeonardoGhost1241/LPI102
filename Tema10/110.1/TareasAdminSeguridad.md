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










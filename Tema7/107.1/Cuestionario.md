# Leccion 1 

### Ejercicios guiadas

1. Para cada uno de los siguientes comandos, identifique el prosposito correspondiente 

|  Comando    | Proposito            |
|-------------|----------------------|
|usermod -L   | Bloquear una cuenta (Lock)    |
|passwd -u    | Desbloquea la cuanta de usuario (quita el simbolo de !)|
|chage -E     | Establece la fecha de caducidad de una cuenta de usuario|
|groupdel     | Elimina un grupo |
|useradd -s   | crear un usuario especificando un shell de uso |
|groupadd -g  | Agrega un grupo agregando un GID de inicio |
|userdel -r   | Elimina a un usuario borrando su directorio home |
|usermod -l   | Cambia el nombre del usuario |
|groupmod -n  | |
|useradd -m   | Agrega un usuario creando un direcotrio principal en $HOME |

2. Para cada uno de los siguientes comandos passwd, identifique el correspondiente comando chage:

|Comoando| equivalente|
|--------|------------|
|passwd -n | chage -m | (passwd:Establece la duracion minima de la contraseña)
| passwd -x | chagr -M | (passwd:Establece la duracion maxima de la contraseña)
| passwd -w |chage -W | (passwd: Determina el numero de dias de advertencia  antes de que la contraseña expire, durante los cuales se advierte al usuario que debe cambiarla)
| passwd -i | chage -I | (passwd: Establece el numero de dias de inactividad despues de que una contraseña expire)
| passwd -S | chage -l | (passwd: Informacion de salida sobre el estado de la contrasela de una cuenta de usuario especifica)


3. Explique en detalle el propotisot de los comandos en la pregunta anterior:

Conocer que con un comando, puedo hacer exactamente lo mismo con otro comando, en este caso passwd tiene equivalencias con chage 

4. ¿Qué comandos puedes usar para bloquear una cuenta de usuario? ¿Y qué comandos para desbloquearla?

Puedes usar para bloquearla:
passwd -l 
usermod -L o usermod --lock 


y para desbloquearla:
passwd -u 
usermod -U  o usermod --unlock 


### Ejercicios de exploracion 
1. Usando el comando groupadd, crea los grupos de administrators y developers. Asume que estás trabajando como root.

```
groupadd administrators ; groupadd developers
```
2. Ahora que ha creado estos grupos, ejecute el siguiente comando: useradd -G administrators, developers kevin. ¿Qué operaciones realiza este comando? Asume que CREATE_HOME y USERGROUPS_ENAB están configurados en yes en el archivo /etc/login.defs

CREATE_HOME - Especifica si un directorio principal debe ser creado por defecto para los nuevos usuarios 
USERGROUPS_ENAB -  al momento de crear un usuario se crea por defecto un grupo con el mismo nombre y se elimina el grpo que se crea al no tener miembros 

El comando añade un nuevo usuario, llamado kevin a la lista de usuarios del sistema, crea su directorio principal (CREATE_HOME está establecido en yes y por lo tanto puedes omitir la opción -m) y crea un nuevo grupo, llamado kevin, como el grupo principal de esta cuenta de usuario (USERGROUPS_ENAB está establecido en yes). Finalmente, los archivos y carpetas dentro del directorio skel se copian al directorio principal de kevin


3. Crea un nuevo grupo llamado designers, renómbrelo a web-designers y añada este nuevo grupo a los grupos secundarios de la cuenta de usuario kevin. Identifica todos los grupos a los que pertenece kevin y sus identificaciones


```
groupadd designers 

groupmod -n web-designers designers 

usermod -a -G web-designers kevin 


groups kevin o getent group kevin o id kevin 

```

4. Elimina sólo el grupo de developers de los grupos secundarios de kevin

```
usermod -G administrators,web-designers kevin 

id kevin 
```

5. Establezca la contraseña de la cuenta de usuario kevin.

```
passwd kevin 
```

6. Usando el comando chage, primero compruebe la fecha de caducidad de la cuenta de usuario kevin y luego cámbiela al 31 de diciembre de 2022. ¿Qué otro comando puedes usar para cambiar la fecha de caducidad de una cuenta de usuario?

```
chage -l kevin | grep "Account expires"

chage -E "2022-12-31"

otros comandos:
passwd -S 

passwd -x
usermod -e 
chage -E 
```

7. Añade una nueva cuenta de usuario llamada emma con el UID 1050 y establezca administrators como su grupo primario y developers y web-designers como sus grupos secundarios

```
useradd -g administratosr -G developers,web-designers -u 1050 emma
```

8. Cambie el shell de inicio de sesión de emma a /bin/sh.
```
usermod -s /bin/sh emma
```

9. Elimina las cuentas de usuario emma y kevin y los grupos de administrators, developers y web-designers

```
userdel -r emma ; userdel -r kevim

groupdel developers ; groupdel administrators ; groupdel web-designers 
```

# Leccion 2

### Ejercicios guiadas

1. Observe la siguiente salida y responda a las siguientes preguntas:

```
# cat /etc/passwd | grep '\(root\|mail\|catherine\|kevin\)'
root:x:0:0:root:/root:/bin/bash
mail:x:8:8:mail:/var/spool/mail:/sbin/nologin
catherine:x:1030:1025:User Chaterine:/home/catherine:/bin/bash
kevin:x:1040:1015:User Kevin:/home/kevin:/bin/bash
# cat /etc/group | grep '\(root\|mail\|db-admin\|app-developer\)'
root:x:0:
mail:x:8:
db-admin:x:1015:emma,grace
app-developer:x:1016:catherine,dave,christian
# cat /etc/shadow | grep '\(root\|mail\|catherine\|kevin\)'
root:$6$1u36Ipok$ljt8ooPMLewAhkQPf.lYgGopAB.jClTO6ljsdczxvkLPkpi/amgp.zyfAN680zrLLp2avvpdKA0llpssdfcPppOp:18015:0:99999:7:::
mail:*:18015:0:99999:7:::
catherine:$6$ABCD25jlld14hpPthEFGnnssEWw1234yioMpliABCdef1f3478kAfhhAfgbAMjY1/BAeeAsl/FeEdddKd12345g6kPACcik:18015:20:90:5:::
kevin:$6$DEFGabc123WrLp223fsvp0ddx3dbA7pPPc4LMaa123u6Lp02Lpvm123456pyphhh5ps012vbArL245.PR1345kkA3Gas12P:18015:0:60:7:2::
# cat /etc/gshadow | grep '\(root\|mail\|db-admin\|app-developer\)'
root:*::
mail:*::
db-admin:!:emma:emma,grace
app-developer:!::catherine,dave,christian

``` 

- ¿Cuál es el ID de usuario (UID) y el ID de grupo (GID) de root y catherine ?
de root,0,0 y de catherine, 1030 UID y 1025 GID

- ¿Cuál es el nombre del grupo primario de Kevin? ¿Hay otros miembros en este grupo?
El grupo principal tiene el id 1015, el nombre es db-admin y los usuarios agregados como grupo secundarios son: emma y grace 

- ¿Cuál shell está asignado para el mail? ¿Qué significa?
tiene asignado /sbin/nologin, el cual significa que no tiene alguna shell asignada.
De hecho, las cuentas de usuario del sistema como mail, ftp, news y daemon se utilizan para realizar tareas administrativas y por lo tanto se debe evitar el inicio de sesión normal para estas cuentas. Por eso el shell está normalmente configurado como /sbin/nologin o /bin/false

- ¿Quiénes son los miembros del grupo de app-developer? ¿Cuáles de estos miembros son los administradores del grupo y cuáles son los miembros ordinarios?
Los miembros son:catherine,dave,christian y no tiene administrador, todos son usuarios ordinarios 

- ¿Cuál es la duración mínima de la contraseña para catherine? ¿Y cuál es la duración máxima de la contraseña?
la duracion minima es de 20 dias y el maximo de 90 


- ¿Cuál es el período de inactividad de la contraseña para kevin?
El período de inactividad de la contraseña es de 2 días. Durante este período kevin debe actualizar la contraseña, de lo contrario la cuenta será desactivada


2. Por convención, ¿Qué identificaciones se asignan a las cuentas del sistema y cuáles a los usuarios ordinarios?
del 0 al 9999 son cuentas de administracion y los usuarios ordinarios, van del 1000 al 60000, aunque estos valores, los podemos modificar en el arhcivo /etc/login.defs, en las variables UID_MIN y UID_MAX

3. ¿Cómo puede saber si una cuenta de usuario, que antes podía acceder al sistema, ahora se encuentra bloqueada? Supongamos que su sistema utiliza contraseñas en la sombra.
En el apartado de la contraseña, podremos ver una x, que refleja que se usa el archivo /etc/shadow y si la cuenta esta bloqueada, veremos un simbolo de exclamacion ! antes de la passwd 


### Ejercicios de exploracion 
1. Crea una cuenta de usuario llamada christian usando el comando useradd -m e identifica su ID de usuario (UID), ID de grupo (GID) y el shell.

```
useradd -m cris

        UID  GID      HOME        SHELL
cris:x:1663:1664::/home/cris:/usr/bin/bash



```

2. Identifica el nombre del grupo primario de christian. ¿Qué puedes deducir?

```
getent group cris 

cris:x:1664:


o podemos verlo con:
cat /etc/group | grep "1664"

```
Por lo que tiene un grupo igual que su nombre

3. Usando el comando getent, revisa la información de la contraseña de la cuenta del usuario christian.

```
getent shadow cris 

cris:!:20553:1:1:1:::

```

4. Añade el grupo editor a los grupos secundarios de christian. Supongamos que este grupo ya contiene a Emma, Dave y Frank como miembros ordinarios. ¿Cómo puedes verificar que no hay administradores para este grupo?

```
usermod -a -G editor cris 

getent gshadow editor
editor:!::emma,dave,frank,cris

dado a que el tercer campo de la ultima linea, esta solo, el archivo gshadow, se compone por:  nombreGrupo:ContraseniaGrupo:Admins:miembros


```

5. Ejecute el comando ls -l /etc/passwd /etc/group /etc/shadow /etc/gshadow y describa la salida que imprime en términos de permisos de archivo. ¿Cuál de estos cuatro archivos están con "shadow" por razones de seguridad? Supongamos que tu sistema utiliza contraseñas shadow.

```
-rw-r--r-- 1 root root  889 Apr 10 18:52 /etc/group  --> Tiene permisos de lectura, dado a que se pueden ver solo los usuarios y es publico y ademas de que si tiene una passwd, en el campo, estara un x
-rw------- 1 root root  772 Apr 10 18:52 /etc/gshadow --> Aqui estan las contraseñas cifradas, por eso solo root, puede verlo
-rw-r--r-- 1 root root 1501 Apr 10 18:51 /etc/passwd --> usuarios
-rw------- 1 root root  947 Apr 10 18:51 /etc/shadow --> Contraseñas encriptadas

```

shadow esta con passwd, donde se alojan sus contraseñas de cada usuario registrado, solo root puede verlos e incluso por usuarios pertenecientes al grupo shadow 





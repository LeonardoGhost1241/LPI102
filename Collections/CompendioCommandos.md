
# Compendio de comandos 

## 105: Shells y script 
#### Personalizar el entorno de shell

bash - Es usado para ejecutar terminales 
    -l o --login    Invocara un shell de inicio de sesion 
    -i  Invocara un shell interactivo 
    --noprofile Con shell de inicio de sesión ignorará tanto el archivo de inicio de todo el sistema /etc/profile como los archivos de inicio a nivel de usuario ~/.bash_profile, ~/.bash_login y ~/.profile
    --norc  Con el shell interactivo ignorara tanto el archivo de inicio del sistema /etc/bash/bashrc como el archivo del inicio de sesion a nivel de usuario ~/.bashrc


su - Cambia el ID de usuario o lo convierte en superusuario (root), con este comando porde invocar ambos shells, el de inicio de sesion y sin inicio de sesion:
    `su - user2`, `su -l user2` o `su --login user2` iniciara un shell de inicio de sesion interactivo como user2
    `su user2` iniciara un shell interactivo y sin inicio de sesioncomo `user2`
    `su - root` o `su -` iniciara un shell de inicio de sesion interactivo como root
    `su root` o `su` iniciara un shell interactivo y sin inicio de sesion como root

sudo - Ejecuta comandos como otro usuario (incluyendo el superusuario). Debido a que este comando se usa principalmnte para obtener privilegios temporales de root, el usuario que lo use, debe de estar en el archivo sudoers o agregarlo a grupo sudo (wheel en otras distribuciones). Asi como su, sudo nos permite invocar tanto shells de inicio de sesion como los de no de inicio de sesion
    `sudo su - user2`, `sudo su -l user2`, `sudo su --login user2` iniciara un shell de inicio de sesion interactivo como user2
    `sudo su user2` iniciara un shell interactivo sin inicio de sesion como user2
    `sudo -u user2 -s` iniciara un shell interactivo sin inicio de sesion como `user2`
    `sudo su - root` o `sudo su -` iniciara un shell de inicio de sesion interactivo como `root`
    `sudo -i` iniciara un shell de inicio de sesion interactivo como root
    `sudo -i <some_command>` iniciara un shell de inicio de sesion interactivo como root, ejecuta el comando y volvera al usuario original
    `sudo su root` o `sudo su ` iniciara un shell interactivo sin inicio de seison como root
    `sudo -s o sudo -u root -s` iniciara un shell sin inicio de sesion como `root`


. y source - son usados para ejecutar scripts desde otros, sin la necesidad de reiniciar el equipo, un ejemplo es cuando modificamos el .bashrc


#### 105.2 Personalizar y scribir scripts sencillos

exec - Cambia el comportamiento que tiene la temrinal, cuando acaba un script, la terminal tambien se cierra
    Ejemplo: exec script.sh 

declare - declara arrays en bash
    Ejemplo: declare -a arr

printf - Es otro comando para imprimir, sin embargo este esta mas especializado en lo que es el formato del usuario final, mas personalizable 





## 107: Tareas administrativas
#### Administrar cuentas de usuario y de grupo y los archivos de sistema relacionados con ellas


useradd - Agregar una cuenta de usuario (necesita permisos root)
    Ejemplo: useradd leonardo
    -c Crea una nueva cuenta de usuario con comentarios personalizados (por ejemplo, el nombre completo del usuario)
    -d Crea una nueva cuenta de usuario con un directorio de inicio especifico
    -e Crea una nueva cuenta de usuario estableciendo una fecha especifica en la que se desactivara
    -f Crea una nueva cuenta de usuario estableciendo el numero despues de que expire una contraseña, durante los cuales el usuario debe actualizarla (de lo contrario, la cuenta se desactivara)
    -g Crea una nueva cuenta de usuario con un GID especifico
    -G Crea una nueva cuenta de usuario añadiendola a multiples grupos secundarios
    -k Crea una nueva cuenta de usuario copiando los archivos del "skel" de un directorio personalizado especifico ("Esta opcion solo es valida si se especifica la opcion m o crear cada")
    -m Cra una nueva cuenta de usuario con su directorio  (si no existe)
    -M Crea una nueva cuenta de usuario sin su directorio principal 
    -s Crea una nueva cuenta de usuario con un shell de acceso especifico
    -u Crea una nueva cuenta de usuario con un UID especifico 

id - Ver grupos a los que pertenece un usuario
    Ejemplo: id leonardo

groups - Ver grupos a los que pertenece un usuario
    Ejemplo: groups 

passwd - Cambiar contrasena propia o de un usuario si tiene privilegios
    Ejemplo: passwd cambia la constraseña de la cuenta actual y passwd user cambia la contraseña de la cuenta user


usermod - Modifica una cuenta de usuario cambiando sus atributos, como nombre de usuario, el shell, la fecha de caducidad de la contraseña, etc (necesita permisos root)
    -c Agrefa un breve comentario a la cuenta de usuario
    -d Cambia el directorio principal de la cuenta. Cuando se usa con la opcion -m, los contenidos del directorio principal actual se mueven al directorio nuevo principal, que a su vez se crea de no ser existente 
    -e Establece la fecha de expiracion de la cuenta de usuario
    -f Establece el numero de dias despues de que una contraseña expira, durante los cuales el usuario debe actualizar la contraseña (de lo contrario la cuenta se desactivara)
    -g Cambia el grupo primario de la cuenta de usuario (el grupo debe de existir)
    -G Añade grupos secundarios a la cuenta de usuario especificada. Cada grupo debe existir y debe estar separado del siguiente por una coma, sin espacios en blanco, si se usa sola, esta opcion elimina todos los grupos existentes a los que el usuario pertenece, mientras que cuando se usa con la opcion -a, simplemente añade nuevos grupos secundarios a los ya existentes 
    -l cambia el nombre de usuario de la cuenta de usuario especificada (Cuando cambia el nombre de inicio de sesion, debe de cambiar el nombre del directorio principal)
    -L Bloquea la cuenta de usuario especificada. Esto pone un signo de exclamacion delante de la contraseña encriptada dentro del archivo `/etc/shadow`, deshabilitado asi el acceso con una contraseña para ese usuario
    -s Cambia el shell de acceso de la cuenta de usuario especificada
    -u Cambia el UID de la cuenta de usuario especificada
    -U Desbloquea la cuenta de usuario especificada. Esto elimina el signo de exclamacion delante de la contraseña cifrada con el archivo `/etc/shadow`

userdel - Elimina una cuenta de usuario, actualiza la informacion almacenada en las bases de datos de las cuentas, borrando todas las entradas referentes al usuario especificado
    -r Elimina la cuenta de usuaior y ademas elimina el directorio principal del usuario y todos sus contenidos, junto con el spool de correo del usuario  (Otros archivos, localizados en otros lugares deben ser buscados y eliminados manualmente 


groupadd - Agrega un grupo al sistema (necesita permisos root)
    -g especifica el id del grupo, Ejemplo:groupadd -g 1090 developer 

**Nota:**
- Recuerde que cuando agrega una nueva cuenta de usuario, el grupo primario y los grupos secundarios a los que pertenece deben existir antes de lanzar el comando useradd

groupmod - Modifica la informacion de un grupo (necesita permisos root)
    -g Cambia el id del grupo (Tenga en cuenta que si cambia el gid del grupo, debe de cambiar el GID de todos los archivos y directorios que tengan este mismo id)
    -n Cambia el nombre del grupo 

**Nota:**
- Recuerde que si cambia el GID usando la opcion -g, debe cambiar el GID de todos los archivos y directorios que deben seguir perteneciendo al grupo


groupdel - Elimina el grupo del sistema (necesita permisos root)
    Ejemplo: groupdel groupo

**Nota:**
- No se podra eliminar un grupo si es el grupo principal de una cuenta de usuario


passwd - Cambia la contraseña del usuario 
    Cualquier usuario puede cambiar su contraseña, pero solo root puede cambiar la contraseña de cualquier usuario 
    -d Borrar la contraseña de una cuenta de usuario (deshabilitando asi al usuario)
    -e Forzar la cuente de usuario a cambiar la contraseña
    -i Establece el numero de dias de inactividad despues de que una contraseña expire, durante los cuales el usuario debe actualizar la contraseña (de lo contrario, la cuenta sera desactivada)
    -l Bloquea la cuenta de usuario (la contraseña cifrada se prefija con un signo de exclamacion en el archivo `/etc/shadow`)
    -n Establece la durecion minima de la contraseña
    -S Informacion de salida sobre el estado de la contraseña de una cuenta de usuario especifica 
    -U Desbloquea la cuenta de usuario (El signo de exclamacion se elimina del campo de la contraseña en el archivo `/etc/shadow`)
    -x Establece la duracion maxima de la contraseña 
    -w Determina el numero de dias de advertencia antes de que la contraseña expire, durante los cuales se advierte al usuario que debe cambiarla 


gpasswd - Cambiar la password de un grupo
    Los usuarios que no son miembros de un grupo, pero que conocen la password, pueden unirse temporalmente usando el comando newgrp


**Nota:**
- Ademas de esta funcion, se puede usar para agregar o eliminar usuarios de un grupo y para establecer la lista de administradores y miembros ordinarios del grupo


chage - Se usa para cambiar el periodo de una password de un usuario 
    -l lista el tiempo de una password
    -d Establece el ultimo cambio de password para una cuenta de usuario
    -E establece la fecha de caducidad de una cuenta de usuario 
    -I Establece el numero de dias de inactividad despues de que una password expira, durante los cuales el usuario debera actualizarla (de lo contrario la cuenta sera desactivada)
    -m Establece la duracion minima de la password para una cuenta de usuario
    -M Establece la duracion maxima de la password para una cuenta de usuario
    -W Establece el numero de dias de advertencia  antes de que la contrasenia expire, durante los cuales se le advierte al usuario que deberia cambiarla



#### 107.3 - Localizacion e internacionalizacion 

date - Comando usado para mostrar la hora y la zona horaria 

timedatectl - Esta disponible en distribuciones que utilizan systemd y muestra mas detalles sobre la hora y la fecha del sistema 


tzselect - Facilita la seleccion de la zona horaria al ejecutarlo sin argumentos, este comando deberia de estar  por defecto en todas las distribuciones de Linux 


localectl - disponible en los sistemas que emeplean systemd como gestor de sistema, tambien puede ser consultado para consultar y cambiar el locale del sistema 
    Ejemplo: localectl set-locale LANG=es_US.UTF-8

locale - Mostrara todas las variables definidas en la configuracion del actual host

iconv - Convertir un archivo de su codificacion de caracteres originales a una deseada
    Ejemplo: Para convertir un archivo llamado "original.txt" de codificación ISO-8859-1 al archivo llamado "converted.txt" usando la codificación UTF-8, se puede usar el siguiente comando 

```
iconv -f ISO-8859-1 -t UTF-8 original.txt > converted.txt
```

La opción -f ISO-8859-1 (o --from-code=ISO-8859-1) establece la codificación del archivo original y la opción -t UTF-8 (o --to-code=UTF-8) establece el del archivo convertido. Todas las codificaciones soportadas por el comando iconv se listan con el comando iconv -l o iconv --list. En lugar de usar la redirección de la salida, como en el ejemplo, también se puede usar la opción -o converted.txt o --output converted.txt


 



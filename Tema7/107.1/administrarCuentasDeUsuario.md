# Administrar cuentas de usuario y de grupo y los archivos de sistema relacionados con ella


# leccion 1 

### Agregar cuentas de usuarios 

useradd - agregar un usuario 
    ejem: useradd michael  -> se agrega el usuario michael, se crea un grupo, se crea un directorio principal para el nuevo usuario
    -c Crea una nueva cuenta de usuario con comentarios personalizados (por ejemplo, el nombre completo del usuario)
    -d crea una nueva cuenta de usuario con un directorio 
    -e crea una nueva cuenta de usuario estableciendo una fecha especifica en la que se desactivara 
    -f Crear una nueva cuenta de usuario estableciendo el número de días después de que expire una contraseña, durante los cuales el usuario debe actualizarla (de lo contrario, la cuenta se desactivará)
    -g Crea una nueva cuenta de usuario con un GID especifico 
    -G Crea una nueva cuenta de usuario añadiendo a multiples grupos secundarios 
    -m crea una nueva cuenta de usuario con su directorio principal (si no existe)
    -M Crea una nueva cuenta de usuario sin su directorio principal 
    -s Crea una nueva cuenta de usuario con un shell de acceso especifico
    -u Crea una nueva cuenta de usuario con un UID especifico 


passwd user - cambia la password de un usuario

id user - revisar el ID del usuario (UID)

groups user - ver los grupos a los que pertenece un usuario


### Modificacion de las cuentas de usuario 

    usermod -s "/bin/bash" michael 
    usermod -c "Michael User Account" michael 

    -c agrega un breve comentario a la cuenta de usuario 
    -d Cambia el directorio principal de la cuenta de usuario. Cuando se usa con la opcion -m, los contenidos del directorio principal actual se mueven al nuevo directorio principal, que a su vez se crea de no ser existente
    -e Establece la fecha de expriracion de la cuenta de usuario
    -f establece el numero de dias despues de que una contraseña expira, durante los cuales el usuario debe actualizar la contraseña (de lo contrario la cuenta se desactivara)
    -g Cambia el grupo primario de la cuenta de usuario (el grupo debe de existir)
    -G Añade grupos secundarios a la cuenta de usuario especificada. Cada grupo debe existir y debe estar separado del siguiente por una compa, sin espacios en blanco. Si se usa sola, esta opcion elimina todos los grupos existentes a los que el usuario pertenece, mientras que cuando se usa con la opcion -a , simplemente añade nuevos grupos secundarios a los ya existentes
    -l Cambia el nombre de usuario de la cuenta de usuario especificada 
    -L Bloquea la cuenta de usuario especificada. Esto pone un signo de exclamacion delate de la contraseña encriptada dentro del archivo /etc/shadow, deshabilitando asi el acceso con una constrasela para ese usuario
    -s Cambia el shell de acceso de la cuenta de usuario especificada
    -u Cambia el UID de la cuenta de usuario especificada
    -U Desbloquea la cuenta de usuario especificada. Elimina el signo de exclamacion delante de la contraseña cifrada con el archivo /etc/shadow 

### Eliminando cuentas de usuario 

userdel - eliminar una cuenta de usuario 
    si se usa con la opcion -r, se elimina tambien la carpeta home que esta asignado al usuario 


**Asi como los usuarios, se pueden agregar, eliminar y modificar grupo con groupadd, groupdel y groupmod respectivamente, con privilegios de root**

Crear un grupo llamado developer
groupadd -g 1090 developer
    donde:
        -g crea un grupo con un GID especifico 

**Recuerda que cuando se añade una nueva cuenta de usuario, el grupo primario y los grupos secundarios  a los que pertenece deben existir antes de lanzar el comando useradd**

Si se desea renombrar el grupo de developer a web-developer y cambiar su GID, puedes ejecutar lo siguiente:

groupmod -n web-developer -g 1050 developer 

**Recuerde que si cambia el GID usando la opcion -g, debe cambiar el GID de todos los archivos y directorios que deben seguir perteneciendo al grupo**

Si se quiere borrar el grupo, usamos:

groupdel web-developer 

**No se puede eliminar un grupo si es el grupo principal de un usuario, se tendria que eliminar al usuario, en cuanto al los usuarios, si elimina un grupo, los arhcivos pertenecientes a ese grupo permanecen en su sistema de archivos y no se eliminan ni se asignan a otro grupo**


### Directorio skel

Cuando añades una nueva cuenta de usuario, incluso creando su directorio principal, el directorio principal recién creado se carga de archivos y carpetas que se copian del directorio skel (por defecto /etc/skel)

Aqui se agregan archivos que quieran que aprezcan cuando creemos un usuario, lo que este dentro de esta carpeta seran los archivos que apareceran 


### El archivo /etc/login.defs

En este archivo se especifica los parametros de configuracion que controlan la creacion de usuarios y grupos. Ademas, los comandos mostrados en las secciones anteriores toman por defecto valores de este archivo 

Algunas de las directrices mas importantes son: 

* UID_MIN y UID_MAX    El rango de ID de usuario que puede ser asignado a los nuevos usuarios ordinarios.
* GID_MIN y GID_MAX    El rango de ID de grupo que puede ser asignado a nuevos grupos ordinarios.
* CREATE_HOME    Especifica si un directorio principal debe ser creado por defecto para los nuevos usuarios.
* USERGROUPS_ENAB    Especifica si el sistema debe crear por defecto un nuevo grupo para cada nueva cuenta de usuario con su mismo nombre, y a su vez al eliminar la cuenta también se debe eliminar el grupo principal del usuario si ya no contiene miembros.
* MAIL_DIR    El directorio de la cola de correo.
* PASS_MAX_DAYS    El número máximo de días que una contraseña puede ser usada.
* PASS_MIN_DAYS    El número mínimo de días permitido entre los cambios de contraseña.
* PASS_MIN_LEN    La longitud mínima aceptable de la contraseña.
* PASS_WARN_AGE    El número de días de advertencia antes de que una contraseña expire

**Cuando se administre usuarios y grupos, compruebe siempre este archivo para ver y eventualmente cambiar el comportamiento por defecto del sistema (si fuese necesario)**

### El comando passwd
Este comando se usa para cambiar la pasword de un usuario

Cualquier usuario puede cambiar su contraseña, pero root puede cambiar la pasword de cualquier usuario, esto es asi, por que el comando passwd tiene el bit SUID puesto, que significa que se ejecuta con los privilegios del propietario del archivo (por lo tanto, root)

Dependiendo de las opciones usadas,  puedes controlar aspectos especificos del envejecimiento de las contraseñas tales como:
    -d borrar la contraseña de una cuenta de usuario (deshabilitando asi al usuario)
    -e Forzar a la cuenta de usuario a cambiar la contraseña 
    -i Establecer el numero de dias de inactividad despues de que una contraseña expira, durante los cuales el usuario debe actualizar la contraseña (de lo contrario, la cuenta sera desctivada)
    -l bloquea la cuenta de usuario (la contraseña cifrada se prefija con un signo de exclamacion en el archivo /etc/shadow)
    -n Establece la duracion minima de la contraseña 
    -S informacion de salida sobre el estado de la contraseña de una cuenta de usuario especifica 
    -u Desbloquea la cuenta de usuario (el signo de exclamacion se elimina del campo de la contraseña en el archivo /etc/shadow)
    -x Establece la duracion maxima de la contraseña
    -w Determina el numero de dias de advertencia antes de que la contraseña expira, dirante los cuales se advierte al usuario que deba cambiarla

**Los grupos tambien pueden tener una constraseña, que puede ser establecida usando el comando gpasswd. Los usuarios que no son miembros de un grupo pero conocen su contraseña, pueden unirse de forma temporal usando el comando newgrp. Recuerda que gpasswd  tambien se usa para añadir y eliminar usuarios de un grupo y para establecer la lista de administradores y miembros ordinarios del grupo**


gpasswd - cambiar contraseña de un grupo, tambien se usa para añadir y eliminar usuarios de un grupo
newgrp - unirse temporalmente a un grupo




### El comando change 
Este comando determinado como "change age", se utiliza para cambiar el periodo de la contraseña de un usuario. El comando change esta restringido a root, excepto la opcion -l, que puede ser usada por usuarios ordinarios para listar el tiempo de su contraseña

Las otras opciones que se aplican al comando son:
    -d establece el ultimo cambio de contraseña para una cuenta de usuario 
    -E establece la fecha de caducidad de una cuenta de usuario
    -I Establece el numero de dias de inactividad despues de que una contraseña expira, durante los cuales el usuario debera actualizarla (de lo contrario ls cuenta sera desactivada)
    -m Establece la durecion minima de la contraseña para una cuenta de usuario
    -M Establece la duracion maxima de la contraseña para una cuenta de usuario
    -W Establece el numero de dias de advertencia antes que la contraseña expire, durante los cuales se advierte al usuario que debera cambiarla






# leccion 2
LOs anteriores comandos modifican archivos para persisitir, estos archivos se encuentran en el directorio /etc/:

- /etc/passwd : Archvivo de 7 sampos delimitados por dos puntos que contienen informacion basica sobre los usuarios 
- /etc/group : Un archivo de cuatro campos delimitados por dos puntos que contienen informacion basica sobre los grupos
- /etc/shadow : Un archivo de nueve campos delimitados por dos puntos que contienen contraseñas encriptadas de usuario 
- /etc/gshadow : Un archivo de cuatro campos delimitados por dos puntos que contienen contraselas de grupos encriptadas 

**Aunque se editan en texto plano, nunda se deben de editar directamente, deben ser con los comandos mencionados en la leccion anterior**


### /etc/passwd 
Contiene una lista de usuarios, cada uno en una linea separada, cada linea tiene siete campo delimitados por dos puntos ":"

- Nombre de usuario -> Usado cuando este se conecta al sistema 
- Password -> Se encuentra encriptada (o una x si se usa el archivo /etc/shadow)
- ID de usuario (UID) -> El numero de identificacion asignado al usuario en el sistema 
- Id de grupo (GID) -> El numero de grupo primario de usuario en el sistema
- GECOS -> Campo de comentario opcional, se usa para añadir info adicional, aqui se aloja lo que definimos en la opcion -c del comando useradd 
- Homer directory -> La ruta absoluta del directorio principal del usuario 
- Shell -> La ruta absoluta del programa que abr cuando el usuario se conecta al sistema (normalmente un shell interactivo como /bin/bash)


### /etc/group 
Contiene una lista de grupos, cada uno en una linea separada, cada linea consiste en cuatro campos 

- Nombre del grupo 
- Contraseña del grupo
- ID del grupo (GID)
- Lista de miembros -> Lista delimitada por comas de los usuarios que pertenecen al grupo, excepto aquellos para los que estaen en el grupo principal 


### /etc/shadow 
Legible solo por root y por usuarios con privilegios de root, aqui se alojan las contraseñas cifradas, cada lina consiste en nueve campo delimitados por puntos 

- Nombre de usuario -> Nombre usado al conectarse al sistema
- Constraseña encriptada -> La contraseña encriptada del usuario (Si el valor comienza con !, la cuenta esta bloqueada)
- Fecha del ultimo cambio de contraseña -> Con numero de dias desde le 01/01/1970 (Un valor de 0, significa que el usuario debe cambiar la contraseña la proxima vez que se conecte)
- Edad minima de la contraseña -> (El numero de dias despues de un cambio de contraseñas, que debe pasar antes de que el usuario pueda cambiar la contrasela la proxima vez que se conecte )
- Edad maxima de la contraseña -> El numero maximo de dias que debe pasar antes de que se requiera un cambio de contraseña
- Periodo de alerta de la contraseña -> El numero de dias, antes de que la contraseña exprire, durante los cuales se advierte al usuario que debe cambiarla 
- Periodo de inactividad de la contraseña -> El numero de dias despues de que la contraseña expira, durante los cuales el usuario debe actualizarla, despues de este periodo, si el usuario no cambia la contraseña, la cuenta se desactivara 
- Fechas de vencimiento de la cuenta -> La fecha expresada como el numero de dias desde el 01/01/1970, en que la cuenta de usuario sera desactivada (un campo vacia significa que la cuenta nunca expirara)
- Campo reservado -> Un campo para un uso futuro 


### /etc/gshadow 
Archivo legible solo por root y por usuarios con privilegios de root que contiene contraseñas de grupo cifradas, cada una en una linea separada, cada linea consta de cuatro campos delimitados por dos puntos


- Nombre del grupo 
- Contraseña encriptada
- Administradores del grupo -> Una lista delimitada por comas de los administradores del grupo. Ellos pueden cambiar la contraseña del grupo y pueden agregar o quitar miembros del grupo con el comando gpasswd 
- Miembros del grupo -> Una lista delimitada por comas de los miembors del grupo 



### Filtrar las bases de datos de contraseñas y grupos 
Para inspeccionar el archivo, puede usar el comando cat, cut o grep para filtrar datos, sin embargo otra forma de acceder a estas bases de datos son:

getent - muestra las entradas de las bases de datos soportadas por las bibliotecas Name Service Switch (NSS) y requiere el nombre de la base de datos y una clave de busqueda 
    Si no se proporciona ningun argumento, se muestran todas las entradas de la base de datos especifica (a menos que la base de datos no soporte la enumeracion)

ejemplo:
$ getent passwd emma 
$ getent grouo docker 

este comando no requiere la autoridad de la raiz, solo necesita poder leer la base de datos de la que quieras recuperar 


**getent solo puede acceder a las bases de datos configuradas en el archivo /etc/nsswitch.conf**






# Cuestionarios 

## Tema 105: Shells y Scripts
### 105.1 - Personalizar y usar el entorno de shell

-> Leccion 1 
#### Ejercicios Guiados

1. Estudie cómo se han iniciado los shells en la columna "Iniciado con…" y complete la información requerida:

|                   Iniciando con...                                | Interactivo?  | Inicio de sesion? | Resultado de echo $0 |
|-------------------------------------------------------------------|---------------|-------------------|----------------------|
|               sudo ssh user@machine2                              |       x       |         x         |       -bash          |
|                   Ctrl + Alt + F2                                 |       x       |         x         |       -bash          |
|                     su - user2                                    |       x       |         x         |       -bash          |
|                    gnome-terminal                                 |       x       |                   |        bash          |
|Un usuario regular usa konsole para iniciar una instancia de sakura|       x       |                   |        bash          |
| Un script llamado test.sh que contiene el comando echo $0         |               |                   |        test.sh       |


2. Escriba los comandos `su` y `sudo` para lanzar el shell especificado: 

- Shell de inicio de sesion interactivo como user2

```
    su - user2
    sudo su - user2

    su --login user2 
    su -l user2
```

- Shell de inicio de sesion interactivo como root

```
    su - root (o su -)
    sudo su - root

    sudo -i  
```

- Shell interactivo sin inicio de sesion como root

```
    su root 

    sudo su root
```

- Shell interactivo sin inicio de sesion como user2

```
    su user2

    sudo su user2
```


3. Que arhcivo de inicio de sesion se lee cuando se inicia el shell bajo "Tipo de shell"


|               Tipo de shell                       | `/etc/profile` | `/etc/bash.bashrc` | `~/.profile` | `~/.bashrc` |
|---------------------------------------------------|----------------|--------------------|--------------|-------------|
|Shell de inicio de sesion interactivo como `user2` |       x        |       x            |      x       |     x       |
|Shell de inicio de sesion interactivo como root    |       x        |       x            |      x       |     x       |
|Shell interactivo sin inicio de sesion como root   |                |       x            |              |     x       |
|Shell interactivo sin inicio de sesion como user2  |                |       x            |              |     x       |


#### Ejercicios Exploratorios 
1. En Bash podemos escribir una simple función "¡Hola mundo!" incluyendo el siguiente código en un archivo vacío:

```
function hello() {
  echo "Hello world!"
}
```

- ¿Qué deberíamos hacer a continuación para que la función esté disponible para shell?

Debemos de agregarlo con source para que sea una funcion de entorno

- Una vez que esté disponible para el shell actual, ¿cómo lo invocarías?

Simplemente con el nombre de a funcion `hello`

- Para automatizar las cosas, ¿en qué archivo agregaría la función y su invocación para que se ejecute cuando user2 abra una terminal de una sesión de Ventanas X (X Windows)? ¿Qué tipo de shell es?

Se agregaria solamente al archivo local de user2 /home/user2/.bashrc


- ¿En qué archivo pondrías la función y su invocación para que se ejecute cuando root lance un nuevo shell interactivo independientemente de si es de inicio de sesión o no?

Lo podemos poner en el archivo bashrc local de root o /etc/bash.bashrc para que sea global


2. Observemos el siguiente script, ¡Hola mundo! de Bash:

```
#!/bin/bash
#hello_world: a simple bash script to discuss interaction in scripts.
echo "Hello world!"
```

- Supongamos que establecemos permisos de ejecución y lo ejecutamos. ¿Sería un script interactivo? ¿Por qué?

No, ya que no hay interaccion humana y no se pueden ingresar comandos

- ¿Qué hace que un script sea interactivo?

El hecho de que requiera la intervencion de un usuario


3. Imagina que has cambiado los valores de algunas variables en ~/.bashrc y quieres que esos cambios surtan efecto sin reiniciar. Desde tu directorio principal, ¿cómo podrías lograrlo de dos maneras diferentes?

con:

```
    . ~/.bashrc

    source ~/.bashrc
```

4. John acaba de iniciar una sesión de X wIndows en un servidor Linux. Abre un emulador de terminal para realizar algunas tareas administrativas pero, sorprendentemente, la sesión se congela y necesita abrir un shell de texto.

- ¿Cómo puede abrir esa tty?

Mediante la combinacion de teclas Ctrl+Atl+Fn{1..7}

- ¿Qué archivos de inicio se obtendrán?

Como sera con un inicio de sesion y ademas interactivo, se ejecutaran el bashrc local como global (/etc/bashrc) y los archivos de inicio de sesion, tanto local como global (/etc/profile)


5. Linda es una usuaria de un servidor Linux. Le pide amablemente al administrador que tenga un archivo ~/.bash_login para que pueda tener la hora y la fecha impresa en la pantalla cuando se conecte. A otros usuarios les gusta la idea y siguen el ejemplo. El administrador tiene dificultades para crear el archivo para los otros usuarios del servidor, así que decide añadir una nueva política y crear un archivo ~/.bash_login para todos los nuevos usuarios. ¿Cómo puede el administrador realizar esa tarea?


Agregar el archivo bash_login en el directorio /etc/skel para que cuando se agregue un suario lo tenga


-> Leccion 2 
#### Ejercicios Guiados
1. Observe la asignación de la variable en la columna “Comando(s)” e indique si la variable resultante es “Local” o “Global”

|       Comandos       | Local   | Global   |
|----------------------|---------|----------|
|debian=mother         |    x    |          |
|ubuntu=deb-based      |    x    |          |
|mint=ubuntu-based;export mint | |     x    |
|export suse=rpm-based |         |     x    |
|zorin=ubuntu-based    |    x    |          |


2. Estudie el “Comando” y la “Salida” y explica el significado:

|   Comando                              |       Salida            |            Signficado              | 
|----------------------------------------|-------------------------|------------------------------------|
|echo $HISTCONTROL                       |     ignoreboth          |Tanto los comandos que inicien con un espacio y ademas sea el mismo que le antecede, no se guardara|
|echo ~                                  |     /home/carol         | Este es el directorio home del usuario que esta siendo usado |
|echo $DISPLAY                           |     reptilium:0:2       |En relacion con el servidor X, donde hostname:PantallaDelaComputadora, por lo que hay 2 pantallas |
|echo $MAILCHECK                         |          60             | Es el numero de segundos que bash comprueba si hay mensajes de correo |
|echo $HISTFILE                          |/home/carol/.bash_history| Muestra en donde se aloja el archivo de historial de comandos |



3. Las variables están siendo puestas incorrectamente en la columna “Comando erróneo”. Proporcione la información que falta en “Comando correcto” y “Referencia de la variable” para que obtengamos la “Salida esperada”:

| Comando erroneo           |Comando correcto              | Referencias de la variable |
|---------------------------|------------------------------|----------------------------|
|lizard =chameleon          | lizard=chameleon             |         chameleon          |
|cool lizard=chameleon      | cool_lizard=chameleon        |         chameleon          |
|lizard=cha|me|leon         |  lizard="cha|me|leon"        |        cha|me|leon         |
|lizard=/**chameleon **/    |  lizard="/**chameleon**/"    |     /** chamelon **/       |
|win_path=C:\path\to\dir\   |win_path="C:/path/to/dir/"|       C:\path\to\dir\      |


4. Considere el propósito y escriba el comando apropiado:

|                                       Proposito                                                         |                     Comando                          |
|---------------------------------------------------------------------------------------------------------|------------------------------------------------------|
| Establecer el lenguaje del actual shell al español UTF-8 (es_ES.UTF-8).                                 | export LANG=es_ES.UTF-8
| Imprime el nombre del directorio actual.                                                                | echo $PWD
| Referencia a la variable de entorno que almacena la información sobre las conexiones ssh.               |  echo $SSH_CONNECTION
| Establecer el PATH para incluir /home/carol/scripts como el último directorio para buscar ejecutables.  | export PATH="$PATH:/home/carol/scripts"
| Establecer el valor de`my_path` en PATH.                                                                | my_path=PATH
| Establecer el valor de my_path en el de PATH                                                            | export my_path=$PATH


5.  Crear una variable local llamada "mammal" y asígnale el valor gnu:

```
    mammal=gnu
```

6. Usando la sustitución de variables, cree otra variable local llamada var_sub con el valor apropiado para que cuando se haga referencia a través de echo $var_sub obtengamos: The value of mammal is gnu:

```
    var_sub="The value of mammal is $mammal"
```

7. Convierte a mammal en una variable de entorno:

```
    export $mammmal
```

8. Búscalo con set y grep:

```
    set | grep "mammal"
```

9. Búscalo con env y grep:

```
    env | grep "mammal"
```

10. Crear, en dos comandos consecutivos, una variable de entorno llamada BIRD cuyo valor es penguin:

```
    BIRD="penguin" ; export BIRD
```

11. Crear en dos comandos consecutivos, una variable de entorno llamada NEW_BIRD cuyo valor es yellow-eyed penguin:

```
    NEW_BIRD="yellow-eyed penguin" ; export NEW_BIRD
```

12. Asumiendo que eres user2, cree una carpeta llamada bin en tu directorio principal:

```
    mkdir ~/bin
```

13. Escriba el comando para agregar la carpeta ~/bin a su PATH para que sea la primera carpeta en la que bash busque binarios:

```
    export PATH="/user2/bin:$PATH"
```

14. Para garantizar que el valor de PATH permanezca inalterado en los reinicios, ¿Qué parte del código — en forma de una declaración if — agregarías en el ~/.profile?

```
if [ -d "/home/user2/bin" ];then
    export PATH="/home/user2/bin:$PATH"
fi
```


#### Ejercicios Exploratorios                                                           (PENDIENTE)















-> Leccion 3 
#### Ejercicios Guiados

1. Completa la tabla con Sí o No considerando las capacidades de los alias y las funciones:


|                   Caracteristicas                       |     Alias     |    funciones    | 
|---------------------------------------------------------|---------------|-----------------|
Las variables locales pueden ser usadas                   |       x       |         x       |
Las variables de entorno pueden ser utilizadas            |       x       |         x       |
Se puede escapar con \                                    |       x       |                 |
Puede ser recursivo.                                      |       x       |         x       |
Muy productivo cuando se usa con parámetros posicionales  |               |         x       |


2. Introduzca el comando que lista todos los alias en su sistema:

```
    alias
```
 
3. Escribe un alias llamado logg que enumere todos los archivos ogg en ~/Music — uno por línea:

```
    alias logg='ls -l ~/Music'
```

4. Invoca el alias para probar que funciona:

```
    logg
```

5. Ahora, modifica el alias para que imprima el usuario de la sesión y dos puntos antes del listado:

```
alias logg='Usuario: echo$USER ; ls -l ~/Music'
```

6. Invóquelo de nuevo para probar que esta nueva versión también funciona:

```
    logg
```

7. Enumera todos los alias de nuevo y comprueba que tu alias de logg aparece en el listado:

```
$ alias

alias logg='echo "Usuario $USER" ; ls -l ~/Music'
```

8. Quite el alias:

```
unalias logg
```

9. Estudie las columnas Nombre del alias,Comando(s) aliado(s) y asigne los alias a sus valores correctamente:


|Nombre del alias     | Comando(s) aliados                             | Asignacion de alias | 
|---------------------|------------------------------------------------|---------------------|
|b                    |               bash                             | alias b="bash"
|bash_info            |   which bash + echo "$BASH_VERSION"            | alias bash_info='which bash ; echo "$BASH_VERSION"'
|kernel_info          |               uname -r                         | alias kernel_info='uname -r'
|greet                |           echo Hi, $USER!                      | alias greet='echo "Hi, $USER!"'
|computer             |   pc=slimbook + echo My computer is a $pc      | alias computer='pc=slimbook ; echo "My coputer is a $pc"'


**Las comillas sinmples pueden ser remplazadas por comillas dobles, teniendo en cuenta que las simples son dinamicas mientras que las dobles son estaticas**

10. Como root, escribe una función llamada my_fun en etc/bash.bashrc. La función debe saludar al usuario y decirle cuál es su PATH. Invóque la función para que el usuario reciba los dos mensajes cada vez que inicie sesión:

```
my_fun(){

    echo "Hola $USER"
    echo "Tu path es $HOME"

}

```


11. Ingrese como user2 para comprobar que funciona:

```
su - user2
```

12. Escriba la misma función en una sola línea:

```

    my_fun() { echo "Hola $USER"; echo "Tu path es $PATH";}

```

13. Invoca la funcion

```
my_fun
```

Podemos exportarla para que sea global con:

```
    export my_fun
```

14. Remover la función:

```
unset -f my_fun
```


15. Esta es una versión modificada de la función special_vars:

```

$ special_vars2() {
> echo $#
> echo $_
> echo $1
> echo $4
> echo $6
> echo $7
> echo $_
> echo $@
> echo $?
>}
```


y este es el comando usado para invocarla

```
$ special_vars2 crying cockles and mussels alive alive oh
```

Que contenido tendra cada variable? 




16. Basándonos en la función de muestra (check_vids) en la sección “Función dentro de una función”, escribe una función llamada check_music para incluirla en un script de inicio bash que acepte parámetros posicionales para que podamos modificarla fácilmente:
◦ El tipo de archivo que se comprueba: ogg
◦ El directorio en el que se guardan los archivos: ~/Music
◦ El tipo de archivo que se guarda: music
◦ El número de archivos que se guardan: 7


```
check_vids(){
    
    # Directorio
    dir="~/Music"

    # Cantidad de elmentos
    cElements=`ls | wc -l`
    
    if [ $cElements -gt 7 ]; then
        echo "Solo son 7 archivos los que deberia de haber en esta carpeta"
        exit
    fi

    for i in *; do

        if [ ! $i == "*.ogg" ];then
            echo "Uno o mas arcivos no siguen la estructura song.ogg"
        fi 

        # Comprobar si es musica

    done


}
```


#### Ejercicios Exploratorios                                                               (PENDIENTE)

1. Las funciones de sólo lectura son aquellas cuyo contenido no podemos modificar. Haga una investigación sobre funciones de sólo lectura y complete la siguiente tabla:

| Nombre de la funcion | Haz que sea de solo lectura |  Lista de todas las funciones de solo lectura | 
|----------------------|-----------------------------|-----------------------------------------------|
|my_fun


2. Busca en la web cómo modificar PS1 y cualquier otra cosa que necesites para escribir una función llamada fyi (que se colocará en un script de inicio) que le da al usuario la siguiente información:
◦ Nombre del usuario
◦ Directorio principal
◦ Nombre del host
◦ Tipo de sistema operativo
◦ Buscar la ruta (PATH) de ejecutables
◦ Directorio de correo
◦ Con qué frecuencia se revisa el correo
◦ ¿Cuántos shells tiene la sesión actual?
◦ Prompt (deberías modificarlo para que muestre <user>@<host-date>)




### 105.2 - Personalizar y escribir scripts sencillos
-> Leccion 1 
#### Ejercicios Guiados
#### Ejercicios Exploratorios 

-> Leccion 1 
#### Ejercicios Guiados
#### Ejercicios Exploratorios 









## Tema 106: Interfaces de Usuario y Escritorios 
### 106.1 - Intalar y configurar X11
-> Leccion 1 
#### Ejercicios Guiados
#### Ejercicios Exploratorios 


### 106.2 - Escritorios graficos 

-> Leccion 1 
#### Ejercicios Guiados
#### Ejercicios Exploratorios 

# 106.3 Accesibilidad





## Tema 107: Tareas administrativas
### 107.1 - Administrar cuentas de usuario y de grupo y los arhcivos de sistema relacionados con ellas


-> Leccion 1 
#### Ejercicios Guiados


1. Para cada uno de los siguientes comandos, identifique el proposito correspondiente

| usermod -L        | Bloquear la cuenta de usuario | 
| passwd -u         | Desbloquear la cuenta de usuaio | 
| chage -E          | Setear la fecha de expiracion de una cuenta | 
| groupdel          | Eliminar un grupo | 
| useradd -s        | Setear el tipo de shell que va a usar un usuario nuevo | 
| groupadd -g       | agregar un grupo primario |
| userdel -r        | Eliminar un usuario y su directorio principal | 
| usermod -l        | Cambiar el nombre de la cuenta de usuario |
| groupmod -n       | Renombrar el grupo | 
| useradd -m        | Hacer que cuando se cree un usuario, se cree automaticamente su directorio home |

2. Para cada uno de los siguients comandos passwd, identifique el correspondiente comando `chage`:

| passwd -n | chage -m |
| passwd -x | chage -M | 
| passwd -w | chage -W |
| passwd -i | chage -I |  
| passwd -S | chage -l |

3. Explique en detalle el propósito de los comandos en la pregunta anterior:

- Establece la duracion minima de la contrasenia para una cuenta de usuario 
- Establece la duracion maxima de la contrasenia para una cuenta de usuario 
- Establece el numero de dias de advertencia antes que la contrasenia expire, durante los cuales se le advierte al usuario que debera cambiarla
- Establece el numero de dias de inactividad despues de que una contrasenia expira, durante los cuales el usuario debera actualizarla
- Muestra la informacion de una contrasenia 


4. ¿Qué comandos puedes usar para bloquear una cuenta de usuario? ¿Y qué comandos para desbloquearla?

Bloquear 

```
passwd -l user

chage -L
```

Desbloquear 

```
passwd -u user

chage -U user
```



#### Ejercicios Exploratorios 

1. Usando el comando groupadd, crea los grupos de administrators y developers. Asume que estás trabajando como root.

```
groupadd administrators

groupadd developers 
```


2. Ahora que ha creado estos grupos, ejecute el siguiente comando: useradd -G administrators, developers kevin. ¿Qué operaciones realiza este comando? Asume que  CREATE_HOME y USERGROUPS_ENAB están configurados en yes en el archivo /etc/login.defs.

```
Las opciones CREATE_HOME es usada para crear por defecto un directorio principal y USERGROUPS_ENAB especifica si se debe de crear po defecto un nuevo grupo para cada nueva cuenta de usuario con su mismo nombre 

El comando crear el usuario kevin, donde su grupo principal es kevin, su directorio por defecto es /home/kevin y ademas se le agrega a los grupo de administrators y developers

```


3. Crea un nuevo grupo llamado designers, renómbrelo a web-designers y añada este nuevo grupo a los grupos secundarios de la cuenta de usuario kevin. Identifica todos los grupos a los que pertenece kevin y sus identificaciones.

```
groupadd designers

groupmod -n web-desiners designers 

usermod -aG web-designers kevin

groups kevin o getent passwd kevin 

```

4. Elimina sólo el grupo de developers de los grupos secundarios de kevin.

```
    usermod -rG developers kevin 
```

5. Establezca la contraseña de la cuenta de usuario kevin.

```
passwd kevin
```


6. Usando el comando chage, primero compruebe la fecha de caducidad de la cuenta de usuario kevin y luego cámbiela al 31 de diciembre de 2022. ¿Qué otro comando puedes usar para cambiar la fecha de caducidad de una cuenta de usuario?

```
chage -l kevin 

chage -E "2022-12-31" kevin

usermod -e "2022-12-31"
```


7. Añade una nueva cuenta de usuario llamada emma con el UID 1050 y establezca administrators como su grupo primario y developers y web-designers como sus grupos secundarios.

```
useradd -u 1050 -g administratos -G developers,webdisigners emma
```

8. Cambie el shell de inicio de sesión de emma a /bin/sh.

```
usermod -s /bin/sh emma
```

9. Elimina las cuentas de usuario emma y kevin y los grupos de administrators, developers y web-designers

```
userdel emma

userdel kevin 

groupdel administratos
groupdel developers web-designers
```

-> Leccion 2
#### Ejercicios Guiados

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
root:$6$1u36Ipok$ljt8ooPMLewAhkQPf.lYgGopAB.jClTO6ljsdczxvkLPkpi/amgp.zyfAN680zrLLp2avvpd
KA0llpssdfcPppOp:18015:0:99999:7:::
mail:*:18015:0:99999:7:::
catherine:$6$ABCD25jlld14hpPthEFGnnssEWw1234yioMpliABCdef1f3478kAfhhAfgbAMjY1/BAeeAsl/FeE
dddKd12345g6kPACcik:18015:20:90:5:::
kevin:$6$DEFGabc123WrLp223fsvp0ddx3dbA7pPPc4LMaa123u6Lp02Lpvm123456pyphhh5ps012vbArL245.P
R1345kkA3Gas12P:18015:0:60:7:2::
# cat /etc/gshadow | grep '\(root\|mail\|db-admin\|app-developer\)'
root:*::
mail:*::
db-admin:!:emma:emma,grace
app-developer:!::catherine,dave,christian
```

- ¿Cuál es el ID de usuario (UID) y el ID de grupo (GID) de root y catherine ?

Para root es 0 y 0

Para catherine es 1030 y 1025

- ¿Cuál es el nombre del grupo primario de Kevin? ¿Hay otros miembros en este grupo?

Es db-admin y ademas de kevin, esta emma y grace

- ¿Cuál shell está asignado para el mail? ¿Qué significa?

/sbin/nologin

Significa que no se puede loggear como este usuario


- ¿Quiénes son los miembros del grupo de app-developer? ¿Cuáles de estos miembros son los administradores del grupo y cuáles son los miembros ordinarios?

los miemrbos del grupo app-developer son catherine,dave, christian y todos son miembros ordinarios 

◦ ¿Cuál es la duración mínima de la contraseña para catherine? ¿Y cuál es la duración máxima de la contraseña?

la duracion minima es 20 dias y la duracion maxima es 90

◦ ¿Cuál es el período de inactividad de la contraseña para kevin?

2 dias, durante este periodo kevin debe de actualizar la contrasenia, de lo contrario la cuenta sera desactivada


2. Por convención, ¿Qué identificaciones se asignan a las cuentas del sistema y cuáles a los usuarios ordinarios?

puede variar segun la configuracion del archivo login.cfg, sin embargo, lo mas ahnitual es que los usuaurios inicien desde 1000 hacia arriba

3. ¿Cómo puede saber si una cuenta de usuario, que antes podía acceder al sistema, ahora se encuentra bloqueada? Supongamos que su sistema utiliza contraseñas en la sombra.

Con el simbolo de ! antes de su password



#### Ejercicios Exploratorios 

1. Crea una cuenta de usuario llamada christian usando el comando useradd -m e identifica su ID de usuario (UID), ID de grupo (GID) y el shell.

```
# useradd -m christian
# cat /etc/passwd | grep christian
christian:x:1050:1060::/home/christian:/bin/bash

el UID es 1050 y el GUID es 1060, el tipo de bash es /bin/bash
```

2. Identifica el nombre del grupo primario de christian. ¿Qué puedes deducir?

```
# cat /etc/group | grep 1060
christian:x:1060:
```

El grupo primario fue creado al inicio por default y con el mismo nombre, por lo que la cofiguracion por default esta asi


3. Usando el comando getent, revisa la información de la contraseña de la cuenta del usuario christian.


```
getent shadow christian
```

4. Añade el grupo editor a los grupos secundarios de christian. Supongamos que este grupo ya contiene a Emma, Dave y Frank como miembros ordinarios. ¿Cómo puedes verificar que no hay administradores para este grupo?

```
usermod -aG editor christian


getent gshadow christian
```

Debemos de revisar el ultimo de los  apartados delimitados por dos puntos, por lo que no deberia de haber algun nombre o usuario 


5. Ejecute el comando ls -l /etc/passwd /etc/group /etc/shadow /etc/gshadow y describa la salida que imprime en términos de permisos de archivo. ¿Cuál de estos cuatro archivos están con "shadow" por razones de seguridad? Supongamos que tu sistema utiliza contraseñas shadow.

```
-rw-r--r-- 1 root root  934 Aug  6 10:08 /etc/group
-rw------- 1 root root  825 Aug  6 10:08 /etc/gshadow
-rw-r--r-- 1 root root 1601 Aug  6 10:08 /etc/passwd
-rw------- 1 root root  826 Aug  6 10:08 /etc/shadow

Tanto group y passwd se pueden leer sin ser parte de algun grupo o ser algun usuairo en especifico, y los demas con las contrasenias que es gshadow o shadow no son visibles ya que contiene las contrasenias de grupos y de usuarios
```




### Tema 107.2 - Automatizar tareas administrativas del sistema mediante la programacion de eventos


-> Leccion 1 
#### Ejercicios Guiados

1. Para cada uno de los siguientes atajos crontab, indique la especificación de tiempo correspondiente (es decir, las cinco primeras columnas de un archivo crontab de usuario):


| @hourly  |  00 * * * *    |
| @daily   |  00 00 * * *   |
| @weekly  |  00 00 * * 1   |
| @monthly |  00 00 1 * *   |
| @annually|  00 00 1 1 *   |

2. Para cada uno de los siguientes atajos OnCalendar, indique la especificación de tiempo correspondiente (la forma más larga):

| hourly  | OnCalendar=* *-*-* *:00:00 |
| dily    | OnCalendar=* *-*-* 00:00:00 | 
| weekly  | OnCalendar=Mon *-*-* 00:00:00 |
| monthly | OnCalendar=* *-*-1 00:00:00 |
| yearly  | OnCalendar=* *-1-1 00:00:00 |

3. Explique el significado de las siguientes especificaciones de tiempo que se encuentran en un archivo crontab

| 30 13 * * 1-5         | De lunes a viernes a las 13 hrs todos los meses | 
| 00 09-18 * * *        | Todos los dias, cada hora, desde las 9am hasta las 6pm | 
| 30 08 1 1 *           | Todos los anios, primero de enero a las 8:30 am |
| 0,20,40 11 * * Sun    | Todos los Domingos, a las 11:00, 11:20 y 11:40 |
| 00 09 10-20 1-3 *     | Todos los dias 10 hasta el dia 20 de los meses de  Enero, Febrero y Marzo a las 9am en punto |
| */20 * * * *          |  Todos los dias a los 20 minutos despues de pasar cada hora|


4. Explique el significado de las siguientes especificaciones de tiempo utilizadas en la opción OnCalendar de un archivo de temporizador:

| *-*-* 08:30:00             | Todos los dias a las 8:30am |
| Sat,Sun *-*-* 05:00:00     | Todos los Sabados y Domingos a las 5:00 am |
| *-*-01 13:15,30,45:00      | Todos los dias pimero a las 13:15, 13:30, a las 13:45
| Fri *-09..12-* 16:20:00    | Todos los Viernes del mes de Sep,Oct,Nov y Dic, a las 16:20
| Mon,Tue *-*-1,15 08:30:00  | Todos Lunes y Martes de los primeros quince dias de cada mes a las 8:30am
| *-*-* *:00/05:00           | Todos los dias



#### Ejercicios Exploratorios                                                                               (PENDIENTE)
1. Asumiendo que usted está autorizado a programar trabajos con cron como un usuario ordinario, ¿Qué comando usaría para crear su propio archivo crontab?

Como usuario normal

```
crontab -e 
```

2. Cree un trabajo simple y programado que ejecute el comando date todos los viernes a la 01:00 pm. ¿Dónde puede ver la salida de este trabajo? (????????????????)


```
00 13 * * fri /bin/date
```

La salida se envia cada minuto por correo, para visualizarlo se usa el comando mail


3. Cree otro trabajo programado que ejecute el script foobar.sh cada minuto, redirigiendo la salida al archivo output.log en su directorio de origen para que sólo se le envíe el error estándar por correo electrónico.

```
0/1 * * * * ./foobar.sh >> ~/output.log 
```

4. Mire la entrada crontab del nuevo trabajo programado. ¿Por qué no es necesario especificar la ruta absoluta del archivo en el que se guarda la salida estándar? ¿Y por qué puede usar el comando ./foobar.sh para ejecutar el script? (???????????????????)


cron invoca los comandos desde el directorio home del usuario, a menos que se especifique otra ubicación por la variable de entorno HOME dentro del archivo crontab. Por esta razón, puede utilizar la ruta relativa del archivo de salida y ejecutar el script con ./foobar.sh


5. Edite la entrada anterior crontab eliminando la redirección de salida y desactive el primer trabajo cron que había creado.

```
#00 13 * * fri /bin/bash

0/1 * * * * ./foobar.sh
```

6. ¿Cómo puede enviar la salida y los errores de un trabajo programado a la cuenta de usuario emma por correo electrónico? ¿Y cómo puede evitar enviar la salida y los errores estándar por correo electrónico?

Para ello debemos de agregar una variable `MAILTO` al archivo /etc/crontab

```
MAILTO="emma"
```

si no queremos enviarle a ningun usuario, 

```
MAILTO=""
```


7. Ejecute el comando ls -l /usr/bin/crontab. ¿Qué bit especial se establece y cuál es su significado?

El comando crontab tiene el bit SGID establecido (el caracter s en lugar del flag ejecutable para el grupo), lo que significa que se ejecuta con los privilegios del grupo (por lo tanto crontab). Es por esto que los usuarios comunes pueden editar su archivo crontab usando el comando crontab. Tenga en cuenta que muchas distribuciones tienen permisos de archivo establecidos de tal manera que los archivos crontab sólo pueden ser editados mediante el comando crontab.


-> Leccion 2
#### Ejercicios Guiados

1. Para cada una de las siguientes especificaciones de tiempo, indique cuál es válida y cuál no lo es para at:

| at 08:30 AM next week     | Correcto   | 
| at midday                 | Incorrecto |
| at 01-01-2020 07:30 PM    | Incorrecto |
| at 21:50 01.01.20         | Correcto   |
| at now +4 days            | Correcto   |
| at 10:15 PM 31/03/2021    | Incorrecto |
| at tomorrow 08:30 AM      | Incorrecto | La forma es at 8:30 AM tomorrow

2. Una vez que ha programado una tarea con at, ¿cómo puede revisar sus comandos?

```
    at -c Numero_de_la_tarea
```

3. ¿Qué comandos puede usar para revisar tu trabajos pendientes? ¿Qué comandos usaría para borrarlos?

```
    atrm numero_del_trabajo

    at -r numero_del_trabajo
```


4. Con systemd, ¿qué comando se utiliza como alternativa a at


```
systemd-run --on-calendar"YYYY-MM-DD HH:MM:SS" comando
```


#### Ejercicios Exploratorios 


1. Cree un trabajo que ejecute el script foo.sh ubicado en su directorio personal, a las 10:30 am del próximo 31 de octubre. Asuma que está actuando como un usuario ordinario.

No hace falta especificar la variable HOME, ya que por defecto, cron ejecuta desde el directorio personal principal

```
30 10 31 10 * foo.sh
```

2. Entre en el sistema como otro usuario ordinario y cree otra tarea con at que ejecute el script bar.sh mañana a las 10:00 am. Supongamos que el script se encuentra en el directorio principal del usuario.

```
at tomorrow 10 AM
>./user/bar.sh
```


3. Entra en el sistema como otro usuario ordinario y crea otra tarea con at que ejecute el script foobar.sh justo después de 30 minutos. Supongamos que el script se encuentra en el directorio principal del usuario.

```
at now +30 minutes
>./foobar.sh
```

4. Ahora como root, ejecute el comando atq para revisar los trabajos at programados de todos los usuarios. ¿Qué pasa si un usuario ordinario ejecuta este comando?

```
atq

atq -l 
```

Si un usuario ordinario ejecuta atq o at -l, solo vera los trabajos creador por el, mientras que root, podra ver los de todos lo usuarios

5. Como root, borre todos estos trabajos pendientes en at usando un solo comando.

```
for i in {1..50}; do atrm $i &>/dev/null; done
```

6. Ejecute el comando ls -l /usr/bin/at y examine sus permisos.

En esta distribución, el comando at tiene establecidos los bits SUID (el caracter s en lugar de la marca ejecutable para el propietario) y SGID (el caracter s en lugar de la marca ejecutable para el grupo), lo que significa que se ejecuta con los privilegios del propietario y del grupo del archivo (daemon para ambos). Es por eso que los usuarios comunes pueden programar trabajos con at.


### Tema 107.3 - Localizacion e internacionalizacion 

-> Leccion 1 
#### Ejercicios Guiados

1. Basado en la siguiente salida del comando date, ¿cuál es la zona horaria del sistema en notación GMT? 

```
$ date
Mon Oct 21 18:45:21 +05 2019
```

GMT+5


2. ¿A qué archivo debe apuntar el enlace simbólico /etc/localtime para que Europa/Bruselas sea la hora local por defecto del sistema?

debe de ser un enlace debil al siguiente archivo 

```
ln -s /usr/share/zoneinfo/Europe/Brussels /etc/localtime 

```


3. Es posible que los caracteres de los archivos de texto no se representen correctamente en un sistema con una codificación de caracteres diferente de la utilizada en el documento de texto. ¿Cómo podría usarse iconv para convertir el archivo codificado WINDOWS-1252 old.txt en el archivo new.txt usando la codificación UTF-8


```
$ iconv -f WINDOWS-1252 -t UTF-8 old.txt > new.txt
```


#### Ejercicios Exploratorios 
1. ¿Qué comando hará que Pacific/Auckland sea la zona horaria por defecto para la sesión de shell actual?

Esto se hace modificando la variable TZ, por lo que:

```
export TZ="Pacific/Auckland"
```


2. El comando uptime muestra, entre otras cosas, el promedio de carga del sistema en números fraccionarios. Utiliza la configuración actual de la región para decidir si el separador de decimales debe ser un punto o una coma. Si, por ejemplo, el locale actual está configurado como de_DE.UTF-8 (el locale estándar de Alemania), uptime utilizará una coma como separador. Sabiendo que en el idioma inglés americano el punto se usa como separador, ¿qué comando hará que uptime muestre las fracciones usando un punto en lugar de una coma para el resto de la sesión actual?

Para la separacion de decimales, modificaremos la siguiente variable 

```
LC_NUMERIC="en_US.UTF8"
```


3. El comando iconv reemplazará todos los caracteres fuera del conjunto con un signo de interrogación. Si se añade //TRANSLIT a la codificación de destino, los caracteres no representados en el conjunto de caracteres destino serán reemplazados (transliterados) por uno o más caracteres de aspecto similar. ¿Cómo podría usarse este método para convertir un archivo de texto UTF-8 llamado readme.txt a un archivo ASCII plano llamado ascii.txt?


```
iconv -f UTF-8 -t ASCII//TRANSLIT -o ascii.txt readme.txt
```







## Tama 108: Servicios esenciales del sistema
### 108.1 - Mantener la hora del sistema 

-> Leccion 1 
#### Ejercicios Guiados

1. Indique si los siguientes comandos están mostrando o modificando la hora del sistema o la hora del hardware:

|       Comando(s)                    | Sistema | Hardware | Ambos |
|-------------------------------------|---------|----------|-------|
|       'date -u'                     |   x     |          |       |
| 'hwclock --set --date "12:00:00"'   |         |     x    |       |
|       'timedatectl'                 |         |          |  X    |
|     'timedatectl | grep RTC'        |         |     x    |       |
|     'hwclock --hctosys'             |   x     |          |       |
|   'date +%T -s "08:00:00"'          |   x     |          |       |
|  'timedatectl set-time 1980-01-10'  |         |          |   X   |


2. Observe la siguiente salida, y luego corrija el formato del argumento para que el comando sea exitoso:

```
$ date --debug --date "20/20/12 0:10 -3"

date: warning: value 20 has less than 4 digits. Assuming MM/DD/YY[YY]
date: parsed date part: (Y-M-D) 0002-20-20
date: parsed time part: 00:10:00 UTC-03
date: input timezone: parsed date/time string (-03)
date: using specified time as starting value: '00:10:00'
date: error: invalid date/time value:
date:     user provided time: '(Y-M-D) 0002-20-20 00:10:00 TZ=-03'
date:        normalized time: '(Y-M-D) 0003-08-20 00:10:00 TZ=-03'
date:                                  ---- --
date:      possible reasons:
date:        numeric values overflow;
date:        incorrect timezone
date: invalid date ‘20/20/2 0:10 -3’
```

El formato de la fecha esta mal, segun la salida del comando el formato es anio, mes y dia, por lo que entonces el comando correcto es:

```
date --debug --sate "2020/02/12 00:10 -3"
```


3. Use el comando date y las secuencias para que el mes del sistema sea febrero. Deje el resto de la fecha y la hora sin cambios.

```
date +%m -s "2"

date --set="YYYY/02/DD"  -> Sustituyendo YYYY por el año y DD por el dia 
date -s "2020/02/12"

o 

date +%Y --set="02"

```

4. Asumiendo que el comando anterior tuvo éxito, use hwclock para ajustar el reloj del hardware desde el reloj del sistema.

```
hwclock --systohc
```

5. Hay un lugar llamado eucla. ¿De qué continente forma parte? Use el comando grep para averiguarlo.

```
find /usr/share/zoneinfo/ -iname "eucla"
```

6. Establezca su zona horaria actual en la de eucla.

Puede ser de varias maneras como:

```
rm /etc/localtime && ln -s $(find /usr/share/zoneinfo/ -iname "eucla") /etc/localtime

```

o puede ser como:

```
timedatectl set-timezone "Australia/Eucla"
```


#### Ejercicios Exploratorios 

1. ¿Qué método de ajuste de tiempo es el óptimo? ¿En qué escenario podría ser imposible el método preferido?

El metodo mas seguro es usando el comando timedatectl,

2. ¿Por qué cree que hay tantos métodos para lograr lo mismo, es decir, establecer la fecha y hora del sistema?

Generalmente algunas distribuciones no tienen systemd y no tiene el comando timedatectl por lo que es necesario usar comando como date o hwclock para ajustar mediante el, el reloj del sistema

3. Después del 19 de enero de 2038, Linux System Time requerirá un número de 64 bits para almacenar. Sin embargo, es posible que podamos elegir simplemente establecer un “nuevo epoch”. Por ejemplo, el 1 de enero de 2038 a medianoche podría establecerse una nueva época de 0. ¿Por qué cree que esto no se ha convertido en la solución preferida?


-> Leccion 2 

#### Ejercicios Guiados 
#### Ejercicios Exploratorios 


### 108.2 - Registros del sistema

-> Leccion 1
#### Ejercicios Guiados 

1. Qué utilidades/comandos utilizarías en los siguientes escenarios:

| Finalidad y archivo de registro                    |      Utilidad       |
|----------------------------------------------------|---------------------|
| Leer `/var/log/syslog.7.gz`                        | zless `/var/log/syslog.7.gz` |
| Leer `/var/log/syslog`                             | less `/var/log/syslog`  |
| Filtrar la palabra `renewal` en `/var/log/syslog`  | grep "renewal" `/var/log/syslog` |
| Leer `/var/log/faillog`                            | faillog -a  \| less  |
| Leer `/var/log/syslog` dinamicamente               | tail -f `/var/log/syslog` |


2. Reorganice las siguientes entradas de registro de manera que representen un mensaje de registro válido con la estructura adecuada: 

- debian-server

- sshd

- [515]:

- Sep 13 21:47:56

- Server listening on 0.0.0.0 port 22

El orden correcto es:

```
Sep 13 21:47:56 debian-server sshd [515]: Server listening on 0.0.0.0 port 22
```

3. Qué reglas añadirías a /etc/rsyslog.conf para cumplir con cada una de las siguientes:

- Enviar todos los mensajes de la instalación mail y una prioridad/gravedad de crit (y superior) a /var/log/mail.crit:

Clasical configuration:

```
mail.crit    /var/log/mail.crit
```

RainerScript configuration:

```
if ( $syslogfacility-text == "mail" and $syslogfacility == 2 ) then {
        action(type="omfile" file="/var/log/mail.crit")
}
```


- Envía todos los mensajes de la instalación mail con prioridades de alerta y emergencia a /var/log/mail.urgent:

Clasical configuration:

```
mail.alert;mail.emerg    /var/log/mail.urgent
```

RainerScript:

```
if ( $syslogfacility-text == "mail"  and ($syslogseverity-text == "alert" or $syslogseverity-text == "emerg")) then {
        action(type="omfile" file="/var/log/mail.urgent")
    }
```

- Excepto los procedentes de las instalaciones cron y ntp, envía todos los mensajes -independientemente de su facilidades y prioridad - a /var/log/allmessages:

Clasical Configuration:

```
*.*;cron,ntp.none   /var/log/allmessages
```

RainerScript Configuration:

```
if ( $syslogfacility-text != "cron" and $syslogfacility-text != "ntp" ) then {
        action(type="omfile" file="/var/log/allmessages")
    }
```

- Con todos los ajustes requeridos correctamente configurados primero, envíe todos los mensajes de la instalación mail a un host remoto cuya dirección IP es 192.168.1.88 usando TCP y especificando el puerto por defecto:

Para ello se mostrara la configuracion tanto del servidor como del cliente en ambos casos:


Classical Configuration:

Server 
```
# Ajustes previos requeridos para activar la recepción TCP:
$ModLoad imtcp
$InputTCPServerRun 514


$template RemoteLogs,"/var/log/remotehosts/mailRemote.log"
if $FROMHOST-IP=='192.168.1.4' then ?RemoteLogs
& stop
```

Client
```
mail.* @@192.168.1.88:514  # Para mensajes TCP
```

RainerScript:

Server
```
# Ajustes previos requeridos para activar la recepción TCP en RainerScript:
module(load="imtcp")
input(type="imtcp" port="514")

# Creación de la plantilla para el archivo dinámico
template(name="RemoteLogs" type="string" string="/var/log/remotehost/mailRemote.log")

if ( $FROMHOST-IP=="192.168.1.4" ) then{
       action(type="omfile" dynaFile="RemoteLogs")
       stop
}

```

Client
```
if($syslogfacility-text == "mail" ) then {
        #action(type="omfile" file="/var/log/mail.log")
        action(type="omfwd" target="192.168.1.88" port="514" protocol="tcp")
    }
```

- Independientemente de su facilidad, envía todos los mensajes con la prioridad warning (sólo con la prioridad warning`) a `/var/log/warnings evitando la escritura excesiva en el disco:

Clasical Configuration:
```
*.warn   -/var/log/warnings
```

RainerScrip:
```
if ($syslogseverity-text == "warning") then {
        action(type="omfile" file="/var/log/mail.crit" sync="off")
    }
```



4. Considere la siguiente sección de `/etc/logrotate.d/samba` y explique las diferentes opciones:

```
carol@debian:~$ sudo head -n 11 /etc/logrotate.d/samba
/var/log/samba/log.smbd {
        weekly
        missingok
        rotate 7
        postrotate
                [ ! -f /var/run/samba/smbd.pid ] || /etc/init.d/smbd reload > /dev/null
        endscript
        compress
        delaycompress
        notifempty
}
```

|       Opción	    |     Significado       |
|-------------------|-----------------------|
|       weekly      | Es el intervalo de rotacion, en este caso es semanalmente |
|      missingok    | Pasa al siguiente archivo sin emitir algun mensaje de error en caso de que no exista|
|       rotate 7    | Va a rotar los archivos 7 veces, por lo que hara log1.smbd hasta el log7.smbd|
|      postrotate   | Este script se ejecutara cada que sea rotado el documento|
|      endscript    | Aqui termina el scrip |
|       compress    | Comprime con gzip los archivos log|
|    delaycompress  | En combinación con comprimir, pospone la compresión al siguiente ciclo de rotación|
|    notifyempty    | No gire el registro si esta vacio |



#### Ejercicios Exploratorios 

1. En la sección “Plantillas y condiciones de filtrado” hemos utilizado uno basado en expresiones como condición de filtrado. Los filtros basados en propiedades son otro tipo de exclusivo de rsyslogd. Convierta nuestro filtro basado en expresiones en uno basado en propiedades:

|        Filtro basado en expresiones                 |     Filtro basado en propiedades        |
|-----------------------------------------------------|-----------------------------------------|
| `if $FROMHOST-IP=='192.168.1.4' then ?              | : fromhost-ip, isequal, "192.168.1.4" ?RemoteLog |
|   RemoteLogs`                                       | |


2. `omusrmsg` es un módulo integrado en `rsyslog` que facilita la notificación a los usuarios (envía mensajes de registro al terminal del usuario). Escribe una regla para enviar todos los mensajes de emergencia de todas las instalaciones tanto a `root` como al usuario regular `carol`

Classical Configuration
```
*.emerg                        :omusrmsg:root,carol
```

RainerScript
```
if ( $syslogpriority-text == "emerg") then { # prioridad emerg O SUPERIOR
    action(type="omusrmsg" users="root,carol")
}

Para aplicarlo, podemos usar la funcion de syslog prifilt()

if (prifilt("*.emerg")) then{
        action(type="omusrmsg" users="root,carol")
    }

```

-> Leccion 2 
#### Ejercicios Guiados 

1. Asumiendo que eres root, completa la tabla con el comando journalctl apropiado:

|                  Proposito                                                                   |              Comando         |
|----------------------------------------------------------------------------------------------|------------------------------|
| Imprimir entradas de kernel                                                                  | journalctl -k 
| Imprimir los mensajes del segundo arranque empezando por el principio del diario             | journalctl -b 1
|Imprimir los mensajes del segundo arranque comenzando por el final del diario                 | journalctl -b 1 -e        
| Imprimir los mensajes mas recientes del diario y seguir vigilando los nuevos                 | journalctl -f 
| Imprime solo los mensajes nuevos desde ahora, y actualiza la salida continuamente            | journalctl -f 
| Imprime los mensajes del arranque anterior con prioridad de advertencia y en orden inverso   | journalctl -p alert -r  o journalctl PRIORITY=1 -r 



2. El comportamiento del demonio del diario en relación con el almacenamiento está controlado principalmente por el valor de la opción Storage en /etc/systemd/journald.conf. Indique qué comportamiento está relacionado con qué valor en la siguiente tabla:

|                                               Comportamiento                                                                                | storage=auto |  Storage=none  | Storage=persistent | Storage=volatile | 
|---------------------------------------------------------------------------------------------------------------------------------------------|--------------|----------------|--------------------|------------------|
| Los datos del registro se desechan pero es posible el reenvio                                                                               |              |       X        |                    |                  |   
| Una vez que el sistema ha arrancado, los datos de registro se almacenaran en `/var/log/journal`. Si no esta presente, se crea el directorio |              |                |          X         |                  |
| Una vez arrancando el sistema, los datos de registro se almacenaran en `/var/log/journal`. Si no esta presente, el directorio no se creara  |       X      |                |                    |                  |
| Los datos de registro se almacenaran en `/var/run/journal` pero no existiran despues de los reinicios                                       |              |                |                    |         X        |  



3. Como ha aprendido, el diario se puede vaciar manualmente en función del tiempo, el tamaño y el número de archivos. Complete las siguientes tareas utilizando journalctl y las opciones apropiadas:

- Compruebe cuanto espacio de disco ocupan los archivos del diario

```
journalctl --disk-usage
```

- Reducir la cantidad de espacio reservado para los ficheros de diario archivados y fijarlo en 200MiB

Para ello modificaremos el archivo `/etc/systemd/journalctl.conf` y aregaremos lo siguiente 

```
SystemMaxFileSize=200M
RuntimeMaxFileSize=200M
```

- Vuelva a comprobar el espacio en disco y explique los resultados 

```
journalctl --disk-usage 
```

No hay correlación porque --disk-usage muestra el espacio ocupado tanto por los ficheros de diario activos como por los archivados, mientras que --vacuum-size sólo se aplica a los ficheros archivados.



#### Ejercicios Exploratorios 

1. ¿Qué opciones deberías modificar en /etc/systemd/journald.conf para que los mensajes sean reenviados a /dev/tty5? ¿Qué valores deberían tener las opciones?

Modificamos el archivo `/etc/systemd/journald.conf` y agregamos estas siguientes lineas 

```
ForwardToConsole=yes
TTYPath=/dev/tty5
```



2. Proporciona el filtro correcto journalctl para imrpimir lo siguiente:

|           Proposito                                               |               Filtro + Valor              |
|-------------------------------------------------------------------|-------------------------------------------|
| Imprimir los mensajes de un usuario especifico                    |             _ID=<user-id>
| Imprimir mensajes de un host llamado debian                       |            _HOSTNAME=debian
| Imprimir los mensajes de un grupo especifico                      |           _GID=<group-id>
| Imprime los mensajes que pertenecen a root                        |              _ID=0
| Basado en la ruta del ejecutable, imprime los mensajes de sudo    |            _EXE=/usr/bin/sudo
| Basado en el nombre del comando, imprime los mensajes de sudo     |             _COMM=sudo


3. Al filtrar por prioridad, los registros con una prioridad superior a la indicada también se incluirán en el listado; por ejemplo, el comando journalctl -p err imprimirá los mensajes de error, crítico, alerta y emergencia. Sin embargo, puedes hacer que journalctl muestre sólo un rango específico. ¿Qué comando usarías para que journalctl imprima sólo los mensajes de los niveles de prioridad warning, error y critical?

```

journalctl -p err..warning 
```

4. Los niveles de prioridad también se pueden especificar numéricamente. Vuelva a escribir el comando del ejercicio anterior utilizando la representación numérica de los niveles de prioridad:

```
journalctl -p 2..4

```




### 108.3 - Conceptos basicos del agente de tranferencia de correo

#### Ejercicios Guiados 

1. Sin más opciones o argumentos, el comando mail henry@lab3.campus entra en el modo de entrada para que el usuario pueda escribir el mensaje a henry@lab3.campus. Después de terminar el mensaje, ¿qué tecla cerrará el modo de entrada y enviará el correo electrónico?





2. ¿Qué comando puede ejecutar el usuario root para listar los mensajes no entregados que se originaron en el sistema local?

3. ¿Cómo puede un usuario sin privilegios utilizar el método MTA estándar para reenviar automáticamente todo su correo entrante a la dirección dave@lab2.campus?


#### Ejercicios Exploratorios 



1. Utilizando el comando mail proporcionado por mailx, ¿qué comando enviará un mensaje a emma@lab1.campus con el archivo logs.tar.gz como adjunto y la salida del comando uname -a como cuerpo del correo electrónico?

2. Un administrador de servicios de correo electrónico quiere supervisar las transferencias de correo electrónico a través de la red, pero no quiere saturar su buzón con mensajes de prueba. ¿Cómo podría este administrador configurar un alias de correo electrónico en todo el sistema para redirigir todo el correo electrónico enviado al usuario test al archivo /dev/null?

3. ¿Qué comando, además de newaliases, podría utilizarse para actualizar la base de datos de alias después de añadir un nuevo alias a /etc/aliases?




### 108.4 - Gestion de la impresion y de las impresoras

#### Ejercicios Guiados 

#### Ejercicios Exploratorios 



















## Tema 109: Fundamentos de redes
### 109.1 - Fundamentos de los protocolos de Internet

-> Leccion 1 
#### Ejercicios Guiados

1. Utilizando la dirección IP 172.16.30.230 y la máscara de red 255.255.255.224, identifique:

| La notación CIDR para la máscara de red                                           | /27               |
| Dirección de la red                                                               | 172.16.30.224       |  
| Dirección de Broadcast                                                            | 172.16.30.255     |
| Número de direcciones IP que se pueden utilizar para los hosts en esta subred     | 30


2. ¿Qué configuración se requiere en un host para permitir una comunicación IP con un host en una red lógica diferente?

Agregar la  Ruta por defecto en cada host y esto conectarlos a un router o switch

#### Ejercicios Exploratorios 

1. ¿Por qué los rangos de direcciones IP que empiezan por 127 y el rango posterior a 224 no están incluidos en las clases de direcciones IP A, B o C?

Son de otro tipo de red, generalmente estas apuntan a la propia maquina

2. Uno de los campos pertenecientes a un paquete IP que es muy importante es el TTL (Time To Live). ¿Cuál es la función de este campo y cómo funciona?

El TTL define el tiempo de vida de un paquete. Esto se implementa a través de un contador en el que el valor inicial definido en el origen se decrementa en cada puerta de enlace/enrutador por el que pasa el paquete, que también se denomina “salto”. Si este contador llega a 0, el paquete se descarta

3. Explique la función de NAT y cuándo se utiliza

La funcion nat sirve para traducir una direccion a otra, si en una red se usa 192.168.1.12, esta cuando pasa el router o el switch se convierte a 10.0.3.2 por ejmplo


-> Leccion 2 
#### Ejercicios Guiados

1. ¿Qué puerto es el predeterminado para el protocolo SMTP?

25


2. ¿Cuántos puertos diferentes hay disponibles en un sistema?

65532

3. ¿Qué protocolo de transporte garantiza que todos los paquetes se entreguen correctamente, verificando la integridad y el orden de los mismos?

TCP

4. ¿Qué tipo de dirección IPv6 se utiliza para enviar un paquete a todas las interfaces que
pertenecen a un grupo de hosts?

Broadcast



#### Ejercicios Exploratorios 

1. Mencione 4 ejemplos de servicios que utilizan el protocolo TCP por defecto.

ssh, http, mysql


2. ¿Cuál es el nombre del campo en el paquete de cabecera IPv6 que implementa el mismo recurso de TTL en IPv4?

hop limit


3. ¿Qué tipo de información es capaz de descubrir el Protocolo de Descubrimiento de Vecinos (NDP)?

Rutas arp 




### 109.2 - Configuracion de red persistente


-> Leccion 1 
#### Ejercicios Guiados

1. ¿Qué comandos se pueden utilizar para enumerar los adaptadores de red presentes en el sistema?

```
ip link

nmcli devices
```


2. ¿Cuál es el tipo de adaptador de red cuyo nombre de interfaz es wlo1?

```
Es el primer adaptador que el kernel encontro Wireless local area network (WLAN)
```

3. ¿Qué papel juega el archivo /etc/network/interfaces durante el arranque?

Ayuda a Activar una determinada interfaz durante el arranque, estas estan confgiruadas como `auto` y en el orden en que aparecen en la lista


4. ¿Qué entrada en /etc/network/interfaces configura la interfaz eno1 para obtener su configuración IP con DHCP?

```
auto eno1
iface eno1 inet dhcp

```

#### Ejercicios Exploratorios 
1. ¿Cómo podría usarse el comando hostnamectl para cambiar sólo el nombre de host estático de la máquina local a firewall?

```
hostnamectl set-hostname --static "firewall"
```

2. ¿Qué detalles, además de los nombres de host, pueden ser modificados por el comando hostnamectl?

hostnamectl también puede establecer el icono por defecto de la máquina local, su tipo de chasis, la ubicación y el entorno de despliegue

3. ¿Qué entrada en /etc/hosts asocia los nombres firewall y router con la IP 10.8.0.1?

```
10.8.0.1    firewall router
```

4. ¿Cómo se podría modificar el archivo /etc/resolv.conf para enviar todas las peticiones DNS a 1.1.1.1?

```
nameserver 1.1.1.1
```

-> Leccion 2
#### Ejercicios Guiados
1. ¿Qué significa la palabra Portal en la columna CONNECTIVITY en la salida del comando nmcli general status?

Significa que la red requiere de pasos adicionales para autenticarse (Normalmente a travez de un navegador)


2. En un terminal de consola, ¿cómo puede un usuario normal utilizar el comando nmcli para conectarse a la red inalámbrica MyWifi protegida por la contraseña MyPassword?

```
nmcli dev wifi connect MyWifi password "MyPassword"
```

3. ¿Qué comando puede encender el adaptador inalámbrico si el sistema operativo lo ha desactivado previamente?

```
nmcli radio wifi on
```

4. ¿En qué directorio deben colocarse los archivos de configuración personalizados cuando systemd-networkd gestiona las interfaces de red?

/lib/systemd/network El directorio de la red del sistema.

/run/systemd/network El directorio de red volátil en tiempo de ejecución.

/etc/systemd/network El directorio de red de la administración local.


Siendo el ultimo el de mayor prioridad


#### Ejercicios Exploratorios 

1. ¿Cómo puede un usuario ejecutar el comando nmcli para eliminar una conexión no utilizada llamada Hotel Internet?

```
nmcli connection del "Hotel Internet"
```

2. NetworkManager escanea las redes wi-fi periódicamente y el comando nmcli device wifi list sólo lista los puntos de acceso encontrados en el último escaneo. ¿Cómo debería usarse el comando nmcli para pedir a NetworkManager que vuelva a escanear inmediatamente todos los puntos de acceso disponibles?

```
nmcli dev wifi --rescan 
```


3. ¿Qué entrada name debe utilizarse en la sección [Match] de un archivo de configuración systemd-networkd para que coincida con todas las interfaces ethernet?

La entrada name=en*, ya que en es el prefijo de las interfaces ethernet en Linux y systemdnetworkd acepta globos de tipo shell

4. ¿Cómo debe ejecutarse el comando wpa_passphrase para utilizar la frase de paso dada como argumento y no desde la entrada estándar?

La contraseña debe darse justo después del SSID, como en wpa_passphrase MyWifi MyPassword.




### 109.3 - Resolucion de problemas basicos de red

-> Leccion 1 
#### Ejercicios Guiados
1. ¿Qué comandos se pueden utilizar para listar las interfaces de red?

```
ifconfig 

ip link

ls -l /sys/class/net/
```

2. ¿Cómo se desactiva temporalmente una interfaz? ¿Cómo se vuelve a habilitar?

```
ip link set wlan0 down

ifconfig wlan0 down 

```

3. ¿Cuál de las siguientes es una máscara de subred razonable para IPv4?

| 0.0.0.255    | incorrecto |
| 255.0.255.0  | incorrecto |
| 255.252.0.0  | correcto   |
| /24          | correcto   |

4. ¿Qué comandos puede utilizar para verificar su ruta por defecto?

```
ip route 

route 
```

5. ¿Cómo se añade una segunda dirección IP a una interfaz?

```
ip addr add 192.168.1.33/24 dev enp0s3

```


#### Ejercicios Exploratorios                                                                                                                   (PENDIENTE)
1. ¿Qué subcomando de ip se puede utilizar para configurar el etiquetado vlan?

```
```

2. ¿Cómo se configura una ruta por defecto?

3. ¿Cómo se puede obtener información detallada sobre el comando ip neighbour? ¿Qué sucede si lo ejecuta por sí mismo?

4. ¿Cómo se hace una copia de seguridad de la tabla de enrutamiento? ¿Cómo se restaura desde ella?

5. ¿Qué subcomando ip puede utilizarse para configurar las opciones del árbol de expansión?


-> Leccion 2
#### Ejercicios Guiados


1. ¿Qué comando(s) utilizaría para enviar un eco ICMP a learning.lpi.org?

```
ping learning.lpi.org
```

2. ¿Cómo podría determinar la ruta a 8.8.8.8?

```
traceroute 8.8.8.8
```

3. ¿Qué comando le mostraría si algún proceso está escuchando en el puerto TCP 80?

```

ss -ln | grep ":80"
netstat -ln | grep ":80"

lsof -Pi:80
```

4. ¿Cómo se puede saber qué proceso está escuchando en un puerto?

```
ss -lnp | grep ":22"
netstat -lnp | grep ":22"
```

5. ¿Cómo se puede determinar la MTU máxima de una ruta de red?

```
```


#### Ejercicios Exploratorios 








### 109.4 Configuracion DNS del lado del cliente
-> Leccion 2
#### Ejercicios Guiados
#### Ejercicios Exploratorios 



## Tema 110: Seguridad
### 110.1 - Tareas de administracion de seguridad
### 110.2 - Configuracion de la seguridad del sistema 
### 110.3 - Proteccion de datos mediante cifrado











## Tema 105: Shells y Scripts
### 105.1 - Personalizar y usar el entorno de shell
### 105.2 - Personalizar y escribir scripts sencillos

## Tema 106: Interfaces de Usuario y Escritorios 
### 106.1 - Intalar y configurar X11
### 106.2 - Escritorios graficos 
### 106.3 Accesibilidad

## Tema 107: Tareas administrativas
### 107.1 - Administrar cuentas de usuario y de grupo y los arhcivos de sistema relacionados con ellas
### 107.2 - Automatizar tareas administrativas del sistema mediante la programacion de eventos
### 103.2 - Localizacion e internacionalizacion 

## Tama 108: Servicios esenciales del sistema
### 108.1 - Mantener la hora del sistema 
### 108.2 - Registros del sistema
### 108.3 - Conceptos basicos del agente de tranferencia de correo
### 108.4 - Gestion de la impresion y de las impresoras

## Tema 109: Fundamentos de redes
### 109.1 - Fundamentos de los protocolos de Internet
### 109.2 - Configuracion de red persistente
### 109.3 - Resolucion de problemas basicos de red
### 109.4 - Configuracion DNS en el lado del cliente 

## Tema 110: Seguridad
### 110.1 - Tareas de administracion de seguridad
### 110.2 - Configuracion de la seguridad del sistema 
### 110.3 - Proteccion de datos mediante cifrado


# Personalizar el entorno de shell

Lo primero que el bash o cualquier otra  shell haria, es ejecutar un serie de scripts de inicio. Estos scripts personalizan el entorno de sesion, en estos script spodemos elegir las preferencias o configuraciones que mejor se adapten a las necesidades

### Tipos de shell: Interactivo vs No interactivo y Inicio de sesion vs Sin inicio de sesion 
Comencemos aclarando los conceptos de interactuvo e inicio de sesion en el contexto de shells:

- Shells interactivos / no interactivos
Este tipo de shell se refiere a la intercomunicación entre el usuario y el shell: Mediante el teclado el usuario proporciona la entrada digitando comandos en la terminal y su vez el shell proporciona la salida imprimiendo mensajes en la pantalla

- Shells de inicio de sesion / Shells sin inicio de sesion 
Este tipo de shell se refiere al evento de un usuario cuando accede a un sistema informático por medio sus credenciales, como el nombre de usuario y la contraseña


- shells Interactivo de inicio de sesión se ejecutan cuando los usuarios se conectan al sistema y se utilizan para personalizar las configuraciones de los usuarios según sus necesidades
- Para shells Interactivos sin inicio de sesión nos referimos a cualquier otro shell abierto por el usuario después de entrar en el sistema


```
Los shells de inicio de sesión no interactivos son bastante raros y poco prácticos. Sus usos son virtualmente inexistentes y sólo los comentaremos para comprender su comportamiento. Algunos ejemplos extraños incluyen forzar un script a ser ejecutado desde un shell de inicio de sesión con /bin/bash --login <some_script> o canalizar la salida estándar (stdout) de un comando a la entrada estándar (stdin) de una conexión ssh:

<some_command> | ssh <some_user>@<some_server>

En cuanto el shell interactivo sin inicio de sesión no hay interacción ni login en nombre del usuario, por lo que aquí nos referimos al uso de scripts automatizados. Estos scripts se utilizan principalmente para llevar a cabo tareas administrativas y de mantenimiento repetitivas como las incluidas en los cronjobs. En algunos casos, bash no lee ningún archivo de inicio.
```

### Iniciando una terminal 
Cuando se entra a un entorno de escritorio, se puede abrir un terminal o cambiar a una de las consolas del sistema y esta shell es un pts cuando se abre desde un emulador de terminal en el GUI o una tty cuando se ejcuta desde una consola del sistema 

* tty(Teletype): se refiere a los dispositivos de consola reales, como los accesibles con ctrl+alt+F1 a F6, para ver las que estan conectadas, usamos who y saber en cual estamos usamos tty, la f7 nos devolvera de vuelta a la sesion de escritorio

* pts (Pseudo terminal slave): Se crean cuando abres una terminal en modo grafico o inicias una sesion remota, accesibles en /dev/pts
   

En resumen, si estás frente a la máquina en la consola de texto, usarás un tty. Si estás conectado por SSH o en una ventana de terminal en escritorio, estás usando pts


pts un emulador de terminales, como parte de las sesiones graficas, los emuladores de terminales como gnome-terminal o konsole son muy amplios en carcateristicas 

Los emuladores de terminal menos extenso en carcateristicas incluyen --entre otros-- Xterm y sakura


### Ejecutando shells con bash 
Cuando ejecutamos "bash" en una terminal, estariamos abriendo un proceso hijo, o hijo del shell actual

```
bash -l o bash --login  Invocara un shell de inicio de sesion

bash -i Invocara un shell interactivo 

bash --noprofile    Con shell de inicio de sesion ignorara tanto el archivo de inicio de toodo el sistema /etc/profile como archivos de inicio a nivel de usuario ~/bash_profile, ~/.bash_login y ~/.profile

bash --norc Con shell interactivo ignorará tanto el archivo de inicio del sistema /etc/bash.bashrc como el archivo de inicio a nivel de usuario ~/.bashrc

bash --rcfile <file>    Con shell interactivo tomara <file> como el archivo de inicio ignorando a nivel de sistema /etc/bash.bashrc y a nivel de usuario ~/.bashrch 
```


### Ejecutando shells con su u sudo 

su - Cambia el ID de usuario o lo convierte en superusuario (root), podemos obtener ambos shells, el de inicio de sesion y sin inicio de sesion 

```
su - user2, su -l user2 o su --login user2 iniciará un shell de inicio de sesión interactivo como user2.

su user2 iniciará un shell interactivo y sin inicio de sesión como user2.

su - root o su - iniciará un shell de inicio de sesión interactivo como root.

su root o su iniciará un shell interactivo y sin inicio de sesión como root
```

sudo - Ejecuta comandos como otro usuario (incluyendo el superusuario). Debido a que este comando se usa principalmente para obtener privilegios temporales de root, el usuario que lo use debe estar en el archivo sudoers

```
Para añadir usuarios a sudoers necesitamos convertirnos en root y luego ejecutar:

root@debian:~# usermod -aG sudo user2

Así como su, sudo nos permite invocar tanto los shells de inicio de sesión como los de no de inicio de sesión:

    sudo su - user2, sudo su -l user2 o sudo su --login user2 iniciará un shell de inicio de sesión interactivo como user2.

    sudo su user2 iniciará un shell interactivo sin inicio de sesión como user2.

    sudo -u user2 -s iniciará un shell interactivo sin inicio de sesión como user2.

    sudo su - root or sudo su - iniciará un shell de inicio de sesión interactivo como root.

    sudo -i iniciará un shell de inicio de sesión interactivo como root.

    sudo -i <some_command> iniciará un shell de inicio de sesión interactivo como root, ejecuta el comando y volverá al usuario original.

    sudo su root or sudo su iniciará un shell interactivo sin inicio de sesión como root.

    sudo -s or sudo -u root -s iniciará un shell sin inicio de sesión como`root`.


```

### Que tipo de shell tenemos ? 
La variable $0 nos dice en que terminal estamos trabajando, podemos obtener la siguiente salida:
- Inicio de sesion interactivo 
    -bash or -su 

- Sin inicio de sesion interactivo
    bash or /bin/bash

- Sin inicio de sesion no interactivo (scripts)
    <name_of_script>


#### Cuantas shell tenemos?
para pbtener el numerp de shells que tenemos, podemos usar el siguiente comando:

```
user2@debian:~$ ps aux | grep bash
user2       5270  0.1  0.1  25532  5664 pts/0    Ss   23:03   0:00 bash
user2       5411  0.3  0.1  25608  5268 tty1     S+   23:03   0:00 -bash
user2       5452  0.0  0.0  16760   940 pts/0    S+   23:04   0:00 grep --color=auto bash

Observemos que en el apartado de comando esta bash y -bash 

bash  shell interactivo SIN login
-bash login shell
 
```

### De donde shell obtiene la configuracion: Archivos de inicio???

**Shell interactivo de inicio de sesion**

**Nivel global**

- /etc/profile  Este es el archivo .profile de todo el sistema para el shell Bourne y los shells compatibles con Bourune (incluido bash )
- /etc/profile.d/*  Este directorio puede contener scripts que son ejecutados por /etc/profile


**Nivel local**

- ~/.bash_profile Este archivo especifoc de bash se usa para configurar el entorno del ususario, tambien para crear el ~/.bash_login y ~/.profile.

- ~/.bash_login   Este archivo (específicamente), sólo se ejecutará si no hay un archivo ~/.bash_profile. Su nombre sugiere que debería ser usado para ejecutar los comandos necesarios para el inicio de sesión.

- ~/.profile    Este archivo no es específico de Bash y se obtiene sólo si no existe ~/.bash_profile ni ~/.bash_login, que es lo que normalmente ocurre. Por lo tanto, el propósito principal de ~/.profile es el de revisar si se está ejecutando un shell de Bash, y si fuese afirmativo, obtener ~/.bashrc (si existe). Normalmente establece la variable PATH para que incluya el directorio privado del usuario ~/bin (si existe).

- ~/.bash_logout    Si existe, este archivo específico de Bash hace algunas operaciones de limpieza al salir del shell. Esto puede ser conveniente en casos como los de las sesiones remotas.


**Shell interactivo sin inicio de sesion**

**Nivel global**

- /etc/bash.bashrc    Este es el archivo .bashrc de todo el sistema para los shells interactivos bash. A través de su ejecución, bash se asegura de que se está ejecutando interactivamente, comprueba el tamaño de la ventana después de cada comando (actualizando los valores de LÍNEAS y COLUMNAS, si es necesario) y establece algunas variables.

**Nivel Local**
- ~/.bashrc   este archivo específico de Bash suele establecer algunas variables de historial y origen ~/.bash_aliases (si existe). Aparte de eso, este archivo se utiliza normalmente para almacenar alias y funciones específicas de los usuarios



**Tanto el archivo global como el local seran leidos. Aunque los archivos locales tienen prioridad sobre los globales**

**Tipicamente /etc/profile y ~/.profile sea aseguran de que tanto /etc/bash.bashrc como ~/.bashrc se ejcuten despues de un inicio de sesion exitoso. La salida del siguiente comando muestra este fenomeno:**

``` 
root@debian:~# su - user2
Hello from /etc/bash.bashrc
Hello from /etc/profile
Hello from /home/user2/.bashrc
Hello from /home/user2/.profile

Con su - <usuer> (o con -l y --login), se garantiza la invocacion de un shell de acceso, mientras que  su <user>, solo habria invocado /etc/bash.bashrc y ~/.bashrc
```

Teniendo en cuenta que se agregaron lineas a los archivos de configuracion para ver su ejecucion, este fue el flujo que salio:

1. Hello from /etc/bash.bashrc significa /etc/profile se ha obtenido /etc/bash.bashrc.
2. Hello from /etc/profile significa /etc/profile ha sido completamente leído y ejecutado.
3. Hello from /home/user2/.bashrc significa ~/.profile se ha obtenido ~/.bashrc.
4. Hello from /home/user2/.profile significa ~/.profile ha sido completamente leído y ejecutado.


### Archivos fuente
Este mecanismo se llama "sourcing" y se explica en esta seccion 

**Ejecutando archivos con "."**
Esto lo podemos ver en el siguiente script

``` 
    #include .bashrc if exist
    if [ -f "$HOME/ .bashrc" ]; then
        . "$HOME/.bashrc"
    fi
```

Ademas, podemos usar el . siempre que hayuamos modificado un archivo de inicio y queramos hacer efectivos los cambios sin necesidad de reiniciar

```
. ~/.bashrc
```

**Ejecutando archivos con "source"**
El comando source es un sinonimo de ".", por lo que podemos ejecutar tambien asi:

```
    source ~/.bashrc
```

**El origen de los archivos de inicio de Shell: SKEL**
Cuando creamos a un usuario con la oopcion -m, se crea un directorio para este mismo, los archivos que estan por defecto,son gracias a este directorio, donde se encuentra lo que deberia de haber en un inicio, por ejemplo:

Si en nuestro directorio /etc/skell esta este archivo
- README  
- init.sh
- .bashrc
- .profile


cuando creemos un usuario con
```
    useradd -m -s /bin/bash user

    Y listemos su contenido, sera:
    cd /home/user

    drwx------. 1 user user  108 Mar 11 04:50 .
    dr-xr-xr-x. 1 root root   40 Mar 11 04:50 ..
    -rw-r--r--. 1 user user    0 Mar 11 04:49 .bashrc
    -rw-r--r--. 1 user user    0 Mar 11 04:49 .profile
    -rw-r--r--. 1 user user   31 Mar 11 04:28 README
    -rw-r--r--. 1 user user   31 Mar 11 04:28 init.sh
```




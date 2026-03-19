# Ejercicios Guiados 


1. Estudie como se ha iniciado los shells en la columna "iniciando con.." y complete la informacion requerida:


| Iniciando con                                                       | Interactivo? | Inicio de sesion ? | Resultado de echo $0 |
|:--------------------------------------------------------------------|:-------------|:-------------------|:---------------------|
| sudo ssh user2@machine2                                             |     *        |        *           |         -bash        |
| Ctrl+Alt+F2                                                         |     *        |        *           |         -bash        |
| su - user2                                                          |     *        |        *           |         -bash        |  
| gnome-terminal                                                      |     *        |                    |         bash         |
| Un usuario regular usa konsole para iniciar una instancia de sakura |     *        |                    |         /bin/bash    | 
| Un script llamado test.sh que contiene el comando echo $0           |              |                    |         ./test.sh    | 

2. Escriba los comandos su y sudo para lanzar el shell especificado 

Shell de inicio de sesión interactivo como user2
    su: su - user2
    sudo: sudo --login user2

Shell de inicio de sesión interactivo como root
    su: su - root
    sudo: sudo -l root


Shell interactivo sin inicio de sesión como root
    su: su root
    sudo: sudo root

Shell interactivo sin inicio de sesión como user2
    su: su user2
    sudo: sudo user2 

3. Que archivo de inicio se lee cuando se inicia el shell bajo "Tipo de shell"??

Tipo de shell                                       | /etc/profile |  /etc/bash.bashrc  |   ~/.profile   |  ~/.bashrc  |
|:--------------------------------------------------|:-------------|:-------------------|:---------------|:------------|
Shell de inicio de sesion interactivo como user2    |     *        |                    |        *       |             |  
Shell de inicio de sesion interactivo como root     |     *        |                    |        *       |             |
Shell interactivo sin inicio de sesion como root    |              |         *          |                |      *      | 
Shell interacitvo sin inicio de sesion como user2   |              |         *          |                |      *      |


# Ejercicios exploratorios 

1. En bash podemos escribir una simple función "¡Hola mundo!" incluyendo el siguiente código en un archivo vacío:

```
    function hello(){
        echo "Hello world"
    }
```

* ¿Qué deberíamos hacer a continuación para que la función esté disponible para shell?    
Usamos source o ".", esto hara que esta funcion sea interpretada en la sesion actual y por eso queda disponible, lo podemos ver con el conmadno set, que nos mostrara la configuracion para la sesion actual ahi podremos ver la funcion agregada

* Una vez que esté disponible para el shell actual, ¿cómo lo invocarías?
Como un simple comando, usando 

```
    $> hello
```

Esto nos retornara el mensaje de "Hello world"

* Para automatizar las cosas, ¿en qué archivo agregaría la función y su invocación para que se ejecute cuando user2 abra una terminal de una sesión de Ventanas X (X Windows)? ¿Qué tipo de shell es?
Podemos agregarlo en dos, lo agregamos en /etc/bash.bashrc si queremos que se ejcute para CUALQUIER usuario y lo agregamos a ~/.bashrc para que este disponible para el usuario user2 solamente


* ¿En qué archivo pondrías la función y su invocación para que se ejecute cuando root lance un nuevo shell interactivo independientemente de si es de inicio de sesión o no?
En el archivo /etc/bash.bashrc que es el archivo de shell interactivo sin inicio de sesion 

2. Observemos el siguiente script, ¡Hola mundo! de Bash:

```
#!/bin/bash

#hello_world: a simple bash script to discuss interaction in scripts.

echo "Hello world!"
```

* Supongamos que establecemos permisos de ejecución y lo ejecutamos. ¿Sería un script interactivo? ¿Por qué?
* ¿Qué hace que un script sea interactivo?

3. Imagina que has cambiado los valores de algunas variables en ~/.bashrc y quieres que esos cambios surtan efecto sin reiniciar. Desde tu directorio principal, ¿cómo podrías lograrlo de dos maneras diferentes?

Lo podemos hacer con source <archivo> o con . <archivo>

4. John acaba de iniciar una sesión de X wIndows en un servidor Linux. Abre un emulador de terminal para realizar algunas tareas administrativas pero, sorprendentemente, la sesión se congela y necesita abrir un shell de texto.
* ¿Cómo puede abrir esa tty?
Con Ctrl + alt + F1-7

* ¿Qué archivos de inicio se obtendrán?
/etc/profile o ~/.profile

5. Linda es una usuaria de un servidor Linux. Le pide amablemente al administrador que tenga un archivo ~/.bash_login para que pueda tener la hora y la fecha impresa en la pantalla cuando se conecte. A otros usuarios les gusta la idea y siguen el ejemplo. El administrador tiene dificultades para crear el archivo para los otros usuarios del servidor, así que decide añadir una nueva política y crear un archivo ~/.bash_login para todos los nuevos usuarios. ¿Cómo puede el administrador realizar esa tarea?

Puede hacer tres cosas, agregar un archivo .bash_profile en el directorio /etc/skell,  modificar el /etc/profile o agregar un script que imprima la hora y fecha en /etc/profile.d/ 
**puede ser .bash_profile, .bash_login o .profile**




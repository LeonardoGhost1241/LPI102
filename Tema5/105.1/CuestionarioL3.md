# Ejercicios guiados 

1. Completa la tabla con Si o No considerando la capacidad de los alias y las funciones

| Caracteristicas   | Alias | Funciones |
|-------------------|-------|-----------|
|Las variables locales pueden ser usadas | SI | Si |
|Las variables de entorno pueden ser utilizadas | SI | SI|
|Se puede escapar con "\\" | Si | No |
|Puede ser recursivo | Si |SI|
|Muy productivo cuando se usa con parametros posicionales| NO | SI |


2. Introduzca el comando que lista todos los alias en sus sitema

```
$ alias
```

3. Escribe un alias llamado logg que enumere todos los archivos ogg en ~/Music -- uno por uno

```
alias logg='ls -l ~/Music/*ogg*' --> Como no se si es una extension o que sea, puse dos comodines de asterisco en cada lado
```

4. Invoca el alias para probar que funciona
Probe pero con archivos pdf de mi sistema en mi carpeta de Downloads

```
 [leonardo@192.168.1.77]$  alias logg='ls -l ~/Downloads/*.pdf'
 [leonardo@192.168.1.77]$  logg
-rw-r--r-- 1 leonardo leonardo  939297 Mar 16 03:02 '/home/leonardo/Downloads/1.1.+Introduccion+[pres]-1.pdf'
-rw-r--r-- 1 leonardo leonardo  939297 Mar  7 02:33 '/home/leonardo/Downloads/1.1.+Introduccion+[pres].pdf'
-rw-r--r-- 1 leonardo leonardo  597628 Mar 16 02:40 '/home/leonardo/Downloads/1.2.+Curso+Python+[Pres]-1.pdf'
-rw-r--r-- 1 leonardo leonardo  597628 Mar  7 02:30 '/home/leonardo/Downloads/1.2.+Curso+Python+[Pres].pdf'
-rw-r--r-- 1 leonardo leonardo 1985632 Mar 16 11:55 '/home/leonardo/Downloads/2.+Analisis+de+datos+[pres].pdf'
-rw-r--r-- 1 leonardo leonardo  116254 Mar 10 18:02 '/home/leonardo/Downloads/Codigo de barras de Amazon.pdf'
 [leonardo@192.168.1.77]$

Por lo que funciona
```

5. Ahora modifica el alias para que imprima el usuario de la sesion y dos puntos antes del listado 

```
alias logg="echo : $USER"
```

6. Invoquelo de nuevo para probar que esta nueva version tambien funciona

```
[leonardo@192.168.1.77]$  alias
alias grep='grep --color=auto'
alias logg='echo : leonardo'
alias ls='ls --color=auto'
[leonardo@192.168.1.77]$  logg
: leonardo
[leonardo@192.168.1.77]$

```

7. Enumera todos los alias de nuevo y comprueba que tu alias de logg aparece listado

```
 [leonardo@192.168.1.77]$  alias
alias grep='grep --color=auto'
alias logg='echo : leonardo'
alias ls='ls --color=auto'
 [leonardo@192.168.1.77]$ 
```

8. Quita el alias
```
 [leonardo@192.168.1.77]$  alias
alias grep='grep --color=auto'
alias ls='ls --color=auto'
 [leonardo@192.168.1.77]$
```

9. Estudie las columnas Nombre del alias,Comando(s) aliado(s) y asigne los alias a sus valores correctamente:

| Nombre del alias | Comando(s) aliado(s)| Asignacion de alias|
|------------------|---------------------|--------------------|
|b  |   bash    |   alias b=bash    |
|bash_info  |   which bash + echo "$BASH_VERSION"   |   alias bash_info='which bash && echo "$BASH_VERSION"'    |
|kernel_info    |   uname -r    |   alias kernel_info='uname -r'    |
|greet  |   echo Hi, $USER! |   alias greet='echo "Hi, $USER!"'|
|computer   |   pc=slimbook + echo My computer is a $pc| alias computer='pc=slimbook ; echo "My computer is a $pc"'|


10. Como root, escribe una función llamada my_fun en etc/bash.bashrc. La función debe saludar al usuario y decirle cuál es su PATH. Invóque la función para que el usuario reciba los dos mensajes cada vez que inicie sesión

Para que salude cada vez que ingrese al sistema primero modificaremos el archivo con 

```
    vim /etc/bash.bashrc

    Ya dentro escribimos lo siguiente:
    my_fun(){
        echo "Hola, $USER, BIENVENIDO!!!!"
        echo "Este $HOME es tu directorio"
    }

```

Despues de agregarlo con source /etc/bash.bashrc

11. Ingrese como user2 para comprobar

```
 [leonardo@192.168.1.77]$  vim bashrrc
 [leonardo@192.168.1.77]$  source bashrrc 
 [leonardo@192.168.1.77]$  my_fun 
Hola, leonardo, BIENVENIDO!!!!
Este /home/leonardo es tu directorio
 [leonardo@192.168.1.77]$  
```

12. Escriba la misma funcion en una sola linea 
```
myfun() {echo "Hola, $USER, BIENVENIDO!!!!"; echo "Este $HOME es tu directorio"; }
```

13. Invoque la funcion 

```
[leonardo@192.168.1.77]$  myfun 
Hola, leonardo, BIENVENIDO!!!!
Este /home/leonardo es tu directorio
[leonardo@192.168.1.77]$  
```

14. Remueva la funcion 

Para ello  usamos set
```
[leonardo@192.168.1.77]$  source bashrrc 
[leonardo@192.168.1.77]$  myfun 
Hola, leonardo, BIENVENIDO!!!!
Este /home/leonardo es tu directorio
[leonardo@192.168.1.77]$  unset myfun
[leonardo@192.168.1.77]$  
```

15. Esta es un version modificada de la funcion special_vars:

```
$ special_vars2() {  #REALIZADA DESDE LA CLI
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

y lo invocamos con:
```
$ special_vars2 crying cockles and mussels alive alive oh
```

Adivina las salidas:

|Referencia |   Valor   |
|-----------|-----------|
|echo $#    |   7   |
|echo $_    |   7  |
|echo $1    |   crying  |
|echo $4    |   mussels |
|echo $6    |   alive   |
|echo $7    |   oh   |
|echo $_    |   oh  |
|echo $@    |   crying cockles and mussels alive alive oh   |
|echo $?    |   0   |


16.Basándonos en la función de muestra (check_vids) en la sección “Función dentro de una función”, escribe una función llamada check_music para incluirla en un script de inicio bash que acepte parámetros posicionales para que podamos modificarla fácilmente:

- El tipo de archivo que se comprueba: ogg
- El directorio en el que se guardan los archivos: ~/Music
- El tipo de archivo que se guarda: music
- El número de archivos que se guardan: 7

```
#!/bin/bash

check_music() {
    ls -1 ~/$2/*.$1 > /dev/null 2>&1
    if [ "$?" = "0" ];then
        echo -e "Remember, you must not keep more than $4 $3 files in your Video folder.\nThanks."
    else
	echo -e "You do not have any videos in the Video folder. You can keep up to 5.\nThanks."
    fi
}

check_vids ogg Music music 7 


```



# Ejercicios exploratorios 
1. Las funciones de sólo lectura son aquellas cuyo contenido no podemos modificar. Haga una investigación sobre funciones de sólo lectura y complete la siguiente tabla:

| Nombre de la funcion | Has que sea solo lectura   | lista de todas las funciones de solo lectura  |
|----------------------|----------------------------|-----------------------------------------------|
|my_fun | readonly my_fun   |   readonly -p|

2. Busca en la web cómo modificar PS1 y cualquier otra cosa que necesites para escribir una función llamada fyi (que se colocará en un script de inicio) que le da al usuario la siguiente información:

    Nombre del usuario
    Directorio principal
    Nombre del host
    Tipo de sistema operativo
    Buscar la ruta (PATH) de ejecutables
    Directorio de correo
    Con qué frecuencia se revisa el correo
    ¿Cuántos shells tiene la sesión actual?
    Prompt (deberías modificarlo para que muestre <user>@<host-date>)



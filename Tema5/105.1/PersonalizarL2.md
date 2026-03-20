# 

Bash clasifica las variables como:

- shell/local :  Las que se ubican solo dentro de los limites del shell en el que fueron creadas 
- entorno/global : (Las que son heredadas por shells y/o procesos hijos)



## Variable: Asignacion y referencia

```
distro=zorinos
```

imprimir variable 

```
echo $distro
```

#### Variable
- Una variable se asigna del siguiente modo SIN ESPACIOS ENTRE EL IGUAL
- El nombre de una variable puede contener letras (a-z, A-Z), numeros(0-9) y guiones bajos (_)
- NO puede empuezar con numeros 
- El nombre de la variable no puede contener espacios (ni siquiera usando comillas, en su lugar se pueden usar los guiones bajos ejem "my distro" --> my_distro)


#### Valores de las variables
- Las variables pueden contener cualquier  caracter alfanumerico (a-z, A-Z, 0-9) asi como la mayoria de caracteres (!,",#,$,%,&,/,(,),etc..)
- Los valores de las variables deben ser encerrados entre comillas si contiene espacios o carcateres como los usados en la redireccion (<,>) o el simbolo "pípe" (|) ejem: distro=>zorin  --> Lo unico que hace el siguiente comando es crear un archivo vacio llamado zorin, a diferencia de: distro=">zorin"
- Si el valor de la variable contiene ciertos espacio como la siguiente: var="      genus | uromastyz   " y despues queremos imprimirla con echo $var, el resultado sera "genus | uromastyx", si hay espacios a proposito y queremos que se impriman, usaremos comillas dobles cuando lo hagamos, por ejemplo: echo "$var"
- Si hay un signo de "!", este debe de ser el ultimo, por que si no lo interpretara como ejecucion del ultimo comando, por ejemplo: distro=zorin.?!os   --> echo $distro --> dara error o se ejecutara en caso de que encuentre un ultimo comando ejecutado registrado en hisotry que comience con os, por lo que este signo deberia de ser el ultimo como: distro=zorin.?os!
- Cualquier backslash debe escapar por otro, si se define como var=zorinoz\ la salida sera esto >, por lo que para evitar eso, usaremos doble slash con \, ejemplo var=zorinoz\\


#### "" y ''
""  Permiten sustitucion de la variable, expresa carcateres como \n o $var no de forma literal
''Toman todos los carcateres de valor de la variable literalmente



### Variables locales o de shell
- Solo existen en el shell en el que se crean 
- Por convencion las variables locales van en minusculas

Podemos hacer que nuestra variable sea de solo lectura con:
```
    readonly var=tortoise

    o 

    var=tortoise
    readonly var

```
Para listar todas las variables de solo lectura en nuestra sesion actual, escriba readonly o readonly -p en la terminal

Para remover cualquier variable (ya sea local o global) usamos el comando unset:
```
    unset reptile  Note que no se usa $ antes del nombre de la variable
```

### Variables globales o de entorno 
- Existen en todo el entorno de shell actual, asi como para los procesos subsecuentes que se derivan de este
- Por convencion las variables de entorno se escriben en mayusculas 

Para que una variable de shell LOCAL se convierta a una variable de entorno, se debe de usar el comando export, para que pueda ser usada por los hijos del bash

```
    VAR="qwerty"
    echo $VAR

    export var


    o lo puede asignar una varible y exportarla al mismo tiempo con:
    export VAR="qwerty"

```


Nota: con export -n <Variable-Name> la variable se convertira de nuevo en una variable de la shell local

El comando "export -p" nos dara una lista de todas las variables de entorno existente, donde "declare -x" es quivalente a "export"

Dos comandos para imprimir yuna lista de todas las variables de entorno son:
- env ver todas las variables de entorno
- printenv  Ademas de ser un sinonimo de env se puede usar para ver el valor de una variable como: printenv PWD (Note que el nombre de la variable no esta precedido por $)


### Variables comunes de entorno
**DISPLAY**
En relacion con el sevidor X, se compone de 3 elementos 
- El hostname (la ausencia de este significa localhost) donde se ejecuta el servidor X
- Dos puntos como delimitador
- Un numero (normalmente es 0 y se refiere a la pantalla de la computadora)

    $ printenv DISPLAY
    :0

Un valor vacio para esta variable significa un servidor sin un sistem X WIndow. Un numero extra --como en mi.xserver:0:1 -- se referira al numero de pantalla (si existe mas de uno)


**HISTCONTROL**
Esta variable controla que comandos se guardan en HISTFILE (ver abajo), hasy tres valores posibles
ignorespace : Los comandos que empiecen con un espacio no se guardaran
ignoredups : Un comando que es el mismo que el anterior no se guardara
ignoreboth: Los comandos que caen en cualquiera de las dos categorias anteriores no se guardaran

echo $HISTCONTROL
ignoreboth


**HISTSIZE**
Esto establece el numero de comandos que se almacenaran en la memoria mientras dure la sesion de shell

echo $HISTSIZE
1000

**HISTFILESIZE**
Esto establece el numero de comandos que se guardaran en HISTFILE tanto al principio como al final de la sesion 

echo $HISTFILESIZE
2000

**HISTFILE**
El nombre del archivo que almacena todos los comandos a medida que se escriben. Por defecto este archivo se encuentra en  ~/.bash_history

    echo $HISTFILE
    /home/user2/.bash_history

    Para ver todos los comandoas almacenados usamos history


**HOME**
Almacena la ruta principal del usuario y se establece cuando el usuario se conecta

**HOSTNAME**
Variable que almacena el nombre del computador de red
echo $hostname 
debian


**HOSTTYPE**
echo $HOSTTYPE
x86_64

**LANG**
Esta variable guarda informacion de localizacion que utiliza el sistema:
echo $LANG
en_UK.UTF-8


**LD_LIBRARY_PATH**
Esta variable consiste en un conjunto de directorios separados por dos puntos donde las bibliotecas compartidas (share librearies) son compartidad por los programas
echo $LS_LIBRARY_PATH
/usr/local/lib

**MAIL**
Almacena el archivo en el que bash revisa el correo electronico
echo $MAIL
/var/mail/carol

otro valor comun para esta variable es /var/spool/mail/$USER

**MAILCHECK**
Almacena un valor numerico que indica en segundos la frecuencia con la que bash comprueba si hay correo nuevo

echo $MAILCHECK
60

**PATH**
Esta variable de entorno almacena la lista de directorios donde Bash busca los archivos ejecutables cuando se le indica que ejecute cualquier programa.

**PS1**
Esta variable almacena el valor del indicador BASH

```
En la siguiente parte de código (también de /etc/profile), la sentencia if comprueba la identidad del usuario y en consecuencia brinda un indicador muy discreto ( # para root o $ para usuarios regulares):

if [ "`id -u`" -eq 0 ]; then
  PS1='# '
else
  PS1='$ '
fi
```

**PS2**
Normalmente se establece en > y se usa como un mensaje de continuidad para comandos largos de varias lineas

**PS3**
Usado como el indicador para el comando select

**PS4**
Normalmente se establece en + y se usa para la depueracion 

**SHELL**
Esta variable almacena la ruta absoluta del shell actual

echo $SHELL
/bin/bash

**USER**
Esto almacena el nombre del usuario actual
echo $USER
carol






# Alias y funciones en personalizacion de shell

### Creando alias 
Es un nombre sustituto que se pone en lugar de un comando (Un alias es un nombre sustituto de otro(s) comando(s). Puede ejecutarse como un comando normal, pero en su lugar ejecuta otro comando según la definición de alias)


para declararlo usamos:

```
alias alias_name=commands(s)

alias oldshell=sh


Podemos crear comandos concatenados como:
alias git_info='which git;git --version'

alias greet='reptile=tortoise; echo Hello $reptile!'

```

```
Podemos escapar de un alias con \:

$ alias where?='echo $PWD'
$ where?
/home/user2
$ \where?
-bash: where?: command not found

Escapar de un alias es útil cuando un alias tiene el mismo nombre que un comando normal. En este caso, el alias tiene prioridad sobre el comando original, sin embargo, este sigue siendo accesible al escapar del alias

```

Nota: 
- Por si solo muestra todos los alias en el sistema
- Podemos colocar un alias dentro de un alias

### Expancion y evaluacion de las comillas en los alias

Cuando se usan comillas simples, hace que la expancion sea DINAMICA, toma varios valores conforme cambian las cosas
```
$ alias where?='echo $PWD'
$ where?
/home/user2
$ cd Music
$ where?
/home/user2/Music
```

Cuando las comillas dobles se usan, la expancion se hace de forma ESTATICA, toma un valor y ya
```
$ alias where?="echo $PWD"
$ where?
/home/user2
$ cd Music
$ where?
/home/user2
```

Para que los alias persistan podemos agregarlos a un script de iniciacion, como .bashrc, algunos archivos .bashrc, nos dan la opcion de crear un archivo, el nombre puede variar, sin embargo usaremos .bash_aliases

```
en el archivo .bashrc

if [ -f ~/.bash_aliases ]; then
    ~/.bash_aliases
fi

```


#### Creando funciones 
en comparacion con los aliases, las funciones son mas programables, podemos pensar en una funcion como un comando que incluye la logica a travez de bloques o colecciones de otros comandos.

hay dos sintaxis validas para definir las funciones 

```
Usando la palabra funcion

function function_name{
        command 1
        command 2
        etc
    
       ...
    }

```

```
O usando () como:

function_name(){
    command 1
    command 2
    etc
    ....
    }

```

Tambien se puede crear funciones dentro de un terminal como:

Es común agregar funciones en archivos o scripts. Sin embargo, también se puede escribir con cada comando en el shell prompt, en una línea diferente — note PS2(>) que indica una nueva línea después de un salto de línea:

```
$ greet() {
> greeting="Hello world!"
> echo $greeting
> }
```

Otra forma como comando deberia de estar separado por un punto y una coma:

```
$ greet() { greeting="Hello world!"; echo $greeting; }

Usas ";" para separar las lineas 
```

Al igual que con las variables y los alias, si queremos que las funciones sean persistentes a travez de los reinicios del sistema tenemos que agregarlas en los scripts de inicializacion de shell como /etc/bash.bashrc (global) o ~/.bashrc (local)

Nota: Despues de añadir alias o funciones a cualquier archivo desde script de inicio, debe ejecutar tales archivos con . o source para que los cambios surtan efecto si no quiere salir y volver a entrar o reiniciar el sistema


#### Variables especiales incorporadas en bash 
Son especiales por que pueden ser referenciadas y NO asignadas, algunas de las mas relevantes son:

- $?  La referencia de esta variable se da hasta la ultima ejecucion del comando. Valor de 0 significa exito, mientras que un valor diferente de 0, significa error
- $$ Se expande hasta el PID del shell (proces ID)
- $! Se expande al PID del ultimo trabajo en el background 
- $[0-9] se expande a los argumentos que se le puede pasar a un funcion 

```
Aunque pasar parámetros posicionales a los alias es técnicamente posible, no es en absoluto funcional ya que - con los alias - los parámetros posicionales siempre se pasan al final:

$ alias great_editor='echo $1 is a great text editor'
$ great_editor emacs
is a great text editor emacs
```

- $# Se expande al numero de argumentos recibidos 
- $@, $* Se extienden a los argumentos pasados al comando (muestra todos los argumentos)
- $_ Se expande hasta el ultimo paramatro o el nombre del script 


#### Funciones en Script 
solo se agrega el shebang, que es la linea inicial de un script

```
#!/bin/bash
```


**Unset se puede usar para eliminar variables o funciones del entorno del shell actual. unset -v para variables y unset -f para funciones. Sin opciones unset tratara de desajustar una variable primero y si falla, enotnces tratara de desajustar una funcion**



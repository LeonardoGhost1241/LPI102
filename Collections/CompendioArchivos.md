# Compendio de comandos 

## 105: Shells y script 

#### 105.1 Personalizar el entorno de shell

**Nota: Los archivos de inicio de sesion se puedne dividir en Bash especificos (los que se limitan solo a las configuraciones y comandos bash) y generales (relacionados con la mayoria de los shells)**

**Shell interactivo de inicio de sesion**

Nivel global

- `/etc/profile`: Este es el archivo .profile de todo el sistema para el shell Bourne y los shells compatibles con Bourne (Incluido bash). A través de una serie de declaraciones if este archivo establece un número de variables como PATH y PS1, así como origen — si existe — tanto del archivo /etc/bash.bashrc como los del directorio /etc/profile.d.

-`/etc/profile.d/*`: Este directorio puede contener scripts que son ejecutados por /etc/profile

Nivel local

- `~/.bash_profile`: Este archivo específico de Bash se utiliza para configurar el entorno del usuario. También puede ser usado para crear el ~/.bash_login y ~/.profile.

- `~/.bash_login`: Este archivo (específicamente), sólo se ejecutará si no hay un archivo ~/.bash_profile. Su nombre sugiere que debería ser usado para ejecutar los comandos necesarios para el inicio de sesión.

- `~/.profile`: Este archivo no es específico de Bash y se obtiene sólo si no existe ~/.bash_profile ni ~/.bash_login, que es lo que normalmente ocurre. Por lo tanto, el propósito principal de ~/.profile es el de revisar si se está ejecutando un shell de Bash, y si fuese afirmativo, obtener ~/.bashrc (si existe). Normalmente establece la variable PATH para que incluya el directorio privado del usuario ~/bin (si existe).

- `~/.bash_logout`: Si existe, este archivo específico de Bash hace algunas operaciones de limpieza al salir del shell. Esto puede ser conveniente en casos como los de las sesiones remotas.


**Shell interactivo sin inicio de sesion**

Nivel global 

- `/etc/bash.bashrc`: Este es el archivo .bashrc de todo el sistema para los shells interactivos bash. A través de su ejecución, bash se asegura de que se está ejecutando interactivamente, comprueba el tamaño de la ventana después de cada comando (actualizando los valores de LÍNEAS y COLUMNAS, si es necesario) y establece algunas variables

Nivel local

- `~/.bashrc`: Además de llevar a cabo tareas similares a las descritas para /etc/bash.bashrc a nivel de usuario (como comprobar el tamaño de la ventana o si se está ejecutando de forma interactiva), este archivo específico de Bash suele establecer algunas variables de historial y origen ~/.bash_aliases (si existe). Aparte de eso, este archivo se utiliza normalmente para almacenar alias y funciones específicas de los usuarios. 

`~/.bashrc` se lee si bash detecta que su <stdin> es una conexion de red (por ejemplo de la conexion Secure Shell (ssh))


**Los arhcivos locales tienen  prioridad sobre los globales**

Note en como con su - <nombre de usuario> (también su -l <nombre de usuario> y su --login <nombre de usuario>) garantizamos la invocación de un shell de acceso, mientras que su <nombre de usuario> sólo habría invocado /etc/bash.bashrc y ~/.bashrc


`/etc/skell` - Este archivo es usado para almacenar los archivos iniciales al cear un usuario


##### 105.2 Personalizar el entorno de shell (LECCION 2)

Reglas de variables en basj
1. 
2. 
3. 



readonly - lista todas las variables que estan en nuestro sistema que son SOLO de lectura
    -p Puede agregar esta bandera, que dara el mismo resultado con o sin 

set - ver todas las variables y funciones que estan definidas en nuestra shell

unset - Remover cualquier variable (ya sea local o global), usamos este comando
    ejemplo: unset myvar (sin el $)
    -v para remover variables (lo hace igual sin agregar esta bandera)
    -f para remover funciones (Ejemplo: unset -f myfunc)

**Nota:**
- Si se usa sin opciones, unset tratara de desajustar una variable primero, y si falla, entonces tratara de desajustar una funcion


export - Este comando hara que una variable local se convierta en una variable de entorno para que los shells hijos puedan reconocerla y usarla. De la misma manera, export puede ser usado para asignar y exportar una variable a la vez, como:
    Ejemplo: export myvar="fvdsfvesv"
    -n La variable se convertira de nuevo en una variable de la shell local

**Nota:**
- El comando declare -x es equivalente al comando export

env y printenv - Son usado para imprimir una lista de todas las variables de entorno
    

**Nota:**
- Aveces podemos usar printenv para comprobar el valor de una variable (Sin el signo $, Ejemplo: printenv PWD)

- El comando env tambien se puede utilizar para definir o inyectar variables de entorno a un script, recordemos que cuando hablamos de los shells no interactivos sin inicio de sesión, observamos como los scripts no leen ningún archivo de inicio estándar sino que buscan el valor de la variable BASH_ENV y lo usan como su archivo de inicio si existe, a continuacion un ejemplo:

Se creara un archivo con las variables que vamos a ocupar y en la ruta donde vaya a estar

ruta de nuestro archivo: /home/leonardo

contenido:

IP=192.168.1.66
hostname=leonardo

para inyectar estas variables ejecutaremos el siguiente comando

env BASH_ENV=/home/leonardo/variables.sh ./script.sh

o podemos hacer: 

env BASH_ENV=/home/leonardo/variables.sh ./script.sh

ya que esta implicito el comando



##### 105.2 Personalizar el entorno de shell (LECCION 3)

alias - Permite ejecutar un comando dado a un nombre sustituto
    Ejemplo: alias show=ls -l (show sera interpretado como ls -l )
    Solo nos proporcionara una lista de los aliases que hay en el sistema

**Notas:**

- Se pueden definir variables dentro de los aliases, ejemplo

```
$ alias greet='reptile=tortoise; echo Hello $reptile!'
$ greet
Hello tortoise!
```

- Podemos escapar de un alias con \: (util, cuando el alias tiene el mismo nombre que un comando)

```
$ alias where?='echo $PWD'
$ where?
/home/user2
$ \where?
-bash: where?: command not found
```

- Podemos colocar un alias dentro de otro alias

```
$ where?
/home/user2
$ alias my_home=where?
$ my_home
/home/user2
```

- Comillas simples (El resultado sera dinamico, cambiara conforme haya algun cambio)

```
$ alias where?='echo $PWD'
$ where?
/home/user2
$ cd Music
$ where?
/home/user2/Music
```

- Comillas doble (El resultado sera estatico, no cambiara aunque haya algun cambio)

```
$ alias where?="echo $PWD"
$ where?
/home/user2
$ cd Music
$ where?
/home/user2
```












unalias - Elimina un alias 
    Ejemplo: unalias show












## 107: Tareas administrativas
#### Administrar cuentas de usuario y de grupo y los archivos de sistema relacionados con ellas

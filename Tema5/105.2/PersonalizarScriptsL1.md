# Personalizar y escribir scripts sencillos 


## Estructura y ejecucion del script 

- Un archivo script es una secuencia ordenada de comandos que debe ser ejecutada por un interprete 
- En un script el indicador en la primera linea debe ser #!/bin/bash, donde #! es conocido como (shebang)
- No es obligatorio agregar .sh, pero ayuda a identificar los scripts de shell a la hora de listarlos 
- Es importante agregarle atributos de ejecucion con chmod +x file.sh 
- Cuando se ejecuta un script, los comandos contenidos en este no se ejecutan directamente en la sesion actual, sino que se ejecutan mediante un nuevo proceso Bash, llamado sub-shell, esto se hace asi para evitar que el script sobreescriba variables de entorno y que deje modificaciones desatendidas, en caso de que queramos que el script sea ejecutado con la sesion de shell actual, usaremos source file o . file 
- Cuando se ejecuta un script, se crea un proceso hijo (o una terminal hija) y como es un proceso, su codigo de estado se almacenara en la variable $?, sin embargo para cambiar este comportamiento usamos exec, donde hara que no se cree ningun proceso hijo y correra el programa en la terminal, por lo que acabando el programa o comando, este saldra del terminal
- Por convencion, las letras mayusculas se usan para los nombres de las variables, pero no es obligatorio


El comando "echo", por defecto agregara una nueva linea al finalizar su contenido, para evitar esto, usaremos -n


## Variables 
- $* Todos los argumentos pasaron al script
- $@ Todos los argumentos pasados al script. Si se usa con comillas dobles como en "$@", todos los argumentos seran encerrados entre comillas
- $# El numero de argumentos 
- $0 Nombre del script 
- $! PID del ultimo programa ejecutado
- $$ PID del shell actual
- $? El status (codigo) del ultumo comando terminado, en los procesos POSIX, un valor numerico de 0, significa que se ejecuto con exito, aplica a scripts de shells


Parametro posicional, es un parametro denotado por uno o mas digitos, por ejemplo la variable $1 corresponde al primer argumento dado al script, $2, etc., si el parametro es mayor que 9, debe de ser referenciado con llaves, como ${10} o ${11}, etc

Las variables ordinarias son variables que se asignan de forma manual, con read, ejemplo:

```
echo "Do you want to continue (y/n)?"
read ANSWER


# A menos de que no se especifca el nombre delante del read como: read -p "Do you want..." ANSWER

Se  puede usar para leer mas de una variable simltaneamente como:

echo "Nombre y apellido"
read Name SURNAME # SIN -p

echo $NAME $LASTNAME


read -p "numero al azar " numero
echo $numero

```

Nota: Si el numero de terminos dados es mayor que el numero de variables, los terminos excedentes se almacenan en la ultima variable


Los script aveces necesita info de la salida de un comando, esto se llama backtic, se logra con comillas simples invertidas o con $(), como:

```

OS=`uname -o`

OS=$(uname -o)

Para devolver la cantidad de CARACTERES que contiene, se devuelve preparando un hash antes del nombre de la variable, como:
echo ${#OS}

```

## Matrices en Bash 

Se declaran de la siguiente manera:


```
declare -a CARS

o 

colors=("rojo" "verde" "azul" )
for i in "${colors[@]}"; do
    echo $i
done

Como los indices de matriz comienzan en 0, el contenido del primer elemento esta en ${SIZES[0]}, el segundo elemento esta en ${SIZES[1]}, etc...

Cambio de un elemento:
${SIZES[1]}="1234"


Al igual que con las variables ordinarias, la longitud de un elemento de una matriz devuelve con el carcater numeral, ejemplo: ${#SIZES[0]}, para la longitud del primer elemento en la matriz

Nos devuelve el numero total de elemento en una matriz si se usa @ o * como indice:

echo ${colors[*]}
echo ${colors[@]}


Tambien se pueden declarar las matrices utilizando la salida de un comando como elementos iniciales mediante la sustitucion de comandos, como:

space=( $(df -h) )

```


## Expresiones aritmeticas 
Para hacer una operacion, se puede usar el comando expr, se puede usar como:

```

suma=`expr $a + $b` #Con comillas simples invertidas

o con el signo $(())

suma=$(($a+$b))

Tambien se puede usar como sustitucion de comando 

```


## Ejecucion condicional 
Bash evalua el exito de un comando, por ejemplo:

comand && command 1 && comando2

El comando 2 se ejecutara si y solo si su estado de salida es igual a 0

El comportamiento apuesto ocurre cuado se separan con || 


Condicion if.
Esta estructura se ejecuta si el comando dado como argumento devuelve un codigo de estado de "0 (exito)", otro comando, test se puede usar para evaluar muchos criterios especiales, como:


```

if test -x /bin/bash ; then
    echo "Confirmed: /bin/bash os executable"
fi


se puede sustituir test por  [], como:

if [ -x /bin/bash ] ; then
    echo "Confirmed: /bin/bash os executable"
fi


la otra estructura del if es:

if [ -x /bin/bash ] ; then
  echo "Confirmed: /bin/bash is executable."
else
  echo "No, /bin/bash is not executable."
fi


```


## Salida de un script 
Con la opcion -e, el comando echo es capaz de mostrar carcateres especials usando secuencias de escape (una secuencia de barra invertida que designa un carcater especial), ejemplo:


```
#!/bin/bash

# Obtiene el nombre genérico del sistema operativo
OS=$(uname -o)

# Obtiene la cantidad de memoria libre en bytes
FREE=$(( 1000 * `sed -nre '2s/[^[:digit:]]//gp' < /proc/meminfo` ))

echo -e "Operating system:\t$OS"
echo -e "Unallocated RAM:\t$(( $FREE / 1024**2 )) MB"


Donde se usa el catvayer \t, para que se rendericen bien se usa -e, hay otros como \”

```


El comando printf, brinda mas contrll sobre como mostrar variables como:

```
printf "Operating system:\t%s\nUnallocated RAM:\t%d MB\n" $OS $(( $FREE / 1024**2 ))

```

con print las variables van fuera del patron del texto, como:

```
MSG='Operating system:\t%s\nUnallocated RAM:\t%d MB\n'
printf "$MSG" $OS $(( $FREE / 1024**2 ))

```

Este método es particularmente útil para mostrar distintos formatos de salida según las necesidades del usuario. Por ejemplo, facilita la escritura de un script que utilice un patrón de texto distinto, como puede ser una lista CSV (Comma Separated Values) en lugar de un mensaje de salida predeterminado











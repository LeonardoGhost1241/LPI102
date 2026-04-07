# Personalizar y escribir scripts sencillos 

test - es un comando de bash script que permite evuar propiedades de objetos del sistema de archivos,  esencialemnete archivos o directorios 

Hay dos formas de usarlo
1. Las expresiones a evauar se pasan por argumento al comando test o 
2. Colocar la expresion entre corchetes


```
# Forma 1
test -d /etc

# Forma 2

[ -d /etc/]

```

Algunos argumentos que podemos usar dentro de test son:


-a "$VAR"   Evalua su la ruta en VAR esta abierto en un terminal
-u "$VAR"   Evaluar si la ruta en VAR tiene el permiso SUID 
-w "$VAR"   Evaluar si la ruta en VAR es escribible por el ususairo actual 
-x "$VAR"   Evaluar si la ruta en VAR es ejecutable por el usuario 
-O "$VAR"   Evalua si la ruta en VAR es propiedad del usuario actual
-G "$VAR"   Evaluar si la ruta en VAR pertenece al grupo del ususario actual 
-N "$VAR"   Evaluar si la ruta en VAR ha sido modificado desde la ultima vez que accedio 
"$VAR1" -nt "$VAR2"    Evaluar si la ruta en VAR1 es mas nuevo que la ruta en el "VAR2", segun sus fechas de modificacion 
"$VAR1" -ot "$VAR2"    Evaluar si la ruta en VAR1 es mas antiguo que el VAR2
"$VAR1" -ef "$VAR2"    esta expresion evalua a  "True" si la ruta en VAR1 es un enlace duro (hardlink) con VAR2



**NOTA: Se recomienda usar comillas dobles en una variable probada, por que si la variables resulta estar vacia, podria causar un error para el comando test**


Tambien hay pruebas para variables de texto arbitrarias, que se describen a continuacion:

-z "$TXT"   ->  Evalua si la variable TXT esta vacia (tamaño cero)
-n "$TXT" o test "$TXT" ->  Evalua si la variable TXT NO esta vacia 
"$TXT1" = "$TXT2" o "$TXT1" == "$TXT2"  ->  Evalua si la variable TXT1 y TXT2 son iguales
"$TXT1" < "$TXT2"   ->  Evalua si TXT esta antes que TXT2, en orden alfabetico 
"$TXT1" > "$TXT2"   -> Evalua si TXT1 esta despues que TXT2, en orden alfabetico 

**Nota: Los distintos idiomas pueden tener reglas diferentes para el orden alfabetico. Para obtener resultados consistentes, independientemente de la configuracion de localizacion del sistema donde se ejecuta el script, se recomienda establecer la variable de entorno LANG a C, como: LANG=C**



Para las comparaciones numericas:
$NUM1 -lt $NUM2    Evalúa si NUM1 es menor que NUM2.
$NUM1 -gt $NUM2    Evalúa si NUM1 es mayor que NUM2.
$NUM1 -le $NUM2    Evalúa si NUM1 es menor o igual que NUM2.
$NUM1 -ge $NUM2    Evalúa si NUM1 es mayor o igual que NUM2.
$NUM1 -eq $NUM2    Evalúa si NUM1 es igual a NUM2.
$NUM1 -ne $NUM2    Evalúa si NUM1 no es igual a NUM2.

Todas las anteriores sentencias se pueden usar alguno de los siguientes modificadores


! EXPR    Evalúa si la expresión EXPR es falsa.
EXPR1 -a EXPR2    Evalúa si tanto EXPR1 como EXPR2 son verdaderos.
EXPR1 -o EXPR2    Evalúa si al menos una de las dos expresiones es verdadera



Construccion condicional case: ejecutara una lista de comandos dados si un item especificado

```
#!/bin/bash
#

paq=""

case $1 in 
        ubuntu|debian|mint)
                paq="deb"
                ;;
        centos|fedora|opensuse)
                paq="rpm"
                ;;
        arch)
                paq="pacman" 
                ;;
        *)
                echo "desconocido"
                ;;
esac

echo "Distribution $1 uses $paq"

```

Cada lista de patrones y los comandos asociados deben terminar con ;;, ;& o ;;&

El ultimo patro, es un astericos que coincidira con ningun patron

Finalizando con esac 


Hay una opcion de bash llamada nocasematch, que permite la coincidencia de patrones no sensibles a mayusculas y minusculas para la construccion del case. Para ello usamos el comando shopt que cambia los valores de las configuraciones que controlan el comportamiento del shell

- shopt -s para (set) habilitar
- shopt -u para (unset) deshabilitar

or lo tanto, si se coloca shopt -s nocasematch antes de la construcción de mayúsculas y minúsculas, se habilitará la coincidencia de patrones no sensibles a estas

Esto solo afectara a la sesion actual, por lo que las  opciones modificadas dentro de los scripts se ejecutaran en una sub-shell



Construcciones de bucle

Hay 3 instrucciones de bucle
- for
- until
- while

La sentencia for usa la lista de valores para cada iteracion 

```
for VARNAME in LIST
    do
        commands
    done
```


```
#!/bin/bash

for NUM in 1 1 2 3 5 8 13
do
	echo -n "$NUM is "
	if [ $(( $NUM % 2 )) -ne 0 ]
	then
		echo "odd."
	else
		echo "even."
  fi
done

```


***Los carcateres delimitadores validos que dividen los elementos de la lista estan definidos por la variable de entorno IFS, el valor predeterminado es un espacio " ", si fuera el caso de separarlos por comas en la liasta 1,2,3,4,5,6,7,  tendriamos que definir la variable IFS como ",", para que pueda iterar entre esa lista de elementos***

en bash tambien hay un formato alternativo a for

```

#!/bin/bash

SEQ=( 1 1 2 3 5 8 13 )

for (( IDX = 0; IDX < ${#SEQ[*]}; IDX++ ))
do
	echo -n "${SEQ[$IDX]} is "
	if [ $(( ${SEQ[$IDX]} % 2 )) -ne 0 ]
	then
		echo "odd."
	else
		echo "even."
  fi
done


```

Esta notacion es util para trabajar con arreglos 


Otra estrcutura es until, esta se ejecuta hasta que la condicion sea verdadera, funciona al reves que el bucle while

```
#!/bin/bash
contador=1
# El bucle se ejecuta hasta que la condición (contador > 5) sea verdadera

until [ $contador -gt 5 ]
do
    echo "Contador: $contador"
    ((contador++)) # Incrementa el contador
done

```

La instruccion while es similar a until, pero while se repite  si el comando de prueba termina con el estado 0 (exit0)


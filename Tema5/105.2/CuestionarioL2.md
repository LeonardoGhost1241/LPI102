# Ejercicios guiados 


1. ¿Cómo podría usarse el comando test para verificar si la ruta del archivo almacenado en la variable FROM es más reciente que el archivo cuya ruta está en la variable TO?

```
if test "$FROM" -nt "$TO" ; then
    echo "$FROM es mas nuevo que "$TO"
fi
```


2. El siguiente guión debe imprimir una secuencia numérica del 0 al 9, pero en cambio imprime indefinidamente el 0. ¿Qué se debe hacer para obtener el resultado esperado?

```
#!/bin/bash

COUNTER=0

while [ $COUNTER -lt 10 ]
do
  echo $COUNTER
done

```

No esta incrementando el valor, para corregirlo, debemos de agregar esta linea:

```
COUNTER=$(( $COUNTER + 1 ))
```

3. Supongamos que un usuario escribió un script que requiere una lista ordenada de nombres de usuario. La lista ordenada resultante se presenta como la siguiente en su computadora:

```
carol
Dave
emma
Frank
Grace
henry
```

Sin embargo, la misma lista está ordenada como la siguiente en la computadora de su colega:

```
Dave
Frank
Grace
carol
emma
henry
```

¿Cómo podría explicar las diferencias entre las dos listas clasificadas?

La clasificación se basa en la ubicación del sistema actual. Para evitar las inconsistencias, las tareas de clasificación deben ser realizadas con la variable de entorno LANG puesta en C.



# Ejercicios exploratorios 

1. ¿Cómo podrían usarse todos los argumentos en la línea de comandos del script para inicializar una matriz Bash?

```
array=("$@")

o 

array=($*)

```

2. ¿Por qué, contrariamente a la intuición, el comando `test 1 > 2` se evalúa como verdadero?
Por que redirige la salida estandar 1> hacia la salida de stderr, por eso se evalua como verdaero, para evluar numeros debemos de usar lo siguiente, estos simbolos estan pensados en bash script para cadena de racateres no para numeros.


```
test 1 -gt 2
```

3. ¿Cómo podría un usuario cambiar temporalmente el separador de campos predeterminado por el carácter de nueva línea solamente, sin dejar de poder revertirlo a su contenido original?
Con la variable IFS

definimos IFS=",",  por ejemplo separador por comas, esto sera temporal por que el script se ejecuta en una subshell


# Cuestionario 105.1 - leccion 2


## Ejeercicios guiados 
1. Observe la asignación de la variable en la columna “Comando(s)” e indique si la variable resultante es “Local” o “Global”:

| Comando(s) | Local | Global |
| -----------|-------|--------|
|debian=mother| * | |
|ubuntu=deb-based|  * | |
|mint=buntu-based; export mint| |   *   |
|export suse=rpm-based  |   |   *   |
|zorin=ubuntu-based|  *  |  |


2. Estudie el “Comando” y la “Salida” y explica el significado:

| Comando | Salida  |   Significado |
|---------|---------|---------------|
| echo $HISTCONTROL|    ignoreboth  |   que los comandos que empiecen con un espacio y los comandos que sean el mismo que el que le precede tampoco se guardara |
| echo ~ | /home/carol | Es el directorio del usuario logeado |
| echo $DISPLAY | reptilum:0:2  |   reptilum la maquina tiene un servidor X funcionando y estamos usando la segunda pantalla |
| echo $MAILCHECK | 60 |  Son los segundos de la frecuecnia con la que bash comprueba si hay correos  |
| echo $HISTFILE  | /home/carol/.bash_history   |  Este es el archivo donde se guardan los comandos ejecutados, este es el archivo del usuario carol  |


3. Las variables están siendo puestas incorrectamente en la columna “Comando erróneo”. Proporcione la información que falta en “Comando correcto” y “Referencia de la variable” para que obtengamos la “Salida esperada”:

|Comando erroneo | Comando correcto  | Referencia de la variable | Salida esperada |
|----------------|-------------------|---------------------------|-----------------|
|lizard =chameleon  |   lizard=chamaleaon |  echo $lizard  |  chameleon  |
|cool lizard=chameleon  |  cool_lizard=chamelon  | echo $cool_lizard   |  chameleon  |
|lizard=cha\|me\|leon |  lizard="cha\|me\|leon"   | echo "lizard"   |  cha\|me\|leon  |
|lizard=/** chameleon **/   |  lizard="/** chamelon /**" |  echo "$lizard" |  /** chamelon **/  |
| win_path=C:\path\to\dir\  |   win_path=C:\\path\\to\\dir\\ |  echo "win_path" |   C:\path\to\dir\ |




4. Considere el propósito y escriba el comando apropiado:

|Proposito |Comando |
|----------|--------|
|Establecer el lenguae del actual shell al español UTF-8 (es_ES.UTF-8).| Como es para la shell actual, no hace falta exportarla, por lo que solamente hariamos: LANG="es_ES.UTF-8" |
|Imprime el nombre del directorio actual. | echo $PWD o pwd | 
|Referencia a la variable de entorno que almacena la información sobre las conexiones ssh. | echo $SSH_CONECTION |
|Establecer el PATH para incluir /home/carol/scripts como el último directorio para buscar ejecutables. | PATH="$PATH:/home/carol/scripts" |
|Establecer el valor de`my_path` en PATH. | my_path=PATH |
|Establecer el valor de my_path en el de PATH | my_path=$PATH |


5. Crear una variable local llamada "mammal" y asígnale el valor gnu:

``` 
mammal=gnu
```

6. Usando la sustitución de variables, cree otra variable local llamada var_sub con el valor apropiado para que cuando se haga referencia a través de echo $var_sub obtengamos: The value of mammal is gnu:

```
var_sub="The value of mammal is $mammal"
```

7. Convierte a mammal en una variable de entorno:

```
export $mammal
```

8. Búscalo con set y grep:

```
set | grep "mammal"
```

9. Búscalo con env y grep:

```
evn | grep "mammal"
```

10. Crear, en dos comandos consecutivos, una variable de entorno llamada BIRD cuyo valor es penguin:

```
BIRD="penguin" ; export BIRD
```

11. Crear en dos comandos consecutivos, una variable de entorno llamada NEW_BIRD cuyo valor es yellow-eyed penguin:

```
NEW_BIRD="yellow-eyed penguin" ;  export NEW_BIRD
```

12. Asumiendo que eres user2, cree una carpeta llamada bin en tu directorio principal:

```
mkdir ~/bin
```

13. Escriba el comando para agregar la carpeta ~/bin a su PATH para que sea la primera carpeta en la que bash busque binarios:

```
PATH="$HOME/bin:$PATH"
```

14. Para garantizar que el valor de PATH permanezca inalterado en los reinicios, ¿Qué parte del código — en forma de una declaración if — agregarías en el ~/.profile?

```
if [ -d "$HOME/bin" ]; then
    export PATH="$HOME/bin:$PATH"
fi
```


## Ejercicios exploratorios 
1. let: más que la evaluación de la expresión aritmética::
    Haz una búsqueda en la página web de let y sus implicaciones al establecer las variables y crea una nueva variable local llamada my_val cuyo valor es 10 — como resultado de sumar 5 + 5:
    Ahora crea otra variable llamada your_val, cuyo valor es 5 — como resultado de dividir el valor de my_val entre 2:

```
    let "my_val = 5 + 5"
```

```
    let 'your_val = $my_val / 2 '

```



2. ¿El resultado de un comando en una variable? Por supuesto, eso es posible; se llama sustitución de comandos. Investiga y estudia la siguiente función llamada music_info:

```
music_info(){
latest_music=`ls -l1t ~/Music | head -n 6`
echo -e "Your latest 5 music files:\n$latest_music"
}
```

El resultado del comando ls -l1t ~/Music | head -n 6 se convierte en el valor de la variable latest_music. Luego, se hace referencia a la variable latest_music en el comando echo (que genera el número total de bytes ocupados por la carpeta Music y los últimos cinco archivos de música almacenados en la carpeta Music — uno por línea).

¿Cuál de los siguientes es un sinónimo válido?
latest_music=`ls -l1t ~/Music | head -n 6`

Opción A: latest_music=$(ls -l1t ~/Music| head -n 6) --> Esta es la opcion que representa un sinonimo
Opción B: latest_music="(ls -l1t ~/Music| head -n 6)"
Opción C: latest_music=((ls -l1t ~/Music| head -n 6))






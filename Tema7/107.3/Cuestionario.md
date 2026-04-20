# 


# Ejercicios guiados 

1. Basado en la siguiente salida del comando date, ¿cuál es la zona horaria del sistema en notación GMT?
```
$ date
Mon Oct 21 18:45:21 +05 2019
```

Etc/GMT+5



2. ¿A qué archivo debe apuntar el enlace simbólico /etc/localtime para que Europa/Bruselas sea la hora local por defecto del sistema?
A el directorio /usr/share/zoneinfo/Europa/Bruselas


3. Es posible que los caracteres de los archivos de texto no se representen correctamente en un sistema con una codificación de caracteres diferente de la utilizada en el documento de texto. ¿Cómo podría usarse iconv para convertir el archivo codificado WINDOWS-1252 old.txt en el archivo new.txt usando la codificación UTF-8?

```
iconv -f  WINDOWS-1252 -t UTF-8 old.txt > new.txt 

iconv --from-code=WINDOWS-1252 --to-code=UTF-8 old.txt > new.txt


otra opcion (formato de salida)

iconv --from-code=WINDOWS-1252 --to-code=UTF-8 original.txt -o converted.txt

```



# Ejercicios de exploracion 

1. ¿Qué comando hará que Pacific/Auckland sea la zona horaria por defecto para la sesión de shell actual?


Modificando la variable TZ y despues exportandola, como:
```
export TZ="Pacific/Auckland"
```


2. El comando uptime muestra, entre otras cosas, el promedio de carga del sistema en números fraccionarios. Utiliza la configuración actual de la región para decidir si el separador de decimales debe ser un punto o una coma. Si, por ejemplo, el locale actual está configurado como de_DE.UTF-8 (el locale estándar de Alemania), uptime utilizará una coma como separador. Sabiendo que en el idioma inglés americano el punto se usa como separador, ¿qué comando hará que uptime muestre las fracciones usando un punto en lugar de una coma para el resto de la sesión actual?

Moficiando la variable LC_NUMERIC del locale, usamos:

```
export LC_NUMERIC="en_US.UTF-8"
```


3. El comando iconv reemplazará todos los caracteres fuera del conjunto con un signo de interrogación. Si se añade //TRANSLIT a la codificación de destino, los caracteres no representados en el conjunto de caracteres destino serán reemplazados (transliterados) por uno o más caracteres de aspecto similar. ¿Cómo podría usarse este método para convertir un archivo de texto UTF-8 llamado readme.txt a un archivo ASCII plano llamado ascii.txt?


```
iconv --from-code=UTF-8 --to-code=ASCII//TRANSLIST readme.txt > ascii.txt

```









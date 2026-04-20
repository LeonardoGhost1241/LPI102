# Localizacion e internacionalizacion 


### Zonas horarias

#### Archivos inportantes
- /etc/timezone: Zona horaria por defecto del sistema (America/Sao_Paulo o Etc/GMT+3)
- /etc/localtime: enlace simbolico al archivo de datos real en /usr/share/zoneinfo
- /usr/share/zoneinfo: Directorio con todos los archivos de zonas horarias


#### Comandos esenciales
- date → muestra fecha, hora y offset UTC actual
- timedatectl → más detalle (requiere systemd); muestra hora local, UTC, RTC y zona horaria
- tzselect → guía interactiva para identificar la zona horaria correcta (no la cambia, solo la muestra)

#### Variable de entorno 
- TZ='Zone/City'; export TZ → cambia la zona horaria para la sesión del usuario
- Agregarla a ~/.profile para hacerla permanente
- Uso temporal: env TZ='Africa/Cairo' date

#### Convenciones de nombres
- Por ofset: Etc/GMT+3, GMT-5
- Por localidad: America/Sao_Paulo, Africa/Cairo
- Ojo: en la notación GMT-5, el sistema va 5 horas atrás del UTC


### Lenguaje y codificacion de carcateres
la configuracion de los carcateres esta en la variable LANG.
El contenido de esta variable sigue este formato: ab_CD, donde:
- ab: es el codigo del idioma
- CD es el codigo de la region 

El contenido del idioma debe de seguir la norma ISO-639 y el codigo de la region  debe de seguir la norma ISO-3166

Ejempolo para un sistema configurado para usar portugues: pt_BR.UTF-8

- /etc/locale.conf: Archivo de configuracion del los ajustes de localizaicion, lavariable LANG y otras variables relacionadas se asignana a este archiivo como una variable de shell, ejemplo:

```
$ cat /etc/locale.conf
LANG=pt_BR.UTF-8
```


Se puede hacer una configuracion personalizada en la sesion actual, modificando la variable LANG, sin embargo para que el cambio persista, debe de agregar la linea a ~/.bash_profile o ~/.profile


- localectl: Usado para consultar y cambiar el locale del sistema: por ejemplo:

```
localectl set-locale LANG=es_US.UTF-8
```

Estas son otras variables de entorno que afectan al ssitema:

- LC_COLLATE    Establece el orden alfabético. Uno de sus propósitos es definir el orden en que los archivos y directorios son listados.
- LC_CTYPE    Establece cómo el sistema tratará ciertos conjuntos de caracteres. Define, por ejemplo, qué caracteres considera como mayúsculas o minúsculas.
- LC_MESSAGES    Establece el lenguaje a mostrar para los mensajes del programa (en su mayoría programas GNU).
- LC_MONETARY    Establece la unidad monetaria y el formato de la moneda.
- LC_NUMERIC    Establece el formato numérico para los valores no monetarios. Su principal propósito es definir los separadores de miles y decimales.
- LC_TIME    Establece el formato de la hora y la fecha.
- LC_PAPER    Establece el tamaño de papel estándar.
- LC_ALL    Anula todas las demás variables, incluyendo LANG



- locale: Para ver todas las variables definidas en la configuracion de locale actual

La unica variable que no esta definida es la LC_ALL, que se puede para anular temporalmente todos los ajustes de localizacion


```
$ date
seg out 21 10:45:21 -03 2019
$ env LC_ALL=en_US.UTF-8 date
Mon Oct 21 10:45:21 -03 2019
```

La modificación de la variable LC_ALL hizo que las abreviaturas del día de la semana y del mes se mostraran en inglés americano (en_US)

Se recomienda usar LANG=C en scripts porque las localizaciones del sistema afectan cómo los programas ordenan listas y formatean números. Al fijar LANG=C, el script produce resultados predecibles en cualquier sistema, ya que este locale hace comparaciones byte por byte, lo que además mejora el rendimiento.


### Conversion de la codificacion
Algunos textos pueden no ser correctamente mostrados dado a configuraciones diferentes de codificacion, para ello esta el comando iconv que nos permite convertir el archivo a una codificacion deseada

Por ejemplo, para convertir un archivo llamado "original.txt" de la codificación ISO-8859-1 al archivo llamado "converted.txt" usando la codificación UTF-8, se puede usar el siguiente comando:

```
$ iconv -f ISO-8859-1 -t UTF-8 original.txt > converted.txt
```

La opción -f ISO-8859-1 (o --from-code=ISO-8859-1) establece la codificación del archivo original y la opción -t UTF-8 (o --to-code=UTF-8) establece el del archivo convertido. Todas las codificaciones soportadas por el comando iconv se listan con el comando iconv -l o iconv --list. En lugar de usar la redirección de la salida, como en el ejemplo, también se puede usar la opción -o converted.txt o --output converted.txt

Para ver la codificacion actual de un archivo usamos file ARCHIVO o file -i ARCHIVO








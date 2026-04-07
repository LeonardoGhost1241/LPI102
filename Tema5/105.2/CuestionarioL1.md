# Ejercicios guiados 

1. La opción -s del comando read es útil para introducir contraseñas, ya que no mostrará el contenido que se está escribiendo en la pantalla. ¿Cómo podría usarse este comando para almacenar la entrada del usuario en la variable PASSWORD mientras se oculta el contenido escrito?

```
read -s "Password: " PASSWORD
```

2. El único propósito del comando whoami es mostrar el nombre de usuario que lo ha llamado, por lo que se utiliza principalmente dentro de los scripts para identificar al usuario que lo está ejecutando. Dentro de un script Bash, ¿cómo podría la salida del comando whoami ser almacenada en la variable llamada WHO?

```
WHO=$(whoami)

o 

WHO=`whoami`

```


3. ¿Qué operador de Bash debería estar entre los comandos apt-get dist-upgrade y systemctl reboot, si el usuario root quisiera ejecutar este último y solo si el comando apt-get dist-upgrade haya terminado con éxito?

Para que se ejecute siempre y cuando termine con exito, usamos &&, de la siguiente manera

```
apt-get dist-upgrade && systemctl reboot
```




# Ejercicios exploratorios 

1. Después de intentar ejecutar un script Bash recién creado, el usuario recibe el siguiente mensaje de error:

```
bash: ./script.sh: Permission denied
```
Considerando que el archivo ./script.sh fue creado por el mismo usuario, ¿cuál sería la causa probable de este error?

No tiene permisos para ejecutar o simplemente el comando umask crea archivos sin el bit de ejecucion


2. Supongamos que un script llamado do.sh es ejecutable y el enlace simbólico llamado do.sh apunta a este. Dentro del script, ¿cómo podrías identificar si el nombre del archivo de llamada era do.sh o undo.sh

Con la variable $0, la cual nos retornara el nombre del script 


3. En un sistema con un servicio de correo electrónico correctamente configurado, el comando mail -s "Error de mantenimiento" root <<<"Error de tarea programada" envía el mensaje de correo electrónico de aviso al usuario root. Tal comando podría ser usado en tareas desatendidas, como cronjobs, para informar al administrador del sistema sobre un problema inesperado. Escriba una construcción if que ejecutará comando mencionado mail si el estado de salida (sea cual sea) no tiene éxito.

```
if [ "$?" -ne 0 ]; then
    mail -s "Error de mantenimiento" root <<<"Error de tarea programada"
fi

o en una sola linea como:

if [ "$?" -ne 0 ]; then mail -s "Error de mantenimiento" root <<<"Error de tarea programada"; fi 

```


# Ejercicios guiados 
¿Qué comando utilizaría para determinar qué extensiones Xorg están disponibles en un sistema?
xdpyinfo 



Acaba de recibir un nuevo mouse de 10 botones para su computadora, sin embargo, requerirá una configuración extra para que todos los botones funcionen correctamente. Sin modificar el resto de la configuración del servidor X. ¿Qué directorio utilizaría para crear un nuevo archivo de configuración para este mouse y qué sección de configuración específica se utilizaría en este archivo?
El directorios /etc/X11/xorg.confi.d, y la seccion especifica a configurar del mause seria InputDevice 


¿Qué componente de una instalación de Linux es responsable de mantener un servidor X funcionando?
El administrador de pantalla (Display manager)

¿Qué opción en la línea de comandos se utiliza con el comando X para crear un nuevo archivo de configuración xorg.conf?

-configure, el comando completo es X -configure o Xorg -configure 


# Ejercicios exploratorios 
¿Cuál sería el contenido de la variable de entorno DISPLAY en un sistema llamado lab01 con una configuración de pantalla única? Supongamos que la variable de entorno DISPLAY se visualiza en un emulador de terminal en la tercera pantalla independiente.
Como es una tercera pantalla e independiente, el resultado deberia ser algo como :0.2, ya que la primera es :0.0, la segunda :0.1 y la tercera :0.2

¿Qué comando se puede usar para crear un archivo de configuración del teclado para ser usado por el Sistema X Window?
el comando localectl 

En una instalación típica de Linux un usuario puede cambiar a una terminal virtual presionando las teclas kbd:[Ctrl+Alt+F1]-kbd:[F6] en un teclado. Se le ha pedido que configure un sistema de kiosco con una interfaz gráfica y necesita que esta función esté desactivada para evitar la manipulación no autorizada del sistema. Usted decide crear un archivo de configuración /etc/X11/xorg.conf.d/10-kiosk.conf. Utilizando una sección ServerFlags (que se utiliza para establecer las opciones globales de Xorg en el servidor). ¿Qué opción habría que especificar? Revise la página del manual de Xorg(1) para buscar la opción.

la opcion es DontVTSwitch y se le asigna un valor booleano, por lo que el formato sera:

Section "ServerFlags"
    Option "DontVTSwitch" "True"
EndSection 








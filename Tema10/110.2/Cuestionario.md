
# Ejercicios Exploratorios 

1. ¿Cómo se puede desbloquear la cuenta emma previamente bloqueada?

Se pude hacer con dos comandos 

usermod -L leonardo o usermod --lock leonardo
passwd -l leonardo o passwd --lock leonardo

2. Anteriormente la cuenta emma tenía una fecha de caducidad establecida. ¿Cómo se puede fijar la fecha de caducidad en nunca?
Se puede usar el comando chage, como:
chage -E -1 leonardo

3. Imagine que el servicio de impresión CUPS que gestiona los trabajos de impresión no es necesario en su servidor. 
¿Cómo puede desactivar el servicio de forma permanente? ¿Cómo puede comprobar que el puerto correspondiente ya no está activo?

systemctl disable cups 

y lo comprobarica con: lsof -i:631


4. Ha instalado el servidor web nginx. Cómo puede comprobar si nginx admite TCP wrappers ?

ldd $(which nginx) y comrpobaria si se esta usando la libreria libwrap, si esta presente se puede usar TCP wrappers 


# Ejercicios Guiados 

1. Compruebe si la existencia del archivo /etc/nologin impide el inicio de sesión del usuario root.

El archivo nologin solo impide ingresar al sistema con usuarios diferentes a root. Root puede ingresar al sistema sin problema, aunque este presente nologin

2. ¿La existencia del archivo /etc/nologin impide el inicio de sesión sin contraseña con claves SSH?
Si, tambien los inicios de sesion sin contrasenias 

3. ¿Qué sucede en el inicio de sesión, cuando el archivo /etc/nologin contiene esta línea de texto login currently is not possible solamente?
Esta linea se imprime cuando un usuario sin privilegios intenta ingresar al sistema 

4. ¿Puede un usuario ordinario emma obtener información sobre el usuario root contenida en el fichero /etc/passwd por ejemplo con el comando grep root /etc/passwd?
Si, ya que este es legible para cualquier usuario sin privilegios, lo que vera en el apartado de la passwor sera solamente la x, ya que la pasword esta en el archivo shadow

5. ¿Puede un usuario ordinario emma obtener información sobre el usuario root contenida en el fichero /etc/passwd por ejemplo con el comando grep root /etc/passwd?

6. ¿Qué pasos hay que dar para habilitar y comprobar que el servicio heredado daytime sea manejado por xinetd? Tenga en cuenta que esto es sólo un ejercicio de exploración no hacer esto en un entorno de producción.
	1. Comprobar que tengamos instalados tanto daytime y xinetd
	2. Apagar el servicio daytime
	3. Agregar las siguientes lineas a un archivo llamado daytime en /etc/xinet.d/
		```
		service daytime
		{
			disable		= no
			socket_type	= stream
			protocol	= tcp
			wait		= no
			user		= root
			server		= /usr/sbin/daytime
			server_args 	= -i
			flags		= IPv4
		}
		```
	4. Habilitar el servicio xinetd 
	5. Comprobar el puerto con lsof -i, deberia de darnos como salida que se esta ejecutando el servicio xinetd en el puerto 13 

# Configuracion de la seguridad del sistema 

En esta leccion se explicaran cuatro formas basicas de mejorar la seguridad del host:

1. Algunos comandos basicos y ajustes de configuracion para mejorar la seguridad de la autenticacion con contraseñas shadow
2. Como utilizar los superdeamons para escuchar las conexiones de red entrantes
3. Comprobacion de los servicios de red en busca de demonios innecesarios
4. TCP wrappers como una espcie de firewall simple 


## Algunos comandos basicos y ajustes de configuracion para mejorar la seguridad de la autenticacion con contraseñas shadow

Tanto passwd y chage son usados para la configuracion de las contrasenias.

Como superusuario puede configurar la contrasenia para el usuario `emma` con el siguiente comando

```
sudo passwd emma
New password:
Retype new password:
passwd: password updated successfully
```

Para listar el tiempo de expiracion de la contrasenia y otros ajustes de la misma, usaremos, chage

```
sudo chage -l emma
```

Para evitar que el usuario emma se registre en el sistema, el superusuario puede establecer una fecha de caducidad de la contrasenia que preceda de la fecha actual, por ejemplo:

```
sudo chage -E 2020-03-26 emma
```

Otra alternativa es usar passwd como:

```
sudo passwd -l emma
```

Para evitar que todos los usuario, exepto el usuario root, inicien sesion en el sistema temporalmente, el superusuario puede crear un archivo llamado `/etc/nologin`.
Este archivo puede contener un mensaje notificandoles la causa de por que no puede acceder al sistema.

Tenga en cuenta que tambien hay un comando `nologin` que se puede utilizar para evitar un inicio de sesion cuando se establece un shell por defecto para un usuario.

ejemplo:

```
$ sudo usermod -s /sbin/nologin emma
```

Para mas detalles, puedes consultar la pagina man

```
man 5 nologin
```


## Como utilizar los superdeamons para escuchar las conexiones de red entrantes













## Comprobacion de los servicios de red en busca de demonios innecesarios
## TCP wrappers como una espcie de firewall simple 





















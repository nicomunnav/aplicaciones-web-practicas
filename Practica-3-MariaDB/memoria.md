

# Práctica MariaDB y PHP en Ubuntu 24.04

## Apartado 1. Preparación del sistema

Primero desactivamos los sitios virtuales anteriores y dejamos activo el sitio predeterminado de Apache.

### Activar el sitio predeterminado


sudo a2ensite 000-default.conf
sudo systemctl reload apache2

Estos comandos activan el sitio predeterminado y recargan Apache para aplicar los cambios.

### Actualizar el sistema

sudo apt update
sudo apt upgrade -y


El primer comando actualiza la lista de paquetes y el segundo instala las actualizaciones disponibles.

### Comprobar Apache

systemctl is-active apache2


El resultado esperado es active, que significa que Apache está funcionando.

**Captura 1:** Comprobación de Apache.

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%201%20Active%20.png)



## Apartado 2. Instalación de MariaDB

### Instalar MariaDB

sudo apt install mariadb-server -y

Este comando instala MariaDB, el programa que se encarga de gestionar las bases de datos.

### Comprobar la versión

mariadb --version

Con este comando podemos ver la versión de MariaDB instalada.

**Captura 2:** Versión de MariaDB.

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%202%20instalaci%C3%B3n%20mariadb.png)

## Apartado 3. Comprobación del funcionamiento

### Comprobar el estado del servicio


sudo systemctl status mariadb

Este comando muestra si MariaDB está funcionando. Debe aparecer `active (running)`. Para salir, pulsamos `q`.

### Comprobar el inicio automático

systemctl is-enabled mariadb

Este comando comprueba si MariaDB se inicia automáticamente al encender el equipo. Debe aparecer `enabled`.

### Entrar en MariaDB
sudo mariadb

Este comando abre la consola de MariaDB y permite comprobar que podemos entrar.

Para salir:
exit


**Captura 3:** Estado del servicio y entrada en MariaDB.

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%203%20Estado%20MariaDB.png)



## Apartado 4. Comandos principales


## Apartado 5. ¿Qué pasa si MariaDB está parado?

### Detener el servicio


sudo systemctl stop mariadb

Este comando detiene MariaDB para comprobar qué ocurre cuando no funciona.

### Comprobar el estado


systemctl is-active mariadb


El resultado esperado es `inactive`, porque el servicio está parado.

### Intentar entrar en MariaDB


sudo mariadb
```

Al estar parado el servicio, puede aparecer un error de conexión.

### Volver a arrancar MariaDB


sudo systemctl start mariadb
systemctl is-active mariadb
```

El primer comando arranca el servicio y el segundo comprueba que vuelve a funcionar. Debe aparecer `active`.

**Pregunta:** ¿Qué dos comandos usarías para saber si MariaDB está parado?

**Respuesta:** Usaría `systemctl is-active mariadb` para comprobar el estado y `sudo mariadb` para intentar entrar en la base de datos.

**Captura 4:** Error y recuperación del servicio.

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%205%205.%20Comprobar%20qu%C3%A9%20pasa%20si%20MariaDB%20est%C3%A1%20parado.png)

---

## Apartado 6. Instalación de PHP

### Instalar PHP y los paquetes necesarios


sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y


Este comando instala PHP y los paquetes necesarios para trabajar con Apache y MariaDB.

### Comprobar la versión de PHP

php -v


Muestra la versión de PHP instalada.

### Reiniciar Apache


sudo systemctl restart apache2


Reinicia Apache para que cargue los cambios.

### Comprobar los módulos de Apache y PHP


sudo apache2ctl -M | grep -E "mpm|php"


Comprueba los módulos relacionados con Apache y PHP.

### Comprobar los módulos de MySQL


php -m | grep -i mysql

Comprueba los módulos que permiten a PHP conectarse con MariaDB. Deben aparecer `mysqli`, `mysqlnd` y `pdo_mysql`.

### Crear una página de prueba


echo '<?php phpinfo();' | sudo tee /var/www/html/info.php


Crea una página que muestra información de PHP.

Para conocer la IP de Ubuntu:

hostname -I


Después, abrimos en el navegador `http://IP_DE_TU_SERVIDOR/info.php`, sustituyendo la dirección por la IP que aparezca en el terminal.

### Borrar el archivo de prueba


sudo rm /var/www/html/info.php


Borra el archivo de prueba para no dejar información del servidor publicada.


ls /var/www/html


Comprueba los archivos que quedan en la carpeta web.

**Pregunta:** ¿Por qué es peligroso dejar `info.php` y para qué sirve `php-mysql`?

**Respuesta:** `info.php` muestra información interna del servidor y podría ser utilizada por otras personas. El paquete `php-mysql` permite que PHP se conecte a MariaDB.

**Captura 5:** Versión de PHP, módulos y página de prueba.

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

---

## Apartado 7. Comprobación final

### Comprobar los servicios

systemctl is-active apache2 mariadb

Los dos servicios deben mostrar `active`. En mi caso, ambos están funcionando correctamente.

### Comprobar PHP
php -v

Este comando muestra la versión instalada. En mi caso, aparece PHP 8.3.6.

**Captura 6:** Comprobación final de Apache, MariaDB y PHP.

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)



## Conclusiones

En esta práctica he aprendido a instalar MariaDB y PHP en Ubuntu 24.04 y a comprobar que funcionan junto con Apache.

También he aprendido a controlar los servicios, comprobar sus versiones y solucionar un problema cuando MariaDB está parado.

Al terminar, el servidor está preparado para continuar con la instalación de WordPress.

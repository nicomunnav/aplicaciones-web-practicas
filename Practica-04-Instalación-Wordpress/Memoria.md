# Práctica guiada WordPress

### Instalación de WordPress en Ubuntu 24.04

<br>

**Módulo:** Aplicaciones Web

**Alumno:** Nicolás Muñoz Navarro

**Curso:** 2 SMX

**Fecha:** 28/10/2026

<br>
---
**Memoria de la actividad práctica**
---

# Índice

1. [Conexión por SSH y comprobación del servidor](#apartado-1)
2. [Dejar Apache con un único sitio](#apartado-2)
3. [Crear la base de datos de WordPress](#apartado-3)
4. [Descargar e instalar los ficheros de WordPress](#apartado-4)
5. [Terminar la instalación desde el navegador](#apartado-5)
6. [Comprobamos que WordPress funciona](#apartado-6)
7. [Para que no se rompa si cambia la IP](#apartado-7)
8. [Dos ajustes para trabajar cómodamente](#apartado-8)
9. [¿Qué pasa si MariaDB se para?](#apartado-9)
10. [Comprobación final](#apartado-10)
11. [Conclusiones](#apartado-11)

---

## Apartado 1

**Nos conectamos por SSH y comprobamos que el servidor funciona.**

- **hostname -I**
- **ssh ls_nmunoz@192.168.56.22**
- **systemctl is-active apache2 mariadb**
- **php -v**

El primer comando muestra la IP del servidor, que es 192.168.56.22. El segundo nos conecta al servidor desde la terminal. El tercero comprueba si Apache y MariaDB están activos y el cuarto muestra la versión de PHP.

### Captura 1. Conexión SSH y comprobación del servidor

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura%201.png)

#### ¿Qué dos puertas usamos para trabajar con el servidor?

SSH (puerto 22), que usamos desde la terminal para administrar el servidor, y HTTP (puerto 80), que usamos desde el navegador para ver WordPress y su panel.

---

## Apartado 2

**Dejamos Apache con un único sitio.**

- **sudo a2dissite sitio1.conf sitio2.conf**
- **sudo a2ensite 000-default.conf**
- **sudo apache2ctl configtest**
- **sudo systemctl reload apache2**

Desactivamos sitio1 y sitio2, activamos el sitio por defecto, comprobamos la configuración y recargamos Apache.

- **ls /etc/apache2/sites-enabled/**
- **grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf**

### Captura 2. Un solo sitio activo y su DocumentRoot

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura2.png)

### Captura 3. Página en el navegador

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura3.png)

#### ¿Qué diferencia hay entre desactivar un sitio con a2dissite y borrar sus ficheros?

Con a2dissite el sitio solo se desactiva y los ficheros siguen ahí, así que se puede volver a activar. Si se borran los ficheros se pierde el contenido.

#### ¿Por qué ahora vemos la página por defecto y antes veíamos sitio1?

Porque ahora el único sitio activo es el de por defecto, que usa /var/www/html. Antes entrábamos con sitio1.local y Apache nos enseñaba ese sitio.

---

## Apartado 3

**Creamos la base de datos y el usuario de WordPress.**

- **sudo mariadb**
- **CREATE DATABASE wordpress;**
- **CREATE USER 'wp_user'@'localhost' IDENTIFIED BY '\*\*\*\*\*\*\*\*';**
- **GRANT ALL ON wordpress.\* TO 'wp_user'@'localhost';**
- **exit;**

El primero crea la base de datos, el segundo crea el usuario con su contraseña y el tercero le da permisos solo sobre la base wordpress.

- **mariadb -u wp_user -p wordpress**

Con este comando entramos con el nuevo usuario para comprobar que funciona.

### Captura 4. Base de datos y usuario de WordPress

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura%204.png)

#### ¿Por qué WordPress usa wp_user y no root?

Por seguridad. Si alguien entra en WordPress con root podría tocar todas las bases de datos del servidor, y con wp_user solo puede tocar la de WordPress.

---

## Apartado 4

**Descargamos WordPress y lo copiamos a Apache.**

- **sudo apt install wget -y**
- **cd /tmp**
- **wget https://wordpress.org/latest.tar.gz**
- **tar -xzf latest.tar.gz**
- **ls wordpress**
- **sudo rm /var/www/html/index.html**
- **sudo cp -a /tmp/wordpress/. /var/www/html/**
- **rm -r /tmp/wordpress /tmp/latest.tar.gz**
- **ls /var/www/html**

#### ¿Qué hace cada comando?

**sudo apt install wget -y** instala wget para poder descargar ficheros. **cd /tmp** nos mueve a la carpeta temporal. **wget** descarga la última versión de WordPress. **tar -xzf** la descomprime y crea la carpeta wordpress. **ls wordpress** muestra lo que hay dentro. **sudo rm /var/www/html/index.html** borra la página de bienvenida de Apache para que no tenga prioridad sobre WordPress. **sudo cp -a** copia todo el contenido de WordPress a la carpeta de Apache. **rm -r** borra la carpeta y el comprimido que ya no hacen falta y **ls /var/www/html** comprueba que se ha copiado todo.

Después le damos los permisos:

- **sudo chown -R www-data:www-data /var/www/html**
- **sudo find /var/www/html -type d -exec chmod 755 {} \;**
- **sudo find /var/www/html -type f -exec chmod 644 {} \;**
- **ls -ld /var/www/html**

#### ¿Qué hace cada orden de los permisos?

**chown -R www-data:www-data** cambia el propietario y el grupo de toda la carpeta a www-data. Los dos **find** ponen permisos 755 a las carpetas y 644 a los ficheros. **ls -ld** muestra el propietario y los permisos de la carpeta.

#### ¿Por qué hay que cambiar el propietario a www-data?

Porque Apache funciona con el usuario www-data. Si la carpeta siguiera siendo de mi usuario, WordPress no podría escribir en ella y fallaría al crear el wp-config.php, subir imágenes o instalar plugins.

---

## Apartado 5

**Terminamos la instalación desde el navegador.**

Abrimos http://192.168.56.22 en el navegador y seguimos el asistente: idioma Español, datos de la base de datos y datos del sitio.

### Captura 5. Datos de la base de datos

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura5.png)

En la base de datos puse wordpress, el usuario wp_user, la contraseña \*\*\*\*\*\*\*\*, el servidor localhost y el prefijo wp_. En los datos del sitio puse como título "Servidor de Nico" y como usuario nico.

#### ¿Qué partes se han hecho en la terminal y cuáles en el navegador?

En la terminal preparamos Apache, creamos la base de datos, descargamos WordPress, lo copiamos y le dimos permisos. En el navegador terminamos la instalación con el asistente.

#### ¿Por qué el servidor no necesita interfaz gráfica?

Porque WordPress es una web. El servidor solo envía las páginas con Apache y es el navegador del ordenador el que las dibuja.

---

## Apartado 6

**Comprobamos que WordPress funciona.**

Abrimos la web en http://192.168.56.22 y el panel en http://192.168.56.22/wp-admin. Publicamos una entrada de prueba llamada "Instalación WordPress" con mi nombre y la fecha, que se puede ver en la captura 15.

- **sudo mariadb wordpress -e "SHOW TABLES;"**

### Captura 6. Tablas de WordPress

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Apartado%206.4.png)

Salen las tablas que ha creado WordPress y todas empiezan por wp_. wp_posts guarda las entradas, wp_users los usuarios y wp_options la configuración del sitio.

#### ¿Dónde guarda WordPress las entradas?

En la base de datos, no en ficheros. Lo comprobé con **SHOW TABLES;**, donde aparece la tabla wp_posts.

---

## Apartado 7

**Hacemos que WordPress funcione aunque cambie la IP.**

- **sudo nano /var/www/html/wp-config.php**

Antes de la línea que empieza por "That's all, stop editing!" añadimos estas dos líneas:

- **define( 'WP_HOME', 'http://' . $_SERVER['HTTP_HOST'] );**
- **define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] );**

Así WordPress usa siempre la dirección con la que entramos a la web.

- **php -l /var/www/html/wp-config.php**

### Captura 7. Líneas añadidas en wp-config.php

![Captura 7](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap%207.png)

### Captura 8. Comprobación con php -l

![Captura 8](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap%207.2.png)

---

## Apartado 8

**Hacemos dos ajustes para trabajar mejor con WordPress.**

Primero permitimos a Apache leer el fichero .htaccess:

- **sudo nano /etc/apache2/conf-available/wordpress.conf**

Y escribimos dentro:

- **\<Directory /var/www/html\>**
- **AllowOverride All**
- **\</Directory\>**

### Captura 9. Fichero wordpress.conf

![Captura 9](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap%208.1.png)

- **sudo a2enmod rewrite**
- **sudo a2enconf wordpress**
- **sudo apache2ctl configtest**
- **sudo systemctl restart apache2**

### Captura 10. Activación de la configuración

![Captura 10](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8.1.2.png)

Después, en el panel de WordPress, fui a Ajustes, Enlaces permanentes y elegí "Nombre de la entrada".

### Captura 11. Enlaces permanentes

![Captura 11](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap%208.1.3.png)

- **ls -la /var/www/html/.htaccess**

### Captura 12. Fichero .htaccess

![Captura 12](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8.1.4.png)

Ahora subimos el límite de subida de ficheros de PHP a 64 MB:

- **printf 'upload_max_filesize = 64M\npost_max_size = 64M\n' | sudo tee /etc/php/8.3/apache2/conf.d/99-wordpress.ini**
- **sudo systemctl restart apache2**

### Captura 13. Límite de subida de PHP

![Captura 13](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8.2.png)

#### ¿Por qué las direcciones bonitas dan error 404 si Apache no puede leer el .htaccess?

Porque la dirección bonita no es un fichero real. El .htaccess es el que manda la petición a index.php para que WordPress busque la entrada. Si Apache lo ignora, busca esa ruta como fichero, no la encuentra y da error 404.

---

## Apartado 9

**Comprobamos qué pasa si MariaDB se para.**

- **sudo systemctl stop mariadb**

Después recargamos la web en el navegador.

### Captura 14. Error de conexión con la base de datos

![Captura 14](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9.png)

Sale el error de conexión con la base de datos porque WordPress no puede funcionar sin MariaDB. Para arreglarlo la arrancamos otra vez:

- **sudo systemctl start mariadb**
- **systemctl is-active mariadb**

### Captura 15. MariaDB arrancado y web funcionando

![Captura 15](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9.2.png)

#### ¿Qué dos cosas comprobarías si sale ese error y en qué orden?

Primero si MariaDB está en marcha con **systemctl is-active mariadb**. Después si el usuario y la contraseña siguen bien, probando a entrar con **mariadb -u wp_user -p wordpress**.

---

## Apartado 10

**Comprobación final.**

- **systemctl is-active apache2 mariadb**
- **php -v**
- **ls /etc/apache2/sites-enabled/**
- **sudo mariadb wordpress -e "SHOW TABLES;"**

### Captura 16. Comprobación final

![Captura 16](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap10.png)

Estado final del servidor:

Linux: Ubuntu 24.04, instalado.

Apache: Apache2 con un único sitio, funcionando.

MariaDB: MariaDB 10.11, funcionando y con la base de datos wordpress.

PHP: PHP 8.3, funcionando.

WordPress: instalado en /var/www/html, funcionando y accesible desde el navegador.

---

## Apartado 11

**Conclusiones.**

En esta práctica hemos instalado WordPress sobre Apache, PHP y MariaDB. He aprendido que WordPress es una web y por eso el servidor no necesita interfaz gráfica, y que guarda todo en la base de datos, así que sin MariaDB no funciona.

---

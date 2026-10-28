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
4. [Descargar e instalar WordPress](#apartado-4)
5. [Terminar la instalación desde el navegador](#apartado-5)
6. [Comprobamos que WordPress funciona](#apartado-6)
7. [Para que no se rompa si cambia la IP](#apartado-7)
8. [Dos ajustes para trabajar cómodamente](#apartado-8)
9. [¿Qué pasa si MariaDB se para?](#apartado-9)
10. [Comprobación final](#apartado-10)
11. [Conclusiones](#apartado-11)

---

## Apartado 1

**Nos conectamos al servidor por SSH y comprobamos que todo sigue funcionando.**

- "hostname -I"

Este comando muestra la IP del servidor. La mía es 192.168.56.22.

- "ssh ls_nmunoz@192.168.56.22"

Con este comando nos conectamos al servidor desde la terminal. Pide la contraseña del usuario.

- "systemctl is-active apache2 mariadb"
- "php -v"

Con estos comandos comprobamos que Apache y MariaDB están activos y que PHP funciona.

### Captura 1. Conexión SSH y comprobación del servidor

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura_1.png)

En la captura Apache salía como `inactive`, así que lo arranqué con "sudo systemctl start apache2".

#### ¿Qué dos puertas usamos para trabajar con el servidor?

SSH (puerto 22), que se usa desde la terminal para administrar el servidor (instalar, configurar, copiar ficheros). Y HTTP (puerto 80), que se usa desde el navegador para ver WordPress y su panel.

---

## Apartado 2

**Dejamos Apache con un único sitio, que será WordPress.**

- "sudo a2dissite sitio1.conf sitio2.conf"
- "sudo a2ensite 000-default.conf"
- "sudo apache2ctl configtest"
- "sudo systemctl reload apache2"

Los dos primeros desactivan sitio1 y sitio2 y activan el sitio por defecto. El tercero comprueba que la configuración está bien (tiene que salir `Syntax OK`) y el último recarga Apache.

Después comprobamos que solo hay un sitio activo:

- "ls /etc/apache2/sites-enabled/"
- "grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf"

### Captura 2. Sitio activo y DocumentRoot

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura2.png)

Solo aparece `000-default.conf` y la carpeta es `/var/www/html`.

### Captura 3. Página en el navegador

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura3.png)

Entrando con la IP del servidor sale la página "Servidor de Nico" de la primera práctica.

#### ¿Qué diferencia hay entre desactivar un sitio con a2dissite y borrar sus ficheros?

Con a2dissite el sitio solo se desactiva y los ficheros siguen ahí, así que se puede volver a activar. Si se borran los ficheros se pierde el contenido.

#### ¿Por qué ahora vemos la página por defecto y antes veíamos sitio1?

Porque ahora el único sitio activo es el de por defecto, que usa `/var/www/html`. Antes entrábamos con el nombre sitio1.local y Apache nos enseñaba ese sitio.

---

## Apartado 3

**Creamos la base de datos y el usuario para WordPress.**

- "sudo mariadb"

Dentro de MariaDB ejecutamos:

- "CREATE DATABASE wordpress;"
- "CREATE USER 'wp_user'@'localhost' IDENTIFIED BY '********';"
- "GRANT ALL ON wordpress.* TO 'wp_user'@'localhost';"
- "exit;"

El primero crea la base de datos, el segundo crea el usuario con su contraseña y el tercero le da permisos solo sobre la base `wordpress`.

Para comprobar que funciona entramos con el nuevo usuario:

- "mariadb -u wp_user -p wordpress"

### Captura 4. Base de datos y usuario de WordPress

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura_4.png)

Cada orden responde `Query OK` y al entrar con wp_user sale el prompt `MariaDB [wordpress]>`, así que funciona.

#### ¿Por qué WordPress usa wp_user y no root?

Por seguridad. Si alguien consigue entrar en WordPress con root podría tocar todas las bases de datos del servidor. Con wp_user solo puede tocar la base de WordPress.

---

## Apartado 4

**Descargamos WordPress y lo copiamos a la carpeta de Apache.**

- "sudo apt install wget -y"
- "cd /tmp"
- "wget https://wordpress.org/latest.tar.gz"
- "tar -xzf latest.tar.gz"
- "ls wordpress"

Instalamos wget, descargamos WordPress en la carpeta temporal y lo descomprimimos.

- "sudo rm /var/www/html/index.html"
- "sudo cp -a /tmp/wordpress/. /var/www/html/"
- "rm -r /tmp/wordpress /tmp/latest.tar.gz"
- "ls /var/www/html"

Borramos la página de bienvenida de Apache, copiamos WordPress a `/var/www/html`, borramos los ficheros temporales y comprobamos que está todo.

#### ¿Qué hace cada comando?

- `apt install wget -y`: instala wget para poder descargar ficheros.
- `cd /tmp`: nos movemos a la carpeta temporal.
- `wget`: descarga la última versión de WordPress.
- `tar -xzf`: descomprime el fichero y crea la carpeta wordpress.
- `ls wordpress`: muestra su contenido.
- `rm index.html`: quita la página de Apache para que no tenga prioridad sobre WordPress.
- `cp -a`: copia todo el contenido de WordPress a la carpeta web.
- `rm -r`: borra la carpeta y el comprimido, que ya no hacen falta.
- `ls /var/www/html`: comprueba que se ha copiado.

Ahora le damos los permisos:

- "sudo chown -R www-data:www-data /var/www/html"
- "sudo find /var/www/html -type d -exec chmod 755 {} \;"
- "sudo find /var/www/html -type f -exec chmod 644 {} \;"
- "ls -ld /var/www/html"

<!-- FALTA CAPTURA: ls /var/www/html y ls -ld /var/www/html -->

#### ¿Qué hace cada orden de los permisos?

- `chown -R www-data:www-data`: cambia el propietario y el grupo de toda la carpeta a www-data.
- `find ... -type d ... chmod 755`: pone permisos 755 a todas las carpetas.
- `find ... -type f ... chmod 644`: pone permisos 644 a todos los ficheros.
- `ls -ld`: muestra el propietario y los permisos de la carpeta.

#### ¿Por qué hay que cambiar el propietario a www-data?

Porque Apache funciona con el usuario www-data. Si la carpeta siguiera siendo de mi usuario, WordPress no podría escribir en ella y fallaría al crear el wp-config.php, subir imágenes o instalar plugins.

---

## Apartado 5

**Terminamos la instalación desde el navegador.**

Abrimos `http://192.168.56.22` en el navegador y seguimos el asistente: idioma Español, datos de la base de datos y datos del sitio.

### Captura 5. Datos de la base de datos

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura5.png)

Datos usados: base de datos `wordpress`, usuario `wp_user`, contraseña `********`, servidor `localhost` y prefijo `wp_`.

<!-- FALTA CAPTURA: pantalla 5 del instalador (datos del sitio, contraseña tapada) -->

<!-- FALTA CAPTURA: escritorio de WordPress una vez dentro -->

En la pantalla de los datos del sitio puse como título "Servidor de Nico" y como usuario `nico` (no admin, porque es el primero que prueban los atacantes). También marqué que se disuadan los motores de búsqueda porque es un sitio de prácticas.

Al enviar los datos de la base, WordPress crea él solo el fichero `wp-config.php`. Después crea las tablas y el usuario administrador.

#### ¿Qué partes se han hecho en la terminal y cuáles en el navegador?

En la terminal: preparar Apache, crear la base de datos, descargar WordPress, copiarlo y darle permisos. En el navegador: terminar la instalación con el asistente y usar el panel.

#### ¿Por qué el servidor no necesita interfaz gráfica?

Porque WordPress es una web. El servidor solo envía las páginas con Apache y es el navegador del ordenador el que las dibuja.

---

## Apartado 6

**Comprobamos que WordPress funciona.**

Abrimos la web (`http://192.168.56.22`) y el panel (`http://192.168.56.22/wp-admin`). Después publicamos una entrada de prueba en *Entradas → Añadir nueva* llamada "Instalación WordPress", con mi nombre y la fecha. La entrada publicada se ve en la captura 15.

Para ver dónde se guarda, miramos las tablas de la base de datos:

- "sudo mariadb wordpress -e "SHOW TABLES;""

### Captura 6. Tablas de WordPress

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Apartado_6_4.png)

Salen las 12 tablas que ha creado WordPress, todas empiezan por `wp_`. La tabla `wp_posts` guarda las entradas, `wp_users` los usuarios y `wp_options` la configuración.

#### ¿Dónde guarda WordPress las entradas?

En la base de datos, no en ficheros. Lo comprobé con `SHOW TABLES;`, donde aparece `wp_posts`. En `/var/www/html` solo está el código, los temas, los plugins y las imágenes.

---

## Apartado 7

**Hacemos que WordPress funcione aunque cambie la IP.**

- "sudo nano /var/www/html/wp-config.php"

Antes de la línea `That's all, stop editing!` añadimos estas dos líneas:

```php
define( 'WP_HOME',    'http://' . $_SERVER['HTTP_HOST'] );
define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] );
```

Así WordPress usa siempre la dirección con la que se entra a la web, en vez de la que se guardó al instalar.

- "php -l /var/www/html/wp-config.php"

Este comando comprueba que el fichero no tiene errores.

### Captura 7. Líneas añadidas en wp-config.php

![Captura 7](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_7.png)

### Captura 8. Comprobación con php -l

![Captura 8](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_7_2.png)

Sale `No syntax errors detected`, así que está bien.

---

## Apartado 8

**Hacemos dos ajustes para trabajar mejor con WordPress.**

### 8.1. Direcciones bonitas

Primero permitimos a Apache leer el fichero .htaccess:

- "sudo nano /etc/apache2/conf-available/wordpress.conf"

Con este contenido:

```apache
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

### Captura 9. Fichero wordpress.conf

![Captura 9](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_8_1.png)

Después lo activamos:

- "sudo a2enmod rewrite"
- "sudo a2enconf wordpress"
- "sudo apache2ctl configtest"
- "sudo systemctl restart apache2"

### Captura 10. Activación de la configuración

![Captura 10](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_1_2.png)

El módulo rewrite ya estaba activado, wordpress se ha activado y el configtest sale `Syntax OK`.

Después en el panel de WordPress fui a *Ajustes → Enlaces permanentes* y elegí "Nombre de la entrada".

### Captura 11. Enlaces permanentes

![Captura 11](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_8_1_3.png)

- "ls -la /var/www/html/.htaccess"

### Captura 12. Fichero .htaccess

![Captura 12](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_1_4.png)

WordPress ha creado el .htaccess él solo (es de www-data). La dirección bonita de la entrada se ve en la captura 15.

### 8.2. Subir ficheros grandes

- "printf 'upload_max_filesize = 64M\npost_max_size = 64M\n' | sudo tee /etc/php/8.3/apache2/conf.d/99-wordpress.ini"
- "sudo systemctl restart apache2"

PHP solo deja subir 2 MB por defecto. Con este comando creamos un fichero de configuración que lo sube a 64 MB y reiniciamos Apache para que lo lea.

### Captura 13. Límite de subida de PHP

![Captura 13](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_2.png)

<!-- FALTA CAPTURA: Medios → Añadir nuevo con el tamaño máximo de 64 MB -->

#### ¿Por qué las direcciones bonitas dan error 404 si Apache no puede leer el .htaccess?

Porque la dirección bonita no es un fichero real. El .htaccess es el que manda la petición a index.php para que WordPress busque la entrada. Si Apache lo ignora, busca esa ruta como fichero, no la encuentra y da error 404.

---

## Apartado 9

**Comprobamos qué pasa si MariaDB se para.**

- "sudo systemctl stop mariadb"

Después recargamos la web en el navegador.

### Captura 14. Error de conexión con la base de datos

![Captura 14](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9.png)

Sale el error *Error establishing a database connection*, porque WordPress no puede funcionar sin la base de datos.

Para arreglarlo arrancamos MariaDB otra vez:

- "sudo systemctl start mariadb"
- "systemctl is-active mariadb"

### Captura 15. MariaDB arrancado y web funcionando

![Captura 15](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9_2.png)

A la izquierda se ve el arranque de MariaDB y a la derecha la web funcionando otra vez, con la entrada publicada y su dirección bonita.

#### ¿Qué dos cosas comprobarías si sale ese error y en qué orden?

Primero si MariaDB está en marcha con `systemctl is-active mariadb`. Después si el usuario y la contraseña siguen bien, probando a entrar con `mariadb -u wp_user -p wordpress`.

---

## Apartado 10

**Comprobación final.**

- "systemctl is-active apache2 mariadb"
- "php -v"
- "ls /etc/apache2/sites-enabled/"
- "sudo mariadb wordpress -e "SHOW TABLES;""

### Captura 16. Comprobación final

![Captura 16](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap10.png)

Apache y MariaDB están activos, PHP es la 8.3.6, solo hay un sitio activo y la base de datos tiene sus tablas.

### Estado final del servidor

| Pieza | Programa | Estado |
| --- | --- | --- |
| **L**inux | Ubuntu 24.04 | Instalado |
| **A**pache | Apache2, con un único sitio | Funcionando |
| **M**ariaDB | MariaDB 10.11 | Funcionando, con la base wordpress |
| **P**HP | PHP 8.3 | Funcionando |
| **WordPress** | Instalado en /var/www/html | Funcionando y accesible desde el navegador |

---

## Apartado 11

**Conclusiones.**

En esta práctica hemos instalado WordPress sobre Apache, PHP y MariaDB. He aprendido que WordPress es una web y por eso el servidor no necesita interfaz gráfica: se administra por SSH y se usa desde el navegador. También que guarda todo en la base de datos y que sin MariaDB no funciona.

#### Problemas encontrados

- Al principio Apache salía `inactive` y lo arranqué con "sudo systemctl start apache2".
- Sin permitir el .htaccess en Apache las direcciones bonitas darían error 404, así que creé el wordpress.conf con `AllowOverride All`.

---# Práctica guiada WordPress

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
4. [Descargar e instalar WordPress](#apartado-4)
5. [Terminar la instalación desde el navegador](#apartado-5)
6. [Comprobamos que WordPress funciona](#apartado-6)
7. [Para que no se rompa si cambia la IP](#apartado-7)
8. [Dos ajustes para trabajar cómodamente](#apartado-8)
9. [¿Qué pasa si MariaDB se para?](#apartado-9)
10. [Comprobación final](#apartado-10)
11. [Conclusiones](#apartado-11)

---

## Apartado 1

**Nos conectamos al servidor por SSH y comprobamos que todo sigue funcionando.**

- **hostname -I**

Este comando muestra la IP del servidor. La mía es 192.168.56.22.

- **ssh ls_nmunoz@192.168.56.22**

Con este comando nos conectamos al servidor desde la terminal. Pide la contraseña del usuario.

- **systemctl is-active apache2 mariadb**
- **php -v**

Con estos comandos comprobamos que Apache y MariaDB están activos y que PHP funciona.

### Captura 1. Conexión SSH y comprobación del servidor

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura_1.png)

En la captura Apache salía como **inactive**, así que lo arranqué con **sudo systemctl start apache2**.

#### ¿Qué dos puertas usamos para trabajar con el servidor?

SSH (puerto 22), que se usa desde la terminal para administrar el servidor (instalar, configurar, copiar ficheros). Y HTTP (puerto 80), que se usa desde el navegador para ver WordPress y su panel.

---

## Apartado 2

**Dejamos Apache con un único sitio, que será WordPress.**

- **sudo a2dissite sitio1.conf sitio2.conf**
- **sudo a2ensite 000-default.conf**
- **sudo apache2ctl configtest**
- **sudo systemctl reload apache2**

Los dos primeros desactivan sitio1 y sitio2 y activan el sitio por defecto. El tercero comprueba que la configuración está bien (tiene que salir **Syntax OK**) y el último recarga Apache.

Después comprobamos que solo hay un sitio activo:

- **ls /etc/apache2/sites-enabled/**
- **grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf**

### Captura 2. Sitio activo y DocumentRoot

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura2.png)

Solo aparece **000-default.conf** y la carpeta es **/var/www/html**.

### Captura 3. Página en el navegador

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura3.png)

Entrando con la IP del servidor sale la página "Servidor de Nico" de la primera práctica.

#### ¿Qué diferencia hay entre desactivar un sitio con a2dissite y borrar sus ficheros?

Con a2dissite el sitio solo se desactiva y los ficheros siguen ahí, así que se puede volver a activar. Si se borran los ficheros se pierde el contenido.

#### ¿Por qué ahora vemos la página por defecto y antes veíamos sitio1?

Porque ahora el único sitio activo es el de por defecto, que usa **/var/www/html**. Antes entrábamos con el nombre sitio1.local y Apache nos enseñaba ese sitio.

---

## Apartado 3

**Creamos la base de datos y el usuario para WordPress.**

- **sudo mariadb**

Dentro de MariaDB ejecutamos:

- **CREATE DATABASE wordpress;**
- **CREATE USER 'wp_user'@'localhost' IDENTIFIED BY '\*\*\*\*\*\*\*\*';**
- **GRANT ALL ON wordpress.* TO 'wp_user'@'localhost';**
- **exit;**

El primero crea la base de datos, el segundo crea el usuario con su contraseña y el tercero le da permisos solo sobre la base **wordpress**.

Para comprobar que funciona entramos con el nuevo usuario:

- **mariadb -u wp_user -p wordpress**

### Captura 4. Base de datos y usuario de WordPress

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura_4.png)

Cada orden responde **Query OK** y al entrar con wp_user sale el prompt **MariaDB [wordpress]>**, así que funciona.

#### ¿Por qué WordPress usa wp_user y no root?

Por seguridad. Si alguien consigue entrar en WordPress con root podría tocar todas las bases de datos del servidor. Con wp_user solo puede tocar la base de WordPress.

---

## Apartado 4

**Descargamos WordPress y lo copiamos a la carpeta de Apache.**

- **sudo apt install wget -y**
- **cd /tmp**
- **wget https://wordpress.org/latest.tar.gz**
- **tar -xzf latest.tar.gz**
- **ls wordpress**

Instalamos wget, descargamos WordPress en la carpeta temporal y lo descomprimimos.

- **sudo rm /var/www/html/index.html**
- **sudo cp -a /tmp/wordpress/. /var/www/html/**
- **rm -r /tmp/wordpress /tmp/latest.tar.gz**
- **ls /var/www/html**

Borramos la página de bienvenida de Apache, copiamos WordPress a **/var/www/html**, borramos los ficheros temporales y comprobamos que está todo.

#### ¿Qué hace cada comando?

- **apt install wget -y**: instala wget para poder descargar ficheros.
- **cd /tmp**: nos movemos a la carpeta temporal.
- **wget**: descarga la última versión de WordPress.
- **tar -xzf**: descomprime el fichero y crea la carpeta wordpress.
- **ls wordpress**: muestra su contenido.
- **rm index.html**: quita la página de Apache para que no tenga prioridad sobre WordPress.
- **cp -a**: copia todo el contenido de WordPress a la carpeta web.
- **rm -r**: borra la carpeta y el comprimido, que ya no hacen falta.
- **ls /var/www/html**: comprueba que se ha copiado.

Ahora le damos los permisos:

- **sudo chown -R www-data:www-data /var/www/html**
- **sudo find /var/www/html -type d -exec chmod 755 {} \;**
- **sudo find /var/www/html -type f -exec chmod 644 {} \;**
- **ls -ld /var/www/html**

<!-- FALTA CAPTURA: ls /var/www/html y ls -ld /var/www/html -->

#### ¿Qué hace cada orden de los permisos?

- **chown -R www-data:www-data**: cambia el propietario y el grupo de toda la carpeta a www-data.
- **find ... -type d ... chmod 755**: pone permisos 755 a todas las carpetas.
- **find ... -type f ... chmod 644**: pone permisos 644 a todos los ficheros.
- **ls -ld**: muestra el propietario y los permisos de la carpeta.

#### ¿Por qué hay que cambiar el propietario a www-data?

Porque Apache funciona con el usuario www-data. Si la carpeta siguiera siendo de mi usuario, WordPress no podría escribir en ella y fallaría al crear el wp-config.php, subir imágenes o instalar plugins.

---

## Apartado 5

**Terminamos la instalación desde el navegador.**

Abrimos **http://192.168.56.22** en el navegador y seguimos el asistente: idioma Español, datos de la base de datos y datos del sitio.

### Captura 5. Datos de la base de datos

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Captura5.png)

Datos usados: base de datos **wordpress**, usuario **wp_user**, contraseña **\*\*\*\*\*\*\*\***, servidor **localhost** y prefijo **wp_**.

<!-- FALTA CAPTURA: pantalla 5 del instalador (datos del sitio, contraseña tapada) -->

<!-- FALTA CAPTURA: escritorio de WordPress una vez dentro -->

En la pantalla de los datos del sitio puse como título "Servidor de Nico" y como usuario **nico** (no admin, porque es el primero que prueban los atacantes). También marqué que se disuadan los motores de búsqueda porque es un sitio de prácticas.

Al enviar los datos de la base, WordPress crea él solo el fichero **wp-config.php**. Después crea las tablas y el usuario administrador.

#### ¿Qué partes se han hecho en la terminal y cuáles en el navegador?

En la terminal: preparar Apache, crear la base de datos, descargar WordPress, copiarlo y darle permisos. En el navegador: terminar la instalación con el asistente y usar el panel.

#### ¿Por qué el servidor no necesita interfaz gráfica?

Porque WordPress es una web. El servidor solo envía las páginas con Apache y es el navegador del ordenador el que las dibuja.

---

## Apartado 6

**Comprobamos que WordPress funciona.**

Abrimos la web (**http://192.168.56.22**) y el panel (**http://192.168.56.22/wp-admin**). Después publicamos una entrada de prueba en *Entradas → Añadir nueva* llamada "Instalación WordPress", con mi nombre y la fecha. La entrada publicada se ve en la captura 15.

Para ver dónde se guarda, miramos las tablas de la base de datos:

- **sudo mariadb wordpress -e "SHOW TABLES;"**

### Captura 6. Tablas de WordPress

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Apartado_6_4.png)

Salen las 12 tablas que ha creado WordPress, todas empiezan por **wp_**. La tabla **wp_posts** guarda las entradas, **wp_users** los usuarios y **wp_options** la configuración.

#### ¿Dónde guarda WordPress las entradas?

En la base de datos, no en ficheros. Lo comprobé con **SHOW TABLES;**, donde aparece **wp_posts**. En **/var/www/html** solo está el código, los temas, los plugins y las imágenes.

---

## Apartado 7

**Hacemos que WordPress funcione aunque cambie la IP.**

- **sudo nano /var/www/html/wp-config.php**

Antes de la línea **That's all, stop editing!** añadimos estas dos líneas:

- **define( 'WP_HOME', 'http://' . $_SERVER['HTTP_HOST'] );**
- **define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] );**

Así WordPress usa siempre la dirección con la que se entra a la web, en vez de la que se guardó al instalar.

- **php -l /var/www/html/wp-config.php**

Este comando comprueba que el fichero no tiene errores.

### Captura 7. Líneas añadidas en wp-config.php

![Captura 7](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_7.png)

### Captura 8. Comprobación con php -l

![Captura 8](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_7_2.png)

Sale **No syntax errors detected**, así que está bien.

---

## Apartado 8

**Hacemos dos ajustes para trabajar mejor con WordPress.**

### 8.1. Direcciones bonitas

Primero permitimos a Apache leer el fichero .htaccess:

- **sudo nano /etc/apache2/conf-available/wordpress.conf**

Con este contenido:

- **\<Directory /var/www/html\>**
- **AllowOverride All**
- **\</Directory\>**

### Captura 9. Fichero wordpress.conf

![Captura 9](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_8_1.png)

Después lo activamos:

- **sudo a2enmod rewrite**
- **sudo a2enconf wordpress**
- **sudo apache2ctl configtest**
- **sudo systemctl restart apache2**

### Captura 10. Activación de la configuración

![Captura 10](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_1_2.png)

El módulo rewrite ya estaba activado, wordpress se ha activado y el configtest sale **Syntax OK**.

Después en el panel de WordPress fui a *Ajustes → Enlaces permanentes* y elegí "Nombre de la entrada".

### Captura 11. Enlaces permanentes

![Captura 11](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap_8_1_3.png)

- **ls -la /var/www/html/.htaccess**

### Captura 12. Fichero .htaccess

![Captura 12](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_1_4.png)

WordPress ha creado el .htaccess él solo (es de www-data). La dirección bonita de la entrada se ve en la captura 15.

### 8.2. Subir ficheros grandes

- **printf 'upload_max_filesize = 64M\npost_max_size = 64M\n' | sudo tee /etc/php/8.3/apache2/conf.d/99-wordpress.ini**
- **sudo systemctl restart apache2**

PHP solo deja subir 2 MB por defecto. Con este comando creamos un fichero de configuración que lo sube a 64 MB y reiniciamos Apache para que lo lea.

### Captura 13. Límite de subida de PHP

![Captura 13](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/8_2.png)

<!-- FALTA CAPTURA: Medios → Añadir nuevo con el tamaño máximo de 64 MB -->

#### ¿Por qué las direcciones bonitas dan error 404 si Apache no puede leer el .htaccess?

Porque la dirección bonita no es un fichero real. El .htaccess es el que manda la petición a index.php para que WordPress busque la entrada. Si Apache lo ignora, busca esa ruta como fichero, no la encuentra y da error 404.

---

## Apartado 9

**Comprobamos qué pasa si MariaDB se para.**

- **sudo systemctl stop mariadb**

Después recargamos la web en el navegador.

### Captura 14. Error de conexión con la base de datos

![Captura 14](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9.png)

Sale el error *Error establishing a database connection*, porque WordPress no puede funcionar sin la base de datos.

Para arreglarlo arrancamos MariaDB otra vez:

- **sudo systemctl start mariadb**
- **systemctl is-active mariadb**

### Captura 15. MariaDB arrancado y web funcionando

![Captura 15](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-04-Instalaci%C3%B3n-Wordpress/Foto/Cap9_2.png)

A la izquierda se ve el arranque de MariaDB y a la derecha la web funcionando otra vez, con la entrada publicada y su dirección bonita.

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

Apache y MariaDB están activos, PHP es la 8.3.6, solo hay un sitio activo y la base de datos tiene sus tablas.

### Estado final del servidor

Linux: Ubuntu 24.04, instalado.

Apache: Apache2 con un único sitio, funcionando.

MariaDB: MariaDB 10.11, funcionando y con la base de datos wordpress.

PHP: PHP 8.3, funcionando.

WordPress: instalado en /var/www/html, funcionando y accesible desde el navegador.

---

## Apartado 11

**Conclusiones.**

En esta práctica hemos instalado WordPress sobre Apache, PHP y MariaDB. He aprendido que WordPress es una web y por eso el servidor no necesita interfaz gráfica: se administra por SSH y se usa desde el navegador. También que guarda todo en la base de datos y que sin MariaDB no funciona.

#### Problemas encontrados

- Al principio Apache salía **inactive** y lo arranqué con **sudo systemctl start apache2**.
- Sin permitir el .htaccess en Apache las direcciones bonitas darían error 404, así que creé el wordpress.conf con **AllowOverride All**.

---

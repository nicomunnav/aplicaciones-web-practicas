**Módulo:** Aplicaciones Web

**Alumno:** Nicolás Muñoz Navarro

**Curso:** 2 SMX

---

**Memoria de la actividad práctica**

# Práctica MariaDB y PHP en Ubuntu 24.04

---

# Índice

1. [Preparación del sistema](#apartado-1-preparación-del-sistema)
2. [Instalación de MariaDB](#apartado-2-instalación-de-mariadb)
3. [Comprobación del funcionamiento](#apartado-3-comprobación-del-funcionamiento)
4. [Comandos principales](#apartado-4-comandos-principales)
5. [Qué pasa si MariaDB está parado](#apartado-5-qué-pasa-si-mariadb-está-parado)
6. [Instalación de PHP](#apartado-6-instalación-de-php)
7. [Comprobación final](#apartado-7-comprobación-final)
8. [Conclusiones](#conclusiones)

---

# Apartado 1. Preparación del sistema

Primero desactivamos los sitios virtuales anteriores y dejamos activo el sitio predeterminado de Apache.

## Activar el sitio predeterminado

Comando utilizado:

sudo a2ensite 000-default.conf
sudo systemctl reload apache2

Estos comandos activan el sitio predeterminado y recargan Apache para aplicar los cambios.

## Actualizar el sistema

Comandos utilizados:

sudo apt update
**Módulo:** Aplicaciones Web

**Alumno:** Nicolás Muñoz Navarro

**Curso:** 2 SMX

---

**Memoria de la actividad práctica**

# Práctica MariaDB y PHP en Ubuntu 24.04

---

# Índice

1. [Preparación del sistema](#apartado-1-preparación-del-sistema)
2. [Instalación de MariaDB](#apartado-2-instalación-de-mariadb)
3. [Comprobación del funcionamiento](#apartado-3-comprobación-del-funcionamiento)
4. [Comandos principales](#apartado-4-comandos-principales)
5. [Qué pasa si MariaDB está parado](#apartado-5-qué-pasa-si-mariadb-está-parado)
6. [Instalación de PHP](#apartado-6-instalación-de-php)
7. [Comprobación final](#apartado-7-comprobación-final)
8. [Conclusiones](#conclusiones)

---

# Apartado 1. Preparación del sistema

Primero desactivamos los sitios virtuales anteriores y dejamos activo el sitio predeterminado de Apache.

## Activar el sitio predeterminado

Comandos utilizados:

*sudo a2ensite 000-default.conf*

*sudo systemctl reload apache2*

Estos comandos activan el sitio predeterminado y recargan Apache para aplicar los cambios.

## Actualizar el sistema

Comandos utilizados:

*sudo apt update*

*sudo apt upgrade -y*

El primer comando actualiza la lista de paquetes y el segundo instala las actualizaciones disponibles.

## Comprobar Apache

Comando utilizado:

*systemctl is-active apache2*

El resultado esperado es active, que significa que Apache está funcionando.

**Captura 1.** Comprobación de Apache.

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%201%20Active%20.png)

---

# Apartado 2. Instalación de MariaDB

## Instalar MariaDB

Comando utilizado:

*sudo apt install mariadb-server -y*

Este comando instala MariaDB, el programa que se encarga de gestionar las bases de datos.

## Comprobar la versión

Comando utilizado:

*mariadb --version*

Con este comando podemos ver la versión de MariaDB instalada.

**Captura 2.** Versión de MariaDB.

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%202%20instalaci%C3%B3n%20mariadb.png)

---

# Apartado 3. Comprobación del funcionamiento

## Comprobar el estado del servicio

Comando utilizado:

*sudo systemctl status mariadb*

Este comando muestra si MariaDB está funcionando. Debe aparecer active (running). Para salir, pulsamos la tecla q.

## Comprobar el inicio automático

Comando utilizado:

*systemctl is-enabled mariadb*

Este comando comprueba si MariaDB se inicia automáticamente al encender el equipo. Debe aparecer enabled.

## Entrar en MariaDB

Comando utilizado:

*sudo mariadb*

Este comando abre la consola de MariaDB y permite comprobar que podemos entrar.

Para salir, escribimos *exit*.

**Captura 3.** Estado del servicio y entrada en MariaDB.

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%203%20Estado%20MariaDB.png)

---

# Apartado 4. Comandos principales

En este apartado aparecen algunos comandos que sirven para controlar MariaDB.

**Consultar el estado del servicio**

*systemctl is-active mariadb*

Permite comprobar si MariaDB está funcionando.

**Iniciar MariaDB**

*sudo systemctl start mariadb*

Inicia el servicio de MariaDB.

**Detener MariaDB**

*sudo systemctl stop mariadb*

Detiene el servicio de MariaDB.

**Reiniciar MariaDB**

*sudo systemctl restart mariadb*

Reinicia el servicio.

**Entrar en MariaDB**

*sudo mariadb*

Permite entrar en la consola para trabajar con las bases de datos.

---

# Apartado 5. ¿Qué pasa si MariaDB está parado?

## Detener el servicio

Comando utilizado:

*sudo systemctl stop mariadb*

Este comando detiene MariaDB para comprobar qué ocurre cuando no funciona.

## Comprobar el estado

Comando utilizado:

*systemctl is-active mariadb*

El resultado esperado es inactive, porque el servicio está parado.

## Intentar entrar en MariaDB

Comando utilizado:

*sudo mariadb*

Al estar parado el servicio, puede aparecer un error de conexión.

## Volver a arrancar MariaDB

Comandos utilizados:

*sudo systemctl start mariadb*

*systemctl is-active mariadb*

El primer comando arranca el servicio y el segundo comprueba que vuelve a funcionar. Debe aparecer active.

**Pregunta:** ¿Qué dos comandos usarías para saber si MariaDB está parado?

**Respuesta:** Usaría *systemctl is-active mariadb* para comprobar el estado y *sudo mariadb* para intentar entrar en la base de datos.

**Captura 4.** Error y recuperación del servicio.

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%205%205.%20Comprobar%20qu%C3%A9%20pasa%20si%20MariaDB%20est%C3%A1%20parado.png)

---

# Apartado 6. Instalación de PHP

## Instalar PHP y los paquetes necesarios

Comando utilizado:

*sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y*

Este comando instala PHP y los paquetes necesarios para trabajar con Apache y MariaDB.

## Comprobar la versión de PHP

Comando utilizado:

*php -v*

Muestra la versión de PHP instalada.

## Reiniciar Apache

Comando utilizado:

*sudo systemctl restart apache2*

Reinicia Apache para que cargue los cambios.

## Comprobar los módulos de Apache y PHP

Comando utilizado:

*sudo apache2ctl -M | grep -E "mpm|php"*

Comprueba los módulos relacionados con Apache y PHP.

## Comprobar los módulos de MySQL

Comando utilizado:

*php -m | grep -i mysql*

Comprueba los módulos que permiten a PHP conectarse con MariaDB. Deben aparecer mysqli, mysqlnd y pdo_mysql.

## Crear una página de prueba

Comando utilizado:

*echo '<?php phpinfo();' | sudo tee /var/www/html/info.php*

Crea una página que muestra información de PHP.

Para conocer la IP de Ubuntu, utilizamos el siguiente comando:

*hostname -I*

Después, abrimos en el navegador la dirección http://IP_DE_TU_SERVIDOR/info.php, sustituyendo el texto por la IP que aparezca en el terminal.

## Borrar el archivo de prueba

Comando utilizado:

*sudo rm /var/www/html/info.php*

Borra el archivo de prueba para no dejar información del servidor publicada.

Para comprobar los archivos que quedan en la carpeta web, utilizamos:

*ls /var/www/html*

**Pregunta:** ¿Por qué es peligroso dejar info.php y para qué sirve php-mysql?

**Respuesta:** info.php muestra información interna del servidor y podría ser utilizada por otras personas. El paquete php-mysql permite que PHP se conecte a MariaDB.

**Captura 5.** Versión de PHP, módulos y página de prueba.

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

---

# Apartado 7. Comprobación final

## Comprobar los servicios

Comando utilizado:

*systemctl is-active apache2 mariadb*

Los dos servicios deben mostrar active. En mi caso, ambos están funcionando correctamente.

## Comprobar PHP

Comando utilizado:

*php -v*

Este comando muestra la versión instalada. En mi caso, aparece PHP 8.3.6.

**Captura 6.** Comprobación final de Apache, MariaDB y PHP.

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

---

# Conclusiones

En esta práctica he aprendido a instalar MariaDB y PHP en Ubuntu 24.04 y a comprobar que funcionan junto con Apache.

También he aprendido a controlar los servicios, comprobar sus versiones y solucionar un problema cuando MariaDB está parado.

Al terminar, el servidor está preparado para continuar con la instalación de WordPress.sudo apt upgrade -y

El primer comando actualiza la lista de paquetes y el segundo instala las actualizaciones disponibles.

## Comprobar Apache

Comando utilizado:

    systemctl is-active apache2

El resultado esperado es active, que significa que Apache está funcionando.

**Captura 1.** Comprobación de Apache.

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%201%20Active%20.png)

---

# Apartado 2. Instalación de MariaDB

## Instalar MariaDB

Comando utilizado:

    sudo apt install mariadb-server -y

Este comando instala MariaDB, el programa que se encarga de gestionar las bases de datos.

## Comprobar la versión

Comando utilizado:

    mariadb --version

Con este comando podemos ver la versión de MariaDB instalada.

**Captura 2.** Versión de MariaDB.

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%202%20instalaci%C3%B3n%20mariadb.png)

---

# Apartado 3. Comprobación del funcionamiento

## Comprobar el estado del servicio

Comando utilizado:

    sudo systemctl status mariadb

Este comando muestra si MariaDB está funcionando. Debe aparecer active (running). Para salir, pulsamos la tecla q.

## Comprobar el inicio automático

Comando utilizado:

    systemctl is-enabled mariadb

Este comando comprueba si MariaDB se inicia automáticamente al encender el equipo. Debe aparecer enabled.

## Entrar en MariaDB

Comando utilizado:

    sudo mariadb

Este comando abre la consola de MariaDB y permite comprobar que podemos entrar.

Para salir, escribimos exit.

**Captura 3.** Estado del servicio y entrada en MariaDB.

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%203%20Estado%20MariaDB.png)

---

# Apartado 4. Comandos principales

En este apartado aparecen algunos comandos que sirven para controlar MariaDB.

**Consultar el estado del servicio**

    systemctl is-active mariadb

Permite comprobar si MariaDB está funcionando.

**Iniciar MariaDB**

    sudo systemctl start mariadb

Inicia el servicio de MariaDB.

**Detener MariaDB**

    sudo systemctl stop mariadb

Detiene el servicio de MariaDB.

**Reiniciar MariaDB**

    sudo systemctl restart mariadb

Reinicia el servicio.

**Entrar en MariaDB**

    sudo mariadb

Permite entrar en la consola para trabajar con las bases de datos.

---

# Apartado 5. ¿Qué pasa si MariaDB está parado?

## Detener el servicio

Comando utilizado:

    sudo systemctl stop mariadb

Este comando detiene MariaDB para comprobar qué ocurre cuando no funciona.

## Comprobar el estado

Comando utilizado:

    systemctl is-active mariadb

El resultado esperado es inactive, porque el servicio está parado.

## Intentar entrar en MariaDB

Comando utilizado:

    sudo mariadb

Al estar parado el servicio, puede aparecer un error de conexión.

## Volver a arrancar MariaDB

Comandos utilizados:

    sudo systemctl start mariadb
    systemctl is-active mariadb

El primer comando arranca el servicio y el segundo comprueba que vuelve a funcionar. Debe aparecer active.

**Pregunta:** ¿Qué dos comandos usarías para saber si MariaDB está parado?

**Respuesta:** Usaría systemctl is-active mariadb para comprobar el estado y sudo mariadb para intentar entrar en la base de datos.

**Captura 4.** Error y recuperación del servicio.

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%205%205.%20Comprobar%20qu%C3%A9%20pasa%20si%20MariaDB%20est%C3%A1%20parado.png)

---

# Apartado 6. Instalación de PHP

## Instalar PHP y los paquetes necesarios

Comando utilizado:

    sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y

Este comando instala PHP y los paquetes necesarios para trabajar con Apache y MariaDB.

## Comprobar la versión de PHP

Comando utilizado:

    php -v

Muestra la versión de PHP instalada.

## Reiniciar Apache

Comando utilizado:

    sudo systemctl restart apache2

Reinicia Apache para que cargue los cambios.

## Comprobar los módulos de Apache y PHP

Comando utilizado:

    sudo apache2ctl -M | grep -E "mpm|php"

Comprueba los módulos relacionados con Apache y PHP.

## Comprobar los módulos de MySQL

Comando utilizado:

    php -m | grep -i mysql

Comprueba los módulos que permiten a PHP conectarse con MariaDB. Deben aparecer mysqli, mysqlnd y pdo_mysql.

## Crear una página de prueba

Comando utilizado:

    echo '<?php phpinfo();' | sudo tee /var/www/html/info.php

Crea una página que muestra información de PHP.

Para conocer la IP de Ubuntu, utilizamos el siguiente comando:

    hostname -I

Después, abrimos en el navegador la dirección http://IP_DE_TU_SERVIDOR/info.php, sustituyendo el texto por la IP que aparezca en el terminal.

## Borrar el archivo de prueba

Comando utilizado:

    sudo rm /var/www/html/info.php

Borra el archivo de prueba para no dejar información del servidor publicada.

Para comprobar los archivos que quedan en la carpeta web, utilizamos:

    ls /var/www/html

**Pregunta:** ¿Por qué es peligroso dejar info.php y para qué sirve php-mysql?

**Respuesta:** info.php muestra información interna del servidor y podría ser utilizada por otras personas. El paquete php-mysql permite que PHP se conecte a MariaDB.

**Captura 5.** Versión de PHP, módulos y página de prueba.

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

---

# Apartado 7. Comprobación final

## Comprobar los servicios

Comando utilizado:

    systemctl is-active apache2 mariadb

Los dos servicios deben mostrar active. En mi caso, ambos están funcionando correctamente.

## Comprobar PHP

Comando utilizado:

    php -v

Este comando muestra la versión instalada. En mi caso, aparece PHP 8.3.6.

**Captura 6.** Comprobación final de Apache, MariaDB y PHP.

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

---

# Conclusiones

En esta práctica he aprendido a instalar MariaDB y PHP en Ubuntu 24.04 y a comprobar que funcionan junto con Apache.

También he aprendido a controlar los servicios, comprobar sus versiones y solucionar un problema cuando MariaDB está parado.

Al terminar, el servidor está preparado para continuar con la instalación de WordPress.

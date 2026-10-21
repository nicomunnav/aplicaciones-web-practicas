# Práctica guiada MariaDB y PHP

### Instalación y comprobación de MariaDB y PHP en Ubuntu 24.04

<br>

**Módulo:** Aplicaciones Web

**Alumno:** Nicolás Muñoz Navarro

**Curso:** 2 SMX

<br>
---
**Memoria de la actividad práctica**
---

# Índice

1. [Preparación del sistema](#apartado-1)
2. [Instalación de MariaDB](#apartado-2)
3. [Comprobación del funcionamiento](#apartado-3)
4. [Comandos principales de administración](#apartado-4)
5. [Comprobar qué pasa si MariaDB está parado](#apartado-5)
6. [Instalación de PHP](#apartado-6)
7. [Comprobación del funcionamiento de PHP](#apartado-7)
8. [Conclusiones](#apartado-8)

---

## Apartado 1

**Preparamos la instalación y comprobamos el sistema.**

- "sudo apt update"

Este comando actualiza la lista de paquetes disponibles para instalar.

---

## Apartado 2

**Instalamos MariaDB y comprobamos que se ha instalado correctamente.**

- "sudo apt install mariadb-server"
- "mariadb --version"

El primer comando instala MariaDB y el segundo muestra la versión instalada.

### Captura 1. Actualización e instalación

![Captura 1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%201%20Active%20.png)

### Captura 2. Instalación de MariaDB

![Captura 2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%202%20instalaci%C3%B3n%20mariadb.png)

---

## Apartado 3

**Comprobamos el funcionamiento de MariaDB.**

- "sudo systemctl status mariadb"
- "sudo systemctl is-active mariadb"

Con estos comandos podemos comprobar si el servicio está funcionando correctamente.

Si aparece `active`, significa que MariaDB está activo.

![Captura 3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%203%20Estado%20MariaDB.png)

#### ¿Para qué sirve el comando systemctl status?

Sirve para consultar el estado de un servicio y comprobar si está funcionando, detenido o si ha ocurrido algún error.

---

## Apartado 4

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/APARTADO%204.png)



## Apartado 5

**Comprobamos qué pasa si MariaDB está parado.**

Primero detenemos el servicio:

"sudo systemctl stop mariadb"

Después intentamos acceder a MariaDB:

"sudo mariadb"

Si el servicio está parado, no se podrá establecer una conexión con el servidor de bases de datos.

Para volver a utilizarlo, iniciamos el servicio:

"sudo systemctl start mariadb"

### Captura 4. Comprobar qué pasa si MariaDB está parado

![Captura 4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%205%205.%20Comprobar%20qu%C3%A9%20pasa%20si%20MariaDB%20est%C3%A1%20parado.png)

#### ¿Qué ocurre cuando detenemos MariaDB?

El servidor deja de aceptar nuevas conexiones mientras está parado. Para volver a utilizar la base de datos, tenemos que iniciar el servicio otra vez.

---

## Apartado 6

**Instalamos PHP para poder utilizarlo con Apache.**

- "sudo apt install php libapache2-mod-php"

Este comando instala PHP y el módulo que permite a Apache procesar archivos PHP.

Después comprobamos la versión instalada:

- "php -v"

![Captura 5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

#### ¿Para qué sirve PHP?

PHP es un lenguaje de programación que permite crear páginas web dinámicas y procesar información en el servidor.

---

## Apartado 7

**Comprobamos que PHP funciona correctamente.**

Creamos un archivo de prueba llamado `info.php` en el directorio web.

- "echo '<?php phpinfo();' | sudo tee /var/www/html/info.php"

Después abrimos el navegador y accedemos a la siguiente dirección:

`http://localhost/info.php`

Si aparece la página con la información de PHP, significa que está funcionando correctamente con Apache.

![Captura 6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica-3-MariaDB/fotos/Captura%206%20Instalacion%20PHP.png)

**Importante:** cuando terminemos la comprobación, es recomendable borrar el archivo de información de PHP, porque muestra detalles de la configuración del servidor.

- "sudo rm /var/www/html/info.php"

#### ¿Qué diferencia hay entre PHP y MariaDB?

PHP permite ejecutar código en el servidor para crear páginas dinámicas. MariaDB sirve para almacenar y gestionar bases de datos.

---

## Apartado 8. Conclusiones

Con esta práctica he aprendido a instalar MariaDB y PHP en Ubuntu 24.04.

También he aprendido a comprobar el estado de los servicios, iniciarlos y detenerlos, y verificar que PHP funciona desde el navegador.

La práctica me ha servido para conocer mejor las herramientas que se utilizan para crear y gestionar páginas web.

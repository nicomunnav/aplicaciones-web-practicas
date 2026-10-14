
## Apartado 1
- Conectamos nuestro ordenador con el pc.
- ssh ls_nmunoz@192.168.56.22

**Preparamos la instalación y Comprobamos el sistema.**
- sudo apt update
- sudo apt update -y

## Apartado 2

  **Instalamos Apache y revisamos la version instalada.**
- sudo apt install apache2 -y
- apache2 -v

![cap1](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/sudo%20apt%20install%20apache2%20-y.png?raw=true)

  
  ¿Qué paquetes adicionales se han instalado como dependencias? (pista: revisa la salida de apt)
  Apache

## Apartado 3

**Comprobación del funcionamiento.**
- sudo systemctl status apache2
- sudo ss -tulpn | grep apache2
- curl -I http://localhost

**Vamos a activar el firewall**
sudo ufw status
sudo ufw allow 'Apache'


![cap2](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/sudo%20ufw%20status%20y%20sudo%20ufw%20allow%20'Apache'%20PASO%205!.png?raw=true)

¿Qué diferencia hay entre los perfiles Apache, Apache Full y Apache Secure?
Apache: Sirve para hacer pruebas
Apache Secure:Se usa para obligar a todo el trafico sea seguro
Apache Full:Permite hacer uso de conexiones normales y seguras simultaneamente

## Apartado 4
   1. **Iniciamos el servidor** = sudo systemctl start apache2
   2. **Detenemos el servidor** = sudo systemctl stop apache2
   3. **Reinicia (corta conexiones)** = sudo systemctl restart apache2
   4. **Recarga la configuración sin cortar conexiones** = sudo systemctl reload apache2
   5. **Arranque automático al iniciar el sistema = sudo systemctl enable apache2
   6. **Desactiva el arranque automático** = sudo systemctl disable apache2
   7. **Comprueba la sintaxis de la configuración** = apache2ctl configtest
   8. **Muestra los sitios (virtual hosts) cargados** = apache2ctl -S
   9. **Lista los módulos cargados** = apache2ctl -M
  10. **Activa / desactiva módulos** = a2enmod / a2dismod
  11. **Activa / desactiva sitios** =  a2ensite / a2dissite
  12. **Activa / desactiva fragmentos de configuración** = a2enconf / a2disconf

 ¿Cuándo conviene usar reload en lugar de restart?
 Reload:Cuando quieres que el servidor no pare de funcionar usaria esta opcion encambio si quieres que se pause un tiempo usaria restart

##     Apartado 5
      
       1. Iniciamos el servidor = sudo systemctl start apache2
       2. Fichero de configuración principal = /etc/apache2/apache2.conf
       3. Puertos en los que escucha Apache = /etc/apache2/ports.conf
       4. Sitios disponibles (definidos, no necesariamente activos) = /etc/apache2/sites-available/
       5. Sitios activos (enlaces simbólicos a sites-available) = /etc/apache2/sites-enabled/
       6. Módulos disponibles y activos = /etc/apache2/mods-available/ y mods-enabled/
       7. Fragmentos de configuración disponibles y activos = /etc/apache2/conf-available/ y conf-enabled/
       8. Variables de entorno (usuario y grupo de ejecución, etc.) = /etc/apache2/envvars
       9. Directorio raíz por defecto (DocumentRoot) = /var/www/html/
       10. Registro de accesos = /var/log/apache2/access.log
       11. Registro de errores = /var/log/apache2/error.log
   
 ##    Apartado 6
       
**6.1 Se crea una página web básica personalizada con tu nombre.**
       echo "<h1>servidor de tu nombre</h1>" | sudo tee /var/www/html/index.html
       
**6.2 Se cambia el puerto por defecto de Apache (del 80 al 8080) modificando la configuración y comprobando que funciona.**
       sudo nano /etc/apache2/ports.conf
       sudo nano /etc/apache2/sites-available/000-default.conf
       sudo apache2ctl configtest
       sudo systemctl reload apache2
       curl -I http://localhost:8080
 ![cap3](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/6.2%20.2.png?raw=true)
       
**6.3 Se configura un nombre de servidor (ServerName) para evitar avisos y se aplica la configuración.**
       echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf
       sudo a2enconf servername
       sudo systemctl reload apache2
![cap4](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/6.3.png?raw=true)
       
**6.4 Definir nombre servidor.**
![Cap5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/6.4.png?raw=true)

**6.5 Cambiar correo Administrador.**
![cap5](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/6.5.png?raw=true)

## Apartado 7
apache2ctl -M
ls /etc/apache2/mods-available/
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod ssl
sudo systemctl restart apache2
apache2ctl -M | grep -E "rewrite|headers|ssl"

**¿Qué ocurre si desactivas un módulo del que depende otro?** 
Si desactivas un módulo del que depende otro, Apache detectará el problema y no arrancará o fallará al intentar reiniciar el servicio, mostrando un error en los registros.

## Apartado 8
**8.1. Crear los directorios y páginas**
sudo mkdir -p /var/www/sitio1 /var/www/sitio2
echo "<h1>Bienvenido a SITIO 1</h1>" | sudo tee /var/www/sitio1/index.html
echo "<h1>Bienvenido a SITIO 2</h1>" | sudo tee /var/www/sitio2/index.html
![cap6](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/8.1.png?raw=true)

**8.2. Crear los ficheros de configuración**
![cap7](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/8.2.png?raw=true)

**8.3. Activar los sitios y desactivar el de por defecto**
sudo a2ensite sitio1.conf sitio2.conf
sudo a2dissite 000-default.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
apache2ctl -S
![cap8](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/8.3.png?raw=true)
**8.4. Resolución de nombres.**
sudo nano /etc/apache2/sites-available/sitio2.conf

**8.5. Pruebas**
curl http://sitio1.local
curl http://sitio2.local
![cap9](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/8.5.png?raw=true)

¿Qué diferencia hay entre virtual hosts basados en nombre y basados en IP? ¿Cuál has usado?

## Apartado 9.
ps aux | grep apache2
grep -E "APACHE_RUN_(USER|GROUP)" /etc/apache2/envvars
ls -ld /var/www/sitio1 /var/www/sitio1/index.html

sudo chown -R www-data:www-data /var/www/sitio1
sudo find /var/www/sitio1 -type d -exec chmod 755 {} \;
sudo find /var/www/sitio1 -type f -exec chmod 644 {} \;

**Prueba qué ocurre al poner chmod 000 a index.html (error 403) y consulta el error.log. Después restaura los permisos.**
sudo chmod 000 /var/www/sitio1/index.html
curl http://sitio1.local
sudo tail -n 20 /var/log/apache2/sitio1_error.log

![cap10](https://github.com/nicomunnav/aplicaciones-web-practicas/blob/main/Practica%20guiada%20Apache%20Nico/fotos/Apartado%209.png?raw=true)

# `Practica Apache Nico`

1.Paso: Conectamos nuestro ordenador con el pc.
- ssh ls_nmunoz@192.168.56.22

2.Paso:Preparamos la instalación y Comprobamos el sistema.
- sudo apt update
- sudo apt update -y


3.Paso:Instalamos Apache y revisamos la version instalada.
- sudo apt install apache2 -y
- apache2 -v
  ¿Qué paquetes adicionales se han instalado como dependencias? (pista: revisa la salida de apt)
Apache

4.Paso:Comprobación del funcionamiento.
- sudo systemctl status apache2
- sudo ss -tulpn | grep apache2
- curl -I http://localhost

5.Paso:Vamos a activar el firewall
sudo ufw status
sudo ufw allow 'Apache'

¿Qué diferencia hay entre los perfiles Apache, Apache Full y Apache Secure?
Apache: Sirve para hacer pruebas
Apache Secure:Se usa para obligar a todo el trafico sea seguro
Apache Full:Permite hacer uso de conexiones normales y seguras simultaneamente

6. Paso: Apartado 4
   1. Iniciamos el servidor = sudo systemctl start apache2
   2. Detenemos el servidor = sudo systemctl stop apache2
   3. Reinicia (corta conexiones) = sudo systemctl restart apache2
   4. Recarga la configuración sin cortar conexiones = sudo systemctl reload apache2
   5. Arranque automático al iniciar el sistema = sudo systemctl enable apache2
   6. Desactiva el arranque automático = sudo systemctl disable apache2
   7. Comprueba la sintaxis de la configuración = apache2ctl configtest
   8. Muestra los sitios (virtual hosts) cargados = apache2ctl -S
   9. Lista los módulos cargados = apache2ctl -M
   10. Activa / desactiva módulos = a2enmod / a2dismod
   11. Activa / desactiva sitios =  a2ensite / a2dissite
   12. Activa / desactiva fragmentos de configuración = a2enconf / a2disconf

 ¿Cuándo conviene usar reload en lugar de restart?
 Reload:Cuando quieres que el servidor no pare de funcionar usaria esta opcion encambio si quieres que se pause un tiempo usaria restart

 7. Paso: Apartado 5
    1.Fichero de configuración principal = /etc/apache2/apache2.conf
    2.Puertos en los que escucha Apache = /etc/apache2/ports.conf
    3.Sitios disponibles (definidos, no necesariamente activos) = /etc/apache2/sites-available/
    4.Sitios activos (enlaces simbólicos a sites-available) = /etc/apache2/sites-enabled/
    5.Módulos disponibles y activos = /etc/apache2/mods-available/ y mods-enabled/
    6.Fragmentos de configuración disponibles y activos = /etc/apache2/conf-available/ y conf-        enabled/
    7.Variables de entorno (usuario y grupo de ejecución, etc.) = /etc/apache2/envvars
    8.Directorio raíz por defecto (DocumentRoot) = /var/www/html/
    9.Registro de accesos = /var/log/apache2/access.log
    10.Registro de errores = /var/log/apache2/error.log
    

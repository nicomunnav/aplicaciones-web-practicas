# `Practica Apache Nico`

1.Paso: Conectamos nuestro ordenador con el pc.
- ssh ls_nmunoz@192.168.22.10

2.Paso:Preparamos la instalación y Comprobamos el sistema.
- sudo apt update
- sudo apt update -y


3.Paso:Instalamos Apache y revisamos la version instalada.
- sudo apt install apache2 -y
- apache2 -v

4.Paso:Comprobación del funcionamiento.
- sudo systemctl status apache2
- sudo ss -tulpn | grep apache2

Playbook que instala docker y levanta un contenedor con MySQL
Primero preparamos un contenedor que nos permita correr docker (activamos nesting), para ellos utilizamos los siguientes comandos,
 - lxc profile create docker
 - lxc profile set docker security.nesting "true"
 - lxc profile set docker linux.kernel_modules "overlay, nf_nat, br_netfilter"
 Arrancamos un contenedor con el perfil docker
 - lxc launch Ubuntu2004SSH prueba-docker -p default -p docker
 - vi /etc/hosts -> 10.218.144.230 docker

Al ejecutar el playbook main.yaml
1.- Se ejecuta el playbook servicio_docker.yaml que realiza las siguientes tareas del inventario dockers,
    - Elimina los paquetes de Docker
    - Instala el paquete aptitude
    - Instala los paquetes del sistema necesario (por rendimiento eliminamos el metodo loop y el update_cache)
    - Añadimos la gpg key al repositorio
    - Añadimos el repositorio de Docker
    - Instalamos los paquetes de Docker (mediante un bucle)
    - Levantamos el servicio de Docker y Containerd
    - Instalamos Docker-compose
    - Instalamos el modulo de Docker en Python (mediante pip)
    - Verificamos si existe el grupo docker y sino lo crea
    - Verificamos que el usuario alumno pertenece al grupo docker sino lo añade.
2.- Se ejecuta el playbook docker-mysql.yaml que realiza las siguientes tareas,
    - Se descarga la ultima imagen de MySQL del repositorio de DockerHub
    - Crea un volumen de docker llamado mysql-data
    - Crea un contenedor con la imagen descargada de mysql, se mapea el volumen creado anteriormente al la ruta /var/lib/mysql con permisos de rw y habilita el puerto 3306
    - Le pasamos como variable de entorno la password de root.
    - Desruimos el contenedor
    - Destruimos el volumen


PENDIENTE - Lo intento terminar esta tarde ya que solo me queda pasar los ficheros al git

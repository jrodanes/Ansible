Playbook que instala docker y levanta un contenedor con MySQL
Primero preparamos un contenedor que nos permita correr docker (activamos nesting), para ellos utilizamos los siguientes comandos,
 - lxc profile create docker
 - lxc profile set docker security.nesting "true"
 - lxc profile set docker linux.kernel_modules "overlay, nf_nat, br_netfilter"
 Arrancamos un contenedor con el perfil docker
 - lxc launch Ubuntu2004SSH prueba-docker -p default -p docker
 - vi /etc/hosts -> 10.218.144.230 docker

PENDIENTE - Lo intento terminar esta tarde ya que solo me queda pasar los ficheros al git

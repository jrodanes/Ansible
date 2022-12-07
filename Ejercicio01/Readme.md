Creo nuevo contenedor RockyLinux
 - lxc launch images:rockylinux/8 rocky8
 - lxc exec rocky8 -- /bin/bash
	- dnf install openssh-server
	- useradd alumno
	- passwd alumno
	- vi /etc/sudoers -> alumno ALL=(ALL) ALL
	- vi /etc/sysconfing/network-scripts/ifcg-eth0 -> BOOTPRO=static IPADDR=10.218.144.162 PREFIX=24 GATEWAY=10.218.144.162 DNS1=8.8.8.8 DNS2=1.1.1.1
	- ifdown eth0;ifup eth0
 - ssh-copy-id -i .ssh/id_rsa.pub alumno@10.218.144.162
 - ssh alumno@10.218.144.162
 - lxc exec rocky8 -- /bin/bash
	- vi /etc/ssh/sshd_config -> PasswordAuthentication no PermitRootLogin no
	- systemctl restart sshd
	 
Creo la plantilla nueva para RockyLinuxSSH
 - lxc stop rocky8
 - lxc publish rocky8 --alias=RockyLinuxSSH description="RockyLinux SSH key"
	
Creo un Contenedor nuevo en base a la imagen RockyLinuxSSH con el nombre r1
 - lxc launch RockyLinuxSSH r1
Cambio la dirección ip del contenedor para que sea fija
 - lxc exec r1 -- /bin/bash
	- vi /etc/sysconfing/network-scripts/ifcg-eth0 -> BOOTPRO=static IPADDR=10.218.144.87 PREFIX=24 GATEWAY=10.218.144.162 DNS1=8.8.8.8 DNS2=1.1.1.1
	- ifdown eth0;ifup eth0
	
Meto la ip en el /etc/hosts con el nombre web1
	- sudo vi /etc/hosts -> 10.218.144.87 r1

Genero un playbook que realiza las siguientes tareas sobre el inventario [webservers]
- Declaro como variable mysql_user, mysql_user_password, mysql_user_privileges
- Creo un handler llamado Flush Privileges que reinicia lanza el comando mysql FLUSH PRIVILEGES
- Actualizo todos los paquetes mediante dnf
- Instalo los paquetes de Apache, MariaDB y Python3-PyMySQL (utilizado por ansible para gestionar MariaDB)
- Arranco el apache sino no lo esta con systemd 
- Arranco la MariaDB sino no lo esta con systemd  (con service me dio fallo asi que utilice este para los 2 servicios)
- Instalo Firewalld con packege
- Arranco Firewalld con service
- Abro los servicio (puertos) HTTP(80), HTTPS(443) y MySQL(3306) en la zona publica mediante un bucle 
- Reinicio el Firewalld para habilitar los cambios
- Copio el fichero index.html al directorio /var/www/html/index.html con usuario, grupo apache y permisos 644
- Creo el usuario alumno en MySQL y doy todos los permisos en todas las tablas (usuario administrador), llamo al evento Flush Privileges para recargar los permisos del nuevo usuario.

HECHO.
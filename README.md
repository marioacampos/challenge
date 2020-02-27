# challenge
Ansible_Lab


1.- Objetivo
Se desarrolla solución de Challenge a través de Ansible que nos permitirá automatizar en base a la automatización de software, la gestión de configuraciones
y además el despliegue de aplicaciones. Solución aplicada en base a infraestructura LAN para 50 Equipos SW 9300. Se levanta Laboratorio en modo Controlador-Nodo,
en donde mi localhost es el controlador y los SW pertenecen a los nodos. Recordar además que se útiliza lenguaje YAML a través de playbooks para el desarrollo de este.

2.- Requerimientos.

a)El desarrollo del script se realiza a través de YAML en donde se creamos un Inventory en donde declaramos todos nuestros dispositivos Catalyst 9300. 

	Se declararón las siguientes variables:

		- host IP Managment de cada SW.
		- username Usuario con privilegios para ejecutar comandos
		- password Contraseña del usuario proporcionado en la variable anterior
		- backup_folder Ruta que se creará y donde se almacenaran los logs con la información que devuelvan los comandos
		- cisco_os SO que tienen nuestros SW, utilizaremos esta variable como condicional para ejecutar comandos ios.

Se crea el archivo hosts en donde declaramos todos nuestros dispositivos.

b)Los repositorios los vamos a disponibilizar a través de la siguiente ubicación /etc/ansible/00-backups-catalyst/ en donde se van a crear carpetas por cada SW, de modo de concatenar un orden en caso de restauración de equipamiento. Esto lo vamos a realizar a través de nuestro playbook ML-Challenge.yml en donde definimos que se cree unacarpeta en el root directory con el nombre de cada SW.

c)Para brindar un orden a través en base a la diferenciación entre SW, vamos a definir en nuestro playbook ML-Challenge.yml una variable que cree una carpeta con el nombre de cada SW.

d)Para realizar 7 backups vamos a generar un crontab en donde definiremos lo siguiente.

	- 1 Mensual, primer día del mes = 0 0 1 * * /usr/bin/ansible-playbook /etc/ansible/playbooks/ML-Challenge.yml -i /etc/ansible/inventory/hosts -e "sw=SW-ML01" 
	- 1 Semanal, realizado los domingos = 45 10 * * /usr/bin/ansible-playbook /etc/ansible/playbooks/ML-Challenge.yml -i /etc/ansible/inventory/hosts -e "sw=SW-ML-01"
	- 5 Diarios = 30 2 * * 1-5 /usr/bin/ansible-playbook /etc/ansible/playbooks/ML-Challenge.yml -i /etc/ansible/inventory/hosts -e "sw=SW-ML-01"

*Recordar que si queremos ejecutar la misma tarea programada tendríamos que replicar el mismo crontab para nuestros 50 dispositivos.

3.- Ejecución

a) Para ejecutar nuestro playbook es necesario ejecutarlo de la siguiente manera. 

'ansible-playbook /etc/ansible/playbooks/cisco_backup.yml -i /etc/ansible/inventory/cisco -e "sw=Nombre de SW"'

 


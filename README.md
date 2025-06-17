Trabajo Práctico  – Computacion Aplicada (Debian 11)

Alumno: Juan Pedro Ramognino  
Materia: Computacion Aplicada
Docente: Guillermo Maquieira  
Universidad de Palermo – 2025

---

Índice

1. [Configuración del entorno]
2. [Servicios]
3. [Configuración de red]
4. [Almacenamiento]
5. [Backup y automatización]
6. [Diagrama topológico] Realizado con IA 
7. [Capturas] tuve un problema con esto ya que me olvide de ir realizando las capturas a medida que hacia el trabajo, recien cuando lo termine vi en mis notas este apartado, mil disculpas
8. De todas formas, espero poder ser claro y demostrar el uso asi no darte tanto trabajo en mi corrección, 
9. [Video explicativo (terminar)] 

Quiero pedir disculpas por no haber podido entregar los opcionales de las semanas anteriores ni haber participado de forma más activa en la cursada. 
No fue por falta de compromiso con la materia, sino por algunos problemas laborales que me complicaron bastante el ritmo de estudio y la posibilidad de conectarme o avanzar como me hubiese gustado.
De todas formas, me propuse hacer este trabajo de la mejor manera posible, cumpliendo con todos los puntos y aprendiendo realmente en el proceso. 
Trabajé solo, sin grupo, y aunque me costó en varias partes, pude resolverlo,aprender y dejar todo funcionando correctamente.
Espero que el trabajo esté a la altura de lo esperado y que sirva como muestra del esfuerzo y las ganas que le puse. Muchas gracias profe.

---
1. Configuración del entorno

Primero descargué la máquina virtual Debian 11 desde Blackboard. Venía dividida en varias partes comprimidas con WinRAR, así que las ensamblé y la importé a VirtualBox sin problemas.
Cuando la arranqué, me encontré con que la contraseña del usuario root no estaba disponible. Para poder entrar, edite las opciones de arranque desde GRUB (con la tecla `e`), 
agregue `init=/bin/bash` al final de la línea y arranqué el sistema en modo consola. 
Desde ahi empece a montar el sistema con permisos de escritura usando `mount -o remount,rw /` etc 
cambie la contraseña con `passwd root`. 
Despues reboot y ya pude entrar normalmente. 
Le puse como clave: `palermo`, no recuerdo si habia que poner alguna especifica.

Cuando entre, cambié el hostname de la máquina a `TPServer` con el comando `hostnamectl set-hostname TPServer`, y también edite el archivo `/etc/hosts` para que tenga esta línea: `127.0.1.1    TPServer`.
Todo quedo funcionando bien despues de eso. Espero con la captura siguiente poder cubrir el punto 1.

![image](https://github.com/user-attachments/assets/24b630c3-eaaa-40a6-a8e8-b6affba68308)

2. Servicios

Para empezar con el punto 2, instal2 el servicio SSH conapt update y luego apt install openssh-server. 
Después edite el archivo /etc/ssh/sshd_config y mire de que estuvieran habilitadas las líneas necesarias: PermitRootLogin prohibit-password y PubkeyAuthentication yes.
Copié la clave pública que me proporcionaron (La ubique en una carpeta llamada claves en el deskktop) dentro del archivo ~/.ssh/authorized_keys del usuario root. 
Tambien configure los permisos: 700 para el directorio .ssh y 600 para el archivo. 
Despues reinicie el servicio SSH con systemctl restart ssh. Desde otra computadora de la red probe el acceso al servidor con la clave privada y pude ingresar tranquilamente.

Con el servidor web, instale Apache y PHP ejecutando apt install apache2 php. 
Despues copie los archivos index.php y logo.png en el directorio /var/www/html. Despues entre al sitio ingresando la IP del servidor (http://192.168.0.253/index.php) y vi que el archivo se mostraba perfecto junto con la imagen. 
(este punto fue uno de los que mas me costo ya que no podia lograr que se visualice bien el logo con el index php, no recuerdo bien el porque, pero fue de lo que mas me costo y al no tener compañeros conocidos o un grupo tuve que recurrir mucho a internet)

Para terminar y sin estirar mucho mas, instale MariaDB con apt install mariadb-server. inicie y habilite el servicio con systemctl start mariadb y systemctl enable mariadb. Para cargar la base de datos, 
utilice el archivo db.sql que estaba tambien en la carpeta claves (esta carpeta es mia, ahi puse todo el material adicional del trabajo) ejecutando mysql -u root < /ruta/db.sql. La base se cargo bien y lo verifique conectandome con mysql -u root.
Asi, dejé configurados los tres servicios del trabajo: SSH (con acceso mediante clave), Apache con PHP y la base de datos MariaDB. 

A continuacion dejo algunas capturas del punto 2 

![image](https://github.com/user-attachments/assets/dc250eab-e258-4f04-9b07-b985b2b45da9) Verificacion de SSH activo y configuracion
![image](https://github.com/user-attachments/assets/da3b191f-f3db-4779-a08f-0ad4a5a6443c) Muestra que PermitRootLogin esta habilitado
![image](https://github.com/user-attachments/assets/390313cb-5b8a-49c3-a916-2e47cc346051) Acceso por clave desde 
![image](https://github.com/user-attachments/assets/97cd4ebf-3542-4e81-a14f-ed0844234350) Sitio web funcionando
![image](https://github.com/user-attachments/assets/f3c83389-5667-49d8-a586-764c2802d80f) Verificacion del servicio MariaDB
![image](https://github.com/user-attachments/assets/0376c076-f400-4295-b46d-f38de872a933) MariaDB esta activo y la base db.sql importada.
![image](https://github.com/user-attachments/assets/e5762fe4-48db-467f-bf3f-b1382c37bc00) se instalo Apache y PHP 


Punto 3 – Instalacion y configuracion del servidor web Apache con soporte PHP

Este punto ya fue cubierto en parte dentro del punto anterior, 
pero lo retomo para dejar claro el proceso. Instale Apache y PHP, y me asegure de que estuvieran funcionando correctamente. Lo verifique accediendo al archivo index.php desde el navegador, ubicado en el directorio /www_dir. 
Al entrar a http://192.168.0.253/index.php, pude ver la informacion de PHP cargada.
Esta prueba sirvio para confirmar que el servidor Apache estaba corriendo y que el soporte PHP estaba activo. 
Dejo de nuevo algunas capturas como evidencia.

![image](https://github.com/user-attachments/assets/b7ddda66-9a79-4a47-ab11-bd5cbf0b5c2b) Estado del servicio Apache
![image](https://github.com/user-attachments/assets/21d1eeb8-0ff0-478d-8baa-dd67d75232fa) Muestr que cree el archivo en /www_dir
 

Punto 4 – Instalacion y configuracion del servicio de base de datos MariaDB

En esta parte instale y configure MariaDB como sist gestor de base de datos. 
Use el comando apt install mariadb-server para instalar el servicio, y luego lo inicie y habilite con systemctl start mariadb y systemctl enable mariadb para que arranque auto con el sistema.
Despues de eso, importe db.sql, que ya tenia en la carpeta "claves" del escritorio. 
Para cargarla, use el comando mysql -u root < /ruta/db.sql. me conecte al motor de base de datos usando mysql -u root y verifique que los datos estuvieran correctamente cargados.
No tuve problemas con esta parte. El servicio quedo activo, y MariaDB funciono.

![image](https://github.com/user-attachments/assets/d647abdf-0cfa-4adb-a9b6-0a2f03087dfb) 
![image](https://github.com/user-attachments/assets/1ea4f423-9f97-4920-9301-15e4c9bc5654)
Estado del servicio MariaDB activo y habilitado
![image](https://github.com/user-attachments/assets/7e8d6479-89c8-46b1-8b85-dbb07f706c3a) Verificación de que se cargo la base

Punto 5 – Automatizacion con cron

Para este punto, configure dos tareas automaticas usando cron con el comando crontab -e.
La primera tarea hace un backup completo del directorio /var/log todos los dias a las 00:00 y lo guarda en /backup_dir con el nombre backup_log.tar.gz.
La segunda tarea hace un backup del directorio /www_dir los lunes, miercoles y viernes a las 23:00 y lo guarda como backup_www.tar.gz, tambien en /backup_dir.
Lo que hice fue crear un script en /opt/scripts/backup_full.sh que compme las carpetas y lo deje ejecutable. Despues edite el crontab del usuario root agregando las dos lineas para automatizar todo.
Probe ambas tareas ejecutando el script manualmente. Funciono bien y los archivos se generanon correctamente en el destino.

![image](https://github.com/user-attachments/assets/9dcfcae2-9b5f-4a0d-adea-5735dc1485dd) contenido del script backup_full.sh
![image](https://github.com/user-attachments/assets/76f978bd-1aea-4e6a-bdb2-3f7bc3b7a1b0) tareas programadas.
![image](https://github.com/user-attachments/assets/52bed833-07e8-4896-8814-4c70b7130b5a) backups generados


Punto 6 – Estructura de archivos y subida a GitHub
Para este ultimo punto, organice todos los archivos del sistema siguiendo las consignas. 
Cree una carpeta principal llamada entregaTrabajo y ahi adentro fui copiando las carpetas /etc, /var, /opt, /home, /mnt. 
Las carpetas se copiaron desde el sistema Debian de la maquina virtual, usando los comandos de cp.
Ademas de los archivos del sistema, agregue una carpeta llamada claves donde puse el material adicional: las claves clave_privada.txt y clave_publica.pub, la base db.sql, el archivo index.php y la imagen logo.png.
Una vez que tuve todo organizado, cree un repositorio en GitHub y subi todo el contenido. Para eso use Git desde la misma VM. Inicialice el repo, hice los commits y lo subi con el comando git push.
Tuve que hacer algunos ajustes para que todo se vea bien y evitar errores de permisos, pero al final quedo todo subido al repo, incluyendo este archivo README y todas las capturas necesarias.


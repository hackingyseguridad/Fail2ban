# Fail2ban

Fail2ban bloquea IP ante antaques DDoS o fuerza bruta; Herramienta Linux para mitigar ataques;
Disponible para Linux: Debian, CentOS, RedHat, .. util para configurar bloqueos temorales o incluir IP en lista negra o blanca;
https://github.com/fail2ban/fail2ban
https://www.fail2ban.org/wiki/index.php/Main_Page
https://www.fail2ban.org/

#hackingyseguridad #cybersecurity #hacking #DDoS #Fuerza #Bruta 

sudo apt update
sudo apt install fail2ban
sudo systemctl status fail2ban

De forma predeterminada, cuando instala Fail2ban, viene con dos archivos de configuración predeterminados que son /etc/fail2ban/jail.conf y  /etc/fail2ban/jail.d/defaults-debian.conf.

/etc/fail2ban/jail.conf
/etc/fail2ban/jail.d/ .conf
/etc/fail2ban/jail.local
/etc/fail2ban/jail.d/ .local

sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

[DEFAULT]
# "ignoreip" can be a list of IP addresses, CIDR masks or DNS hosts. Fail2ban
# will not ban a host that matches an address in this list. Several addresses
# can be defined using space (and/or comma) separator.
ignoreip = 127.0.0.1/8
# "bantime" is the number of seconds that a host is banned.
bantime = 600
# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime = 10m
# "maxretry" is the number of failures before a host gets banned.
maxretry = 5
# "backend" specifies the backend used to get files modification.
# systemd: uses systemd python library to access the systemd journal.
# Specifying "logpath" is not valid for this backend.
# See "journalmatch" in the jails associated filter config
backend=systemd


Findtime : Findtime es la duración entre el número de fallas antes de que se establezca una prohibición.

# A host is banned if it has generated "maxretry" during the last "findtime"
findtime = 10m

Maxretry :   maxretry es la cantidad de fallas antes de que un host sea baneado. El valor predeterminado para  maxretry es  5. Para cambiar su valor, cambie la línea como se muestra a continuación:

# "maxretry" is the number of failures before a host gets banned.
maxretry = 5

Para habilitar la cárcel proftpd, siga las instrucciones a continuación:

[proftpd]
enabled  = true
port     = ftp,ftp-data,ftps,ftps-data
logpath  = %(proftpd_log)s
backend  = %(proftpd_backend)s
La configuración SSH con las configuraciones mencionadas anteriormente, se puede establecer por cárcel como se muestra a continuación:

# SSH servers
[sshd]
enable  = true
bantime = 1d
findtime = 10min
maxretry = 5
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
Cuando haya terminado de editar un archivo de configuración, debe reiniciar el servicio Fail2ban ejecutando el siguiente comando:

sudo systemctl restart fail2ban

fail2ban-client -h
Verifique el estado de la cárcel de Fail2ban :

sudo fail2ban-client status sshd
Desbancar una dirección IP en particular :

sudo fail2ban-client set sshd unbanip 198.178.11.1
Prohibir una dirección IP en particular :

sudo fail2ban-client set sshd banip 198.178.11.1

Registro fail2ban.log
Todas las acciones y medidas realizadas por Fail2ban se registran en el archivo /var/log/fail2ban.log.
sudo less /var/log/fail2ban.log

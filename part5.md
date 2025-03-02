## Installation et vérification du service
```bash
sudo dnf update && sudo dnf install vsftpd -y
```

### Vérification du statut du service
```bash
sudo systemctl status vsftpd

● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled; preset: disabled)
   Active: active (running) since Sat 2025-03-02 22:27:10 CET; 2h 32min ago
 Main PID: 8763 (vsftpd)
   Tasks: 1 (limit: 19823)
   Memory: 2.4M
   CPU: 15ms
   CGroup: /system.slice/vsftpd.service
           └─8763 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf

Mar 02 22:27:10 vbox systemd[1]: Starting Vsftpd ftp daemon...
Mar 02 22:27:10 vbox systemd[1]: Started Vsftpd ftp daemon.
```

## Configuration de vsftpd
```bash
sudo cat /etc/vsftpd.conf
listen=YES
listen_ipv6=NO
ssl_enable=YES
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem
force_local_logins_ssl=YES
force_local_data_ssl=YES
ssl_tlsv1=NO
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
chroot_local_user=YES
allow_writeable_chroot=YES
local_enable=YES
write_enable=YES
user_sub_token=$USER
local_root=/home/$USER/ftp
idle_session_timeout=600
data_connection_timeout=120
file_open_mode=0777
local_umask=022
userlist_enable=YES
userlist_file=/etc/vsftpd/user_list
userlist_deny=NO
```

## Génération et sécurisation du certificat
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/vsftpd.pem \
    -out /etc/ssl/private/vsftpd.pem
```

### Application des permissions les plus restrictives
```bash
sudo chmod 600 /etc/ssl/private/vsftpd.pem
```

### Vérification des permissions
```bash
ls -l /etc/ssl/private/vsftpd.pem
```

## Configuration du pare-feu
```bash
sudo firewall-cmd --add-service=ftp --permanent
sudo firewall-cmd --reload
```

### Vérification des ports ouverts
```bash
sudo firewall-cmd --list-ports
21/tcp 990/tcp 2222/tcp
```


## Test de connexion FTP
```bash
ftp localhost
Trying ::1...
ftp: connect to address ::1Connection refused
Trying 127.0.0.1...
Connected to localhost (127.0.0.1).
220 (vsFTPd 3.0.5)
Name (localhost:root):
```
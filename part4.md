# Gestion des utilisateurs et groupes

## Création des utilisateurs avec mot de passe fort
```bash
openssl rand -base64 12
```

## Vérification des utilisateurs et groupes
```bash
cat /etc/passwd | grep -E "suha|daniel|liam|noah|alysha|rose|sadia|jakub|lev|grace|lucia|oliver|nginx"
cat /etc/group | grep -E "managers|admins|sysadmins|artists|devs|rh"
```

# Gestion des permissions

## Vérification des permissions
```bash
ls -alR /data/
getfacl -R /data/
lsattr -R /data/
```

## Configuration des permissions
```bash
chmod 755 /data/
setfacl -m g:managers:rwx /data/projects/
chattr +i /data/projects/README.docx
setfacl -d -m g:artists:rwx /data/projects/the_zoo/
setfacl -m g:artists:rwx /data/projects/the_zoo/
chmod u+s /data/projects/zoo_app/zoo_app
```

# Gestion de sudo

## Configuration de /etc/sudoers
```bash
%sysadmins ALL=(root) ALL
%artists ALL=(sadia) NOPASSWD: /bin/ls /data/, /bin/cat /data/, /bin/vi /data/, /usr/bin/file /data/
alysha ALL=(suha) NOPASSWD: /bin/cat /data/projects/the_zoo/ideas.docx
%devs ALL=(root) NOPASSWD: /usr/bin/dnf install
jakub ALL=(liam) NOPASSWD: /usr/bin/python
%admins ALL=(daniel) NOPASSWD: /usr/bin/free, /usr/bin/top, /bin/df, /usr/bin/du, /bin/ps, /bin/ip
lev ALL=(daniel) NOPASSWD: /usr/bin/openssl *, /usr/bin/dig, /bin/ping, /usr/bin/curl
```

## Vérification des failles
```bash
sudo -u suha /bin/bash  # Accès root obtenu
```

## Amélioration de la configuration sudo
```bash
Defaults !authenticate
Defaults:%sysadmins !authenticate
Defaults:%admins !authenticate

%sysadmins ALL=(root) ALL
%artists ALL=(sadia) NOPASSWD: /bin/ls /data/, /bin/cat /data/, /bin/vi /data/, /usr/bin/file /data/
alysha ALL=(suha) NOPASSWD: /bin/cat /data/projects/the_zoo/ideas.docx
%devs ALL=(root) NOPASSWD: /usr/bin/dnf install
jakub ALL=(liam) NOPASSWD: /usr/bin/python
%admins ALL=(daniel) NOPASSWD: /usr/bin/free, /usr/bin/top, /bin/df, /usr/bin/du, /bin/ps, /bin/ip
lev ALL=(daniel) NOPASSWD: /usr/bin/openssl *, /usr/bin/dig, /bin/ping, /usr/bin/curl

# Interdiction des shells pour tous sauf sysadmins
Defaults:%artists !syslog
Defaults:%devs !syslog
Defaults:%admins !syslog
Defaults:%rh !syslog

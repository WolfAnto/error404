#### Cloud init

ouvrir l'iso
contient : user-data et meta-data

#### Partitionnement

Déplacé /home dans nouvelle partition :
- renommé home
```bash
mv /home /home.old
```
- créer nouveau répertoire home
- créer une nouvelle partition de 1 Go
```bash
fdisk /dev/sdb   
>m       // help
>n       // ajouter partition
>p       // primaire
>+1G     // de 1G
>w       // sauvegarder
```
- la formater au bon file system
```bash
mkfs.ext4 /dev/sda3
```
- la monter dans le nouveau répertoire home
```bash
mount /dev/sda3 /home
```
- copier le contenu de home.old dans le nouveau home
```bash
cp -rp /home.old/* /home
```
**Monter la partition au démarrage de la machine pour éviter de refaire les manipulations**
- identifier l'UUID de la partition
```bash
ls -l /dev/disk/by-uuid/
``` 
- Modifier le fstab (monte les partitions)
- faire une copie par précaution
```bash
sudo cp /etc/fstab /etc/fstab.old
```
- le modifier
```bash
nano /etc/fstab
```
- ajouter la ligne à la fin
```bash
UUID=Mon_UUID /home ext4 default 0 2
```
- ***pour la racine ?***

### SSH

Pour faire un user privilégié autre que root:
- le mettre dans le groupe sudo
```bash
usermod -G nom_groupe nom_user
```
Ckeck SSH
```bash
systemctl status ssh.service
```
- Créer une paire de clé rsa 4096
```bash
ssh-keygen -t rsa -b 4096
ls -lh .ssh/     // endroit où sont stockés les clés
mkdir .ssh/authorized_keys   // pour mettre les clés publiques
cat .ssh/id_rsa.pub >> .ssh/authorized_keys   // rangement
chmod 700 .ssh/
chmod 600 .ssh/authorized_keys
chmod 600 .ssh/id_rsa.pub
chmod 600 .ssh/id_rsa
```
Se connecter en ssh avec le mdp et ajouter sa clé publique windows dans authorized_keys.
- dans /etc/ssh/sshd_config
```bash
Port 22
HostKey /etc/ssh/ssh_host_rsa_key
PermitRootLogin no         // désactiver accès direct en root
PubkeyAuthentication yes
AuthorizedKeysFile ....
PasswordAuthentication no  // désactiver authentification par mdp
ChallengResponseAuthentication no
```
- Restart
```bash
systemctl restart sshd
```


### PHP



### MariaDB


... (6 lignes restantes)

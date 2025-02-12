# Exercice

Le moment est venu de faire un petit exercice récapitulatif. Placez-vous dans le répertoire du sixième atelier pratique :
```
$ cd ~/formation-ansible/atelier-06
```
Voici les quatre machines virtuelles Ubuntu 22.04 de cet atelier :
Machine virtuelle 	Adresse IP
control 	192.168.56.10
target01 	192.168.56.20
target02 	192.168.56.30
target03 	192.168.56.40

Démarrez les VM :
```
$ vagrant up
```
Connectez-vous au Control Host :
```
$ vagrant ssh control
```
Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.
```
  $ sudo vim /etc/hosts
```
```
  127.0.0.1      localhost.localdomain  localhost
  192.168.56.10  control.sandbox.lan    control
  192.168.56.20  target01.sandbox.lan   target01
  192.168.56.30  target02.sandbox.lan   target02
  192.168.56.40  target03.sandbox.lan   target03
```
Configurez l’authentification par clé SSH avec les trois Target Hosts.
```
  $ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:j8WDrx8K6rtj1LYZpr3O0ahM+q26s5f70r+GFfVehcM vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|             . . |
|          .   E .|
|         . .   o |
|        .o  . .  |
|     .  S.+. .   |
|    . =o.= ..    |
|   ..B+*o +      |
|  .+B=*o.o .     |
|  =X@X*+=o.      |
+----[SHA256]-----+
```
```
  $ ssh-copy-id vagrant@target01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.
```
```
  $ ssh-copy-id vagrant@target02
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.
```
```
  $ ssh-copy-id vagrant@target03
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added.
```
Installez Ansible.
```
 $ sudo apt update
```
  $ sudo apt install -y ansible
```
Envoyez un premier ping Ansible sans configuration.
```
  $ ansible all -i target01,target02,target03 -m ping
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Créez un répertoire de projet ~/monprojet.
```
  $ mkdir ~/monprojet
```
Créez un fichier vide ansible.cfg dans ce répertoire.
```
  $ cd monprojet/
```
```
  $ touch ansible.cfg
```
Vérifiez si ce fichier est bien pris en compte par Ansible.
```
  $ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```
Spécifiez un inventaire nommé hosts.
```
  $ touch hosts
```
Activez la journalisation dans ~/journal/ansible.log.
```
  $ mkdir ~/journal
```
```
  $ touch ~/journal/ansible.log
```
Voici le fichier de config :
``` 
[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```
Testez la journalisation.
```
  $ cat ~/journal/ansible.log 
2025-02-12 08:00:38,664 p=3505 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 08:00:38,690 p=3505 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 08:00:38,722 p=3505 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Créez un groupe [testlab] avec vos trois Target Hosts.
```
[testlab]
target01
target02
target03
```
Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
```
[testlab:vars]
ansible_user=vagrant
```
Envoyez un ping Ansible vers le groupe de machines [all].
```
  $ ansible all -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.
```
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_become=yes
```
Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
```
  $ ansible all -a "head -n 1 /etc/shadow"
target01 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target02 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
```

Quittez le Control Host et supprimez toutes les VM de l’atelier.
```
  $ exit
```
```
  $ vagrant destroy -f
```

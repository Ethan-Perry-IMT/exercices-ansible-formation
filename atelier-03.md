# Exercice

Placez-vous dans le répertoire du troisième atelier pratique :  
```
  $ cd ~/formation-ansible/atelier-03  
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
Ansible est déjà installé sur cette machine :  
```
$ type ansible
ansible is /usr/bin/ansible
```
Faites le nécessaire pour réussir un ping Ansible comme ceci :  
```
$ ansible all -i target01,target02,target03 -m ping
target03 | SUCCESS => {
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
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Voici la liste des commandes que j'ai utilisé :
```
  vagrant up
```
```
  vagrant ssh control
```
```
  type ansible
ansible is /usr/bin/ansible
```
```
  sudo vim /etc/hosts
```
```
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  ansible.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```
```
  for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```
```
  ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```
Test de la connexion ssh :
```
  ssh target01
  ssh target02
  ssh target03
```
```
  ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:+TXrMPVV9UuA04ka/I66G4b90zl3RKpMinREE7oo26g vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|        ... +.. .|
|        .= + o. o|
|       .. = .  .o|
|      . .+ .  ..o|
|   . . .S o + o..|
|    = o. + = = o |
|   o o.++ O + o  |
|  .   .ooo X . . |
| E     oo.. + .  |
+----[SHA256]-----+
```
```
  ssh-copy-id vagrant@target01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.
```
```
  ssh-copy-id vagrant@target02
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.
```
```
  ssh-copy-id vagrant@target03
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added.
```
Voici le résultat du ping ansible :
```
  ansible all -i target01,target02,target03 -m ping
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

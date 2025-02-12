# Exercice

Placez-vous dans le répertoire du dixième atelier pratique :
```
$ cd ~/formation-ansible/atelier-10
```
Voici les quatre machines virtuelles de cet atelier :
Machine virtuelle 	Adresse IP
ansible 	192.168.56.10
rocky 	192.168.56.20
debian 	192.168.56.30
suse 	192.168.56.40

Démarrez les VM :
```
$ vagrant up
```
Connectez-vous au Control Host :
```
$ vagrant ssh ansible
```
L’environnement de cet atelier est préconfiguré et prêt à l’emploi :

    Ansible est installé sur le Control Host.
    Le fichier /etc/hosts du Control Host est correctement renseigné.
    L’authentification par clé SSH est établie sur les trois Target Hosts.
    Le répertoire du projet existe et contient une configuration de base et un inventaire.
    Direnv est installé et activé pour le projet.
    Le validateur de syntaxe yamllint est également disponible.

Rendez-vous dans le répertoire du projet :
```
$ cd ansible/projets/ema/
direnv: loading ~/ansible/projets/ema/.envrc
direnv: export +ANSIBLE_CONFIG
$ ls -l
total 8
-rw-r--r--. 1 vagrant vagrant  65 Sep 19 14:26 ansible.cfg
-rw-r--r--. 1 vagrant vagrant 128 Sep 19 14:26 inventory
drwxr-xr-x. 2 vagrant vagrant   6 Sep 19 14:26 playbooks
```
Écrivez trois playbooks :

    Un premier playbook apache-debian.yml qui installe Apache sur l’hôte debian avec une page personnalisée Apache web server running on Debian Linux.
    Un deuxième playbook apache-rocky.yml qui installe Apache sur l’hôte rocky avec une page personnalisée Apache web server running on Rocky Linux.
    Un troisième playbook apache-suse.yml qui installe Apache sur l’hôte suse avec une page personnalisée Apache web server running on SUSE Linux.

Voici quelques pistes de réflexion :

    Pas la peine de rafraîchir le cache de paquets sous Rocky Linux et SUSE.
    Apache n’a pas forcément le même nom sur ces distributions.
    La même chose vaut pour le service correspondant.
    Rocky Linux utilise le gestionnaire de paquets dnf.
    SUSE Linux utilise le gestionnaire de paquets zypper.
    Les modules de gestion de paquets correspondants portent le même nom.
    L’emplacement de la page web par défaut (directive DocumentRoot) peut varier.
    J’ai supprimé le pare-feu FirewallD installé par défaut sous Rocky Linux pour ne pas trop vous embrouiller.

Voici le fichier ansible pour debian :
```
$ cat apache-rocky.yml 
---  # rocky-01.yml

- hosts: rocky

  tasks:

    - name: Update package information
      dnf:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      dnf:
        name: httpd

    - name: Start & enable Apache
      service:
        name: httpd
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>Apache web server running on Rocky Linux</h1>
            </body>
          </html>
```
Voici le résultat pour debian :
```
$ ansible-playbook apache-debian.yml

PLAY [debian] *******************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]

TASK [Update package information] ***********************************************
changed: [debian]

TASK [Install Apache] ***********************************************************
changed: [debian]

TASK [Start & enable Apache] ****************************************************
ok: [debian]

TASK [Install custom web page] **************************************************
changed: [debian]

PLAY RECAP **********************************************************************
debian                     : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
Voici le résultat du curl sur la machine debian :
```
$ curl debian
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
  </head>
  <body>
    <h1>Apache web server running on Debian Linux</h1>
  </body>
</html>
```

Voici le fichier ansible pour rocky :
```
$ cat apache-rocky.yml 
---  # rocky-01.yml

- hosts: rocky

  tasks:

    - name: Update package information
      dnf:
        update_cache: true

    - name: Install Apache
      dnf:
        name: httpd

    - name: Start & enable Apache
      service:
        name: httpd
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>Apache web server running on Rocky Linux</h1>
            </body>
          </html>
```
Voici le résultat pour rocky :
```
$ ansible-playbook apache-rocky.yml 

PLAY [rocky] ********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [rocky]

TASK [Update package information] ***********************************************
ok: [rocky]

TASK [Install Apache] ***********************************************************
changed: [rocky]

TASK [Start & enable Apache] ****************************************************
changed: [rocky]

TASK [Install custom web page] **************************************************
changed: [rocky]

PLAY RECAP **********************************************************************
rocky                      : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```
Voici le résultat du curl sur la machine rocky :
```
$ curl rocky
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
  </head>
  <body>
    <h1>Apache web server running on Rocky Linux</h1>
  </body>
</html>
```

Voici le fichier ansible pour rocky :
```
```
Voici le résultat pour rocky :
```
```
Voici le résultat du curl sur la machine rocky :
```
```

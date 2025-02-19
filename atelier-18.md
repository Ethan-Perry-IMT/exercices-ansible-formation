Exercice

Dans notre précédent exercice, nous avons mis en place la synchronisation NTP avec Chrony sur nos Target Hosts, en installant le fichier de configuration ci-dessous sur chacune des quatre cibles :

# chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony

Écrivez un playbook chrony.yml qui installe un fichier de configuration personnalisé sur vos cibles. La première ligne de commentaire devra indiquer le chemin complet vers le fichier :

Dans certains cas ce sera /etc/chrony/chrony.conf.
Dans d’autres cas ce sera simplement /etc/chrony.conf.

À vous de trouver une solution en utilisant les éléments que nous avons abordés dans cet article.

Voici le playbook :
```
---
- name: Installer et configurer Chrony sur les cibles
  
  hosts: all

  tasks:

    - name: Charger les vars des machines
      include_vars: >
        chrony_{{ ansible_distribution | lower | replace(" ", "-") }}.yml

    - name: Mettre à jour les informations des paquets sur Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Installer Chrony
      package:
        name: "{{ chrony_package_name }}"

    - name: Démarrer et activer Chrony au démarrage
      service:
        name: "{{ chrony_service_name }}"
        state: started
        enabled: true

    - name: Copier le fichier de configuration Chrony
      template:
        src: chrony.conf.j2
        dest: "{{ chrony_config_path }}"
        mode: '0644'
      notify: Redémarrer Chrony

  handlers:

    - name: Redémarrer Chrony
      service:
        name: "{{ chrony_service_name }}"
        state: restarted
```

Voici les fichiers de vars :
```
$ cat vars/chrony_debian.yml 
chrony_package_name: chrony
chrony_service_name: chrony
chrony_config_path: /etc/chrony/chrony.conf
```
```
$ cat vars/chrony_rocky.yml 
chrony_package_name: chrony
chrony_service_name: chronyd
chrony_config_path: /etc/chrony.conf
```
```
$ cat vars/chrony_opensuse-leap.yml 
chrony_package_name: chrony
chrony_service_name: chronyd
chrony_config_path: /etc/chrony.conf
```
```
$ cat vars/chrony_ubuntu.yml 
chrony_package_name: chrony
chrony_service_name: chrony
chrony_config_path: /etc/chrony/chrony.conf
```

Voici le template de la config de chrony :
```
$ cat templates/chrony.conf.j2 
# chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Voici le résultat après plusieurs exécution du playbook :
```
$ ansible-playbook chrony.yml 

PLAY [Installer et configurer Chrony sur les cibles] ****************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [suse]
ok: [ubuntu]
ok: [rocky]

TASK [Charger les vars des machines] ********************************************
ok: [rocky]
ok: [debian]
ok: [suse]
ok: [ubuntu]

TASK [Mettre à jour les informations des paquets sur Debian/Ubuntu] *************
skipping: [rocky]
skipping: [suse]
ok: [debian]
ok: [ubuntu]

TASK [Installer Chrony] *********************************************************
ok: [suse]
ok: [debian]
ok: [ubuntu]
ok: [rocky]

TASK [Démarrer et activer Chrony au démarrage] **********************************
ok: [debian]
ok: [ubuntu]
ok: [rocky]
ok: [suse]

TASK [Copier le fichier de configuration Chrony] ********************************
ok: [ubuntu]
ok: [debian]
ok: [suse]
ok: [rocky]

PLAY RECAP **********************************************************************
debian                     : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=5    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
suse                       : ok=5    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
ubuntu                     : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

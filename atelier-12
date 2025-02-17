Écrivez un playbook chrony.yml qui assure la synchronisation NTP de tous vos Target Hosts :

    Installez le paquet chrony.
    Activez et démarrez le service chronyd correspondant.
    Jetez un œil sur le fichier de configuration /etc/chrony.conf fourni par défaut.
    Installez une configuration personnalisée (cf. ci-dessous).
    Prenez en compte cette nouvelle configuration.
    Vérifiez la syntaxe correcte de votre playbook chrony.yml.
    Vérifiez l’idempotence de votre playbook.


Voici le playbook chrony.yml :
```
---
- name: Synchronisation NTP sur les Target Hosts
  hosts: redhat
  become: true
  tasks:

    - name: Installer le paquet chrony
      dnf:
        name: chrony
        state: present

    - name: Activer et démarrer le service chronyd
      systemd:
        name: chronyd
        enabled: true
        state: started

    - name: Installer une configuration personnalisée pour chrony
      copy:
        dest: /etc/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart chronyd

  handlers:
    - name: Restart chronyd
```
Voici le résultat de l'exécution de ce playbook :
```
$ ansible-playbook chrony.yml 

PLAY [Synchronisation NTP sur les Target Hosts] *********************************

TASK [Gathering Facts] **********************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Installer le paquet chrony] ***********************************************
ok: [target03]
ok: [target01]
ok: [target02]

TASK [Activer et démarrer le service chronyd] ***********************************
ok: [target02]
ok: [target03]
ok: [target01]

TASK [Installer une configuration personnalisée pour chrony] ********************
ok: [target01]
ok: [target02]
ok: [target03]

PLAY RECAP **********************************************************************
target01                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Grâce au handler au remarque que le changement n'a lieu qu'à la première exécution et que par la suite aucun changement n'a lieu.

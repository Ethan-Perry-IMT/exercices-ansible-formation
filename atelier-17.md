Exercice

Écrivez successivement deux playbooks pour mettre en place la synchronisation NTP avec Chrony sur vos quatre Target Hosts sous Debian, Rocky Linux, SUSE Linux et Ubuntu. Il vous faudra identifier le nom du paquet, le service correspondant et le chemin vers le fichier de configuration par défaut pour chacune des distributions.

Voici la configuration qu’il faudra installer sur chacune des quatre cibles :
```
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

Le premier playbook chrony-01.yml utilisera les modules de gestion de paquets natifs apt, dnf et zypper et s’inspirera de la méthode « gros sabots » utilisée plus haut dans cet article.
Voici le playbook :
```
---
- hosts: all
  tasks:

    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on SUSE Linux
      zypper:
        name: chrony
        state: present
      when: ansible_distribution == "openSUSE Leap"

    - name: Start and enable Chrony service
      service:
        name: chronyd
        state: started
        enabled: true

    - name: Configure Chrony
      copy:
        dest: "/etc/chrony.conf"
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Reload Chrony

  handlers:
    - name: Reload Chrony
      service:
        name: chronyd
        state: restarted
```
Voici le résultat après plusieurs exécution :
```
$ ansible-playbook chrony-01.yml 

PLAY [all] **********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [ubuntu]
ok: [rocky]
ok: [suse]

TASK [Install Chrony on Debian/Ubuntu] ******************************************
skipping: [rocky]
skipping: [suse]
ok: [debian]
ok: [ubuntu]

TASK [Install Chrony on Rocky Linux] ********************************************
skipping: [debian]
skipping: [suse]
skipping: [ubuntu]
ok: [rocky]

TASK [Install Chrony on SUSE Linux] *********************************************
skipping: [rocky]
skipping: [debian]
skipping: [ubuntu]
ok: [suse]

TASK [Start and enable Chrony service] ******************************************
ok: [debian]
ok: [ubuntu]
ok: [suse]
ok: [rocky]

TASK [Configure Chrony] *********************************************************
ok: [debian]
ok: [ubuntu]
ok: [rocky]
ok: [suse]

PLAY RECAP **********************************************************************
debian                     : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```
Le deuxième playbook chrony-02.yml définira trois variables chrony_package, chrony_service et chrony_confdir et utilisera le module de gestion de paquets générique package.
Voici le playbook :
```
---
- hosts: all
  vars:
    chrony:
      Debian:
        package_name: chrony
        service_name: chronyd
        confdir: /etc/chrony.conf
      Ubuntu:
        package_name: chrony
        service_name: chronyd
        confdir: /etc/chrony.conf
      Rocky:
        package_name: chrony
        service_name: chronyd
        confdir: /etc/chrony.conf
      openSUSE Leap:
        package_name: chrony
        service_name: chronyd
        confdir: /etc/chrony.conf

  tasks:
    - name: Install Chrony
      package:
        name: "{{ chrony[ansible_distribution].package_name }}"
        state: present

    - name: Start and enable Chrony service
      service:
        name: "{{ chrony[ansible_distribution].service_name }}"
        state: started
        enabled: true

    - name: Configure Chrony
      copy:
        dest: "{{ chrony[ansible_distribution].confdir }}"
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Reload Chrony

  handlers:
    - name: Reload Chrony
      service:
        name: "{{ chrony[ansible_distribution].service_name }}"
        state: restarted
```
Voici le résultat :
```
$ ansible-playbook chrony-02.yml 

PLAY [all] **********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [rocky]
ok: [suse]
ok: [ubuntu]

TASK [Install Chrony] ***********************************************************
ok: [suse]
ok: [debian]
ok: [ubuntu]
ok: [rocky]

TASK [Start and enable Chrony service] ******************************************
ok: [debian]
ok: [ubuntu]
ok: [suse]
ok: [rocky]

TASK [Configure Chrony] *********************************************************
ok: [debian]
ok: [rocky]
ok: [ubuntu]
ok: [suse]

PLAY RECAP **********************************************************************
debian                     : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

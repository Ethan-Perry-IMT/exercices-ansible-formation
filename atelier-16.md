Exercice

Écrivez trois playbooks pour afficher des informations sur chacun des Target Hosts :

pkg-info.yml pour afficher le gestionnaire de paquets utilisé
Voici le playbook :
```
---
- hosts: all
  gather_facts: true

  tasks:
    - name: Affichage du gestionnaire de packages
      debug:
        msg: >
          La machine [{{ inventory_hostname }}] utilise {{ ansible_pkg_mgr  }}
```
Voici le résultat :
```
$ ansible-playbook pkg-info.yml 

PLAY [all] **********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Affichage du gestionnaire de packages] ************************************
ok: [rocky] => {
    "msg": "La machine [rocky] utilise dnf\n"
}
ok: [debian] => {
    "msg": "La machine [debian] utilise apt\n"
}
ok: [suse] => {
    "msg": "La machine [suse] utilise zypper\n"
}

PLAY RECAP **********************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
python-info.yml pour afficher la version de Python installée
Voici le playbook :
```
---
- hosts: all
  gather_facts: true

  tasks:
    - name: Affichage de la version de python
      debug:
        msg: "La machine [{{ inventory_hostname }}] utilise la version de python {{ ansible_python_version }}"

```
Voici le résultat :
```
$ ansible-playbook python-info.yml 

PLAY [all] **********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Affichage de la version de python] ****************************************
ok: [rocky] => {
    "msg": "La machine [rocky] utilise la version de python 3.9.18"
}
ok: [debian] => {
    "msg": "La machine [debian] utilise la version de python 3.11.2"
}
ok: [suse] => {
    "msg": "La machine [suse] utilise la version de python 3.6.15"
}

PLAY RECAP **********************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
dns-info.yml pour afficher le(s) serveur(s) DNS utilisé(s)
Voici le playbook :
```
---
- hosts: all
  gather_facts: true

  tasks:
    - name: Affichage de(s) serveur(s) DNS
      debug:
        msg: "La machine [{{ inventory_hostname }}] utilise le(s) serveur(s) DNS : {{ ansible_dns.nameservers }}"
```
Voici le résultat :
```$ ansible-playbook dns-info.yml 

PLAY [all] **********************************************************************

TASK [Gathering Facts] **********************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Affichage de(s) serveur(s) DNS] *******************************************
ok: [rocky] => {
    "msg": "La machine [rocky] utilise le(s) serveur(s) DNS : ['10.0.2.3']"
}
ok: [debian] => {
    "msg": "La machine [debian] utilise le(s) serveur(s) DNS : ['4.2.2.1', '4.2.2.2', '208.67.220.220']"
}
ok: [suse] => {
    "msg": "La machine [suse] utilise le(s) serveur(s) DNS : ['10.0.2.3']"
}

PLAY RECAP **********************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

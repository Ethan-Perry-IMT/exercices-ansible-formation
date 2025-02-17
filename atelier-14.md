Écrivez un playbook myvars1.yml qui affiche respectivement votre voiture et votre moto préférée en utilisant le module debug et deux variables mycar et mybike définies en tant que play vars.
```
---
- name: Afficher ma voiture et ma moto préférées
  hosts: all
  vars:
    mycar: "nissan gtr r34"
    mybike: "600 hornet"
  tasks:
    - name: Afficher la voiture préférée
      debug:
        msg: "Ma voiture préférée est {{ mycar }}."

    - name: Afficher la moto préférée
      debug:
        msg: "Ma moto préférée est {{ mybike }}."

```
Le résultat :
```
$ ansible-playbook myvars1.yml 

PLAY [Afficher ma voiture et ma moto préférées] *********************************

TASK [Gathering Facts] **********************************************************
ok: [target02]
ok: [target01]
ok: [target03]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est 600 hornet."
}
ok: [target02] => {
    "msg": "Ma moto préférée est 600 hornet."
}
ok: [target03] => {
    "msg": "Ma moto préférée est 600 hornet."
}

PLAY RECAP **********************************************************************
target01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
En utilisant les extra vars, remplacez successivement l’une et l’autre marque – puis les deux à la fois – avant d’exécuter le play.
```
$ ansible-playbook myvars1.yml -e mycar=mx5 -e mybike=yamaha_r1

PLAY [Afficher ma voiture et ma moto préférées] *********************************

TASK [Gathering Facts] **********************************************************
ok: [target02]
ok: [target01]
ok: [target03]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est mx5."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est mx5."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est mx5."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est yamaha_r1."
}
ok: [target02] => {
    "msg": "Ma moto préférée est yamaha_r1."
}
ok: [target03] => {
    "msg": "Ma moto préférée est yamaha_r1."
}

PLAY RECAP **********************************************************************
target01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Écrivez un playbook myvars2.yml qui fait essentiellement la même chose que myvars1.yml, mais en utilisant une tâche avec set_fact pour définir les deux variables.
```
---
- name: Afficher ma voiture et ma moto préférées avec set_fact
  hosts: all
  tasks:
    - name: Définir la voiture préférée
      set_fact:
        mycar: "nissan gtr r34"

    - name: Définir la moto préférée
      set_fact:
        mybike: "600 hornet"

    - name: Afficher la voiture préférée
      debug:
        msg: "Ma voiture préférée est {{ mycar }}."

    - name: Afficher la moto préférée
      debug:
        msg: "Ma moto préférée est {{ mybike }}."

```
Le résultat :
```
$ ansible-playbook myvars2.yml 

PLAY [Afficher ma voiture et ma moto préférées avec set_fact] *******************

TASK [Gathering Facts] **********************************************************
ok: [target03]
ok: [target02]
ok: [target01]

TASK [Définir la voiture préférée] **********************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Définir la moto préférée] *************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est nissan gtr r34."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est 600 hornet."
}
ok: [target02] => {
    "msg": "Ma moto préférée est 600 hornet."
}
ok: [target03] => {
    "msg": "Ma moto préférée est 600 hornet."
}

PLAY RECAP **********************************************************************
target01                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Là aussi, essayez de remplacer les deux variables en utilisant des extra vars avant l’exécution du play.
```
$ ansible-playbook myvars2.yml -e mycar=mx5 -e mybike=yamaha_r1

PLAY [Afficher ma voiture et ma moto préférées avec set_fact] *******************

TASK [Gathering Facts] **********************************************************
ok: [target02]
ok: [target03]
ok: [target01]

TASK [Définir la voiture préférée] **********************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Définir la moto préférée] *************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est mx5."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est mx5."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est mx5."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est yamaha_r1."
}
ok: [target02] => {
    "msg": "Ma moto préférée est yamaha_r1."
}
ok: [target03] => {
    "msg": "Ma moto préférée est yamaha_r1."
}

PLAY RECAP **********************************************************************
target01                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Écrivez un playbook myvars3.yml qui affiche le contenu des deux variables mycar et mybike mais sans les définir. Avant d’exécuter le playbook, définissez VW et BMW comme valeurs par défaut pour mycar et mybike pour tous les hôtes, en utilisant l’endroit approprié.
Voici le playbook :
```
---
- name: Afficher la voiture et la moto sans définir les variables dans le playbook
  hosts: all
  tasks:
    - name: Afficher la voiture préférée
      debug:
        msg: "Ma voiture préférée est {{ mycar }}."

    - name: Afficher la moto préférée
      debug:
        msg: "Ma moto préférée est {{ mybike }}."

```
Voici le fichier group_vars/all.yml :
```
---  # group_vars/all.yml

mycar: VW
mybike: BMW

...
```
Le résultat
```
$ ansible-playbook ../playbooks/myvars3.yml 

PLAY [Afficher la voiture et la moto sans définir les variables dans le playbook] ***

TASK [Gathering Facts] **********************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est VW."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est VW."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est VW."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est BMW."
}
ok: [target02] => {
    "msg": "Ma moto préférée est BMW."
}
ok: [target03] => {
    "msg": "Ma moto préférée est BMW."
}

PLAY RECAP **********************************************************************
target01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
Effectuez le nécessaire pour remplacer VW et BMW par Mercedes et Honda sur l’hôte target02.
Voici le fichier host_vars/target02.yml :
```
---  # host_vars/target02.yml

mycar: Mercedes
mybike: Honda

...
```
Voici le résultat :
```
$ ansible-playbook ../playbooks/myvars3.yml 

PLAY [Afficher la voiture et la moto sans définir les variables dans le playbook] ***

TASK [Gathering Facts] **********************************************************
ok: [target03]
ok: [target01]
ok: [target02]

TASK [Afficher la voiture préférée] *********************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est VW."
}
ok: [target02] => {
    "msg": "Ma voiture préférée est Mercedes."
}
ok: [target03] => {
    "msg": "Ma voiture préférée est VW."
}

TASK [Afficher la moto préférée] ************************************************
ok: [target01] => {
    "msg": "Ma moto préférée est BMW."
}
ok: [target02] => {
    "msg": "Ma moto préférée est Honda."
}
ok: [target03] => {
    "msg": "Ma moto préférée est BMW."
}

PLAY RECAP **********************************************************************
target01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Écrivez un playbook display_user.yml qui affiche un utilisateur et son mot de passe correspondant à l’aide des variables user et password. Ces deux variables devront être saisies de manière interactive pendant l’exécution du playbook. Les valeurs par défaut seront microlinux pour user et yatahongaga pour password. Le mot de passe ne devra pas s’afficher pendant la saisie.
Voici le playbook :
```
---
- name: Afficher l'utilisateur et son mot de passe
  hosts: localhost
  gather_facts: false

  vars_prompt:
    - name: user
      prompt: "Entrez le nom d'utilisateur"
      default: "microlinux"
      private: false

    - name: password
      prompt: "Entrez le mot de passe"
      default: "yatahongaga"
      private: true

  tasks:
    - name: Afficher l'utilisateur et son mot de passe
      debug:
        msg: "L'utilisateur est {{ user }} et le mot de passe est {{ password }}"
```
Voici le résultat :
```
$ ansible-playbook display_user.yml 
Entrez le nom d'utilisateur [microlinux]: ema
Entrez le mot de passe [yatahongaga]: 

PLAY [Afficher l'utilisateur et son mot de passe] *******************************

TASK [Afficher l'utilisateur et son mot de passe] *******************************
ok: [localhost] => {
    "msg": "L'utilisateur est ema et le mot de passe est ema1234"
}

PLAY RECAP **********************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Voici le résultat par défaut :
```
$ ansible-playbook display_user.yml 
Entrez le nom d'utilisateur [microlinux]: 
Entrez le mot de passe [yatahongaga]: 

PLAY [Afficher l'utilisateur et son mot de passe] *******************************

TASK [Afficher l'utilisateur et son mot de passe] *******************************
ok: [localhost] => {
    "msg": "L'utilisateur est microlinux et le mot de passe est yatahongaga"
}

PLAY RECAP **********************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

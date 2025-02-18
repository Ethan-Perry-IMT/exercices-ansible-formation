Exercice

Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg.
Voici le playbook kernel.yml :
```
---
- hosts: all
  gather_facts: false

  tasks:

    - name: Report kernel information
      command: uname -a
      changed_when: false
      register: kernel_info

    - debug:
        msg: "{{ kernel_info.stdout }}"
```
Voici le résultat :
```
$ ansible-playbook kernel.yml 

PLAY [all] **********************************************************************

TASK [Report kernel information] ************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [debug] ********************************************************************
ok: [rocky] => {
    "msg": "Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
}
ok: [debian] => {
    "msg": "Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
}
ok: [suse] => {
    "msg": "Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
}

PLAY RECAP **********************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Essayez d’obtenir le même résultat en utilisant le paramètre var du module debug.
Voici le nouveau playbook kernel.yml :
```
---
- hosts: all
  gather_facts: false

  tasks:

    - name: Report kernel information
      command: uname -a
      changed_when: false
      register: kernel_info

    - debug:
        var: kernel_info.stdout
```
Voici le résultat de ce nouveau playbook :
```
$ ansible-playbook kernel.yml 

PLAY [all] **********************************************************************

TASK [Report kernel information] ************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [debug] ********************************************************************
ok: [rocky] => {
    "kernel_info.stdout": "Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
}
ok: [debian] => {
    "kernel_info.stdout": "Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
}
ok: [suse] => {
    "kernel_info.stdout": "Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
}

PLAY RECAP **********************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Écrivez un playbook packages.yml qui affiche le nombre total de paquets RPM installés sur les hôtes rocky et suse (rpm -qa | wc -l).
Voici le playbook packages.yml :
```
---
- hosts: rocky,suse
  gather_facts: false

  tasks:

    - name: Compte le nombre de packages RPM sur les machines
      shell: rpm -qa | wc -l
      changed_when: false
      register: rpm_count

    - debug:
        msg: "Nombre de packages RPM sur la machine: {{ rpm_count.stdout }}"
```
Voici le résultat :
```
$ ansible-playbook packages.yml 

PLAY [rocky,suse] ***************************************************************

TASK [Compte le nombre de packages RPM sur les machines] ************************
ok: [rocky]
ok: [suse]

TASK [debug] ********************************************************************
ok: [rocky] => {
    "msg": "Nombre de packages RPM sur la machine: 671"
}
ok: [suse] => {
    "msg": "Nombre de packages RPM sur la machine: 917"
}

PLAY RECAP **********************************************************************
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

Obtenir des informations sur le processeur
##########################################
:date: 2006-07-17 16:59
:tags: Matériel

Bon aller j'innaugure la catégorie "Matériel". La encore on va commencer doucement. C'est juste une commande toute simple fonctionnant sur toutes les distributions linux qui permet d'afficher les informations sur le processeur un peu comme le ferait "everest" sous windows. Bien entendu tous ça sans interface graphique.

|image0|

 La commande est la suivante:

.. code-block:: sh

    cat /proc/cpuinfo

Voici les résultat de cette commande pour mon nouveau pc portable (MSI Mega Book S270)

.. code-block:: sh

    roro@ubuntu:~$ cat /proc/cpuinfo
    processor : 0
    vendor\_id : AuthenticAMD
    cpu family : 15
    model : 36
    model name : AMD Turion(tm) 64 Mobile Technology MT-34
    stepping : 2
    cpu MHz : 1790.880
    cache size : 1024 KB
    fpu : yes
    fpu\_exception : yes
    cpuid level : 1
    wp : yes
    flags : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca
    cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx mmxext fxsr\_opt
    lm 3dnowext 3dnow pni lahf\_lm
    bogomips : 3589.08
    TLB size : 1024 4K pages
    clflush size : 64
    cache\_alignment : 64
    address sizes : 40 bits physical, 48 bits virtual
    power management: ts fid vid ttp tm stc

Et voilà! Il existe un autre méthode qui nécessite l'utilisation de x86info. J'en parlerai lors d'un prochain billet.  Ce billet est le premier d'un longue série dans lequel nous apprendront à interroger le système ou les outils mis à notre disposition afin d'obtenir le maximum d'information sur son matériel.

.. |image0| image:: http://img.clubic.com/photo/00028967.jpg

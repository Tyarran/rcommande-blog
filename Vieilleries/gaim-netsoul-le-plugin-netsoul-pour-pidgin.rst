Gaim-netsoul: le plugin Netsoul pour Pidgin
###########################################
:date: 2009-11-15 11:54

|image0|

Netsoul est un protocole de communication réseau réservé aux étudiants du groupe Ionis.  Pidgin est un client de messagerie instantanée multiprotocole et multiplateforme. Une extension nommée Gaim-netsoul permet d'ajouter le support de Netsoul avec Pidgin.

L'installation avec un paquet
-----------------------------

Nous n'aborderons ici que l'installation depuis un paquet pré-compilé car c'est la méthode la plus simple et que cela à parfaitement fonctionné pour moi.
Ren root:

.. code-block:: sh

    wget http://tombcore.free.fr/netsoul_0.2.2-1_i386.deb && dpkg -i netsoul_0.2.2-1_i386.deb

Et voilà pour pouvez maintenant communiquer via le procole Netsoul depuis Pidgin.

Attention cela n'a pas l'air de fonctionner sur une Debian stable, très certainement parce que la version de Pidgin disponible sur cette distribution est trop ancienne. Pour un système 64 bits, privilégier une installation via les sources (documentation `http://doc.ubuntu-fr.org/netsoul`_).

.. _`http://doc.ubuntu-fr.org/netsoul`: http://doc.ubuntu-fr.org/netsoul

.. |image0| image:: http://gabuntu.files.wordpress.com/2009/01/pidgin_dock_icon_by_d4rk_h3lm37.png

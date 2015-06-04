[gentoo] Activer le support de l'HFS et l'HFS+ (système de fichiers Mac)
########################################################################
:date: 2007-07-28 21:23
:tags: Gentoo

.. role:: strike
    :class: strike

.. image:: http://www.chromebook-linux.com/wp-content/uploads/2011/11/gentoolinux1.png
    :align: center
    :width: 200px

Il peut arriver que l'on ait un voisin qui utilise exclusivement des machines de la firme de la Pomme qui a la gentillesse de nous prêter son disque dur externe ou son IPOD qui utilise un système de fichiers MAC (appelé HFS ou HFS+ pour les intimes). Malheureusement, il est impossible de le lire avec la configuration d'origine (genkernel) de Gentoo.
PAS DE PANIQUE! Voici la solution.
Elle convient aussi bien pour Gentoo Linux que pour une autre distribution mais il y a de fortes chances pour que celle-ci ai déjà le support de ce système de fichiers activé par défaut.

Cette manipulation n'est pas bien compliquée mais demande d'aller toucher quelques options dans le noyau ce qui peut être risqué si l'on fait n'importe quoi (normalement ça ne sera pas le cas si vous suivez ces instructions à la lettre ;-) ) .Pensez à toujours garder un noyau valide de coté de façon à ne pas vous retrouver...comment dire...embêter (le fameux "kernel panic"). Dans ce qui suit je ne traiterai pas le compilation du noyau en détail. Ce sera le cas pour un futur billet.

Bon allez, c'est parti! Dans un premier temps il faut se placer dans le répertoire contenant les sources de votre noyau. Il est placé dans le répertoire "/usr/src" sous la forme d'un répertoire nommé "linux-2.6.XX-gentoo-rX" si vous utiliser les "Gentoo sources".  Normalement vous devriez avoir un raccourci nommé "linux" pointant vers la version les sources les plus récentes. Plaçons nous dans ce répertoire.

.. code-block:: text

    su root $ cd /usr/src/linux

Maintenant il faut configurer le noyau. Pour cela il existe trois interfaces différentes: menuconfig, xconfig et gconfig. Toutes font exactement la même chose, mais la première est une interface utilisant la librairie graphique NCURSES, la deuxième est une interface QT et la dernière du GTK. Libre à vous d'utiliser celle qui vous plaira le mieux.  Personnellement j'utilise menuconfig.

.. code-block:: text

    make menuconfig
        --> File systems
        --> Miscellaneous filesystems
            <*> Apple Macintosh file system support (EXPERIMENTAL)
            <*> Apple Extended HFS file system support

Sauvegardez et quittez la configuration du noyau. Maintenant on va le compiler:

.. code-block:: text

    make && make modules_install

Allez vous préparer un petit café (ça peut prendre un petit moment) et voila votre noyau à été construit correctement.

Passons maintenant à l'installation. Pour cela il faut copier le noyau compilé dans le dossier "boot".  Nommer le comme bon vous semble mais pas de la même façon que l'ancien. Si celui-ci pose problème vous pourrez toujours redémarrer sur le précédent.

.. code-block:: text

    cp arch/i386/boot/bzImage /boot/kernel -2.6.XX-gentoo-rX-hfs_support
    #Remplacez les "X" par les numéros de la version de votre noyau``

Dernière étape, on configure Grub pour qu'il puisse booter sur le nouveau noyau. Pour cela il faut éditer le fichier "menu.lst" avec votre éditeur de texte préféré. Pour ma part il s'agît de Vim:

.. code-block:: text

    vim /boot/grub/menu.lst

ajoutez:

.. code-block:: text

    title=Gentoo Linux 2.6.XX avec le support HFS+ #Le nom qui apparaîtra dans le menu de Grub personnalisez le comme vous voudrez
    root (hd0,1) #Recopiez la ligne qui correspond à votre ancien noyau de façon à ne pas faire d'erreur!
    kernel /boot/kernel-2.6.21-gentoo-r3-hfs_support #Le noyau à démarrer. Mettez le même nom que celui que vous avez choisi pour votre noyau

Maintenant rebooter votre machine et choisissez votre nouveau noyau dans le menu de grub. Si vous avez suivi :strike:`si je vous ai bien guidé` toutes les étapes à la lettre vous devriez avoir une machine fonctionnelle avec le support de l'HFS et HFS+. Pour le vérifier il suffit de taper la commande suivante:

.. code-block:: text

    cat /proc/filesystems | grep hfs

Et vous devriez voir apparaître "hfsplus et hfs" dans la liste |:-)| Pour monter vos partitions (ci elle n'ont pas été monté automatiquement) (en root):

.. code-block:: text

    mount -t hfsplus /dev/partition /point_de_montage

Et voila vous avez accès a votre partition HFS/HFS+. J'espère que cela vous aura servi.

.. |;-)| image:: http://www.unblogsurlabanquise.org/themes/default/smilies/wink.png
.. |:-)| image:: http://www.unblogsurlabanquise.org/themes/default/smilies/smile.png

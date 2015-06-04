Astuce: Forcer le "check forced" au prochain démarrage
######################################################
:date: 2007-01-10 16:52
:tags: General

.. figure:: http://www.webtools.epiknet.org/gfx/outils.gif
   :align: center
   :alt:

Une astuce toute simple mais souvent très utile.

Mise en situation
=================

Situation 1
***********

Vous avez redemarré votre système de façon incorrect (bouton ON/OFF, bouton reset, panne de courant...) ce qui n’est pas du tout recommandé car cela aîme votre sytème de fichiers.

Situation 2
***********

Vous avez un ordinateur portable que vous devez emmener en déplacement.  Vous partez et une fois à destination vous allumez votre ordinateur et la “check forced”, 10 minutes de batterie en moins qui aurait pu être évité avec l’astuce suivante.

L'astuce
========

Vous connaissez certainement la commande “shutdown -r now” qui ordonne à vôtre système de redemarrer (il y a aussi “reboot” qui fait exactement la même chose) et bien pour redemarrer en demandant d’effectuer un “check forced” sur vos partitions au prochain démarrage voici comment il faut procéder:

.. code-block:: sh

    shutdown -r -F now

Cette commande aura pour effet de redemarrer votre système d’exploitation et d’effectuer le “check forced”. Bien sur cette commande doit être tapé en tant que superutilisateur.

Voila cétait pas bien compliqué =)

Retrouvez cette astuce sur le wiki à l'adresse suivante:

`http://unblogsurlabanquise.org/doku/doku.php/check`_

.. _`http://unblogsurlabanquise.org/doku/doku.php/check`: http://unblogsurlabanquise.org/doku/doku.php/check

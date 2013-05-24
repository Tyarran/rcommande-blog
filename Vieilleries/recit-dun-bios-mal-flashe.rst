Récit d'un BIOS mal Flashé
##########################
:date: 2009-09-15 20:26
:tags: Matériel

Une mise à jour de BIOS n'est pas une opération anodine, comme le prouve ce récit de ma propre histoire...

En vouloir toujours plus
------------------------

Je possède, depuis bientôt un an, un ordinateur portable Fujitsu-Siemens AMILO XI 1546 et j'en suis très heureux.
Mais voila, un petit problème au niveau de l'`ACPI`_ ("Advanced Configuration and Power Interface" me souffle Wikipedia) ralentissait le démarrage de ma machine sous Linux. Ce ralentissement n'était pas forcement conséquent (seulement une 10ène de secondes de perdu au chargement du noyau) mais comme tout bon informaticien, cela me dérangeait au plus haut point et sa résolution est devenu une priorité.

A la recherche de la solution miracle
-------------------------------------

Désactivation de l'ACPI
~~~~~~~~~~~~~~~~~~~~~~~

La première solution qui me viens à l'esprit est de désactiver l'ACPI au démarrage du système. Pour cela c'est assez simple il suffit de rajouter "acpi=off" dans les options de `GRUB`_ . Sur le principe, cela fonctionne: l'ordinateur démarre beaucoup plus rapidement mais je perds les informations sur la charge de la batterie, la vitesse du processeur etc...Donc finalement aucun intérêt.

Mise à jour (flash) du BIOS
~~~~~~~~~~~~~~~~~~~~~~~~~~~

La deuxième solution que je choisi est de mettre à jour le `BIOS`_: on ne sait jamais, l'équipe de Fujitsu-Siemens à peut-être corrigé la gestion de l'ACPI depuis la dernière version.

Après un petit tour rapide sur le `site du support du constructeur`_ et je m'apperçoit que Fujitsu fournit un certain nombre de correctifs. Tout bon informaticien sait que la mise à jour d'un BIOS est potentiellement risquée, mais j'ai déjà réussi cette opération des dizaines et des dizaines de fois sur tout un tas de machines différentes. "Il n'y a pas de raison que cela ce passe mal", me dis-je. Je télécharge la dernière version du BIOS, j'extrais l'archive et j'obtiens plusieurs fichiers dont: un logiciel de flashage, le fichier binaire contenant le BIOS et, encore mieux, un script batch pour simplifier encore plus la manipulation. C'est parti, je me lance.

Le mieu est l'ennemi du bien
----------------------------

Je ne sais pas si c'est toujours d'actualité, mais normalement le flashage d'un BIOS se fait depuis un DOS complet, c'est à dire que la manipulation n'est pas à faire depuis l'invite de commande MS-DOS sous Windows. J'ai pris l'habitude de le faire depuis un environnement `FREEDOS`_ (clone libre et 100% compatible avec l'original) et cela à toujours bien fonctionné.

Une mise à jour du BIOS ne doit jamais être interrompu. Je vérifie donc si l'alimentation est bien branché et j'ajoute même la batterie sur le portable en cas de coupure EDF.

Une fois l'environnement lancé, je lance le script batch. l'opération commence, un tas de chose incompréhensible pour moi apparaissent (certainement les différentes valeurs écritent lors de la reprogrammation de l'EPROM...) à l'écran. Quand soudain "CLAC!", les haut-parleurs de l'ordinateur claquent et le pc s'éteint.

J'ai pensé (ou plutôt espéré) que la mise à jour soit terminé et que le logiciel est volontairement mis la machine hors-tension. Le "clac" des haut-parleurs est normal et se produit à chaque extinction de la machine et ce depuis toujours. j'appuie sur le bouton ON/OFF de la machine: le voyant s'allume et se ré-éteint immédiatement. Cette fois, c'est définitif, la machine est HS car elle ne dispose plus de BIOS fonctionnel et ne peut donc plus démarrer.

A vouloir trop en faire (perdre 10 secondes au démarrage est loin d'être un drame) j'ai tout perdu.

La fin de l'histoire
--------------------

La machine est à l'heure actuel en voyage pour réparation. A ce sujet je voudrais terminer ce billet sur une petite FAQ afin de mettre fin à certaines erreurs que l'on trouve sur internet.

J'ai lu sur internet qu'après un flashage de BIOS raté la carte mère

**était HS donc bonne pour la poubelle, non?**

**FAUX**! Mon portable n'est pas à la poubelle et cette erreur est souvent rapporté par les personnes ayant contacté le constructeur de leur matériel qui préfère remplacer la carte mère plutôt que reprogrammer l'EPROM de celle-ci.

**Donc n'importe quel BIOS peut-être réparée?**

Oui et non. Techniquement oui, n'importe quel EPROM peut-être reprogrammée. Pour une EPROM installé sur un socket, c'est à dire non-soudé à la carte mère, cela est très simple et des entreprises spécialisées reprogrammerons votre EPROM pour moins de 10€.

**Mais tu ne parles pas du "non". Si mon EPROM est soudé ce n'est pas réparable, c'est bien ça?**

Si c'est réparable, mais c'est plus compliqué et ce n'est pas souvent une bonne idée. Cette opération est plus couteuse que la première (environ 30€ pour un pc de bureau) et connaissant le prix de certaines cartes mères bon marché, il est peu être moins couteux de racheter une nouvelle carte mère. Par contre pour un pc portable, cela coute environ 70€ mais n'est rien à coté d'une carte mère neuve (en général cela correspond au prix d'un ordinateur portable neuf)

**Tu connais une entreprise spécialisé dans la reprogrammation d'EPROM?**

J'ai eu affaire à `FGL Service`_ qui est en France. Elle propose une garantie satisfait ou remboursé, les prix sont interessants et répond très rapidement par email. Maintenant je ne peux pas en dire plus, mon portable n'est pas encore revenu de chez eux (il ne doit même pas etre encore arrivé chez eux en faite :-p)

.. note::
    16-09-2009: FGL Service m'a renvoyé mon portable sous une semaine avec le dernier BIOS restauré. Je recommande vraiment.

**J'ai lu qu'une astuce permet de solutionner le problème en retirant la pile de la carte mère, ça fonctionne?**

Non, on appel ça un CLEAR-CMOS. Certaine carte mère on même un cavalier pour pouvoir faire la même chose sans retirer la pile. Cela permet de supprimer les réglages du BIOS mais en aucun cas cela ne répare un BIOS défectueux. Je ne vais pas vous cacher que j'ai quand même essayé en sachant cela...:-p

Dans cette histoire, j'ai peut-être commis beaucoup d'erreurs. La mise à jour d'un BIOS peut-il amélioré le support de l'ACPI? Pourquoi avoir tenté l'opération depuis Freedos...etc. Mais ce n'est pas problème, le but étant de montrer que le flashage d'un BIOS, même de nos jour, est une opération délicate et ne doit être effectué que s'il est nécessaire (dysfonctionnements importants).

.. _ACPI: http://fr.wikipedia.org/wiki/Advanced_Configuration_and_Power_Interface
.. _GRUB: http://fr.wikipedia.org/wiki/GRand_Unified_Bootloader
.. _BIOS: http://fr.wikipedia.org/wiki/Basic_Input_Output_System
.. _site du support du constructeur: http://support.ts.fujitsu.com/com/support/linkapplication.html?LNG=EN&ProductID=25074
.. _FREEDOS: http://fr.wikipedia.org/wiki/Freedos
.. _FGL Service: http://www.fgl-services.com

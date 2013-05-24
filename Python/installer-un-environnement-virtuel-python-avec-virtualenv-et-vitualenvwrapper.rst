Installer un environnement virtuel Python avec VirtualEnv et VirtualEnvWrapper
==============================================================================
:tags: Développement, Python
:date: 2012-04-22 17:04

Python est un langage de programmation très répandu, surtout sur les Unix-like qui l'utilisent au coeur même du système.

Le soucis, c'est que le système à besoin d'un certain nombre de modules Python dans des versions bien spécifiques pour pouvoir fonctionner correctement. De plus, jongler entre l'usage des gestionnaires de paquets (apt, yum, pacman, et j'en passe) et le gestionnaire de module Python (setuptools, easy_install, pip) n'est vraiment pas conseillé.

Lorsque l'on commence à développer, il y a de forte de chance qu'une mise à jour de ces modules soit nécessaire, or, le risque de conflits avec le système est grand.

Pour finir, il se peut que deux applications Python fassent appel au même module mais dans deux versions différentes. Autant dire que pour beaucoup de monde, cela peut apparaitre comme une vrai galère, et pourtant la solution n'est vraiment pas compliqué...

Virtualenv
##########
C'est exactement ce que viens rédoudre VirtualEnv et VirtualEnvWrapper. Le module Virtualenv va creer un environnement Python virtuel isolé. Il n'y aura plus 1 seul répertoire "site-package" mais autant que d'environnement virtuel. Les problèmes de conflits se sont envollés.

Nous n'utiliserons pas VirtualEnv directement mais une surcouche: VirtualenvWrapper. Il s'agît tout simplement d'un module faisant office de surcouche à VirtualEnv, simplifiant encore plus la gestion des environnements virtuels.

En revanche, et bien que VirtualEnv fonctionne parfaitement sous Windows, ce n'est pas le cas de *VirtualEnvWrapper*.

Installation
------------
L'installation est très simple (comme souvent), le gestionnaire de paquets de la distribution préférée fera parfaitement l'affaire. Dans mon cas sur Arch Linux:

.. code-block:: sh

    pacman -S python-virtualenv python-Virtualenvwrapper

À partir de maintenant, il y a une règle absolue à respecter::

    Il est interdit d'installer le moindre module sur le Python du système. Les environnements sont là pour cela!

Usage
-----

Alors, maitenant, comment ça fonctionne ce truc? C'est très simple. La première chose à faire et de taper la commande suivante:

.. code-block:: sh

    source virtualenvwrapper.sh

Je vous recommande de placer cette ligne dans votre fichier .bashrc ou .zshrc afin de ne pas avoir à la retapper par la suite.
De nouvelles commandes ont été ajoutées:

* mkvirtualenv: créé un nouvel environnement virtuel
* rmvirtualenv: supprime un environnement virtuel
* workon: permet d'entrer dans un environnement virtuel
* deactivate: permet de sortir d'un environnement virtuel

Il y en a certainement d'autres, je ne connais pas tout l'outil, mais ces quelques commandes simplifient vraiment la vie.
Voici un exemple d'usage de VirtualEnvWrapper:

.. code-block:: sh

    23:41 romain@myarch ~% mkvirtualenv -p python2 monenvironnement #Création de monenvironnement
    Running virtualenv with interpreter /usr/bin/python2
    New python executable in monenvironnement/bin/python2
    Also creating executable in monenvironnement/bin/python
    Installing setuptools............................done.
    Installing pip...............done.
    (...)
    (...)
    (monenvironnement)23:42 romain@myarch ~% pip install sqlalchemy==0.5 #On installe sqlalchemy v0.5
    Downloading/unpacking sqlalchemy==0.5
    Downloading SQLAlchemy-0.5.0.tar.gz (1.4Mb): 1.4Mb downloaded
    Running setup.py egg_info for package sqlalchemy

    Installing collected packages: sqlalchemy
    Running setup.py install for sqlalchemy

    (...)
    (...)

    Successfully installed sqlalchemy
    Cleaning up...
    (monenvironnement)23:43 romain@myarch ~% deactivate #on sort de monenvironnement
    23:43 romain@myarch ~% mkvirtualenv -p python2 monenvironnement2 #création de monenvironnement2
    Running virtualenv with interpreter /usr/bin/python2
    New python executable in monenvironnement2/bin/python2
    Also creating executable in monenvironnement2/bin/python
    Installing setuptools............................done.
    Installing pip...............done.
    (...)
    (...)
    (monenvironnement2)23:43 romain@myarch ~% pip install sqlalchemy==0.7 #on installe sqlalchemy v0.7.0
    Downloading/unpacking sqlalchemy==0.7
    Downloading SQLAlchemy-0.7.0.tar.gz (2.3Mb): 2.3Mb downloaded
    Running setup.py egg_info for package sqlalchemy

    (...)
    (...)
    Successfully installed sqlalchemy
    Cleaning up...
    (monenvironnement2)23:44 romain@myarch ~% deactivate #On sort de monenvironnement2
    23:46 romain@myarch ~% rmvirtualenv monenvironnement2 #on détruit monenvironnement2
    Removing monenvironnement2...
    23:46 romain@myarch ~% workon monenvironnement #on retourne dans monenviromment
    (monenvironnement)23:47 romain@myarch ~%

Voila pour cette petite introduction au environnements virtuels Python. Il faut tout de même savoir que leurs usages sont fortement recommandés, voir indispenssables en production.


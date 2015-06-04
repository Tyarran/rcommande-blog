Papaye: le clone de PyPi
########################
:date: 2014-07-25
:tags: Programmation, Développement, Python, Projet

.. role:: underline
    :class: underline


.. image::  ../pictures/papaya.jpg
   :alt: Papaya de ramyo, sur Flickr
   :align: center
   :width: 960px
   :height: 200px


Aujourd'hui,  je vais vous parler d'un petit projet que je viens de mettre sur les rails.
Je l'ai nommé `Papaye`_, comme l'illustration ci-dessus, mais je ne suis pas certain que cela vous aide vraiment à savoir de quoi il s'agit.

C'est tout simplement d'une ré-implémentation du dépôt officiel de modules Python (`PyPI pour "Python Packages Index"`_). Le but étant d'avoir son propre dépôt (sur sa machine ou sur son réseau) et d'y stocker des modules qui n'ont rien à faire sur le dépôt officiel, comme des modules construits par l'intégration continue, pas encore prêt à être diffusés ou tout simplement privés (souvent le cas en entreprise). L'avantage, c'est que l'on conservera toute la puissance de "PIP" pour l'installation !

Autre avantage de l'outil c'est de pouvoir travailler en local et installer des modules sans dépendre du réseau. Et oui, `Papaye`_ fait également office de proxy et de cache pour PyPI ! 

Implémenter ce genre de dépôt n'a vraiment rien de très complexe, d'autant que la majorité de l'intelligence ce situe côté client (via la commande "pip"). De plus, il existe tout un tas de projets similaires (`localshop`_, `pypiserver`_, `pyshop`_, etc...) mais aucun ne correspondait vraiment à ce que je cherchai. J'ai dans un premier temps pensé à contribuer ou "forker" les projets existants, mais j'avais finalement une vision un peu différente des projets que j'avais sous les yeux (l'impression que je pourrai répondre à mon besoin de façon plus simple) et j'avais envie d'un peu ... d'expérimentations! J'ai finalement décidé de commencer le mien "from scratch".

Pourquoi "Papaye" ?
===================

Comme d'habitude, une jeux de mot "tout pourri". Le dépôt officiel s'appelant "PyPI" et étant assez mauvais en anglais et ne n'ai absolument aucune idée de comment cela ce prononce. J'ai remarqué que beaucoup de francophone ont tendance à dire "PaïPaï", certainement parce que "pipi", ce n'est pas génial. Alors, finalement, "Papaye", ça y ressemble beaucoup et il y a une vraie signification en français. J'aime bien l'idée de dire que c'est pareil que le dépôt officiel, mais finalement, un peu différent.

Et puis "Papaye" c'est parfait. Un point c'est tout !

Objectifs
=========

Avant de réaliser ce petit bricolage, voici les objectifs que je me suis fixés :

- **être le plus simple possible à mettre en place**. Si possible, que du Python et tout installable via la commande "PIP". Je ne voulais vraiment pas qu'un utilisateur se tape un manuel d'installation de 45 pages pour installer l'outil sachant que je voulais qu'on puisse utiliser l'application aussi bien depuis un serveur qu'en local sur son poste. Je n'aurais pas supporté de perdre patience lors de la mise en place de ma propre application !
- **ne dépendre d'aucun service externe**. Donc toute sorte de bases de données sous forme de service à démarrer et à configurer avant d'installer l'application n'est pas envisageable. Tous le monde ne sait pas comment cela fonctionne.
- **tout doit se lancer en une seule ligne de commande**. Je passe mon temps à taper plein de commandes en tout genre tout au long de la journée, une seule pour lancer l'application ça suffit largement !
- **faire office de proxy vers le dépôt officiel**. Comme ça je fais toujours pointer les fichiers de configuration de "PIP" vers mon instance Papaye qui, elle, redispatchera au bon endroit, c'est beaucoup moins prise de tête à gérer.
- **faire office de cache local pour le dépôt officiel**. Une panne réseau ? PyPI non disponible ? On continue de bosser !  
- **tailler pour des dépôt de petite et moyenne taille**. Pour le moment, ne soyons pas trop ambicieux .

Présentation technique
======================

Là encore, rien de bien compliqué :

- `Python`_. À bon ? Cela vous surprend toujours ?
- `Pyramid`_. Mon framework web favoris. J'aime vraiment beaucoup ce framework, mais je n'avais jamais eu l'occasion de mener un projet de bout en bout avec (le syndrome du "projet perso", le projet qu'on commence, qui révolutionnera le monde mais, qui ne sortira jamais par manque de temps :-p). Au boulot, c'est plutôt `Django`_.
- `ZODB`_. La base de données NOSQL assez surprenante qui permet de stocker des objets Python directement. C'est ce que j'ai trouvé de plus simple et de plus pythonique pour faire du `traversal`_ avec Pyramid. Si le mode traversal ne vous dit rien, je vous invite à lire la documentation de Pyramid à ce sujet. C'est vraiment une façon intéressante de concevoir une application web qui tranche radicalement avec l'`url dispatch`_. Les deux méthodes sont `combinées`_ dans Papaye.
- `Beaker`_. Pour mettre en cache les réponses venant du dépôt officiel.

Installation
============

Comment installer et faire tourner Papaye en quelques commandes :

.. code-block:: shell-session

    pip install papaye
    wget https://raw.githubusercontent.com/rcommande/papaye/master/production.ini
    papaye_init production.ini
    pserve production.ini

il ne reste plus qu'à vérifier que le serveur nous réponde (par défaut, à l'adresse http://localhost:6543/simple) 

On peut maintenant l'utiliser avec PIP :

.. code-block:: shell-session
    
    pip install -i http://localhost:6543/simple numpy

C'est tout de même plus pratique de configurer le dépôt de façon définitive plutôt que de devoir le préciser à chaque fois. Ça se passe dans le fichier ~/pip.conf. Il suffit d'ajouter la ligne suivante :


.. code-block:: ini

    [install]
    index-url = http://localhost:6543/simple

Ensuite, pour pouvoir envoyer vos modules dans votre instance Papaye, il faut éditer le fichier ~/.pypirc :

.. code-block:: ini

    [distutils]
    index-servers =
        papaye

    [papaye]
    username: <admin>
    password: <password>
    repository: http://localhost:6543/simple

Et pour finir, pour envoyer notre module sur le dépôt :

.. code-block:: shell-session

    cd /chemin/vers/votre/module
    python setup.py sdist upload -v -r papaye

    
Conclusion
==========

Pour le moment seul l'interface "simple" a été implémenté. Ce n'est pas super sexy mais c'est le minimum pour pouvoir fonctionner avec **PIP** et **Setuptools**. En revanche, les fonctions de recherche (commande "pip search <pattern>) ne fonctionneront pas (j'ai manqué d'avoir une crise cardiaque quand j'ai vu que "PIP" communiquait avec le dépôt officiel en XML-RPC ...).

La prochaine étape, c'est une interface pour pouvoir naviguer dans les modules, car, pour le moment, c'est un peu une boite noire et ça ne vend pas du rêve.

Voilà pour mon petit projet du moment. Surtout n'hésitez pas à me faire parvenir vos retours / critiques / contributions / idées / cadeaux / bisous  (rayer les mentions inutiles).

Plus d'infos ? `C'est par ici`_

.. _Papaye: https://github.com/rcommande/papaye
.. _Python: http://www.python.org
.. _Pyramid: http://www.pylonsproject.org/
.. _ZODB: http://www.zodb.org/en/latest/
.. _Beaker: http://beaker.readthedocs.org/en/latest/
.. _traversal: http://docs.pylonsproject.org/projects/pyramid/en/latest/narr/traversal.html
.. _combinées: http://docs.pylonsproject.org/projects/pyramid/en/latest/narr/hybrid.html
.. _PyPI pour "Python Packages Index": http://pypi.python.org/
.. _Django: https://www.djangoproject.com/
.. _url dispatch: http://docs.pylonsproject.org/projects/pyramid/en/latest/narr/urldispatch.html
.. _localshop: https://github.com/mvantellingen/localshop
.. _pypiserver: https://github.com/schmir/pypiserver
.. _pyshop: https://github.com/mardiros/pyshop
.. _C'est par ici: https://github.com/rcommande/papaye

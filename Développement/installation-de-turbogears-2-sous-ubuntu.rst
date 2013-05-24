Installation de Turbogears 2 sous Ubuntu
########################################
:date: 2010-04-28 10:34
:tags: Programmation, Planet-Libre, Python, Turbogears, Ubuntu

Un petit billet rapide pour présenter l'installation de Turbogears 2.0 sous Ubuntu, ayant eu quelques difficultés.

Pour ceux qui ne savent pas ce qu'est Turbogears, demandons à `wikipedia`_:

    TurboGears est un framework orienté Web/Ajax et MVC basé sur des
    templates , des plugins écrits en Python.

Très chère amie, merci pour cette intervention :-).
Tout d'abord, sur Ubuntu, il faut savoir que c'est la version 2.6 de Python qui est installée par défaut. A ce sujet, voici ce que dit la documentation de TG:

    TurboGears works with any version of python between 2.4 and 2.6. The
    most widely deployed version of python at the moment of this writing
    is version 2.5. Both python 2.4 and python 2.6 require additional
    steps which will be covered in the appropriate sections. Python 3.0
    is currently unsupported due to lack of support in many of our
    upstream packages.

Bon en gros, ça doit marcher sur Python 2.4, 2.5 et 2.6 mais, pour la version 2.6, il y a une étape en plus qui sera traitée dans un section adéquate. Malheureusement, je n'ai jamais trouvé cette section dans la documentation...

On va donc installer Turbogears avec la version 2.5 de Python et, oh miracle! Ça fonctionne!

On commence déjà par installer python 2.5 avec le gestionnaire de paquets de la distribution:

.. code-block:: sh

    sudo aptitude install python2.5 python2.5-dev python-virtualenv

Ensuite, on va créer un environnement virtuel python pour notre installation de Turbogears, histoire d'éviter tout conflit avec les modules installés sur le système. Je l'appelerais, comme la documentation officielle, "tg2env":

.. code-block:: sh

    virtualenv --no-site-packages -p python2.5 tg2env
    cd tg2env
    source bin/activate

Normalement, si tout c'est bien passé, "(tg2env)" devrait apparaitre devant chaque ligne du prompt pour informer qu'on est bien dans l'environnement virtuel.

Passons au chose sérieuses, installons Turbogears:

.. code-block:: sh

    easy_install -i http://www.turbogears.org/2.0/downloads/current/index tg.devtools

L'installation est automatique et va prendre quelques 10ène de secondes.  Quand le shell rend la main, c'est bon,Turbogears 2.0 est correctement installé.

On peut, tout de même, valider l'installation:

.. code-block:: sh

    (tg2env)$ paster --help

doit renvoyer l'aide de la commande paster.

.. code-block:: sh

    Usage: paster [paster_options] COMMAND [command_options]

    Options:
      --version         show program's version number and exit
      --plugin=PLUGINS  Add a plugin to the list of commands (plugins are Egg
                        specs; will also require() the Egg)
      -h, --help        Show this help message

    Commands:
      create       Create the file layout for a Python distribution
      help         Display help
      make-config  Install a package and create a fresh config file/directory
      points       Show information about entry points
      post         Run a request for the described application
      request      Run a request for the described application
      serve        Serve the described application
      setup-app    Setup an application, given a config file

    TurboGears2:
      quickstart   Create a new TurboGears 2 project.
      tginfo       Show TurboGears 2 related projects and their versions

J'ai tester l'installation de Turbogears 2.1b1 avec Python 2.6.  Visiblement aucun problème avec cette future version.

Maintenant que tout est en place: a vos marques, prêts,..codez!!!! (mais pas moi, faut d'abord que je bouffe la doc :-p)

.. _wikipedia: http://fr.wikipedia.org/wiki/Turbogears

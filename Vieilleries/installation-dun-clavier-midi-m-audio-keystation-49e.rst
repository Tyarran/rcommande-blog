Installation d'un clavier midi M-Audio keystation 49e
#####################################################
:date: 2008-02-25 16:47
:tags: MAO

Nous allons maintenant voir comment installer un clavier midi maître M-audio keystation 49e sous linux. Ce n'est pas vraiment compliqué, mais j'ai eu quelques dificultées avec la compilation d'un des logiciel (fxload) mais visiblement je ne suis pas le seul.

.. figure:: http://www.unblogsurlabanquise.org/images/keystation_emballagep.jpg
   :align: center
   :alt:

Premièrement il faut télécharger le driver MidiSport. Comme l'indique le fichier README, ce driver fonctionne aussi bien pour:

.. code-block:: sh

    Supported devices:
        MidiSport 1x1
        MidiSport 2x2
        MidiSport 4x4
        MidiSport 8x8
        KeyStation (old models: 49, 61)
        Oxygen
        Radium49
        Radium61
        Uno

Pour fonctionner correctement il faut "fxload". Et c'est la seule petite dificulté de l'installation surgit : sur Archlinux, fxload est disponible sur AUR mais impossible de le compiler. J'ai téléchargé les sources directement depuis le site officiel mais la encore, impossible de les compiler.

Une petite recherche sur le forum francophone d'Arch m'a permis de trouver la solution: il suffit de télécharger le packet RPM disponible sur le site officiel et d'y extraire l'excutable à l'aide de rpmextract.
Bon aller je suis gentil je vous fournis le fichier déjà extrait: `ici`_
Pour installer fxload c'est très simple:

.. code-block:: sh

    sudo cp /chemin_ou_le_fichier_a_été_téléchargé/fxload /usr/bin/fxload

On passe maintenant à l'installation de midisport. Dans un terminal:

.. code-block:: sh

    wget http://ovh.dl.sourceforge.net/sourceforge/usb-midi-fw/midisport-firmware-1.2.tar.gz tar xvf midisport-firmware-1.2.tar.gz cd midisport-firmware-1.2 ./configure make make install    #en super utilisateur

Si tous se passe sans problème, bravo vous avez installer votre clavier midi. Il ne reste plus qu'a l'allumer, à demarrer Rosegarden et à commencer à enregistrer vos chefs d'oeuvres.

|image0|
|image1|

.. _ici: http://minimumserious.free.fr/Files/fxload
.. _|image2|: http://www.unblogsurlabanquise.org/images/bureau.jpg
.. _|image3|: http://www.unblogsurlabanquise.org/images/rosegarden1.png

.. |image0| image:: http://www.unblogsurlabanquise.org/images/bureaup.jpg
.. |image1| image:: http://www.unblogsurlabanquise.org/images/rosegarden1p.png
.. |image2| image:: http://www.unblogsurlabanquise.org/images/bureaup.jpg
.. |image3| image:: http://www.unblogsurlabanquise.org/images/rosegarden1p.png

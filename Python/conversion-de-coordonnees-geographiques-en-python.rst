Conversion de coordonnées géographiques en Python
#################################################
:date: 2011-01-20 22:19
:tags: Programmation, Coordonnées géographique, Développement, Google map, informatique, Proj4, pyproj, Python

.. role:: strike
    :class: strike

Au boulot, j'ai eu à convertir des coordonnées géographiques exprimées en Lambert III carto vers du WGS84 afin de les utiliser avec l'API Google Map.
J'ai eu du mal à trouver comment faire, les algos disponibles sont beaucoup trop complexes pour un non mathématicien comme moi.

Je suis finalement tombé sur une bibliothèque écrite en C, publié sous licence MIT, dédiée à cette tâche: Proj4. Par chance, il existe un binding en Python afin de l'exploiter: pyproj. C'est que du bonheur!

L'installation
--------------

C'est assez simple, cela fonctionne aussi bien sur Windows ou Linux à condition de disposer d'un compilateur. Comme :strike:`toujours` très souvent, l'installation de pyproj s'installe avec la commande easy\_install:

.. code-block:: text

    easy_install pyproj

Un petit test dans le shell interactif pour voir si cela fonctionne correctement:

.. code-block:: py

    import pyproj

La conversion
-------------

Je ne vais pas détailler ici le fonctionnement du module parce ce n'est pas le but, et de toute façon je n'ai fait que l'effleurer très rapidement.

.. code-block:: py

    import pyproj
    lat, lon = 656936.8, 3042238.0
    wgs84 = pyproj.Proj('+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs')
    lambert = pyproj.Proj('+proj=lcc +nadgrids=ntf_r93.gsb,null +towgs84=-168.0000,-60.0000,320.0000 +a=6378249.2000 +rf=293.4660210000000 +pm=2.337229167 +lat_0=44.100000000 +lon_0=0.000000000 +k_0=0.99987750 +lat_1=44.100000000 +x_0=600000.000 +y_0=3200000.000 +units=m +no_defs')
    x, y = pyproj.transform(lambert, wgs84,lat,lon)

Pour les paramètres à passer aux constructeurs, je suis basé sur ce qui a été fourni par l'IGN `ici`_
Voila, en espérant que cela vous dépannera ^^

.. _ici: http://code.google.com/p/pyproj/source/browse/trunk/lib/pyproj/data/IGNF?spec=svn162&r=162

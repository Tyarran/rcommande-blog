Pourquoi il ne faut pas utiliser les paquets Python de sa distibution dans son projet Python 
############################################################################################
:date: 2014-02-04
:tags: Programmation, Python, Distribution, Linux, Packaging
:Status: draft

.. role:: strike
    :class: strike


Un billet en réaction à ce que j'ai pu voir ou entendre. Je vais tenté de vous convaincre qu'il n'est pas une bonne idée d'utiliser des paquets Python venant des distributions (donc installé via APT pour Debian, Pacman pour ArchLinux, etc...) dans son projet Python.

Les paquets des distributions sont facile à installer
-----------------------------------------------------

Effectivement, les paquets des distributions sont en général très simple d'installation. Ça permet d'avoir un système propre. Ils sont également l'avantage de proposer des versions binaires des applications, ce qui évite la phrase fastidieuse de la compilation.

En revanche, ces paquets ne sont pas dans un format Pythonique. Ce n'est pas un soucis en soit, car les Package Manager des distributions sont bien plus avancés que les outils Python. Mais là ou il faut y voir un petit point noir, c'est que le paquet de la distribution à subit une transformation: le repackaging. Sans cela, impossible de le faire fonctionner sur la distribution. Conclusion, notre application d'origine à été **modifié** afin de rentrer dans le moule de la distribution. Globalement, on retrouvera la même chose, mais il se peut qu'il y est des fichiers en plus pour contenir des métadonnées supplémentaires, des fichiers réoganisés pour coller avec les conventions de la distribution, etc.
Il faut donc retenir que ce n'est plus vraiment notre application Python tell que nous l'avons connu initialement puisqu'il a été légèrement modifié.

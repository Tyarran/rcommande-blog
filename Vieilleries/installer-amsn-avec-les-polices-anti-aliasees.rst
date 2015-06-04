Installer AMSN avec les polices anti-aliasées
#############################################
:date: 2007-04-17 16:46
:tags: Linux

|image0|

La version SVN d'AMSN fait un grand pas en avant point de vue beauté de l'interface malgré l'utilisation de la librairie TK qui est, [STRIKEOUT:pour beaucoup] à mon goût, tout sauf esthétique. En revanche, les polices , elles, gardent toujours leurs "effets d'escalier" (aliasing) qui gâchent l'aspect général.

Mais il y a une solution! Il suffit d'utiliser les librairies tcl/tk8.5 qui gèrent l'anti-aliasing. Mais attention tout de même, leur developpements ne sont pas encore terminés.

Pour réaliser ce billet je me suis basé sur la page du wiki d'ubuntu-fr dédié à l'installation d'amsn auquel j'ai apporté quelques petites modifications.

Pour commencer, nous allons télécharger les sources de tcl et décompresser l'archive:

.. code-block:: sh

    cd
    wget http://dfn.dl.sourceforge.net/sourceforge/tcl/tcl8.5a5-src.tar.gz
    tar xfvz tcl8.5a5-src.tar.gz #cd tcl8.5a5/unix/

Maintenant la compilation se fait de façon assez classique. C'est à dire l'utilisation de ./configure + paramettres, make et make install:

.. code-block:: sh

    ./configure --prefix=/usr/local/tcl --enable-threads #make #sudo make install

Si tout c'est bien déroulé, bravo! Vous venez de compiler et d'installer tcl8.5.

.. note::

    Petite precision générale: il est préférable DE NE PAS LANCER"./configure" et "make" en tant que super-utilisateur. Par contre pour "make install" c'est obligatoire d'où l'apparition de "sudo" en début de commande.

On fait de même pour TK:

.. code-block:: sh

    cd
    wget http://prdownloads.sourceforge.net/tcl/tk8.5a5-src.tar.gz
    tar xfvz tk8.5a5-src.tar.gz
    cd tk8.5a5/unix/

Et on compile:

.. code-block:: sh

    ./configure --with-tcl=/usr/local/tcl/lib --prefix=/usr/local/tk --enable-xft --enable-threads
    make
    sudo make install

Encore une fois, si tout c'est bien passé c'est que vous venez de compiler/installer TK8.5 correctement.  Il ne nous reste plus qu'à télécharger la version SVN d'Amsn:

.. code-block:: sh

    cd
    wget http://www.amsn-project.net/amsn_dev.tar.gz
    tar xfvz amsn_dev.tar.gz
    cd msn/

On la compile en indiquant bien les versions de TCL/TK que l'on veut utiliser:

.. code-block:: sh

    ./configure --with-tcl=/usr/local/tcl/lib --with-tk=/usr/local/tk/lib
    make
    sudo make install

Pour terminer, il faut activer l'anti-aliasing dans amsn:
Pour les gnomistes:

.. code-block:: sh

     gksudo gedit /usr/share/amsn/amsn

Pour les utilisateurs de kde:

.. code-block:: sh

    kdesu krite /usr/share/amsn/amsn

 Remplacez la 3ème ligne par:

.. code-block:: sh

    exec /usr/local/tk/bin/wish8.5 $0

On sauvegarde ,on quitte et le tour est joué |;-)|

.. |image0| image:: http://images.linspire.com/application/ams/188910/0_95_0_0_0_50_linspire0_1_0_0_m10_1/amsn.jpg
.. |;-)| image:: http://www.unblogsurlabanquise.org/themes/default/smilies/wink.png

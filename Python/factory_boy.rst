Les tests avec Factory_boy
==========================
:date: 2013-07-25
:tags: Programmation, factory_boy, Développement, Python, tests, fixtures

.. role:: strike
    :class: strike

Les données de test: les fixtures
---------------------------------

Il est courant de peupler la base de données afin de disposer de tout ce qu'il faut pour éxécuter les tests. Ce n'est pas forcement l'étape la plus drôle d'un développement logiciel, surtout si l'on doit faire ça à la main à chaque fois.

Heureusement pour nous, la majorité des frameworks dignes de ce nom fournissent un mécanisme de fixtures. Difficile d'en donner une définition tellement cela dépend du framework ou des modules utilisés.

Pour Django, il s'agît de données sérialisées en JSON ou XML représentant les objets de modèle que le framework va injecter dans la base de données avant chaque tests.

L'alternative: les fabriques
----------------------------

Il existe une alternative aux fixtures, les fabriques. Pour les personnes qui ne sont pas à l'aise avec ce pattern, il y a même `une page qui lui est dédiée`_. Personnellement je préfère leurs usages aux fixtures car:

* Les fabriques, c'est du code! Et en tant que développeur, c'est ce que je préfère manipuler.

* Plus de fichiers de JSON ou XML dans tous les sens, je met en général toutes mes fabriques dans un fichier "factories.py" à la racine de mon projet ou du module de tests. C'est bien plus simple pour s'y retrouver.

* Les tests sont plus lisibles. Pour un test, je n'instancie que les objets dont j'ai besoin.

* Le modèle va :strike:`peut être` changer. C'est plus simple de mettre à jour une fabrique de quelques lignes plutôt que des données sérialisées éparpillées dans différents fichiers.

Factory_boy
------------

Je vais donc présenter un petit module Python bien pratique pour simplifier la mise en oeuvre des tests en utilisant des fabriques `Factory_boy`_. Il a été développé par `Mark Sandstrom`_ et est actuellement maintenu par `Raphaël Barrois`_.
Il ne s'agit que d'une boite à outils permettant d'écrire très facilement des fabriques. Pour cela, factory_boy fourni différents outils dont les plus utiles sont:

* Les `séquences`_, qui permettent d'instancier des objets avec des attributs différents à chaque instance.
* Les Attributs paraisseux (ou `LazyAttributes`_) dont la valeur ne sera évaluée qu'au dernier moment et, qui pouront donc être déterminés en fonction d'autres attributs.
* L'héritage de fabriques. Bien connu des :strike:`fénéants` développeurs à l'aise avec la programmation objet permettant d'en faire plus en écrivant moins.
* Les sous-fabriques (ou SubFactories) permettant d'instancier des objets liés entre eux via des clés étrangères.
* Certainement plein d'autres choses que je vous laisse découvrir.

Nativement, Factory_boy fonctionne avec Django et Mogo (MongoDB). J'ai donc effectuer une petite contribution au projet pour le rendre compatible avec mon ORM préféré: `SQLAlchemy`_. Tout cela est disponible depuis la version 2.1.

J'ai découvert ce petit projet au boulot sur un projet Django (merci `Axel`_ !). Maitenant cela fait parti de ma petite boite à outils indispensables à tout projet Python.

Factoy_boy en action
--------------------

Pas besoin de longs discours. Voici un exemple simple permettant de mettre en place `Factory_boy`_ avec `SQLAlchemy`_:

.. code-block:: sh

    pip install factory_boy sqlalchemy

Comme je suis un :strike:`flémard` informaticien, j'ai repris directement l'exemple de la documentation (rédigé par mes soins, alors je suis a moitier pardonné) que je vais :strike:`bricoler` commenter:

.. code-block:: python

    from sqlalchemy import Column, Integer, Unicode, create_engine
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy.orm import scoped_session, sessionmaker

    session = scoped_session(sessionmaker())
    engine = create_engine('sqlite://')
    session.configure(bind=engine)
    Base = declarative_base()


Rien de particulier ici, on change tout ce qu'il faut pour faire fonctionner notre projet avec `SQLAlchemy`_ en utilisant une base de données `SQLite`_ en mémoire.

Je créé aussi une session `SQLAlchemy`_, c'est super important de l'avoir sous le coude étant donné qu'il faudra la communiquer à notre fabrique par la suite (c'est spécifique au fonctionnement de `SQLAlchemy`_)

.. code-block:: python

    class User(Base):
        """ A SQLAlchemy simple model class who represents a user """
        __tablename__ = 'UserTable'

        id = Column(Integer(), primary_key=True)
        name = Column(Unicode(20))

    Base.metadata.create_all(engine)

C'est notre objet de modèle pour la démonstration. Oui, je sais, je ne me suis pas foulé. C'est juste une classe qui représente un utilisateur avec un identifiant et un nom...

.. code-block:: python

    class UserFactory(SQLAlchemyModelFactory):
        FACTORY_FOR = User
        FACTORY_SESSION = session   # the SQLAlchemy session object

        id = factory.Sequence(lambda n: n)
        name = factory.LazyAttribute(lambda a: 'User {0}'.format(a.id))


Bon voila ce l'on attendait: la fabrique. C'est une classe qui hérite de SQLAlchemyModelFactory, la classe de base à toute fabrique utilisant `SQLAlchemy`_. Il est possible choisir celle qui conviendra en fonction de l'ORM utilisé (DjangoModelFactory, MogoFactory).

Détaillons un peu les attributs:

*  **FACTORY_FOR**: on précise quel type objet sera créé par la fabrique. Ici on veut des instances de "User".
*  **FACTORY_SESSION**: ça c'est du spécifique `SQLAlchemy`_. Il faut passer `l'objet de session SQLAlchemy`_ que vous voulez utiliser pour communiquer avec votre base de données.

C'est fini pour la configuration, place au fonctionnel de la fabrique:

*  **id**: dans cette exemple, l'attribut "id" est une séquence, c'est à dire qu'il sera à chaque création d'instance de "User". Cela prend ne paramètre un fonction appelé pour constuire le contenu. Ici, on utilise une fonction lambda qui ne fait que renvoyé l'incrément 'n'.

Le premier aura donc l'id "1", le second "2", etc... À noter que cela prend en compte les éléments déjà présents en base en commançant à partir du dernier id. Pratique !

*  **name** est un attribut paraisseux (LazyAttribute), c'est à dire qu'il sera évalué au dernier moment et peut donc s'appuyer sur d'autres attributs pour être calculé.
   Là, le nom sera calculé en fonction de l'identifiant. On obtiendra donc 'User 1', 'User 2', etc...


A l'usage, notre nouveau joujou n'est vraiment pas contraignant à manipuler:

.. code-block:: pycon

    >>> session.query(User).all()  # Je triche pas et il n'y a rien dans la base de données
    []

    >>> UserFactory()  # Je veux un utilisateur. UserFactory, à la rescousse!
    <User: User 1>

    >>> session.query(User).all()  # Tadaaaa! L'utilisateur est bien en base!
    [<User: User 1>]


    >>> UserFactory.build()  # Parfois on veut l'instance, mais pas besoin de la mettre en base
    [<User: User 2>]

    >>> session.query(User).all()  # Non je ne ment pas! Toujours un seul utilisateur en base
    [<User: User 1>]

    >>> user = UserFactory(name='Romain')  # Parfois, on a besoin d'attributs spécifiques
    >>> user.name
    'Romain'


Ce n'est qu'une très petite introduction. J'invite tous les curieux à se rendre sur `la doc du projet factory_boy`_ pour voir toute les possibilités qu'offre ce module bien sympa.

.. _Mark Sandstrom: https://github.com/dnerdy
.. _Raphaël Barrois: https://github.com/rbarrois
.. _une page qui lui est dédiée: https://fr.wikipedia.org/wiki/Fabrique_%28patron_de_conception%29
.. _l'objet de session SQLAlchemy: http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html
.. _la doc du projet factory_boy: https://factoryboy.readthedocs.org/en/latest/
.. _Factory boy: https://github.com/rbarrois/factory_boy
.. _Axel: http://www.noirbizarre.info
.. _SQLAlchemy: http://www.sqlalchemy.org/
.. _SQLite: http://www.sqlite.org/
.. _LazyAttributes: https://factoryboy.readthedocs.org/en/latest/#lazy-attributes
.. _séquences: https://factoryboy.readthedocs.org/en/latest/#sequences

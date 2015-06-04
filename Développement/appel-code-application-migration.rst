N'appelez pas votre code applicatif dans vos migrations!
########################################################
:date: 2014-02-06
:tags: Programmation, Développement, Python, migrations, django, south

.. role:: underline
    :class: underline

.. image:: http://static.commentcamarche.net/www.commentcamarche.net/faq/images/4675-istock-000008915796xsmall-s-.png

Un billet en réaction de ce que j'ai pu voir sur un projet cette semaine.

Suite à une refonte applicative assez importe, j'ai eu besoin de supprimer un bon morceau de code devenant maintenant obsolète.

Après avoir supprimer tout un pan de l'historique de l'application (j'ai presque versé une larme...) et mis à jour les tests unitaires qui m'ont beaucoup aidé à faire le nettoyage dans les règles de l'art, j'étais assez confiant.

Sur mon poste, tout semblait fonctionner correctement:

* **Code**: OK
* **Tests unitaires**: OK
* **Tests fonctionnels**: OK

Malgré cela, la plateforme d'intégration continue n'était pas contente du tout. Où est-ce que ça plante? Sur une vielle migration de données qui utilisait le code retiré! Pour détailler un petit peu le contexte ici, il s'agit d'une application `Django`_ utilisant `South`_ pour gerer les migrations de schéma de données. Et oui, les migrations sont du code "one shot" et ne sont pas couvert par les tests unitaires.

Alors s'il vous plaît, n'utiliser pas le code de votre applicatif dans vos migrations car:

* **Un code peut-être supprimé**, on viens de le voir. Hors de question de conserver du code (presque) mort rien que pour faire tourner une migration.
* **Un code peut-être modifié**, et là, :underline:`c'est encore pire`! Il se peut que votre migration n'est plus le comportement attendu.

Essayer de rendre vos migrations les plus independantes possible, ça vous évitera de belles surprises! Dans mon cas, ce fût long et fastidieux mais j'ai pris la décision d'intégrer en dûr le code "historique" dans la migration.

La conclusion du jour, c'est que parfois, supprimer du code, c'est plus complexe que d'en écrire.


.. _Django: https://www.djangoproject.com/
.. _South: http://south.aeracode.org/

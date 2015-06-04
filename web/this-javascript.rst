Comprendre le mot clé "this" en Javascript
==========================================
:date: 2014-01-29
:tags: Programmation, Web, Développement, Javascript, this
:Status: draft

.. role:: strike
    :class: strike

Je vais m'écarter un peu du Python le temps d'un billet et aborder sujet autour du langage Javascript. Qu'on l'aime ou pas, il est devenu incontournable. Cela fait bien longtemps qu'il est sorti de son domaine de prédilection: un petit language de script embarqué dans le navigateur. On peut maintenant développer coté serveur (`NodeJS`_), faire des extensions système (`Gnome 3`_), faire des requètes complexes dans une base de données (`MongoDB`_), etc ...

Beaucoup de développeurs finissent, un jour, par s'y frotter mais lorsqu'on viens d'un autre langage, il y a beaucoup d'habitudes à perdre (ou de nouvelles à prendre, c'est une question de point de vue). Je crois également que je n'ai jamais travaillé dans une entreprise où je n'ai pas entendu un développeur "râler" sur ce langage (je l'ai :strike:`peut être` fait moi même). Cette réaction est :strike:`presque` normal étant donné que ce langage met à mal énormémment de certitudes aquisent au cours d'une expérience de développeur. La fulgurante évolution de ce langage ainsi que son histoire assez atypique y est certainement pour beaucoup.

Essayer d'appliquer les "code patterns" d'un autre langage est donc une erreur. Souvent, sur le papier, tout semble coller, mais la pratique est autre ...

C'est le cas du mot clé "this" qui ne fonctionne pas tout à fait comme ailleurs et qui peut réserver quelques surprises.

Voici quelques petits cas d'usage que j'ai pu identifier.

Notre base de travail
#####################
Partons du code suivant:

.. code-block:: javascript

    <!DOCTYPE html>
    <html>
        <head>
            <title>Test "this"</title>
            <meta charset="UTF-8">
        </head>
        <body>
            <input type="button" value="Bouton inutile" />
        </body>
        <script type="text/javascript" src="http://code.jquery.com/jquery-2.1.0.min.js" ></script>
        <script type="text/javascript">
        (function($) {

            function test1() {
                console.log(this);
            }

            function test2() {
                'use strict';
                console.log(this);
            }

            var Obj = function() {
            };


            Obj.prototype._test3 = function() {
                console.log(this);
            };

            Obj.prototype._test4 = function() {
                'use strict';
                console.log(this);
            };

            Obj.prototype._test5 = function() {
                $('input').each(function(index, value) {
                    console.log(this);
                });
            };

            Obj.prototype._test6 = function() {
                (function() {
                    console.log(this);
                })();
            };

            Obj.prototype._test7 = function() {
                (function() {
                    'use strict';
                    console.log(this);
                })();
            };

            Obj.prototype._test8 = function() {
                (function() {
                    console.log(this);
                }).apply({name: "Javascript"});
            }

            Obj.prototype._test9 = function() {
                var proxy = $.proxy(function() {
                        console.log(this);
                    }, {name: "Romain", lastname: "Commandé"});

                proxy();
            }

            var obj = new Obj();
            test1();
            test2();
            obj._test3();
            obj._test4();
            obj._test5();
            obj._test6();
            obj._test7();
            obj._test8();
            obj._test9();

        })(jQuery);
        </script>
    </html>

Ce n'est pas forcement très beau, mais je voulais qu'il soit directement utilisable afin de permettre aux septiques de tester à leur guise. C'est une page blanche qui n'a pour contenu qu'un "input hidden" et qui possède un code Javascript qui nous servira d'effectuer 5 tests (méthodes test1() à Obj._test5()). À chaque test, on regardera le contenu de la variable "this". Le code est donc à executer dans un navigateur avec Firebug (ou équivalent en fonction de vos habitudes) pour constater les résultats.

Voici les résultats de ce petit morceau de code:

.. figure:: {filename}/pictures/this-result.png
    
    Les resultats de nos tests.

Aïe ! On constate que "this" n'a pas toujours la même valeur . Si ce résultat vous surprend, la suite devrait vous interesser . Sinon, vous avez au moins le même niveau que moi en Javascript, passez donc votre chemin .

Que signifie "this" ?
#####################

Dans la majorité des langages de programmation objet, "this" est une référence l'instance de objet en cours. En Python c'est pareil même si le mot clé "this" n'existe pas. L'équivalent est **le premier argument passé à une méthodes** appelé par convention "self".

En Javascript, on pourrait définir le mot clé "this" comme **la référence vers l'objet sur lequel la fonction en cours est rattachée**. Et là, ça change vraiment tout, ou presque.

Analyse des résultats
#####################

Regardons les résultats des différents tests un à un.


Test1
-----

.. code-block:: javascript

    function test1() {
        console.log(this);
    }

On commence par une simple fonction. On regarde le contenu de **this** et celui-ci est égal à l'objet **window**. Rien de bien étonnant aux vues de la définition du mot clé qu'on a vu précédement. La fonction **test1** est dans l'espace global de notre projet, elle est donc rattachée à l'objet **window**.

Test2
-----

.. code-block:: javascript

    function test2() {
        'use strict';
        console.log(this);
    }

On refait la même chose mais cette fois-ci on rajoute le **strict mode**. Et la, **this** vaut **undefined**. Ça, on ne l'avais pas prévu et, personnement, je n'ai pas d'explication. Il va faloir ce contenter de retenir ce résultat pour ne pas être surpris tôt ou tard.





Test1
-----

.. code-block:: javascript

    Toto.prototype._test1 = function() {
        this._log(this);
    };

Là, rien de particulier. On appel la méthode et on observe que "this" vaut bien l'instance de "Toto". On s'attendait clairement à ce résultat.

Test2
-----
.. code-block:: javascript

    Toto.prototype._test2 = function() {
        $('input').each(function(index, value) {
            Toto._log(this);
        });
    };

Cette fois-ci, "this" vaut l'input de la page. Pourquoi? Parce que jQuery change le contexte (et donc la valeur de "this") pour y placer la valeur de l'itération en cours. C'est tout de même bien pratique mais ça peut réserver des surprise parce que jQuery le fait sans trop nous prévenir.

D'ailleur, pour faire fonctionner l'exemple j'ai du passer par "Toto._log(this)" et non "this._log(this)". "this" pointant maintenant vers l'input, je me serais mangé une belle erreur.

En rien rien de magique, jQuery ne fait qu'appeler la méthode "apply()" de la function passé en paramètre de "each()", mais je n'en dit pas plus pour le moment, je vais revenir sur "apply()" un peu plus loin.

Retenons simplement que jQuery change de contexte pour nous simplifier (a priori) la vie.

Test3
-----

.. code-block:: javascript

    Toto.prototype._test3 = function() {
        (function() {
            Toto._log(this);
        })();
    };

Résultat inattendu: "this" vaut l'objet "window". Remarquez ici l'usage d'une fonction anonyme. Et bien ce qu'il faut savoir que celle-ci sera rattachée à l'objet "window" importe qu'elle soit déclarées ici ou ailleurs. C'est comme ça, et c'est puis c'est tout!

Test4
-----

.. code-block:: javascript

    Toto.prototype._test4 = function() {
        this._test1.apply({name: "Javascript", _log: this._log});
    }

On reviens sur le fameux "apply()" spoiler plus haut. "this" ici vaut l'objet "{name: "Javascript", _log: this._log}" malgrés que l'on appel le test1. Mais regardez bien, le test1 n'est pas appelé directement mais via une méthode "apply()" (rappel: une function peut avoir des méthodes puisque c'est elle même un objet, comme en Python).

En fait cette méthode est disponible sur toute les fonctions et permet de les appeler en leur spécifiant un contexte. Simple ,puissant et magique, mais on voit assez rapidement que le mauvais usage peut devenir un vrai bordel!
À savoir qu'on peut également utiliser "call()" à la place de "apply()", c'est un synonyme.

Test5
-----

.. code-block:: javascript

    Toto.prototype._test5 = function() {
        $.proxy(this._test1, {name: "Romain", lastname: "Commandé", _log: this._log})();
    }

Je ne vais pas m'attarder la dessus, c'est plus ou moins la même chose que "apply()" mais à la sauce jQuery. La différence principale c'est que cela n'appel pas directement la fonction cible, mais renvoi une fonction qui wrappe la première pour lui appliquer le nouveau contexte. On pourra donc appeler la fonction plus tard.

Les deux derniers tests mettent bien en évidence un fait: en regardant uniquement la méthode "_test1()" vous ne pouvez garantir la valeur de "this". Tout dépend de comment vous appelez la méthode. Ce qui dérange le plus, c'est que parfois, on ne sait pas comment la méthode est appelée, et boom!


Voila c'est fini. J'espère que ce petit billet permettra à d'autres développeurs d'un peu moins galérer avec ce langage. Si vous voyez d'autres cas d'exemples, n'hésitez pas à les proposer en commentaire, je me ferais une joie de les rajouter (j'utilise moi-même ce billet comme aide-mémoire ^^).

.. _NodeJS: http://www.nodejs.org
.. _Gnome 3: http://www.gnome.org
.. _MongoDB: http://www.mongodb.org

Questions fréquemment posées
----------------------------

__Q__ : _quelle est la performance de Brython par rapport à Javascript, ou par rapport à d'autres solutions qui permettent d'utiliser Python dans le navigateur ?_

__R__ : par rapport à Javascript, le rapport est naturellement très différent d'un programme à l'autre, mais on peut estimer qu'il est de l'ordre de 3 à 5 fois moins rapide. Une console Javascript est fournie dans la distribution ou [sur le site Brython](http://brython.info/tests/js_console.html), vous pouvez l'utiliser pour mesurer le temps d'exécution d'un script Javascript par rapport à son équivalent en Python dans l'éditeur (en décochant la case "debug").

La différence tient à deux facteurs :

- le temps de traduction de Python en Javascript, réalisé à la volée dans le navigateur. Pour donner une idée, le module datetime (2130 lignes de code Python) est parsé et converti en code Javascript en environ 500 millisecondes sur un PC de puissance moyenne.
- le code Javascript généré par Brython doit être conforme aux spécifications de Python, notamment au caractère dynamique de la recherche d'attributs, ce qui dans certains cas conduit à du code Javascript non optimisé.

Par rapport à d'autres solutions de traduction de Python en Javascript, un test est disponible sur [le blog de Pierre Quentel](https://brythonista.wordpress.com/2015/03/28/comparing-the-speed-of-cpython-brython-skulpt-and-pypy-js/) (le créateur et principal développeur de Brython). Il compare Brython, [Skulpt](http://skulpt.org) et [pypy.js](http://pypyjs.org/demo/). Il faut être prudent avec ce genre de comparaison, mais elle montre que pour le code testé, Brython est généralement plus rapide que pypy.js, lui-même plus rapide que Skulpt. Dans certains cas Brython est plus rapide que l'implémentation de référence de Python, CPython.

__Q__ : _il y a des erreurs 404 dans la console du navigateur quand j'exécute des scripts Brython, pourquoi ?_

__R__ : c'est lié à la façon dont Brython gère les imports. Quand un script veut importer le module X, Brython recherche un fichier ou un paquetage dans différents répertoires : la bibliothèque standard (répertoire libs pour les modules en javascript, Lib pour les modules en Python), le répertoire Lib/site-packages, le répertoire de la page courante. Pour cela, il effectue des appels Ajax vers les url correspondantes ; si le fichier n'est pas trouvé, l'erreur 404 apparait dans la console, mais elle est capturée par Brython qui poursuit la recherche jusqu'à trouver le module, ou déclencher une `ImportError` si tous les chemins ont été essayés.

__Q__ : _pourquoi utiliser l'opérateur `<=` pour construire l'arbre des éléments DOM ? Ce n'est pas pythonique !_

__R__ : Python ne possède pas de structure intégrée pour manipuler les arbres, c'est-à-dire pour ajouter des éléments "enfants" ou "frères" à un noeud de l'arbre. Pour ces opérations, on peut utiliser des fonctions ; la syntaxe favorisée par Brython est d'utiliser des opérateurs : c'est plus facile à saisir (pas de parenthèses) et plus lisible

Pour ajouter un "frère" on utilise l'opérateur `+` 

Pour ajouter un descendant, l'opérateur `<=` a été choisi pour les raisons suivantes :

- il a la forme d'une flèche, ce qui indique visuellement une affectation ; l'annotation de fonctions en Python utilise d'ailleurs un nouvel opérateur `->` qui a été choisi pour sa forme de flèche
- le signe `=` en deuxième position fait aussi penser à un assignement augmenté, comme `+=`
- on ne peut pas confondre avec l'opérateur "inférieur ou égal" parce qu'une ligne comme "document <= elt" ne ferait rien s'il s'agissait de "inférieur ou égal" (qui est toujours utilisé dans une condition ou comme valeur de retour d'une fonction)
- on a tellement l'habitude d'interpréter les deux signes `<` et `=` comme "inférieur ou égal" qu'on oublie que c'est une convention des langages de programmation pour remplacer le "vrai" signe `≤`
- en Python, `<=` est utilisé comme opérateur pour les ensembles (classe set) avec une signification différente de "inférieur ou égal"
- le signe `<` est souvent utilisé en informatique pour signifier autre chose de "strictement inférieur" : en Python et beaucoup d'autres langages, `<<` signifie décalage à gauche ; en HTML les balises sont délimitées par `<` et `>`
- Python utilise le même opérateur `%` pour des opérations très différentes : modulo et formattage de chaines

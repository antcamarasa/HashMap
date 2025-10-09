## Hashing  

### Table des matières
### 1. Get to Know the Hash Table Data Structure
- [Hash Table vs Dictionary](#hash-table-vs-dictionary)
- [Hash Table: An Array With a Hash Function](#hash-table-an-array-with-a-hash-function)

### 2. Understand the Hash Function
- [Examine Python’s Built-in hash()](#examine-pythons-built-in-hash)
- [Dive Deeper Into Python’s hash()](#dive-deeper-into-pythons-hash)
- [Identify Hash Function Properties](#identify-hash-function-properties)
- [Compare an Object’s Identity With Its Hash](#compare-an-objects-identity-with-its-hash)
- [Make Your Own Hash Function](#make-your-own-hash-function)

### 3. Build a Hash Table Prototype in Python With TDD
- [Take a Crash Course in Test-Driven Development](#take-a-crash-course-in-test-driven-development)
- [Define a Custom HashTable Class](#define-a-custom-hashtable-class)
- [Insert a Key-Value Pair](#insert-a-key-value-pair)
- [Find a Value by Key](#find-a-value-by-key)
- [Delete a Key-Value Pair](#delete-a-key-value-pair)
- [Update the Value of an Existing Pair](#update-the-value-of-an-existing-pair)
- [Get the Key-Value Pairs](#get-the-key-value-pairs)
- [Use Defensive Copying](#use-defensive-copying)
- [Get the Keys and Values](#get-the-keys-and-values)
- [Report the Hash Table’s Length](#report-the-hash-tables-length)
- [Make the Hash Table Iterable](#make-the-hash-table-iterable)
- [Represent the Hash Table in Text](#represent-the-hash-table-in-text)
- [Test the Equality of Hash Tables](#test-the-equality-of-hash-tables)

### 4. Resolve Hash Code Collisions
- [Find Collided Keys Through Linear Probing](#find-collided-keys-through-linear-probing)
- [Use Linear Probing in the HashTable Class](#use-linear-probing-in-the-hashtable-class)
- [Let the Hash Table Resize Automatically](#let-the-hash-table-resize-automatically)
- [Calculate the Load Factor](#calculate-the-load-factor)
- [Isolate Collided Keys With Separate Chaining](#isolate-collided-keys-with-separate-chaining)

### 5. Retain Insertion Order in a Hash Table
- [Preserve Order While Maintaining Performance](#preserve-order-while-maintaining-performance)

### 6. Use Hashable Keys
- [Hashability vs Immutability](#hashability-vs-immutability)
- [The Hash-Equal Contract](#the-hash-equal-contract)

### 7. Conclusion
- [Wrap-Up and Key Takeaways](#wrap-up-and-key-takeaways)

--- 

### Introduction
Inventée il y a plus d'un demi siècle, la table de hachage(hash table) est une structure de données classique, fondamentale en programmation. 

Encore aujourd'hui, elle permet de résoudre de nombreux problème concrets, comme :
- l’indexation des tables d’une base de données,
- la mise en cache de valeurs calculées,
- ou encore l’implémentation d’ensembles (sets).

Certains languages possède leur propre table de hachage intégrée, il est indispensable de comprendre comment fonctionne une table de hachage en interne.

On va voir :
- Quelle est la différence entre une table de hachage et un dictionnaire
- Comment implémenter une table de hachage en python depuis zéro.
- Comment gérer les collision de hachage et autres difficultés
- Quelles sont les caractéristiques souhaitables d'une fonction de hachage
- Comment la fonction hash() fonctionne en coulissses

### Découvrir la structure de données "Table de hachage"
Avant d'aller plus loin, il est utile de se familiariser avec le terminologie, car elle peut préter a confusion.
Dans le languge courant, les termes table de hachage (hash table) ou Hashmap sont souvent utilisés comme synonyme du mot dictionnaire (dictionary).
Cependant, il existe une différence subtile :
- La table de hachage est un cas particulier du concept plus général de dicitonnaire(python), Hashmap(java) et objet en JS.

Pour la simpliciter de ce cours on utilisera python pour l'implémentation ainsi que le terme dictionnaire.

### Table de hachage vs Dictionnaire
En informatique, un dictionnaire est un type de donnée abstrait composé de paires : 
- Clé / Valeur

Il définit un ensemble d'opérations fondamentales : 
- Ajouter une paire clé-valeur

      d[key] = value
  
- Supprimer une paire clé-valeur

      d.pop(key)
  
- Mettre a jour une paire clé-valeur

      d.update({key: new_value})
  
- Trouver la valauer associé à une clé donnée

      d[key]

On peut le comparer à un dictionnaire bilingue, où les clés sont des mots étrangers et les valeurs leurs traductions.
Mais la relation entre clé et valeur n’est pas toujours une traduction : par exemple, un annuaire téléphonique est aussi un dictionnaire, associant des noms à des numéros de téléphone.

💡 À chaque fois que tu associes une chose à une autre (une valeur à une clé), tu utilises en réalité une forme de dictionnaire.
C’est pourquoi on appelle aussi ces structures maps ou tableaux associatifs (associative arrays).


#### Propriétés d’un dictionnaire
Un dictionnaire possède plusieurs caractéristiques intéressantes :
1. Paires clé–valeur uniquement → on ne peut pas avoir une clé sans valeur ni une valeur sans clé.
2. Clés et valeurs arbitraires → elles peuvent appartenir à des types différents (nombres, String, tableaux, etc.).
3. Paires non ordonnées → en général, les dictionnaires ne garantissent aucun ordre entre leurs éléments (cela dépend de l’implémentation).
4. Clés uniques → une même clé ne peut pas apparaître deux fois (cela violerait la définition d’une fonction mathématique, et le fondement meme de la clé de hachage).
5. Valeurs non uniques → une même valeur peut être associée à plusieurs clés.

#### Concepts associés
Certains concepts étendent le principe du dictionnaire :
- Un multimap permet d’associer plusieurs valeurs à une même clé.
- Un bidirectional map (ou map bidirectionnelle) associe les clés aux valeurs dans les deux sens.

      >>> glossary = {"BDFL": "Benevolent Dictator For Life"}
      >>> glossary["GIL"] = "Global Interpreter Lock"  # Add
      >>> glossary["BDFL"] = "Guido van Rossum"  # Update
      >>> del glossary["GIL"]  # Delete

      >>> glossary["BDFL"]  # Search
      'Guido van Rossum'

      >>> glossary
      {'BDFL': 'Guido van Rossum'}

Avec la syntaxe des crochets [ ], tu peux ajouter, modifier ou supprimer une paire clé–valeur dans un dictionnaire, et accéder à la valeur d’une clé donnée.

Mais une autre question se pose : comment fonctionne vraiment ce dictionnaire intégré ?, Comment peut-il gérer des clés de tout type et le faire si rapidement ?

Chercher une implémentation efficace de ce type abstrait est ce qu’on appelle le “problème du dictionnaire”. L’une des solutions les plus connues repose sur la table de hachage, que l'on va voir ici, mais d’autres existent, comme celle basée sur un arbre rouge-noir (red-black tree).

### Hash Table: Un Array avec une Hash Function
T’es-tu déjà demandé pourquoi l’accès aux éléments d’une séquence en Python est si rapide, quel que soit l’indice demandé ?

Par exemple, imagine que l'on travaille avec une très longue chaîne de caractères, comme celle-ci :
         
          >>>> import String
          >>>> text = string.ascii_uppercase * 100_000_000
          
          >>>> text[:50] # slice de l'indice 0 à 50, montre que les 50 premier charcatères
          'ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWX'

          >>>> len(text)
          26000000000

La variable text ci-dessus contient 2,6 milliards de caractères, formés par la répétition de lettres ASCII, ce que tu peux vérifier avec la fonction len() de Python.
Et pourtant, accéder au premier, au milieu, au dernier ou à n’importe quel caractère de cette chaîne est tout aussi rapide.

- 🧱 Array (en général, comme en C ou Java) →
✅ l’accès à un élément par index est toujours en O(1), donc le premier, le dernier ou le millième prennent le même temps. (Attention ce n'est pas le cas pour le déplacement ou suppression d’éléments, qui eux ne sont pas en O(1).)

- 🐍 Python list →
même chose : c’est un tableau dynamique, donc l’accès par index est O(1).

- 🧩 dict →
l’accès à une clé est aussi O(1) en moyenne grâce à la table de hachage (mais pas garanti en cas de collisions extrêmes).

- ⚙️ set ->
identique à dict, il utilise une hash table, donc test d’appartenance et insertion en O(1).


      >>> text[0]  # The first element
      'A'

      >>> text[len(text) // 2]  # The middle element
      'A'

      >>> text[-1]  # The last element, same as text[len(text) - 1]
      'Z'

Mais comment est-ce possible ?

Le secret de cette grande rapidité réside dans le fait que les séquences(lst, tuple, str, array, range, bytes) Python reposent sur un tableau (array), une structure de données à accès aléatoire (random access).

Random acces, ou accès aléatoires, ne siginifie pas "Accès au hasard" mais "Accès direct à n'importe quel élément sans parcourir les autres".

Une structure à accès aléatoire (random access data structure) permet d’aller directement à un élément en connaissant son indice (position).

Elle obéit à deux principes :
- Le tableau occupe un bloc de mémoire contigu.
- Chaque élément du tableau a une taille fixe connue à l’avance.

Quand on connaît l’adresse mémoire du tableau (appelée offset), on peut accéder instantanément à n’importe quel élément en appliquant une formule simple :
            
            Adresse d’un élément = offset + (taille_élément × index)

Autrement dit, on part de l’adresse du premier élément (index 0), puis on avance du nombre d’octets correspondant à la taille de l’élément multipliée par son index.

Cette opération prend toujours le même temps, car elle se résume à une addition et une multiplication.

🔎 Remarque : contrairement aux tableaux classiques, les listes Python peuvent contenir des éléments hétérogènes (de tailles différentes), ce qui casserait cette formule.
Pour pallier cela, Python ajoute un niveau d’indirection : la liste contient un tableau de pointeurs vers les zones mémoire réelles où sont stockées les valeurs.

#### Représentation d'une list en python : 

          Array
      ----------------------          0x7f -> "Hello
        0x7f | 0xb5 | 0x7f
      ----------------------
Mon tableau stock des adresse mémoires et non des éléments, une des carractéristique d'une tableau dynamique.

Les pointeurs ne sont en réalité que de simples nombres entiers, qui occupent toujours la même quantité d’espace mémoire. Par convention, les adresses mémoire sont représentées en notation hexadécimale. En Python (et dans d’autres langages), ces nombres sont préfixés par 0x.

En résumé, trouver un élément dans un tableau est rapide; quelle que soit sa position en mémoire. Peut on réutiliser cette idée dans un dictionnaire ? -> Oui!

Les tables de hachage tirent leur nom d’une astuce appelée hachage (hashing), qui permet de traduire une clé quelconque en un nombre entier, utilisable comme indice dans un tableau classique.

Ainsi, au lieu de chercher une valeur via un index numérique, tu peux la retrouver à partir d’une clé arbitraire, sans perte notable de performance — malin, non ?

En pratique, le hachage ne fonctionne pas avec toutes les clés, mais la plupart des types intégrés de Python sont hachables.

Et si tu respectes certaines règles, tu pourras aussi créer tes propres types hachables.

--- 

### Qu’est-ce qu’une fonction de hachage ?

Une fonction de hachage est une formule mathématique (ou un algorithme) qui transforme des données de n’importe quelle taille (un texte, un fichier, un mot de passe, etc.) en une valeur de taille fixe, appelée valeur de hachage ou empreinte (hash value).

👉 Exemple :
- Tu donnes à la fonction le mot "bonjour".
- Elle te renvoie un nombre (ou une suite d’octets) du genre 3984127391.
- Si tu lui redonnes "bonjour", elle renverra toujours le même résultat.

Mais si tu modifies une seule lettre ("Bonjour" avec un B majuscule), tu obtiendras une empreinte complètement différente.


#### A quoi ça sert ? 

Une fonction de hash a deux rôles principaux : 
1. Identifier rapidement une donnée
   - Dans une table de hachage(comme un dict en python), elle sert a retourver un élément très vite, sans parcourir toute la colleciton.
Exemple : Python utilise hash() pour savoir ou stocker et ou retrouver une clé dans un dict ou un set.

2. Vérifier l'intégrité ou protéger des données
   - On peut comparer deux empreintes pour vérifier qu'un fichier ou un mot de passe n'a pas été modifié.
Exemple : quand tu télécharges un fichier Linux avec une somme de contrôle SHA-256, tu compares la valeur de hachage locale à celle fournie sur le site.

#### 🔹 Caractéristiques d’une bonne fonction de hachage

Une bonne fonction de hachage doit :
- toujours produire la même empreinte pour les mêmes données,
- générer des empreintes différentes pour des données différentes,
- être rapide à calculer,
et, idéalement, répartir les valeurs de manière uniforme (pour éviter les collisions, c’est-à-dire deux données différentes donnant le même hash).

| Entrée (donnée) | Fonction de hachage   | Sortie (empreinte)         |
| --------------- | --------------------- | -------------------------- |
| "bonjour"       | `hash()` ou `SHA-256` | `3984127391` ou `aab3f...` |
| "Bonjour"       | `hash()` ou `SHA-256` | `89234aa0` ou `f03b9...`   |
| "bonjour"       | `hash()` ou `SHA-256` | `3984127391` ou `aab3f...` |

On remarque que la lette b en majuscule et en minuscle ne donne pas la meme clé de hash.

Une fonction de hachage, c’est donc :
🔑 Un moyen rapide et fiable de transformer une donnée en une “empreinte” unique et courte, pour l’identifier, la retrouver ou en vérifier l’intégrité.

### Examinons la fonction integrée hash() de python

Avant d’essayer d’implémenter notre propre fonction de hachage, on va prendre un instant pour analyser la fonction hash() intégrée à Python afin d’en dégager les propriétés essentielles.
Cela nous aidera à comprendre les défis liés à la conception d’une fonction de hachage.

💡 Remarque :
Le choix d’une fonction de hachage influence fortement les performances d’une table de hachage (comme un dictionnaire).
C’est pourquoi on se base généralement sur la fonction intégrée hash(), plutôt que d’en créer une soi-même.

#### Que fait hash() ?

La fonction hash() transforme n’importe quelle donnée (nombre, texte, etc.) en un nombre entier.
Ce nombre est la valeur de hachage.

Par exemple :

      >>> hash(3.14)
      322818021289917443

      >>> hash("Lorem")
      7677195529669851635
      
Tu peux l’utiliser sur des nombres, des chaînes, voire des objets plus complexes.

#### Pourquoi les résultats changent ?
Si tu testes hash() sur une même chaîne de caractères ou n'importe quel type hachable(voir plus bas) dans une même session Python, tu obtiens toujours le même résultat :

      >>> hash("Lorem")
      7677195529669851635

      >>> hash("Lorem")
      7677195529669851635

➡️ La fonction est donc déterministe :
Elle renvoie toujours la même valeur pour la même entrée, tant que ton environnement Python ne change pas.

Mais si tu redémarres Python(avec la commande python -c), tu verras que la valeur de hachage pour une chaîne change :
            
            $ python -c 'print(hash("Lorem"))'
            6182913096689556094

            $ python -c 'print(hash("Lorem"))'
            1756821463709528809

Pourquoi ? 
Parce que Python ajoute une part d’aléatoire (appelée hash randomization) pour les chaînes, tuples, bytes, fronzenset, afin d’éviter certaines attaques de sécurité.

Attention cette randomnisation ne s'applique pas à : 
- int, float, bool, None
- Les objets définis par l'utilisateur(sauf exception)

Pourquoi cette distinction ? 

Les attaques DoS visaient principalement les tables de hachage (comme les dictionnaires) en utilisant des clés textuelles provoquant volontairement des collisions, car ce sont ces types qui servent le plus souvent de clés.


#### Qu'est-ce qu'un type hachable et quels sont les types hachables ?

Un type hachables est un type immuable tout simplement. Pour que notre fonction de hash puisse fonctionner notre objet(peu importe son type) ne doit pas changer sinon la clés de hachages ne sera plus la même et on perdra la contenu stocker pointé par notre clé de hash. 

Résumé des types hashable et non hashables :
| Type                           | Déterminisme (dans une même session)  | Même résultat entre plusieurs sessions ?                |
| ------------------------------ | ------------------------------------  | ------------------------------------------------------  |
| `int`, `float`, `bool`, `None` | ✅ Oui                                | ✅ Oui (pas d’aléa)                                     |
| `str`, `bytes`, `tuple`        | ✅ Oui                                | ❌ Non (randomisation activée)                          |
| `frozenset`                    | ✅ Oui                                | ❌ Non (car dépend d’objets potentiellement randomisés) |
| `dict`, `list`, `set`          | ❌ Non hachable → `TypeError`         | —                                                       |

👉 Donc :
- Les objets immuables simples (nombres, booléens) ont un hachage fixe partout.
- Les chaînes, tuples et certains objets immuables complexes ont un hachage fixe uniquement dans la session courante.
- Les objets mutables (list, dict, set) ne sont pas hachables du tout, car leur contenu peut changer.

            # Exemple avec un objet mutable : IMPOSSIBLE
            >>> hash([1, 2, 3])
            Traceback (most recent call last):
            File "<stdin>", line 1, in <module>
            TypeError: unhashable type: 'list'


--- 

### Deep dive dans la fonction hash()

Une autre caractéristique intéressante de hash() est qu’elle produit toujours une sortie de taille fixe, quelle que soit la taille de l'entrée.

En Python, la valeur de hachage est un entier d’une magnitude modérée. Il peut parfois s’agir d’un nombre négatif, il faut donc en tenir compte si l'on souhaite utiliser les valeurs de hachage d’une manière ou d’une autre :

                  >>> hash("Lorem")
                  -972535290375435184

#### Sortie de taille fixe = perte d'information

Une fonction de hachage prend une entrée qui peut être très grand, cela peut être : 
 - Une string 'bonjour'
 - Un long texte "Guerre et paix"
 - un fichier
 - un nombre
Il n'y a aucune limite a la taille de ce que l'on lui donne. Donc le nombre de valeurs possibles en entrée est immense, pratiquement infini.

La fonction de hash fabrique toujours une sortie de taille fixe : 
- SHA-256 produit 256 bits, toujours.
- Une table de hachage peut utiliser 64 bits, toujours.
Même si notre entrée fait 1 ou 10 gigas, notre sortie(le hash) aura exactement la même taille que n'importe quelle autre sortie de la hash function. 

Cela signifie que beacoup d'informations de l'entrée ne peuvent pas être représentées.

Car on compresse une quantité d'informations énorme dans un petit espace fixe(le hash)

Quand deux entrées différentes donnent la même sortie, on appelle ça une collision.
Et puisque la fonction ne garde qu’un nombre fixe de bits, elle oublie le reste de l’information.
Tu ne peux donc jamais remonter à l’entrée originale à partir du hash.

C’est ça la perte d’information.

#### Explications 

La fonction de hachage détruit volontairement une partie de l’information. Elle garde seulement une empreinte compacte, pas le contenu lui-même.

Tu ne peux pas “inverser” la fonction, car il n’y a pas assez de place dans la sortie pour tout représenter.
C’est un processus à sens unique.

Alors à quoi sert le hash si on perd l’info ?

Le but n’est pas de retenir l’information, mais de créer une clé courte, représentative et quasi unique.

| Cas d’usage                          | Rôle du hash                                                                |
| ------------------------------------ | --------------------------------------------------------------------------- |
| **Table de hachage (programmation)** | Trouver ou ranger une valeur plus vite grâce à une clé numérique            |
| **Mot de passe**                     | On stocke le hash, pas le mot de passe (pour la sécurité)                   |
| **Fichier téléchargé**               | Vérifier qu’il n’a pas été modifié (si le hash change, le fichier a changé) |
| **Blockchain**                       | Identifier de manière unique chaque bloc                                    |

Le hash est une étiquette numérique, pas une copie du contenu.

#### Rapport entre Dict python et la fonction hash()

Quand on fait ceci :

            d = {"nom" : "Alice"}

Python fait plusieurs choses en interne : 
1. Il calcule le hash de la clé "Nom" -> un entier (ex -73459201).
2. Il s'en sert pour trouver ou ranger la praire clé-valeur dans la table (comme une adresse de case mémoire)
3. Et surout, il stocke a la fois :
   - Le hash
   - La clé originale ("nom")
   - La valeur associé ("Alice")

                  Par exemple :
                  d = {"nom": "Alice", "âge": 25, "ville": "Paris"}

                  Python va calculer le hash pour chaque clé :
                  hash("nom")   -> 432819
                  hash("age")   -> 992319
                  hash("ville") -> 128334

                  Ensuitre transformer ces gros nombres en index dans le tableau
                  hash % taille_tableau

                  Si la table fait 8 cases :
                  432819 % 8 = 3
                  992312 % 8 = 0
                  128334 % 8 = 6

                  Et placer chaque pair, clé/valeur a cet index :
                  TABLE DE HACHAGE EN MÉMOIRE

                +-----------------------------------------------------------+
                | 0 | → [clé="âge",   valeur=25,     hash=992312]           |
                | 1 | → None                                                |
                | 2 | → None                                                |
                | 3 | → [clé="nom",   valeur="Alice", hash=432819]          |
                | 4 | → None                                                |
                | 5 | → None                                                |
                | 6 | → [clé="ville", valeur="Paris", hash=128334]          |
                | 7 | → None                                                |
                +-----------------------------------------------------------+

               Chaque “flèche” (→) pointe vers une zone mémoire où Python stocke le petit objet (clé, valeur).


Donc Python ne perd jamais l’information d’origine. Le hash sert seulement à accélérer la recherche.


#### Et s’il y a deux clés avec le même hash ? Les collisions de hachage

Comme nous l'avons vu, les collisions sont inévitable car en entré on parle d'espace infini et en sortie d'espace fini. Mais une bonne fonction de hash a pour objectif de limité les coliisions, car si il y en a trop alors la table de hachage n'est plus performante.

Les collisions de hachage sont un concept essentiel dans les tables de hachage, on y reviendra plus tard dans l'implémentation.

Pour l'instant on considère qu'elles sont indésirables, donc a éviter autant que possible, car elles peuvent entraîner des recherches très inefficaces et même être exploitées par des pirates informatiques.

Ainsi, une bonne fonction de hachage doit minimiser la probabilité de collision, à la fois pour des raisons d’efficacité et de sécurité.

### Répartition uniforme des valeurs de hachage
En pratique, cela signifie souvent que la fonction de hachage doit attribuer des valeurs réparties uniformément dans l’espace disponible.

#### L’espace disponible”, c’est quoi ?
C'est l'ensemble des cases possibles dans ta table de hachage.
- Si ta tale a 8 cases -> l'espace disponible, c'est {0, 1, 2, 3, 4, 5, 6, 7}.
- Si elle a 1000 cases → l’espace disponible, c’est {0, 1, 2, ..., 999}

Une bonne fonction de hachage doit générer des nombres qui se répartissent aussi uniformément que possible dans cet espace.

Exemple on a deux cases, il faut qu'elle se remplissent simultanément : 

                        0 ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ (51)
                        1 ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■   (49)

Une mauvaise implémentation serait :

                        0 ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ (100)
                        1 ■                                                     (1)
Pourquoi ?

La case 0 contient 100 élément donc le risque de colission est grand, à l'inverse la case 1 contient 1 seul élément donc le risque de collision est tres faible

En revanche, si la répartition est uniforme :
- Chaque clé est à peu près à une case unique
- L’accès devient quasi instantané (O(1)).

### 🔹 Identifier les propriétés d’une fonction de hachage
Voici un résumé de ces caractéristiques, en comparant une fonction de hachage “classique” et une fonction de hachage cryptographique :

| Caractéristique        | Fonction de hachage | Fonction de hachage cryptographique |
| ---------------------- | ------------------- | ----------------------------------- |
| Déterministe           | ✔️                  | ✔️                                  |
| Entrée universelle     | ✔️                  | ✔️                                  |
| Sortie de taille fixe  | ✔️                  | ✔️                                  |
| Calcul rapide          | ✔️                  | ✔️                                  |
| Répartition uniforme   | ✔️                  | ✔️                                  |
| Répartition aléatoire  |                     | ✔️                                  |
| Graine aléatoire       |                     | ✔️                                  |
| Fonction à sens unique |                     | ✔️                                  |
| Effet d’avalanche      |                     | ✔️                                  |

Deterministe  = Sortie toujours égal pour une même entrée.
Effet d'avalanche signifie que même un changement minime entraine un clé de hash totalement différente

                  hash("Lorem")
                  => 46887654313434

                  hash("lorem")
                  => 139836146678832


#### 🔹 Comparer l’identité d’un objet avec son hash

L’une des fonctions de hachage les plus simples qu’on puisse imaginer en Python est la fonction intégrée id(),
qui te donne l’identité d’un objet.

Dans l’interpréteur Python standard, cette identité correspond à l’adresse mémoire de l’objet, exprimée sous forme d’entier :

                        >>> id("Lorem")
                        139836146678832

La fonction id() possède la plupart des propriétés souhaitées d’une fonction de hachage :
- Elle est trés rapide
- Elle fonctionne avec n'importe quelle entrée
- Elle renvoie un entier de taille fixe de facon deterministe.

De plus, tu ne peux pas facilement retrouver l’objet original à partir de son adresse mémoire.

Les adresses mémoire elles-mêmes sont immutables pendant la durée de vie d’un objet, et sont quelque peu aléatoires entre différentes exécutions de l’interpréteur.

Alors pourquoi Python insiste-t-il pour utiliser une autre fonction que id() pour le hachage ?

1. Tout d’abord, l’intention de id() est différente de celle de hash().
D'autres implémentations de Python pourraient définir l'identité des objets d'une autre manière

2. Ensuite, les adresses mémoires sont prévisibles et ne sont pas uniformément réparties- ce qui les rends inadaptées et peu sûres pour le hachage.

3. Enfin, deux objets égaux devraient normalement produire le même code de hachage, même s’ils ont des identités distinctes (equals & hashcode)

En programmation, il faut distinguer deux notions : Identité et Egalité

| Notion       | Signification                                    | Exemple                                              |
| ------------ | ------------------------------------------------ | ---------------------------------------------------- |
| **Identité** | Est-ce le *même objet* en mémoire ?              | `is` en Python, `==` pour les références en Java     |
| **Égalité**  | Est-ce que les *valeurs contenues* sont égales ? | `==` en Python (via `__eq__`), ou `equals()` en Java |

      a = [1, 2, 3]
      b = [1, 2, 3]

      identité => print(a is b)  # False → objets différents en mémoire
      Egalité  => print(a == b)  # True  → mêmes valeurs

Pourquoi “égaux → même hash” ?

Une fonction de hachage sert souvent à ranger ou comparer des objets dans des structures comme :
- une table de hachage (dict en Python, HashMap en Java),
- un set (ensemble),
- ou toute structure basée sur le hash.

Quand tu mets un objet dans une de ces structures, elle calcule son hash pour savoir où le stocker.

Puis, pour vérifier s’il existe déjà, elle compare la clé de hash et, si besoin, l’égalité logique (equals() ou __eq__).

👉 Si deux objets sont égaux (a.equals(b) ou a == b), ils doivent produire le même code de hachage.

Sinon, la table les rangera à deux endroits différents — donc elle ne les reconnaîtra pas comme égaux.


## 🔹 Créer ta propre fonction de hachage

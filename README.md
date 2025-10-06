# HashMap
Build strong fondation of hashmap

## I. Opérations de base sur un dictionnaire simple

### 1) Fondations CRUD & erreurs
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 18 | Check if a Dictionary is Empty | Tester vide / cas bord | validation, errors | I |
| 2 | 4 | Check Key Existence in Dictionary | Existence clé | lookup, in | I |
| 3 | 2 | Add Key to Dictionary | Création & update | CRUD | I |
| 4 | 12 | Remove a Key from a Dictionary | Suppression sûre | CRUD, pop | I |
| 5 | 53 | Find Length of a Dictionary | Taille | len | I |

### 2) Itérations & vues dynamiques
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 5 | Iterate Over Dictionary Using For Loops | Parcours clés/valeurs | iteration, items | I |
| 2 | 9 | Iterate Over Dictionaries (Alternative) | Variantes de parcours | keys, values, items | I |
| 3 | 31 | Get Key, Value, and Item | Comprendre vues | view-objects | I |
| 4 | 55 | Access Dictionary Key's Element by Index | Ordre d’itération | order, list(keys) | I |

### 3) Compréhensions & transformations
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 6 | Numbers→Squares (1..n) | Construction par compréhension | comp, range | I |
| 2 | 24 | Dict from String (freq) | Fréquences simples | counting, freq | I |
| 3 | 29 | Remove Spaces from Keys | Nettoyage clés | sanitize | I |
| 4 | 49 | Convert String Values→Numeric | Casting valeurs | transform | I |
| 5 | 57 | Filter Even Numbers from Values | Filtrage conditionnel | filter, lists | I |
| 6 | 62 | Extract Values → List of Lists | Extraction structurée | lists, reshape | I |
| 7 | 70 | Map List→Dict via Function | Map fonctionnelle | map, func | I |
| 8 | 76 | Combine Two Lists into Dict | Zip clés/valeurs | zip | I |

### 4) Tri, extrêmes & top-k
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 1 | Sort Dictionary by Value | Tri par valeur ↑/↓ | sort, value-key | I |
| 2 | 14 | Sort Dictionary by Key | Tri par clé ↑/↓ | sort, key | I |
| 3 | 15 | Get Max & Min Values | Extrêmes (clé/valeur) | max/min | I |
| 4 | 22 | Highest 3 Values | Top-k | heap/top-k | I |
| 5 | 30 | Top three items in a shop* (top-k métier),  
| 6 | 59 | Specified # of Max Values | Top-k paramétrable | selection | I |
| 7 | 80 | Key of Maximum Value | Argmax | argmax | I |

### 5) Fusion & résolution de conflits
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 3 | Concatenate Dictionaries | Merge simple | merge | I |
| 2 | 8 | Merge Two Dictionaries | Union + override | merge | I |
| 3 | 19 | Combine by Adding Values | Agrégation sur clés communes | reduce, sum | I |
| 4 | 36 | Dict from Two Lists (keep dups) | Multi-mapping set | multimap, set | I |
| 5 | 68 | Combine Dicts→List of Values | Multi-mapping list | multimap, group | I |
| 6 | 86 | Merge with Custom Conflict Resolution | Stratégie de conflit | policy, resolve | II |

### 6) Inversions & regroupement
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 20 | Print All Distinct Values | Uniques | set, dedup | I |
| 2 | 21 | Letter Combinations from Keys | Produit combinatoire | product | I |
| 3 | 23 | Combine Values in List of Dicts | Group & sum par clé | groupby, sum | I |
| 4 | 46 | Group Pairs into Dict of Lists | Regrouper valeurs | groupby | I |
| 5 | 47 | Split Dict of Lists→List of Dicts | Reshape | transform | I |
| 6 | 61 | Count Frequency of Values | Histogramme valeurs | freq, counter | I |
| 8 | 67 | Invert dict with non-unique values* | mutlti-valeirs | ... | II |
| 8 | 72 | Invert Dict (unique values) | Inversion clé↔valeur | invert | I |

---

## II. Dictionnaires nestés, récursion & parcours (dict comme structure de données)

### 7) Dictionnaires imbriqués & profondeur
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 27 | List → Nested Dict of Keys | Construction imbriquée | nesting | II |
| 2 | 40 | Keys x,y,z → list ranges | Accès indexé dans valeurs | nested-lists | I |
| 3 | 41 | Drop Empty Items | Nettoyage None/vides | sanitize | I |
| 4 | 42 | Filter by Values | Filtrage conditionnel | filter | I |
| 5 | 54 | Get Depth of a Dictionary | Mesurer profondeur | depth | II |
| 6 | 71 | Retrieve Nested by Selector List | Sélecteur de chemin | path, selector | II |
| 7 | 81 | Flatten Nested Dict (tuple path) | Aplatissement récursif | flatten, recursion | II |

### 8) Parcours avancé DFS/BFS & transformations
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 62 | Extract Values (variants) | Accès multiple champs | projection | I |
| 2 | 43 | Multiple Lists → Nested Dict | Jointure → n-levels | nesting, zip | II |
| 3 | 44 | Filter Students (tuple vals) | Filtre sur tuple | predicate | I |
| 4 | 71 | Retrieve by Path | Traversée ciblée | dfs, path | II |
| 5 | 81 | Flatten (rappel) | Visite complète | dfs, stack | II |

---

## III. Patterns “pro” pour être à l’aise avec HashMap (Java)

### 9) Mémoïsation, défauts & pipelines
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 82 | Dictionary-based Memoization | Cache par args | memo, cache | II |
| 2 | 83 | Custom Dict with Default Factory | Auto-nesting | defaultdict, factory | II |
| 3 | 84 | Dictionary Transformation Pipeline | Pipelines de transformations | pipeline, map/reduce | II |

### 10) Graphes basés dict & parcours
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 85 | Graph via Dict (Dijkstra / cycles / topo) | Utiliser dict comme graphe | graph, BFS/DFS | II |

### 11) Structures bi-directionnelles, LRU & fusions avancées
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 87 | Bidirectional Dictionary | Bijection clé↔valeur | bijection, sync | II |
| 2 | 88 | Dictionary-based LRU Cache | Eviction O(1) | cache, LRU | II |
| 3 | 86 | Merge with Custom Conflict Resolution | Stratégies de fusion | resolver | II |

### 12) Sérialisation, références & évaluation
| Ordre | # | Exercice | Objectif clé | Tags | Niveau |
|---:|---:|---|---|---|:--:|
| 1 | 90 | Dictionary-based Expression Evaluator | AST minimal avec dict | eval, recursion | II |
| 2 | 89 | Serialization with Circular Refs | Sérialiser graphes cycliques | serialize, ids | II |

---


> Tu peux cocher/ajouter tes solutions sous chaque table. Cette structure te donne un **filtre par catégorie** et **par niveau** pour travailler efficacement et basculer ensuite vers les **HashMap** en Java sans surprise.

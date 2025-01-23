# Rendu du projet 'Big Data Management'
## Rendu par : GHIZLAN Moqim


Dans ce fichier, vous trouvez la strucure suivante:
- Question
- Resultats


PS : Dans certaines questions, vous pouvez trouver les requêtes avec la caractéristique '\\' (antislash). Cela est dû à la limitation de la longueur de la ligne et pour éviter que le texte soit compris comme des formules mathématiques, notamment apres les caractères '$'. Si vous regarder le rendu après la transformation en markdown, vous ne être pas concerné par ce problème.


Dans ce rendu, vous trouverez les parties suivantes:

  - [Partie 1 | Premières requêtes](#Partie-1)
  - [Partie 2 | Prise en main de quelques étapes d'agrégation](#Partie-2)
  - [Partie 3 | Pipelines d'agrégation](#Partie-3)






## Partie 1 | Premières requêtes

### Question 1 | db.collection.findOne()
db.collection.findOne(): renvoie un document de la collection (doc).
Pour chacune des collections grades et zip, récupérer un document, puis en déduire un type pour les éléments de cette collection.

### Resultats :

```js
//Requête
db.grades.findOne()
```

```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}
```


```js
//Requête
db.zips.findOne()
```

```js
//Résultat
{
    _id: '01001',
    city: 'AGAWAM',
    loc: [
        -72.622739,
        42.070206
    ],
    pop: 15338,
    state: 'MA'
}
```


```js
//Question
En déduire un type pour les éléments de cette collection.
```

```js
//Reponse
On déduit que les éléments de la collection grades sont de type document imbriqué, tandis que les éléments de la collection zip sont de type document simple.
```



### Question 2 | db.collection.find({})
db.collection.find({}): récupère les documents de la collection sans les filtrer (doc).
Récupérer quelques documents de grades et vérifier qu'ils sont conformes au type de la question précédente.


### Resultats :

```js
//Requête
db.grades.find().limit(1)
```

```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}
```


```js
//Requête
db.grades.find(
    {
        student_id: 0
    }
).limit(1)

```

```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ]
}
```




```js
//Requête
db.grades.find(
    {
        class_id: 5
    }
).count()

```

```js
//Résultat
10
```




```js
//Requête
db.grades.find(
    {
        scores: {
            $elemMatch: {
                type: 'homework'
            }
        }
    }
).count()

```

```js
//Résultat
280
```




```js
// Question
Vérifier qu'ils sont conformes au type de la question précédente
```

```js
// Reponse
On trouve que la structure des documents de la collection grades est conforme au type déduit précédemment.
```


### Question 3 | db.collection.find({}).countDocuments()
db.collection.find({}).countDocuments(): compte le nombre de résultats de la requête.

Donner le nombre de résultats de la requête précédente (résultat attendu: 280).

### Resultats :

```js
//Requête
db.grades.countDocuments(
    {
        scores: {
            $elemMatch: {
                type: "homework"
            }
        }
    }
)

```

```js
//Résultat
280
```




### Question 4 | db.collection.find({ field: value })
db.collection.find({ field: value }): récupère les documents dont le champ field a la valeur value. Il est possible de spécifier plusieurs champs.

Récupérer les documents dont le class_id vaut 20 dans la collection grades (7 résultats).


### Resultats :

```js
//Requête
db.grades.find(
    {
        class_id: 20
    }
).count()
```

```js
//Résultat
7
```




### Question 5 | db.collection.find({ field: { op: value } })
db.collection.find({ field: { op: value } }): exprime une condition de sélection sur le champ field: op est la fonctionde comparaison et value la valeur à laquelle on veut comparer la valeur du champ (doc des opérateur de sélection).

Récupérer les documents dont le class_id est inférieur ou égal à 20. L'opérateur inférieur ou égal se note $lte. (188 résultats)


### Resultats :

```js
//Requête
db.grades.find(
    {
        class_id: {
            $lte: 20
        }
    }
).count()

```

```js
//Résultat
188
```


### Question 6 | db.collection.find({ $expr: { arbre de syntaxe de l'expression en
db.collection.find({ \$expr: { arbre de syntaxe de l'expression en js} } ): exprime une condition générique. La syntaxe est la représentation en js de l'expression (doc). Les champs sont représentés par la syntaxe $champ ou \$champ1.souschamp2. Les arguments sont souvent donnés sous forme d'une liste (mais il faut vérifier la documentation de chaque opérateur). Par exemple, { \$lte: [ "\$class_id", 20 ] } est une expression indiquant que le champ class_id (via "\$class_id") est inférieur (\$lte) à 20.

Récupérer les documents pour lesquels le student_id est supérieur ou égal au class_id (188 résultats).


### Resultats :

```js
//Requête
db.grades.find(
    {
        $expr: {
            $gte: ["$student_id", "$class_id"]
        }
    }
).count()

```


```js
//Résultat
188
```


### Question 7 | class_id ({ field: { op1: value1, op2: value2 } }) & $expr & $and
Récupérer les documents dont le class_id est compris entre 10et 20 (100 résultats). Faire une version avec un double filtre sur le  champ class_id (via la syntaxe { field: { op1: value1, op2: value2 } }), puis une autre version avec un $expr contenant un $and.


### Resultats :

```js
//Requête
db.grades.find(
    {
        class_id: {
            $gte: 10,
            $lte: 20
        }
    }
).count()

```

```js
//Résultat
100
```


```js
//Requête
db.grades.find(
    {
        $expr: {
            $and: [
                { $gte: ["$class_id", 10] },
                { $lte: ["$class_id", 20] }
            ]
        }
    }
).count()

```

```js
//Résultat
100
```


### Question 8 | db.collection.find({condition}, { projection spec } )
db.collection.find({condition}, { projection spec } ): permet de choisir les champs de sortie de la requête (doc). Il est possible de supprimer des champs, voir de créer de nouveaux champs en utilisant une expression comme ci-dessus.

Donner tous les documents de la collection en renommant le champ class_id en ue et le champ student_id en etu.


### Resultats :

```js
//Requête
db.grades.find(
    {},
    {
        _id: 0,
        ue: "$class_id",
        etu: "$student_id",
        scores: 1
    }
)

```

```js
//Résultat
{
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ],
  ue: 2,
  etu: 0
}
```


```js
//Requête
db.grades.find(
    {},
    {
        _id: 0,
        ue: "$class_id",
        etu: "$student_id",
        scores: 1
    }
).count()

```

```js
//Résultat
280
```








## Partie 2 | Prise en main de quelques étapes d'agrégation


### Question 1 | { $project: { projection spec }}
{ $project: { projection spec }}: applique une projection spécifiée comme pour le deuxième argument de db.collection.find() (doc).

Donner tous les documents de la collection grades en y ajoutant un champ somme dont la valeur est la somme des champs class_id et student_id. on utilisera pour cela le pipeline d'agrégation avec un $project.

### Resultats :

```js
//Requête
db.grades.aggregate([
    {
        $project: {
            _id: 0,
            student_id: 1,
            class_id: 1,
            scores: 1,
            somme: { $add: ["$class_id", "$student_id"] }
        }
    }
])

```

```js
//Résultat
{
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    }
  ],
}

```


### Question 2 | { $match: { find spec } }
{ $match: { find spec } }: applique un filtre similaire au premier argument de db.collection.find() (doc).

Dans la collection zips récupérer les documents ayant une population supérieure ou égale à 10000 (7634 résultats).

### Resultats :

```js
//Requête
db.zips.aggregate([
    {
        $match: {
            pop: { $gte: 10000 }
        }
    }
])

```


```js
//Résultat
{
  _id: '01001',
  city: 'AGAWAM',
  loc: [
    -72.622739,
    42.070206
  ],
  pop: 15338,
  state: 'MA'
}

```
```js
//Requête
db.zips.aggregate([
    {
        $match: {
            pop: { $gte: 10000 }
        }
    },
    {
        $count: "total_documents"
    }
])

```


```js
//Résultat
{
  total_documents: 7634
}

```


### Question 3 | { $sort: {sort spec } }
{ $sort: {sort spec } }: trie la collection selon les champs spécifiés (doc). Pour chaque champ, on indique s'il est trié par ordre croissant ou décroissant. Les champs sont listés par ordre d'apparition dans l'ordre lexicographique utilisé. Par exemple, sur la collection zips, { state: 1, pop: -1 } inquera de trier par état (state), puis par population (pop) décroissante.

Dans la collection grades, renvoyer les documents triés par class_id, puis par student_id (par ordre croissant pour les 2 champs).

### Resultats :

```js
//Requête
db.grades.aggregate([
    {
        $sort: {
            class_id: 1,
            student_id: 1
        }
    }
])

```


```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c370'),
  student_id: 4,
  class_id: 0,
  scores: [
    {
      type: 'exam',
      score: 67.50593066420024
    },
    {
      type: 'quiz',
      score: 60.43052278095685
    },
    {
      type: 'homework',
      score: 98.634640715799
    },
    {
      type: 'homework',
      score: 6.719346717934882
    },
    {
      type: 'homework',
      score: 43.61408080940507
    },
    {
      type: 'homework',
      score: 92.01607786819618
    }
  ]
}

```
```js
//Requête
db.grades.aggregate([
    {
        $sort: {
            class_id: 1,
            student_id: 1
        }
    },
    {
        $count: "total_documents"
    }
])


```


```js
//Résultat
{
  total_documents: 280
}

```


### Question 4 | { $unwind: chemin}
{ $unwind: chemin} : chemin indique un champ, éventuellement dans un document imbriqué (via la syntaxe commençant avec $). Ce champ contient un tableau. L'opérateur produit un document pour chaque élément du tableau. Les documents produits sont identique au document de départ, sauf pour le champ désigné qui, au lieu du tableau, contient un élément de ce tableau.

Dans la collection grades, produire un document pour chaque élément du tableau contenu dans le champ scores (1241 résultats).

### Resultats :

```js
//Requête
db.grades.aggregate([
    {
        $unwind: "$scores"
    }
])

```


```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: {
    type: 'exam',
    score: 57.92947112575566
  }
}

```
```js
//Requête
db.grades.aggregate([
    {
        $unwind: "$scores"
    },
    {
        $count: "total_documents"
    }
])


```


```js
//Résultat
{
  total_documents: 1241
}

```


### Question 5 | { $group: { _id: spec id, champ1: { agg1: expr1 }, ...} }
{ $group: { _id: spec id, champ1: { agg1: expr1 }, ...} } (doc): regroupe les documents selon la valeur spec id, qui est une expression utilisant la syntaxe des expressions d'agrégation (chemins avec $, opérateurs d'expressions, etc). On peut fabriquer des documents, ce qui permet de représenter des n-uplets pour utiliser des combinaisons de valeurs comme clés. Pour chaque groupe, produit un document avec la valeur de _id, ainsi que chaque champ supplémentaire (champ1, etc) avec la valeur calculée par l'opérateur d'agrégation correspondant (agg1, etc) à qui on aura passé le tableau des valeurs calculées par les expressions (expr1, etc). Les fonctions d'agrégation utilisables sont indiquées dans la documentation. Par exemple { $group: { _id: "$state", total: { $sum: "$pop" }}} va créer un document pour chaque valeur du champ state avec un champ total qui sera la somme des champs pop des documents du groupe.

Dans la collection zips, donner pour chaque ville la population minimale (16584 résultats).

### Resultats :

```js
//Requête
db.zips.aggregate([
    {
        $group: {
            _id: "$city",
            min_population: { $min: "$pop" }
        }
    }
])


```


```js
//Résultat
{
  _id: 'CARLTON',
  min_population: 30
}

```
```js
//Requête
db.zips.aggregate([
    {
        $group: {
            _id: "$city",
            min_population: { $min: "$pop" }
        }
    },
    {
        $count: "total_cities"
    }
])


```


```js
//Résultat

{
  total_cities: 16584
}
```

### Question 6 | { $lookup: { from: coll, localField: champ local, foreignField: champ ext, as: champ out } }
{ $lookup: { from: coll, localField: champ local, foreignField: champ ext, as: champ out } } (doc): effectue une jointure avec coll. La jointure se fait sur une égalité entre le champ champ local pour les documents venant du pipeline courant et le champ champ ext pour les documents venant de la collection coll. Produit un document pour chaque document de la collection du pipeline, avec un champ supplémentaire (champ out) contenant le tableau des documents de coll satisfaisant le critère de jointure.

Effectuer une jointure entre grades et zips sur les champs student_id et pop. Quel est le type du résultat ?

### Resultats :

```js
//Requête
db.grades.aggregate([
    {
        $lookup: {
            from: "zips",
            localField: "student_id",
            foreignField: "pop",
            as: "matched_zips"
        }
    }
])

```


```js
//Résultat
{
  _id: ObjectId('50b59cd75bed76f46522c34e'),
  student_id: 0,
  class_id: 2,
  scores: [
    {
      type: 'exam',
      score: 57.92947112575566
    },
    {
      type: 'quiz',
      score: 21.24542588206755
    },
    {
      type: 'homework',
      score: 68.1956781058743
    },
    {
      type: 'homework',
      score: 67.95019716560351
    },
    {
      type: 'homework',
      score: 18.81037253352722
    }
  ],
    matched_zips: [
    {
      _id: '02163',
      city: 'CAMBRIDGE',
      loc: [
        -71.141879,
        42.364005
      ],
      pop: 0,
      state: 'MA'
    },
    {
      _id: '04013',
      city: 'BUSTINS ISLAND',
      loc: [
        -70.042247,
        43.79602
      ],
      pop: 0,
      state: 'ME'
    },
    {
      _id: '05405',
      city: 'UNIV OF VERMONT',
      loc: [
        -73.2002,
        44.477733
      ],
      pop: 0,
      state: 'VT'
    },
    .,
    .,
    .,
    {
      _id: '99773',
      city: 'SHUNGNAK',
      loc: [
        -157.613496,
        66.958141
      ],
      pop: 0,
      state: 'AK'
    }
  ]
}

```
```js
//Requête
db.grades.aggregate([
    {
        $lookup: {
            from: "zips",
            localField: "student_id",
            foreignField: "pop",
            as: "matched_zips"
        }
    },
    {
        $count: "total_documents"
    }
])


```


```js
//Résultat

{
  total_documents: 280
}
```
```js
//Question
Quel est le type du résultat ?
```


```js
//Reponse
Le résultat est un ensemble de documents MongoDB enrichis provenant de la collection grades. Chaque document comprend un champ supplémentaire appelé matched_zips, qui contient un tableau de documents de la collection zips correspondant à la condition de jointure. Si aucune correspondance n'est trouvée, le tableau matched_zips reste vide, mais les documents originaux de grades demeurent inchangés
```










## Partie 3 | Pipelines d'agrégation

### Question 1
Calculer le nombre de notes de chaque type.
Indice: Commencer par extraire les notes individuellement avec $unwind.
Indice: On peut calculer un nombre d'occurrences en sommant la valeur 1 sur un groupe.

### Resultats :

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: "$scores.type",
        count: { $sum: 1 }
    }}
])
```

```js
//Résultat
{
  _id: 'homework',
  count: 681
},
{
  _id: 'quiz',
  count: 280
},
{
  _id: 'exam',
  count: 280
}
```

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: "$scores.type",
        count: { $sum: 1 }
    }},
    { $count: "total_types" }
])

```

```js
//Résultat
{
  total_types: 3
}
```






### Question 2
Donner pour chaque matière, la meilleure note d'examen.

### Resultats :

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $match: { "scores.type": "exam" } },
    { $group: {
        _id: "$class_id",
        best_exam_score: { $max: "$scores.score" }
    }}
])

```

```js
//Résultat
[
    {
    _id: 25,
    best_exam_score: 88.80822542748272
    },
    {
    _id: 11,
    best_exam_score: 99.40117530792308
    },
    .,
    .,
    .,
    {
    _id: 6,
    best_exam_score: 99.49380951735357
    },
]
```
```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $match: { "scores.type": "exam" } },
    { $group: {
        _id: "$class_id",
        best_exam_score: { $max: "$scores.score" }
    }},
    { $count: "total_classes" }
])


```

```js
//Résultat
{
  total_classes: 31
}
```





### Question 3
Donner les dix villes les plus peuplées.
Indice: On pourra utiliser l'étape $limit.

### Resultats :

```js
//Requête
db.zips.aggregate([
    { $group: {
        _id: "$city",
        total_population: { $sum: "$pop" }
    }},
    { $sort: { total_population: -1 } },
    { $limit: 10 }
])

```

```js
//Résultat
[
    {
        _id: 'CHICAGO',
        total_population: 2452177
    },
    {
        _id: 'BROOKLYN',
        total_population: 2341387
    },
    {
        _id: 'HOUSTON',
        total_population: 2123053
    },
    .,
    .,
    .,
    {
        _id: 'DETROIT',
        total_population: 967468
    }

]

```

```js
//Requête
db.zips.aggregate([
    { $group: {
        _id: "$city",
        total_population: { $sum: "$pop" }
    }},
    { $sort: { total_population: -1 } },
    { $count: "total_cities" }
])

```

```js
//Résultat
{
  total_cities: 16584
}
```





### Question 4
Donner la population moyenne des villes pour chaque état.


### Resultats :

```js
//Requête
db.zips.aggregate([
    { $group: {
        _id: "$state",
        average_city_population: { $avg: "$pop" }
    }},
])

```

```js
//Résultat
[
    {
    _id: 'MS',
    average_city_population: 7088.749311294766
    },
    {
    _id: 'DC',
    average_city_population: 25287.5
    },
    {
    _id: 'UT',
    average_city_population: 8404.146341463415
    }
    .,
    .,
    .,
    {
    _id: 'WV',
    average_city_population: 2733.454268292683
    }
]

```

```js
//Requête
db.zips.aggregate([
    { $group: {
        _id: "$state",
        average_city_population: { $avg: "$pop" }
    }},
    { $count: "total_states" }
])

```

```js
//Résultat
{
  total_states: 51
}
```





### Question 5
Donner pour chaque matière, les étudiants ayant une note d'examen supérieure à 50.
Indice: On pourra utiliser l'opérateur d'agrégation $push dans un $group pour constituer la liste.


### Resultats :

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $match: { "scores.type": "exam", "scores.score": { $gt: 50 } } },
    { $group: {
        _id: "$class_id",
        students_with_high_scores: { $push: "$student_id" }
    }}
])

```

```js
//Résultat
[
    {
    _id: 25,
    students_with_high_scores: [
        3,
        34,
        37,
        45
    ]
    },
    {
    _id: 11,
    students_with_high_scores: [
        0,
        3,
        8,
        12,
        14,
        23,
        44
    ]
    },
    {
    _id: 3,
    students_with_high_scores: [
        3,
        9,
        20,
        30,
        37
    ]
    },
    .,
    .,
    .,
    {
    _id: 6,
    students_with_high_scores: [
        0,
        14,
        29,
        30,
        36,
        45
    ]
    }
]

```


```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $match: { "scores.type": "exam", "scores.score": { $gt: 50 } } },
    { $group: {
        _id: "$class_id",
        students_with_high_scores: { $push: "$student_id" }
    }},
    { $count: "total_states" }
])

```

```js
//Résultat
{
  total_states: 31
}
```






### Question 6
Donner pour chaque étudiant et chaque matière sa note générale. La note générale est la moyenne de chaque type de note. S'il y a plusieurs notes d'un même type, on prendra d'abord la moyenne de ce type avant de l'utiliser pour calculer la moyenne avec les autres types.
Indice: Commencer par calculer pour chaque étudiant, chaque matière et chaque type de note sa moyenne.


### Resultats :

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: {
            student_id: "$student_id",
            class_id: "$class_id",
            type: "$scores.type"
        },
        average_score_per_type: { $avg: "$scores.score" }
    }},
    { $group: {
        _id: {
            student_id: "$_id.student_id",
            class_id: "$_id.class_id"
        },
        overall_average: { $avg: "$average_score_per_type" }
    }},
    { $project: {
        _id: 0,
        student_id: "$_id.student_id",
        class_id: "$_id.class_id",
        general_score: "$overall_average"
    }}
])

```

```js
//Résultat
[
    {
    student_id: 29,
    class_id: 11,
    general_score: 61.86708165994667
    },
    {
    student_id: 38,
    class_id: 17,
    general_score: 41.39964272948872
    },
    {
    student_id: 8,
    class_id: 29,
    general_score: 53.41590432424875
    },
    .,
    .,
    .,
    {
    student_id: 9,
    class_id: 8,
    general_score: 25.805171079654958
    }
]

```

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: {
            student_id: "$student_id",
            class_id: "$class_id",
            type: "$scores.type"
        },
        average_score_per_type: { $avg: "$scores.score" }
    }},
    { $group: {
        _id: {
            student_id: "$_id.student_id",
            class_id: "$_id.class_id"
        },
        overall_average: { $avg: "$average_score_per_type" }
    }},
    { $project: {
        _id: 0,
        student_id: "$_id.student_id",
        class_id: "$_id.class_id",
        general_score: "$overall_average"
    }},
    { $count: "total_states" }
])

```

```js
//Résultat
{
  total_states: 280
}
```





### Question 7
Donner, pour chaque matière le type d'épreuve la mieux réussie. Le type d'épreuve la mieux réussie est celui ayant la meilleure moyenne, calculée sur l'ensemble des notes de ce type pour cette matière.
Indice: Un moyen de faire consiste à calculer la meilleure moyenne (via $max), tout en conservant l'ensemble des moyennes dans un tableau via $push.



### Resultats :

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: {
            class_id: "$class_id",
            type: "$scores.type"
        },
        average_score: { $avg: "$scores.score" }
    }},
    { $group: {
        _id: "$_id.class_id",
        averages: { $push: { type: "$_id.type", avg: "$average_score" } },
        best_average: { $max: "$average_score" }
    }},
    { $project: {
        _id: 0,
        class_id: "$_id",
        best_type: {
            $arrayElemAt: [
                "$averages",
                { $indexOfArray: ["$averages.avg", "$best_average"] }
            ]
        }
    }}
])

```

```js
//Résultat
[
    {
    class_id: 25,
    best_type: {
        type: 'quiz',
        avg: 48.595580262242
    }
    },
    {
    class_id: 11,
    best_type: {
        type: 'quiz',
        avg: 66.78875217934441
    }
    },
    {
    class_id: 3,
    best_type: {
        type: 'homework',
        avg: 52.578328231929646
    }
    },
    .,
    .,
    .,
    {
    class_id: 6,
    best_type: {
        type: 'exam',
        avg: 59.62558171984689
    }
]

```

```js
//Requête
db.grades.aggregate([
    { $unwind: "$scores" },
    { $group: {
        _id: {
            class_id: "$class_id",
            type: "$scores.type"
        },
        average_score: { $avg: "$scores.score" }
    }},
    { $group: {
        _id: "$_id.class_id",
        averages: { $push: { type: "$_id.type", avg: "$average_score" } },
        best_average: { $max: "$average_score" }
    }},
    { $project: {
        _id: 0,
        class_id: "$_id",
        best_type: {
            $arrayElemAt: [
                "$averages",
                { $indexOfArray: ["$averages.avg", "$best_average"] }
            ]
        }
    }},
    { $count: "total_classes" }
])


```

```js
//Résultat
{
  total_classes: 31
}
```

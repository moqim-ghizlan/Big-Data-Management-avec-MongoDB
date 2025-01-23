
# Big Data Management avec MongoDB

## Description
Ce projet illustre l'utilisation de MongoDB pour la gestion et l'analyse de grandes collections de données.
À travers des exemples concrets, il démontre des opérations comme l'agrégation, les projections, les jointures, et le tri pour explorer et analyser des jeux de données volumineux.

---

## Contenu du Projet
- **Exemples de Requêtes MongoDB** : Exemples détaillés utilisant `$match`, `$project`, `$group`, `$sort`, `$unwind`, et `$lookup`.
- **Manipulations de Données** : Techniques pour manipuler et transformer des données stockées dans des collections MongoDB.
- **Données** : Collections comme `grades` et `zips` utilisées pour les analyses.

---

## Installation et Prérequis

### Étape 1 : Cloner le Répertoire
Clonez le dépôt et accédez au répertoire :
```bash
git clone <url-du-dépôt>
cd big_data_management_nosql
```

### Étape 2 : Installer MongoDB
Assurez-vous que MongoDB est installé et en cours d'exécution sur votre machine. Importez les collections nécessaires avant de commencer.

### Étape 3 : Importer les Données
Utilisez la commande suivante pour importer les jeux de données dans MongoDB :
```bash
mongoimport --db database_name --collection collection_name --file datasets/data_file.json
```

---

## Exécution
Ouvrez votre shell MongoDB et exécutez les requêtes fournies pour explorer les fonctionnalités d'agrégation.

---

## Auteurs
- **Moqim Ghizlan**

---

## Licence
Ce projet est sous licence libre.

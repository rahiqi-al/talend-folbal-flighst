# Projet d’intégration de données : Analyse des liaisons aériennes mondiales

## Aperçu du projet
Ce projet vise à analyser les routes aériennes mondiales et la connectivité entre les aéroports à l'aide de Talend Studio. En construisant un pipeline d'intégration de données de bout en bout, nous transformons les données publiques pour fournir des informations sur les opérations aériennes.

### Sources de données
- **airports.dat** : Contient des informations de base sur les aéroports (nom, pays, coordonnées, etc.).
- **routes.dat** : Contient les routes aériennes reliant les aéroports (compagnie, aéroport source, aéroport de destination, etc.).

### Objectifs
1. Ingestion des fichiers CSV depuis GitHub.
2. Nettoyage et transformation des données.
3. Réalisation de jointures et enrichissement des données.
4. Enregistrement des données transformées dans une base de données .
5. Utilisation de variables de contexte pour passer facilement d’un environnement (DEV, TEST, PROD) à un autre.

---

## Étapes du pipeline

### Description des jobs

#### **Job 1: IngestFiles**
- Lire `airports.dat` et `routes.dat` avec `tFileInputDelimited`,'tFileFetch'.
- Afficher les données lues avec `tLogRow`.

#### **Job 2: CleanData**
- **Entrée** : `airports.dat`.
- **Transformations** :
  - Filtrer les lignes invalides (coordonnées manquantes, codes IATA absents).
  - Supprimer les espaces inutiles.
- **Sortie** :
  - Écrire les données nettoyées dans `Cleaned_Airports.csv`.
  - Écrire les lignes rejetées dans `Rejected_Rows.csv`.

#### **Job 3: TransformAndJoin**
- **Entrées** : `Cleaned_Airports.csv` et `routes.dat`.
- **Jointures** :
  - Associer les aéroports source et destination à leurs détails (ville, pays).
- **Transformations** :
  - Générer des colonnes : `SourceAirportCityCountry`, `DestinationAirportCityCountry`.
- **Sortie** : Écrire les données enrichies dans `Joined_Flights.csv`.

#### **Job 4: LoadIntoDB**
- **Processus** :
  - Se connecter à PostgreSQL ou MySQL .....
  - Créer la table `flights_enriched` si elle n’existe pas.
  - Insérer les données de `Joined_Flights.csv`.
- **Gestion des erreurs** :
  - Capturer les erreurs avec `tLogCatcher` et les enregistrer dans un fichier log.

#### **Job 5: AutomatePipeline**
- Utiliser `tRunJob` pour exécuter les jobs dans l’ordre suivant :
  - `IngestFiles`
  - `CleanData`
  - `TransformAndJoin`
  - `LoadIntoDB`

---

## Outils et technologies
- **Talend Studio** : Pour l’intégration des données et les processus ETL.
- **PostgreSQL/MySQL** : Comme base de données cible.
- **GitHub** : Pour accéder aux jeux de données publics.

---

## Instructions d’utilisation
1. Clonez le répertoire du projet depuis GitHub.
2. Ouvrez Talend Studio et importez les jobs fournis.
3. Configurez les variables de contexte pour votre environnement.
4. Exécutez le job `AutomatePipeline` pour automatiser tout le pipeline.
5. Vérifiez les résultats dans la base de données ou les fichiers de sortie.

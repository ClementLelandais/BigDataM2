# GitHub Health Score

## Présentation

GitHub Health Score est une plateforme d'analyse de repositories GitHub permettant :

- d'identifier les projets potentiellement abandonnés ;
- d'estimer le potentiel futur d'un repository ;
- de détecter des dépendances open-source à risque ;
- de calculer un score de santé global compris entre 0 et 100 ;
- d'expliquer automatiquement les prédictions du modèle.

Le projet combine :

- Data Engineering
- Big Data
- Machine Learning
- MLOps

---

# Problématique

Lorsqu'un projet logiciel dépend de bibliothèques open-source externes, il est important de détecter rapidement :

- un arrêt de maintenance ;
- une baisse importante de l'activité ;
- une chute du nombre de contributeurs ;
- l'absence de nouvelles releases.

À l'inverse, il est également intéressant d'identifier des projets émergents présentant des signes de forte croissance :

- augmentation rapide des étoiles GitHub ;
- croissance du nombre de contributeurs ;
- augmentation des forks ;
- activité de développement soutenue.

L'objectif est donc de construire un système capable :

1. de surveiller automatiquement l'activité des repositories GitHub ;
2. de stocker leur historique ;
3. de calculer des métriques pertinentes ;
4. d'entraîner des modèles de Machine Learning ;
5. de produire un score de santé interprétable.

---

# Architecture

```text
GitHub GraphQL API
        │
        ▼
GitRepo Collector
        │
        ▼
PostgreSQL
        │
        ▼
Feature Builder
        │
        ▼
Feature Store
        │
        ▼
Machine Learning
        │
        ▼
Health Score API
        │
        ▼
Dashboard
```

---

# Organisation du projet

```text
github-health-score/

├── docker-compose.yml

├── collector/
│   ├── main.py
│   ├── github_client.py
│   ├── queries.py
│   └── requirements.txt

├── database/
│   ├── init.sql
│   └── postgres.py

├── feature_builder/
│   ├── build_features.py
│   └── requirements.txt

├── ml/
│   ├── train.py
│   ├── evaluate.py
│   ├── predict.py
│   └── requirements.txt

├── api/
│   ├── main.py
│   └── requirements.txt

├── dashboard/
│   ├── app.py
│   └── requirements.txt

├── models/

├── data/

└── README.md
```

---

# Technologies utilisées

## Collecte

- GitHub GraphQL API
- Requests

## Stockage

- PostgreSQL

## Traitement

- Pandas
- NumPy

## Big Data

- Apache Spark
- PySpark

## Machine Learning

- Scikit-Learn
- XGBoost

## Explainability

- SHAP

## API

- FastAPI

## Visualisation

- Streamlit

## Conteneurisation

- Docker
- Docker Compose

## MLOps

- MLflow
- GitHub Actions

---

# Données collectées

Chaque repository est décrit par :

| Variable | Description |
|-----------|------------|
| stars | Nombre d'étoiles GitHub |
| forks | Nombre de forks |
| issues | Nombre d'issues |
| pull requests | Nombre de PR |
| commits | Nombre de commits |
| contributors | Nombre de contributeurs |
| releases | Nombre de releases |
| created_at | Date de création |
| pushed_at | Dernière activité |

---

# Historisation

Le système enregistre régulièrement des snapshots.

Exemple :

```text
repository_id

snapshot_date

stars
forks
contributors

commits_30d
commits_90d
commits_365d
```

Ces snapshots permettent de calculer :

- la croissance du repository ;
- les tendances ;
- les métriques temporelles.

---

# Features construites

## Activité

```text
days_since_last_commit
commits_per_day
commits_90d
commits_365d
```

## Popularité

```text
stars
forks
watchers
```

## Croissance

```text
star_growth_30d
star_growth_90d

fork_growth_30d
fork_growth_90d

contributor_growth
```

## Fiabilité

```text
issue_resolution_rate
pr_acceptance_rate
release_frequency
```

## Tendance technologique

```text
language_trend_score
```

---

# Labels

Le problème est formulé comme une classification multiclasses.

## Classe 0

```text
ABANDONED
```

Critères possibles :

- aucun commit récent ;
- aucune release récente ;
- faible activité.

## Classe 1

```text
STABLE
```

Activité normale.

## Classe 2

```text
PROMISING
```

Croissance importante :

- étoiles ;
- contributeurs ;
- forks ;
- activité.

---

# Machine Learning

## Baseline

Régression logistique :

```text
Logistic Regression
```

## Modèle principal

```text
Random Forest
```

Justification :

- robuste ;
- explicable ;
- performant sur données tabulaires.

## Modèle avancé

```text
XGBoost
```

Comparaison :

- Accuracy
- Precision
- Recall
- F1-score

---

# Health Score

Le modèle produit un score :

```text
0 → 100
```

Interprétation :

| Score | Catégorie |
|---------|----------|
| 0-25 | Abandonné |
| 25-50 | Risque élevé |
| 50-75 | Stable |
| 75-100 | Prometteur |

---

# Explainability

Les prédictions sont expliquées avec SHAP.

Exemple :

```text
Health Score = 87

+ activité importante
+ croissance des stars
+ nombreux contributeurs

- faible fréquence de release
```

---

# API

Exemple :

```http
POST /predict
```

Entrée :

```json
{
  "stars": 15000,
  "forks": 3000,
  "days_since_last_commit": 5
}
```

Réponse :

```json
{
  "health_score": 91,
  "category": "PROMISING"
}
```

---

# Dashboard

Le dashboard permet :

- visualisation des meilleurs repositories ;
- évolution des métriques ;
- détection des projets à risque ;
- consultation des prédictions.

---

# Répartition de l'équipe

## Abart Raphael

Data Engineering

- GitHub GraphQL
- Docker
- PostgreSQL

## Lelandais Clément

Big Data

- Spark
- PySpark
- Feature Engineering

## Ouedraogo Inoussa

Machine Learning / MLOps

- Random Forest
- XGBoost
- MLflow
- FastAPI

---

# Objectif final

Construire une plateforme capable d'analyser automatiquement plusieurs dizaines de milliers de repositories GitHub et d'estimer leur santé ainsi que leur potentiel futur à l'aide de techniques modernes de Data Engineering, Big Data, Machine Learning et MLOps.

# Projet Scoring Crédit (Default Payment)

Ce projet vise à construire un **score d’octroi de crédit** pour estimer la probabilité de défaut de paiement le mois suivant.
Il s’appuie sur le dataset **Default of Credit Card Clients (Taiwan, 2005)** et suit une démarche complète : exploration, feature engineering, sélection de variables et modélisation par régression logistique.

## Objectifs

- Comprendre le profil des clients à risque
- Identifier les variables explicatives du défaut de paiement
- Construire un modèle prédictif interprétable.
- Produire un score exploitable pour la décision d’octroi.

## Structure du projet

- `1_Construire_Un_Score.ipynb` : cadrage, compréhension des données, exploration initiale
- `2_Feature_Engineering_Selction_Variables.ipynb` : préparation des données, regroupements de modalités, analyse de corrélation et sélection des variables
- `3_Regression_Logistique.ipynb` : encodage, split train/test, entraînement et évaluation du modèle logistique
- `UCI_Credit_Card.csv` : données source

## Données

Variable cible :
- `default.payment.next.month` (1 = défaut, 0 = non défaut)

Exemples de variables explicatives :
- Démographie : `SEX`, `EDUCATION`, `MARRIAGE`, `AGE`
- Historique de paiement : `PAY_0`, `PAY_2`, `PAY_3`, `PAY_4`, `PAY_5`, `PAY_6`
- Montants : `BILL_AMT1` à `BILL_AMT6`, `PAY_AMT1` à `PAY_AMT6`

## Environnement recommandé

- Python 3.10+
- Jupyter Notebook / JupyterLab

Bibliothèques utilisées dans les notebooks :
- `pandas`, `numpy`
- `matplotlib`, `seaborn`, `missingno`
- `scikit-learn`, `statsmodels`, `scipy`

## Installation

Depuis la racine du projet :

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1

pip install -U pip
pip install pandas numpy matplotlib seaborn missingno scikit-learn statsmodels scipy jupyter
```

## Exécution

Lancer Jupyter :

```bash
jupyter notebook
```

Exécuter les notebooks dans l’ordre :

1. `1_Construire_Un_Score.ipynb`
2. `2_Feature_Engineering_Selction_Variables.ipynb`
3. `3_Regression_Logistique.ipynb`

## Zoom sur la Partie 2 (Feature Engineering & Sélection de Variables)

Le notebook `2_Feature_Engineering_Selction_Variables.ipynb` contient une phase d’analyse approfondie, utile pour justifier les variables retenues dans le modèle final.

### 1) Préparation et recodage métier

- Regroupement des modalités rares de `EDUCATION` et `MARRIAGE` en `Autres`.
- Harmonisation des statuts de paiement (`PAY_0`, `PAY_2`, ..., `PAY_6`) en classes plus lisibles (`Paiement à temps` / `Retard`).
- Conversion des variables qualitatives au format `category`.

### 2) Relations entre variables quantitatives

- Matrices de corrélation selon trois approches : **Pearson**, **Spearman**, **Kendall**.
- Visualisation de toutes les paires quantitatives via des **nuages de points**.
- Objectif : repérer dépendances, redondances potentielles et signaux utiles pour la modélisation.

### 3) Relations entre variables qualitatives

- Calcul de l’association entre variables catégorielles avec :
	- **Test du Khi-deux** (indépendance)
	- **V de Cramer** (force d’association)
	- **T de Tschuprow** (variante robuste pour tableaux non carrés)
- Visualisations associées :
	- **Barplots croisés** pour les paires de variables
	- **Heatmaps** des matrices V de Cramer / T de Tschuprow

### 4) Relations Quali ↔ Quanti

- Génération de **boxplots** variable quantitative vs variable qualitative.
- Choix du test statistique selon le cas :
	- **T-test** quand la variable qualitative a 2 modalités
	- **Kruskal-Wallis** quand elle a plus de 2 modalités
- Vérification de la normalité via **Shapiro**.

### 5) Classement des variables quantitatives

- Classement par pertinence vis-à-vis de la cible `default.payment.next.month` via **p-valeurs Kruskal-Wallis**.
- Approche complémentaire par tests pair-à-pair :
	- **Student (t-test)**
	- **Mann-Whitney U**
- Restitution sous forme de tableaux triés + graphiques de classement.

### 6) Sélection supervisée avec Random Forest

- Découpage des données en train/test (**stratifié**).
- Encodage des variables via `ColumnTransformer` + `OneHotEncoder`.
- Entraînement d’un `RandomForestClassifier` pour calculer les **feature importances**.
- Sélection initiale des variables dont l’importance est supérieure à la moyenne.
- Visualisation des variables les plus importantes (Top N).

## Pipeline de modélisation (résumé)

1. **Chargement et contrôle qualité** : import des données, vérification des types et des valeurs manquantes.
2. **Prétraitement métier** : regroupement de modalités rares (`EDUCATION`, `MARRIAGE`) et simplification des statuts de paiement.
3. **Feature Engineering** : transformation des variables catégorielles, analyses de corrélation/visualisations.
4. **Préparation modèle** : split stratifié train/test, encodage One-Hot des variables qualitatives.
5. **Modélisation** : régression logistique (interprétable) + évaluation de la performance

## Critères de sélection retenus dans le projet

La sélection des variables s’appuie sur une combinaison de :

- Significativité statistique (Khi-deux, T-test, Mann-Whitney, Kruskal-Wallis)
- Force d’association (V de Cramer / T de Tschuprow)
- Importance supervisée (Random Forest)
- Lecture métier (interprétabilité et cohérence risque crédit)

## Résultats attendus

- Estimation de la probabilité de défaut par client
- Indicateurs de performance (accuracy, precision, recall, F1, courbe ROC/AUC)
- Base pour construire des règles d’acceptation/refus ou un cut-off de score

## Limites et vigilance

- Certaines étapes du notebook 2 sont exploratoires et peuvent être redondantes (itérations successives de tests).
- Les résultats de sélection peuvent varier selon :
	- le découpage train/test,
	- les seuils de p-valeurs,
	- les hyperparamètres du modèle de forêt aléatoire.
- La sélection finale doit être confirmée par des performances hors échantillon et une validation métier.


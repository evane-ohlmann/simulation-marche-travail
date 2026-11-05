# Simulation du marché du travail sur 20 ans

Simulation multi-agents du marché du travail en Python, avec un scénario de choc économique de type Covid.
Le modèle fait évoluer chaque année une population d'environ 30 000 individus et 40 entreprises réparties en quatre secteurs, à partir de règles simples d'offres d'emploi, de recrutement et de départs.

> Projet réalisé en binôme par **Evane Ohlmann** et **Aissatou Ndiaye**.

---

## Objectifs

- Modéliser un marché du travail simplifié en **programmation orientée objet**.
- Projeter l'emploi et le chômage sur 20 ans sous un **scénario de stress économique**.
- Analyser les dynamiques globales, sectorielles et individuelles à l'aide de visualisations.

## Le modèle

**Les agents**

| Classe | Rôle | Principaux attributs |
|---|---|---|
| `Personne` | un individu | statut d'emploi, années d'expérience, entreprise |
| `Entreprise` | un employeur | secteur, effectif, nombre d'offres |
| `OffreEmploi` | un poste ouvert | entreprise, secteur, expérience minimale |

**Les hypothèses par secteur**

| Secteur | Offres ouvertes par an (part de l'effectif) | Expérience minimale |
|---|---|---|
| Agriculture | 3 % | 0 an |
| Industrie | 5 % | 1 an |
| Construction | 7 % | 2 ans |
| Tertiaire | 10 % | 3 ans |

**Le déroulement d'une année**

1. Chaque entreprise ouvre des postes : effectif × coefficient du secteur × facteur économique.
2. Les chômeurs postulent, dans un ordre aléatoire, à une offre compatible avec leur expérience.
3. Une partie des salariés quitte son entreprise : taux de départ de 5 % en temps normal, jusqu'à 15 % en crise forte.
4. Les personnes en emploi gagnent un an d'expérience.
5. Les indicateurs sont enregistrés.

**Le scénario économique**

| Années | Facteur économique | Phase |
|---|---|---|
| 1 à 3 | 1,00 | Économie normale |
| 4 à 6 | 0,70 → 0,30 | Choc économique |
| 7 à 10 | 0,45 → 0,85 | Reprise progressive |
| 11 à 20 | 0,90 → 1,00 | Stabilisation |

Le scénario est défini dans un seul dictionnaire (`FACTEURS_ANNEE`) : tester une autre crise ne demande pas de modifier le moteur de simulation.

## Principaux résultats

*Résultats obtenus avec la graine aléatoire `random.seed(42)`.*

| Indicateur | Valeur |
|---|---|
| Taux de chômage initial | 13,2 % |
| Taux de chômage avant la crise (année 3) | 10,2 % |
| Pic du chômage (année 9) | 35,3 % |
| Taux de chômage final (année 20) | 10,4 % |
| Baisse des offres d'emploi pendant le choc (années 3 à 6) | −73 % |

- **Le pic du chômage est décalé de 3 ans** par rapport au creux de la conjoncture : l'emploi continue de baisser tant que les départs dépassent les embauches.
- **La reprise est plus lente que la crise** : 6 ans pour atteindre le pic, 11 ans pour revenir au niveau initial.
- **Les secteurs réagissent différemment** selon l'écart entre leur taux d'ouverture de postes et le taux de départ : sur 20 ans, le tertiaire progresse de 11 %, tandis que la construction perd 22 %, l'industrie 43 % et l'agriculture 46 %.
- **Le manque d'expérience crée un cercle vicieux** : sans emploi, on ne gagne pas d'expérience, et sans expérience, on accède à peu d'offres.

## Limites

- Le modèle **amplifie les chocs** : un même facteur réduit les offres et augmente les départs, sans mécanisme d'amortissement.
- **L'arrondi sur les petits effectifs** fige l'emploi agricole en fin de simulation (0 offre et 0 départ calculés pour les entreprises d'une quinzaine de salariés).
- Les résultats reposent sur **une seule réalisation aléatoire**.

## Pistes d'amélioration

- Lancer la simulation plusieurs centaines de fois (**méthode de Monte-Carlo**) pour estimer la distribution du taux de chômage et un intervalle de confiance.
- Remplacer l'arrondi par un **tirage aléatoire** (loi binomiale) du nombre d'offres et de départs.
- Calibrer les paramètres sur des **données réelles** (INSEE, Dares).
- Ajouter l'âge et les sorties définitives (retraite) pour obtenir un modèle démographique plus complet.

## Compétences mises en œuvre

- **Programmation orientée objet** : classes, attributs d'instance et de classe, méthodes, encapsulation, composition.
- **Structures de données** : listes, dictionnaires, compréhensions de listes et de dictionnaires.
- **Simulation stochastique** reproductible (graine aléatoire fixée).
- **Contrôles de cohérence** automatiques avec `assert`.
- **Visualisation** avec matplotlib : indice base 100, comparaison avant / après crise.

## Lien avec l'actuariat

La structure de ce projet est celle d'un modèle de projection actuariel :

| Dans ce projet | En actuariat |
|---|---|
| Population d'individus et d'entreprises | Portefeuille de contrats (*model points*) |
| Entrées et sorties d'emploi chaque année | Affaires nouvelles, décès, rachats, résiliations |
| Projection annuelle sur 20 ans | Projection des flux d'un portefeuille (ALM, Best Estimate) |
| Scénario de choc économique | Stress tests Solvabilité II et ORSA |
| Paramètres séparés du moteur de calcul | Hypothèses séparées du modèle (mortalité, rachats) |

## Utilisation

```bash
pip install -r requirements.txt
jupyter notebook simulation_marche_travail.ipynb
```

Exécuter toutes les cellules dans l'ordre (*Kernel > Restart & Run All*).

## Structure du dépôt

```
├── README.md
├── requirements.txt
└── simulation_marche_travail.ipynb
```

# Réseau de centres de santé – Itinéraires et tournées de livraison (Java)

Projet de groupe réalisé en 1re année de BUT Informatique (semestre 2), IUT Lyon 1.

## Objectif

Application Java avec interface graphique qui modélise le réseau routier d'un district sous forme de **graphe**. Les sommets sont des centres de santé (maternités, blocs opératoires, centres de nutrition) et les arêtes sont des routes. L'application calcule des itinéraires et des tournées de livraison en tenant compte de la durée et de la fiabilité des routes.

## Fonctionnalités

- **Création du district** : chargement d'un fichier CSV ou génération aléatoire d'un réseau de *n* centres. La génération construit d'abord un arbre couvrant, ce qui garantit que le réseau est connexe.
- **Affichage du graphe** avec GraphStream, avec une couleur par type de centre.
- **Routes risquées** : mise en évidence des routes dont la fiabilité est inférieure à un seuil choisi.
- **Itinéraires** entre deux centres :
  - le plus rapide, en durée standard (algorithme de **Dijkstra**) ;
  - le plus sûr, en durée estimée qui pénalise les routes peu fiables. Les routes de fiabilité inférieure à 2,5/10 sont interdites.
- **Tournées de livraison** depuis le centre S1, vers tous les centres ou vers un type de centre :
  - matrice des plus courts chemins entre tous les centres (**Floyd-Warshall**) ;
  - tournée construite par un algorithme **glouton** (plus proche voisin) ;
  - amélioration de la tournée par l'algorithme **2-opt** ;
  - répartition entre **deux camions** : la grande tournée est découpée à l'endroit qui minimise la durée du camion le plus lent.
- **Fiche d'un centre** (clic droit sur un sommet) : centre le plus proche de chaque type, distances et durées vers les autres centres.

## Format des données

Les fichiers du dossier `data/` décrivent une matrice d'adjacence. Chaque ligne donne l'identifiant et le type d'un centre (`M`, `O` ou `N`), puis une cellule par centre : `0` s'il n'y a pas de route, sinon un triplet `fiabilité,distance,durée`.

```
S1;M;0;6,9,57;1,55,55;...
```

Fiabilité sur 10, distance en km, durée en minutes. Le dossier contient des graphes de 10 à 150 centres, ainsi que des fichiers volontairement erronés (`grapheBug*.csv`) pour tester la robustesse du chargement.

## Organisation du code

| Dossier / fichier | Rôle |
|---|---|
| `src/projet_sae/Graphe.java` | Chargement, génération, plus courts chemins, tournées (glouton, 2-opt, deux camions) |
| `src/projet_sae/CentreSante.java` | Classe abstraite d'un centre, avec les sous-classes `Maternite`, `BlocOperatoire` et `Nutrition` |
| `src/projet_sae/Route.java` | Route entre deux centres : fiabilité, distance, durée, durée estimée |
| `src/IHM/` | Interface graphique Swing : fenêtre d'accueil et fenêtre de traitement |
| `test/GrapheTest.java` | 20 tests unitaires JUnit |
| `data/` | Jeux de données CSV |

## Lancer le projet

Le projet est un projet NetBeans (Ant).

- **Avec NetBeans** : ouvrir le dossier `Projet_SAE`, puis *Run Project*.
- **Avec le JAR** : `java -jar dist/Projet_SAE.jar`, lancé depuis le dossier `Projet_SAE` pour que le dossier `data/` soit trouvé.
- **Tests** : *Test Project* dans NetBeans (JUnit 4).

Bibliothèques utilisées : GraphStream 1.3 (`lib/`) et JUnit 4.12 (`Junit/`).

## Compétences mises en œuvre

Programmation orientée objet en Java (héritage, classe abstraite), théorie des graphes (Dijkstra, Floyd-Warshall), heuristiques d'optimisation (glouton, 2-opt), interface graphique Swing, tests unitaires JUnit, documentation Javadoc.

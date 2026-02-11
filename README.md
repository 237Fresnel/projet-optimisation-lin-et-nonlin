# Planification Optimale de Centrales de Production Électrique

## Description du projet

Ce projet traite de la planification optimale de la production de quatre centrales électriques sur un horizon de 8 tranches horaires.

L'objectif est de minimiser le coût total de production tout en respectant :

- Les contraintes de capacité des centrales
- La satisfaction de la demande horaire
- Une contrainte de réserve de 10%
- Les contraintes techniques de démarrage et d’arrêt (dans la seconde modélisation)

Deux approches d’optimisation ont été développées et comparées :

1. Optimisation non linéaire continue (SQP – fmincon)
2. Programmation Linéaire Mixte (MILP – intlinprog) avec linéarisation par morceaux

---

## 1. Première Approche : Optimisation Non Linéaire (SQP)

### 🔹 Modélisation

Variables :
- \( P_{j,i} \) : puissance produite par la centrale i à l’instant j

Fonction objectif :
Minimisation du coût quadratique de production :

Cf_i(P) = a0_i + a1_i P^{1.1} + a2_i P^2

Contraintes :
- Bornes de production
- Satisfaction de la demande
- Réserve de 10%

### 🔹 Solveur utilisé

- `fmincon` (algorithme SQP)
- 50 itérations
- 101 évaluations
- Coût optimal : **120 529.3**
- Convergence satisfaisante (First-order optimality = 3.4e-5)
- Temps de calcul : 0.1527 s

---

## 2. Seconde Approche : Modélisation avec Arrêts et Démarrages (MILP)

### 🔹 Extensions du modèle

Ajout de :

- Variables binaires d’état \( x_{j,i} \)
- Variables de démarrage \( y_{j,i} \)
- Coûts fixes
- Coûts de démarrage
- Temps minimum de fonctionnement (tup)
- Temps minimum d’arrêt (tdown)

Le problème devient un **MIQP** (Mixed-Integer Quadratic Programming).

### 🔹 Transformation

Pour utiliser `intlinprog`, la fonction quadratique est linéarisée par approximation linéaire par morceaux.

Le problème devient un **MILP** résolu par Branch & Bound.

### 🔹 Résultats

- Coût optimal : **118 976.25**
- Gap relatif : **0.00% (optimalité globale prouvée)**
- Temps de calcul : 0.0498 s

Gain économique : **1.3%** par rapport au modèle continu.

---

## Analyse des résultats

### 🔹 Comportement des centrales

- **C1 et C2** : centrales de base (fonctionnement continu)
- **C3 et C4** : centrales de pointe (activation stratégique)

### 🔹 Optimisation "au plus juste"

- Respect strict des contraintes de réserve
- Activation minimale des unités coûteuses
- Évitement des cycles inutiles marche/arrêt

### 🔹 Structure des coûts

- 90% coûts variables
- 9.7% coûts fixes
- 0.3% coûts de démarrage

---

## Comparaison des Approches

| Méthode | Type | Optimalité | Coût |
|----------|--------|------------|--------|
| SQP | Non linéaire continu | Minimum local | 120 529.3 |
| MILP | Mixte (global) | Optimum global prouvé | 118 976.25 |

La méthode MILP garantit l’optimalité globale et fournit une planification plus réaliste.

---

## Structure du projet

---

## Outils utilisés

- MATLAB
- fmincon (SQP)
- intlinprog (MILP)
- Approximation linéaire par morceaux
- Branch & Bound

---

## Contexte académique

Projet réalisé dans le cadre du cours :
**Optimisation Linéaire et Non Linéaire**

---

## Auteurs

- KENGNE Fresnel  
- SIBEFEU Emmanuel  

---

## Perspectives d’amélioration

- Intégration d’incertitude (optimisation robuste ou stochastique)
- Ajout de contraintes de rampe
- Modélisation multi-journalière
- Intégration d’énergies renouvelables

---

## Conclusion

L’intégration des décisions discrètes (arrêts/démarrages) permet une réduction significative des coûts et garantit l’optimalité globale.  
L’approche MILP s’avère supérieure pour une planification réaliste et robuste du système électrique.



# PINN-Fourier-Lorenz – Reconstruction de dynamiques chaotiques à partir de mesures partielles

**Réseaux de neurones informés par la physique (PINNs) appliqués au système de Lorenz – de la solution directe à la reconstruction avec un seul capteur**

[![Licence: MIT](https://img.shields.io/badge/Licence-MIT-yellow.svg)](LICENCE)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-1.12%2B-red)

---

## 📌 Présentation du projet

Ce dépôt contient les codes et les résultats de mon projet de recherche de Master 1 (IMDS, UHA) sur l’utilisation des **réseaux de neurones informés par la physique** pour résoudre des équations différentielles ordinaires, avec une application au **système chaotique de Lorenz**.

Deux architectures sont comparées :
- **PINN standard** : un perceptron multicouche (MLP) classique recevant le temps en entrée.
- **PINN avec Fourier Features** : le même MLP, mais l’entrée est enrichie par des fonctions sinusoïdales de fréquences multiples (1, 2, 4, 8, 16, 32) pour lutter contre le biais spectral des réseaux à activation \(\tanh\).

Deux types de problèmes sont étudiés :
1. **Problème direct** : prédire l’état complet \((x(t),y(t),z(t))\) à partir de 500 points de référence issus d’un solveur RK45.
2. **Problèmes inverses** : reconstruire la dynamique complète en n’observant qu’**une seule** variable (soit \(x\), soit \(y\), soit \(z\)), ce qui simule un unique capteur météorologique.

---

## 🎯 Résultats clés

| Problème | Modèle | Erreur L2 globale |
|----------|--------|-------------------|
| **Direct** (toutes les variables) | PINN standard | 0,105 |
| | **PINN-Fourier** | **0,061** (−42 %) |
| **Inverse : observation de \(Z\)** | PINN standard | 0,235 |
| | PINN-Fourier | 0,206 |
| **Inverse : observation de \(Y\)** | PINN standard | 0,251 |
| | **PINN-Fourier** | **0,197** (meilleur) |
| **Inverse : observation de \(X\)** | PINN standard | 0,277 |
| | PINN-Fourier | 0,215 |

- Le PINN enrichi par des Fourier Features surpasse systématiquement le PINN standard.
- La variable \(y\) est la plus informative pour la reconstruction, \(x\) la moins.
- Même avec les Fourier Features, **l’observabilité partielle** reste une limitation fondamentale (les variables cachées divergent à long terme).

---

## 🧠 Méthodologie

- **Données** : 500 points de référence issus d’un solveur RK45 + 1000 points de collocation pour le résidu des équations ODE.
- **Architecture** : 4 couches cachées de 256 neurones, fonction d’activation \(\tanh\), sortie linéaire (indispensable pour une régression sans borne).
- **Fonction de perte** :  
  \(\mathcal{L} = \lambda_{\text{data}}\,\mathcal{L}_{\text{data}} + \lambda_{\text{ODE}}\,\mathcal{L}_{\text{ODE}}\)  
  Les résidus sont normalisés par \(\sigma_u/T\) afin d’équilibrer les contributions des trois variables.
- **Optimisation** : apprentissage par curriculum (pré‑apprentissage supervisé, puis augmentation progressive de \(\lambda_{\text{ODE}}\), enfin raffinement par L‑BFGS).

---

## 🌍 Importance industrielle et climatique

- **Météorologie / Climat** : reconstruire des champs atmosphériques complets à partir d’un seul capteur de vent ou de température. Améliore la prévision d’événements extrêmes (cyclones, canicules).
- **Industrie** : surveiller des processus chaotiques (réacteurs chimiques, turbines, réseaux électriques) avec peu de capteurs → réduction des coûts et maintenance prédictive.
- **Assimilation de données** : fusionner des modèles physiques avec des observations partielles.

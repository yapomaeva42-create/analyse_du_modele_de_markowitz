# analyse_du_modele_de_markowitz

<div align="center">

# Optimisation de portefeuille (Markowitz) avec Python  
## Pré-COVID vs COVID vs Post-COVID — Univers CAC 40 (Euronext Paris)

**Question de recherche :** *dans quelle mesure le choc COVID-19 (2020) fragilise les résultats du modèle mean–variance de Markowitz ?*

</div>

---

## Présentation

Ce dépôt contient une implémentation complète (Python) d’une stratégie de portefeuille inspirée du modèle de Markowitz, appliquée à un univers d’actions du CAC 40.  
L’objectif est de comparer le comportement du modèle sur trois régimes de marché (stabilité, crise, reprise), puis d’étudier l’impact de différents profils de risque, y compris une approche adaptative.

---

## Contenu du dépôt

- `markowitz analysis.ipynb` : notebook principal (préparation des données, optimisation, backtest, graphiques)
- `markowitz_covid_comparison.pdf` : rapport de comparaison (figures et métriques sur trois périodes)
- `PIT YAPO BOTOH.pdf` : mémoire / PIT (méthodologie, extensions, discussion des résultats)

---

## Univers d’actifs (10 actions)

Tickers Euronext Paris :

`AIR.PA, EL.PA, RMS.PA, KER.PA, OR.PA, MC.PA, SAF.PA, SAN.PA, SU.PA, TTE.PA`

---

## Paramètres (baseline)

- Fenêtre d’estimation : `window_size = 252` (environ 1 an de bourse)
- Rebalancement : `rebalance_frequency = 21` (environ mensuel)
- Contrainte de risque : `max_volatility = 0.20` (20 %)
- Valeur initiale : `initial_value = 100`

---

## Résultats — 3 régimes de marché (σmax = 20 %)

| Période | Rendement annualisé | Volatilité annualisée | Sharpe | Max drawdown |
|---|---:|---:|---:|---:|
| Pré-COVID (2016–2019) | 26.49% | 20.72% | 1.28 | -24.69% |
| COVID (2020) | -3.30% | 30.63% | -0.11 | -32.69% |
| Post-COVID (2021–2023) | 10.54% | 21.29% | 0.50 | -35.78% |

---

## Extension (PIT) — Profils de risque et stratégie adaptative

Profils testés :
- Conservateur : 10 %
- Modéré : 15 %
- Équilibré : 20 %
- Agressif : 30 %
- Adaptatif : volatilité cible ajustée selon la volatilité de marché

### Résumé (rendements annualisés)

| Profil | Pré-COVID | COVID | Post-COVID |
|---|---:|---:|---:|
| Conservateur | 12.80% | -4.39% | 16.28% |
| Modéré | 17.65% | 0.15% | 13.59% |
| Équilibré | 25.01% | -2.13% | 12.18% |
| Agressif | 26.02% | 13.58% | 12.90% |
| Adaptatif | 26.76% | 23.58% | 15.99% |

---

## Méthodologie (détails)

<details>
<summary><b>Déplier la méthode (rolling, optimisation, backtest)</b></summary>

### 1) Estimation rolling (µ, Σ)
À chaque date de rebalancement :
- calcul des rendements journaliers
- estimation de µ annualisé
- estimation de la covariance Σ annualisée
- fenêtre glissante de 252 séances

### 2) Optimisation Markowitz (σ-problem)
Objectif : maximiser le rendement espéré sous contraintes :
- ∑ wᵢ = 1
- 0 ≤ wᵢ ≤ 1 (long-only)
- volatilité du portefeuille ≤ σmax  
Résolution via SLSQP (`scipy.optimize.minimize`).

### 3) Backtest
- rebalancement environ mensuel (21 séances)
- recalcul des poids à chaque rebalancement
- suivi de la valeur du portefeuille, des poids et des drawdowns

</details>

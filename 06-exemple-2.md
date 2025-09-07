
# Tableau 0 — Vue d’ensemble

| Total | Oui | Non | p(oui) | p(non) |       H(S) |
| ----: | --: | --: | -----: | -----: | ---------: |
|    14 |   9 |   5 | 0.6429 | 0.3571 | **0.9403** |

---

# Racine — évaluation des attributs

## Tableau 1 — Split par `temps`

| Valeur    | Total | Oui | Non | p(oui) | H(sous-groupe) | Poids (=Total/14) |    Poids×H |
| --------- | ----: | --: | --: | -----: | -------------: | ----------------: | ---------: |
| ensoleilé |     5 |   2 |   3 | 0.4000 |         0.9710 |            0.3571 |     0.3468 |
| nuageux   |     4 |   4 |   0 | 1.0000 |         0.0000 |            0.2857 |     0.0000 |
| pluvieux  |     5 |   3 |   2 | 0.6000 |         0.9710 |            0.3571 |     0.3468 |
|           |       |     |     |        |                |      **H\_après** | **0.6935** |
|           |       |     |     |        |                |            **IG** | **0.2467** |

## Tableau 2 — Split par `humidité`

| Valeur  | Total | Oui | Non | p(oui) |      H |        Poids |    Poids×H |
| ------- | ----: | --: | --: | -----: | -----: | -----------: | ---------: |
| haute   |     7 |   3 |   4 | 0.4286 | 0.9852 |       0.5000 |     0.4926 |
| normale |     7 |   6 |   1 | 0.8571 | 0.5917 |       0.5000 |     0.2958 |
|         |       |     |     |        |        | **H\_après** | **0.7885** |
|         |       |     |     |        |        |       **IG** | **0.1518** |

## Tableau 3 — Split par `vent`

| Valeur | Total | Oui | Non | p(oui) |      H |        Poids |    Poids×H |
| ------ | ----: | --: | --: | -----: | -----: | -----------: | ---------: |
| non    |     8 |   6 |   2 | 0.7500 | 0.8113 |       0.5714 |     0.4636 |
| oui    |     6 |   3 |   3 | 0.5000 | 1.0000 |       0.4286 |     0.4286 |
|        |       |     |     |        |        | **H\_après** | **0.8922** |
|        |       |     |     |        |        |       **IG** | **0.0481** |

## Tableau 4 — Split par `température`

| Valeur  | Total | Oui | Non | p(oui) |      H |        Poids |    Poids×H |
| ------- | ----: | --: | --: | -----: | -----: | -----------: | ---------: |
| chaude  |     4 |   2 |   2 | 0.5000 | 1.0000 |       0.2857 |     0.2857 |
| douce   |     6 |   4 |   2 | 0.6667 | 0.9183 |       0.4286 |     0.3936 |
| fraîche |     4 |   3 |   1 | 0.7500 | 0.8113 |       0.2857 |     0.2318 |
|         |       |     |     |        |        | **H\_après** | **0.9111** |
|         |       |     |     |        |        |       **IG** | **0.0292** |

### Tableau 5 — Résumé IG à la racine

| Attribut    | H\_après |         IG |  Rang |
| ----------- | -------: | ---------: | ----: |
| **temps**   |   0.6935 | **0.2467** | **1** |
| humidité    |   0.7885 |     0.1518 |     2 |
| vent        |   0.8922 |     0.0481 |     3 |
| température |   0.9111 |     0.0292 |     4 |

> Conclusion racine : **choisir `temps`** (IG maximal).

---

# Développement des branches

## Tableau 6 — Branche `temps = ensoleilé` (5 cas, H=0.9710) → choisir `humidité`

| Valeur  | Total (branche) | Oui | Non | p(oui) |      H | Poids (=Total/5) |    Poids×H |
| ------- | --------------: | --: | --: | -----: | -----: | ---------------: | ---------: |
| haute   |               3 |   0 |   3 | 0.0000 | 0.0000 |           0.6000 |     0.0000 |
| normale |               2 |   2 |   0 | 1.0000 | 0.0000 |           0.4000 |     0.0000 |
|         |                 |     |     |        |        |     **H\_après** | **0.0000** |
|         |                 |     |     |        |        |   **IG (local)** | **0.9710** |

⇒ Feuilles :

* ensoleilé & **humidité=haute** → **jouer = non**
* ensoleilé & **humidité=normale** → **jouer = oui**

## Tableau 7 — Branche `temps = pluvieux` (5 cas, H=0.9710) → choisir `vent`

| Valeur | Total (branche) | Oui | Non | p(oui) |      H |          Poids |    Poids×H |
| ------ | --------------: | --: | --: | -----: | -----: | -------------: | ---------: |
| non    |               3 |   3 |   0 | 1.0000 | 0.0000 |         0.6000 |     0.0000 |
| oui    |               2 |   0 |   2 | 0.0000 | 0.0000 |         0.4000 |     0.0000 |
|        |                 |     |     |        |        |   **H\_après** | **0.0000** |
|        |                 |     |     |        |        | **IG (local)** | **0.9710** |

⇒ Feuilles :

* pluvieux & **vent=non** → **jouer = oui**
* pluvieux & **vent=oui** → **jouer = non**

## Tableau 8 — Branche `temps = nuageux` (4 cas)

| Total (branche) | Oui | Non |      H | Décision                  |
| --------------: | --: | --: | -----: | ------------------------- |
|               4 |   4 |   0 | 0.0000 | **feuille : jouer = oui** |

---

# Tableau 9 — Règles finales et probabilités

| Règle | Condition                              | Total | Oui | Non | p(oui) | Classe prédite |
| ----- | -------------------------------------- | ----: | --: | --: | -----: | -------------- |
| R1    | temps = nuageux                        |     4 |   4 |   0 | 1.0000 | oui            |
| R2    | temps = ensoleilé & humidité = haute   |     3 |   0 |   3 | 0.0000 | non            |
| R3    | temps = ensoleilé & humidité = normale |     2 |   2 |   0 | 1.0000 | oui            |
| R4    | temps = pluvieux & vent = non          |     3 |   3 |   0 | 1.0000 | oui            |
| R5    | temps = pluvieux & vent = oui          |     2 |   0 |   2 | 0.0000 | non            |


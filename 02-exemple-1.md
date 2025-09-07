
# Jeu de données (14 lignes)

Variables : `temps` (ensoleilé/nuageux/pluvieux), `température` (chaude/douce/fraîche), `humidité` (haute/normale), `vent` (oui/non), cible `jouer` (oui/non).

| #  | temps     | température | humidité | vent | jouer |
| -- | --------- | ----------- | -------- | ---- | ----- |
| 1  | ensoleilé | chaude      | haute    | non  | non   |
| 2  | ensoleilé | chaude      | haute    | oui  | non   |
| 3  | nuageux   | chaude      | haute    | non  | oui   |
| 4  | pluvieux  | douce       | haute    | non  | oui   |
| 5  | pluvieux  | fraîche     | normale  | non  | oui   |
| 6  | pluvieux  | fraîche     | normale  | oui  | non   |
| 7  | nuageux   | fraîche     | normale  | oui  | oui   |
| 8  | ensoleilé | douce       | haute    | non  | non   |
| 9  | ensoleilé | fraîche     | normale  | non  | oui   |
| 10 | pluvieux  | douce       | normale  | non  | oui   |
| 11 | ensoleilé | douce       | normale  | oui  | oui   |
| 12 | nuageux   | douce       | haute    | oui  | oui   |
| 13 | nuageux   | chaude      | normale  | non  | oui   |
| 14 | pluvieux  | douce       | haute    | oui  | non   |

Comptes initiaux : **9 “oui”**, **5 “non”**.

---

## Étape 1 — Entropie du jeu complet

Formule (entropie de Shannon) :
$H(S)=-\sum_i p_i \log_2 p_i$ avec $p_i$ la proportion de chaque classe.

Ici :

* $p(\text{oui})=9/14 \approx 0{,}643$
* $p(\text{non})=5/14 \approx 0{,}357$

$$
H(S)= -\Big(\tfrac{9}{14}\log_2\tfrac{9}{14} + \tfrac{5}{14}\log_2\tfrac{5}{14}\Big)
\approx 0{,}940
$$

---

## Étape 2 — Gain d’info de chaque attribut (à la racine)

### Attribut `temps`

Découpe en 3 sous-groupes :

* **ensoleilé** (5 cas) : 2 “oui”, 3 “non”
  $H= -\big(\tfrac{2}{5}\log_2\tfrac{2}{5} + \tfrac{3}{5}\log_2\tfrac{3}{5}\big) \approx 0{,}971$
* **nuageux** (4 cas) : 4 “oui”, 0 “non” → $H=0$ (pur)
* **pluvieux** (5 cas) : 3 “oui”, 2 “non” → $H \approx 0{,}971$

Entropie après découpe (moyenne pondérée) :

$$
H_{\text{après}} \approx \tfrac{5}{14}\cdot0{,}971 + \tfrac{4}{14}\cdot0 + \tfrac{5}{14}\cdot0{,}971 \approx 0{,}694
$$

**Gain d’info** :

$$
IG(S,\text{temps}) = 0{,}940 - 0{,}694 \approx \mathbf{0{,}247}
$$

### Attribut `température`

Calcul analogue → **$IG \approx 0{,}029$**

### Attribut `humidité`

Calcul analogue → **$IG \approx 0{,}152$**

### Attribut `vent`

Calcul analogue → **$IG \approx 0{,}048$**

**Conclusion (racine)** : l’attribut avec le **plus grand gain** est **`temps`** (≈ 0,247).
→ **Racine = `temps`**.

---

## Étape 3 — Descendre dans chaque branche

### Branche `temps = nuageux`

Sous-ensemble : 4 cas, **4 “oui”**.
Entropie = 0 → **feuille** : **jouer = oui**.

### Branche `temps = ensoleilé` (5 cas)

Données : 2 “oui”, 3 “non” → $H \approx 0{,}971$.
Tester les gains des attributs restants (`température`, `humidité`, `vent`) :

* **Split par `humidité`** :

  * `haute` → (3 cas) : 0 “oui”, 3 “non” → $H=0$
  * `normale` → (2 cas) : 2 “oui”, 0 “non” → $H=0$
    Moyenne pondérée = 0 → **IG = 0,971** (parfait)

* `température` → IG ≈ 0,571

* `vent` → IG ≈ 0,020

**Conclusion** : sur la branche *ensoleilé*, on choisit **`humidité`**.
Règles feuilles :

* si *humidité = haute* → **jouer = non**
* si *humidité = normale* → **jouer = oui**

### Branche `temps = pluvieux` (5 cas)

Données : 3 “oui”, 2 “non” → $H \approx 0{,}971$.
Tester les attributs restants :

* **Split par `vent`** :

  * `non` → (3 cas) : 3 “oui”, 0 “non” → $H=0$
  * `oui` → (2 cas) : 0 “oui”, 2 “non” → $H=0$
    Moyenne pondérée = 0 → **IG = 0,971** (parfait)

* `température` → IG ≈ 0,020

* `humidité` → IG ≈ 0,020

**Conclusion** : sur la branche *pluvieux*, on choisit **`vent`**.
Règles feuilles :

* si *vent = non* → **jouer = oui**
* si *vent = oui* → **jouer = non**

---

## Arbre final (ID3)

```
                   [temps]
          / nuageux   |   ensoleilé                  \ pluvieux
      (feuille oui)   |                     [vent]
                       |                / non        \ oui
                 [humidité]         (feuille oui)  (feuille non)
               /  haute  \ normale
        (feuille non)  (feuille oui)
```

### Règles équivalentes (faciles pour les étudiants)

1. **Si `temps = nuageux` → jouer = oui**
2. **Si `temps = ensoleilé` et `humidité = haute` → jouer = non**
3. **Si `temps = ensoleilé` et `humidité = normale` → jouer = oui**
4. **Si `temps = pluvieux` et `vent = non` → jouer = oui**
5. **Si `temps = pluvieux` et `vent = oui` → jouer = non**

> Remarque : cet arbre **classe parfaitement** les 14 exemples (entropies finales à 0 sur chaque feuille).

---

## Mini-check (3 prédictions)

* ensoleilé, **haute**, vent=non → règle (2) → **non** ✅
* pluvieux, vent=**non** → règle (4) → **oui** ✅
* nuageux, … → règle (1) → **oui** ✅

---

## Ce qu’il faut retenir (version “recette”)

1. **Entropie** mesure le mélange (0 = pur, \~1 = très mélangé).
2. **Gain d’info** = entropie avant − entropie après (moyenne pondérée).
3. À chaque nœud, **choisir l’attribut au gain maximal**.
4. S’arrêter si le nœud est **pur** (entropie 0) ou si plus d’attributs utiles.
5. Les **règles** se lisent du **haut vers la feuille**.


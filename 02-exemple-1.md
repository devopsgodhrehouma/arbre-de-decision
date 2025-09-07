
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





# Annexe 1 :


# 0) Notation & arrondis

* $n$ = nb d’exemples du nœud.
* $n_{oui}$, $n_{non}$ = comptes par classe.
* $p_{oui} = n_{oui}/n$, $p_{non} = n_{non}/n$.
* $\log_2 x = \ln(x)/\ln(2)$.
* Arrondis à 4 décimales.

---

# 1) Entropie d’un nœud (formule générique)

$$
H(S)= -\sum_{c\in\{\text{oui, non}\}} p_c \log_2(p_c)
= -\big(p_{oui}\log_2 p_{oui}+p_{non}\log_2 p_{non}\big)
$$

**À obtenir (jeu complet)**
$n=14,\ n_{oui}=9,\ n_{non}=5 \Rightarrow p_{oui}=9/14,\ p_{non}=5/14$
$\boxed{H(S)=0.9403}$

---

# 2) Entropie après une découpe par un attribut $A$

Pour chaque valeur $v$ de $A$, on a un sous-ensemble $S_v$ (taille $n_v$).

$$
H_{\text{après}}(A)=\sum_{v} \frac{n_v}{n}\, H(S_v)
$$

$$
\text{Gain d’info } IG(S,A)=H(S)-H_{\text{après}}(A)
$$

## 2.1 Appliquer à `temps`

Sous-groupes (avec $H$ locaux) :

* ensoleilé: $n=5,\ (2\ oui,3\ non)\Rightarrow H=0.9710$
* nuageux: $n=4,\ (4,0)\Rightarrow H=0$
* pluvieux: $n=5,\ (3,2)\Rightarrow H=0.9710$

Formule pondérée :

$$
H_{\text{après}}(\text{temps})=\tfrac{5}{14}\cdot0.9710+\tfrac{4}{14}\cdot0+\tfrac{5}{14}\cdot0.9710
=\boxed{0.6935}
$$

$$
IG=\boxed{0.9403-0.6935=0.2467}
$$

## 2.2 `humidité`

* haute: $n=7,\ (3,4)\Rightarrow H=0.9852$
* normale: $n=7,\ (6,1)\Rightarrow H=0.5917$

$$
H_{\text{après}}(\text{hum})=\tfrac{7}{14}\cdot0.9852+\tfrac{7}{14}\cdot0.5917=\boxed{0.7885}
$$

$$
IG=\boxed{0.1518}
$$

## 2.3 `vent`

* non: $n=8,\ (6,2)\Rightarrow H=0.8113$
* oui: $n=6,\ (3,3)\Rightarrow H=1.0000$

$$
H_{\text{après}}(\text{vent})=\tfrac{8}{14}\cdot0.8113+\tfrac{6}{14}\cdot1.0000=\boxed{0.8922}
$$

$$
IG=\boxed{0.0481}
$$

## 2.4 `température`

* chaude: $n=4,(2,2)\Rightarrow H=1.0000$
* douce: $n=6,(4,2)\Rightarrow H=0.9183$
* fraîche: $n=4,(3,1)\Rightarrow H=0.8113$

$$
H_{\text{après}}(\text{temp})= \tfrac{4}{14}\cdot1.0000+\tfrac{6}{14}\cdot0.9183+\tfrac{4}{14}\cdot0.8113=\boxed{0.9111}
$$

$$
IG=\boxed{0.0292}
$$

**À obtenir (résumé racine)**
$\boxed{IG(\text{temps})=0.2467 > IG(\text{hum})=0.1518 > IG(\text{vent})=0.0481 > IG(\text{temp})=0.0292}$
→ **Choisir `temps` à la racine.**



# 3) Développement des branches (même formules)

## 3.1 Branche `temps = nuageux` (4 cas)

$n_{oui}=4,\ n_{non}=0 \Rightarrow \boxed{H=0} \Rightarrow$ **feuille jouer=oui**.

## 3.2 Branche `temps = ensoleilé` (5 cas)

Tester les attributs restants. Meilleur = `humidité`.

* `humidité=haute`: $n=3,(0,3)\Rightarrow H=0$
* `humidité=normale`: $n=2,(2,0)\Rightarrow H=0$

$$
H_{\text{après}}= \tfrac{3}{5}\cdot0+\tfrac{2}{5}\cdot0=\boxed{0}
\Rightarrow IG_{\text{local}}=\boxed{0.9710}
$$

→ **Feuilles** :
ensoleilé & **haute** → jouer=**non** ; ensoleilé & **normale** → jouer=**oui**.

## 3.3 Branche `temps = pluvieux` (5 cas)

Meilleur = `vent`.

* `vent=non`: $n=3,(3,0)\Rightarrow H=0$
* `vent=oui`: $n=2,(0,2)\Rightarrow H=0$

$$
H_{\text{après}}=\tfrac{3}{5}\cdot0+\tfrac{2}{5}\cdot0=\boxed{0}
\Rightarrow IG_{\text{local}}=\boxed{0.9710}
$$

→ **Feuilles** :
pluvieux & **non** → jouer=**oui** ; pluvieux & **oui** → jouer=**non**.



# 4) Ce que tu dois avoir à la fin (à vérifier en classe)

* **IG à la racine** : $[0.2467,\ 0.1518,\ 0.0481,\ 0.0292]$ pour $[temps,\ humidité,\ vent,\ température]$.
* **Règles finales** (5) :

  1. temps=nuageux → **oui**
  2. ensoleilé & humidité=haute → **non**
  3. ensoleilé & humidité=normale → **oui**
  4. pluvieux & vent=non → **oui**
  5. pluvieux & vent=oui → **non**
* **Accuracy entraînement** : $14/14$.


## Bonus “copier-coller” (Excel / Google Sheets)

* $p$ d’une classe (si la colonne `E` contient oui/non, lignes 2:15) :
  `=COUNTIF($E$2:$E$15,"oui")/ROWS($E$2:$E$15)`
* Entropie binaire d’un nœud (cellules P\_oui en A1, P\_non en B1) :
  `=IF(A1=0,0,-A1*LOG(A1,2)) + IF(B1=0,0,-B1*LOG(B1,2))`
* Entropie pondérée après split (H1..Hk en plage Hs, poids w1..wk en Ws) :
  `=SUMPRODUCT(Ws,Hs)`
* Gain d’info (H\_total en T1, H\_apres en A1) :
  `=T1 - A1`



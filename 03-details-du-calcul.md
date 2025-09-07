# Cas de départ (14 lignes)

Variables : `temps` (ensoleilé/nuageux/pluvieux), `température` (chaude/douce/fraîche), `humidité` (haute/normale), `vent` (oui/non), cible `jouer` (oui/non).

Comptes globaux : **9 “oui”**, **5 “non”**.

---

## Étape 1 — Entropie du jeu complet

$$
p(\text{oui})=\tfrac{9}{14}\approx0{,}643,\quad p(\text{non})=\tfrac{5}{14}\approx0{,}357
$$

$$
H(S)= -\Big(\tfrac{9}{14}\log_2\tfrac{9}{14}+\tfrac{5}{14}\log_2\tfrac{5}{14}\Big)\approx \mathbf{0{,}9403}
$$

---

## Étape 2 — Tester chaque attribut à la racine

### 2.1 `temps`

Sous-groupes et entropies :

* **ensoleilé** (5 cas : 2 oui, 3 non) → $H=0{,}9710$
* **nuageux** (4 cas : 4 oui, 0 non) → $H=0$
* **pluvieux** (5 cas : 3 oui, 2 non) → $H=0{,}9710$

Moyenne pondérée :

$$
H_{\text{après}}=\tfrac{5}{14}\cdot0{,}9710+\tfrac{4}{14}\cdot0+\tfrac{5}{14}\cdot0{,}9710\approx 0{,}6935
$$

**Gain d’info** :

$$
IG(S,\text{temps})=0{,}9403-0{,}6935=\mathbf{0{,}2467}
$$

### 2.2 `humidité`

* **haute** (7 cas : 3 oui, 4 non) → $H=0{,}9852$
* **normale** (7 cas : 6 oui, 1 non) → $H=0{,}5917$

Pondérée :

$$
H_{\text{après}}\approx \tfrac{7}{14}\cdot0{,}9852+\tfrac{7}{14}\cdot0{,}5917=0{,}7885
$$

$$
IG=\;0{,}9403-0{,}7885=\mathbf{0{,}1518}
$$

### 2.3 `vent`

* **non** (8 cas : 6 oui, 2 non) → $H=0{,}8113$
* **oui** (6 cas : 3 oui, 3 non) → $H=1{,}0000$

Pondérée :

$$
H_{\text{après}}=\tfrac{8}{14}\cdot0{,}8113+\tfrac{6}{14}\cdot1{,}0=0{,}8922
\Rightarrow IG=\mathbf{0{,}0481}
$$

### 2.4 `température`

Pondérée → $IG\approx \mathbf{0{,}0292}$

**Conclusion à la racine :** plus grand gain = **`temps`**.
On met **`temps`** à la racine.

---

## Étape 3 — Développer chaque branche de `temps`

### 3.1 Branche `temps = nuageux` (4 cas)

Comptes : 4 oui, 0 non → entropie 0 → **feuille “jouer = oui”**.

### 3.2 Branche `temps = ensoleilé` (5 cas)

Comptes : 2 oui, 3 non → $H\approx0{,}9710$.
Tester attributs restants (`température`, `humidité`, `vent`) :

* **Split par `humidité`**

  * **haute** (3 cas) : 0 oui, 3 non → $H=0$
  * **normale** (2 cas) : 2 oui, 0 non → $H=0$
    Pondérée = 0 ⇒ **IG = 0{,}9710** (séparation parfaite) ✅

Donc sur `ensoleilé` :

* si `humidité = haute` → **feuille “jouer = non”**
* si `humidité = normale` → **feuille “jouer = oui”**

### 3.3 Branche `temps = pluvieux` (5 cas)

Comptes : 3 oui, 2 non → $H\approx0{,}9710$.
Tester attributs restants :

* **Split par `vent`**

  * **non** (3 cas) : 3 oui, 0 non → $H=0$
  * **oui** (2 cas) : 0 oui, 2 non → $H=0$
    Pondérée = 0 ⇒ **IG = 0{,}9710** (parfait) ✅

Donc sur `pluvieux` :

* si `vent = non` → **feuille “jouer = oui”**
* si `vent = oui` → **feuille “jouer = non”**

---

## Étape 4 — Arbre final + règles

```
                   [temps]
          / nuageux   |   ensoleilé                  \ pluvieux
      (feuille oui)   |                     [vent]
                       |                / non        \ oui
                 [humidité]         (feuille oui)  (feuille non)
               /  haute  \ normale
        (feuille non)  (feuille oui)
```

**Règles :**

1. Si `temps = nuageux` → **jouer = oui**
2. Si `temps = ensoleilé` **et** `humidité = haute` → **jouer = non**
3. Si `temps = ensoleilé` **et** `humidité = normale` → **jouer = oui**
4. Si `temps = pluvieux` **et** `vent = non` → **jouer = oui**
5. Si `temps = pluvieux` **et** `vent = oui` → **jouer = non**

**Performance sur le jeu d’entraînement :** 14/14 bien classés (chaque feuille est pure).

---

## Résumé “recette” (à coller au tableau)

1. **Compter** oui/non, calculer $H(S)$.
2. Pour **chaque attribut** :

   * découper en sous-groupes,
   * calculer **entropie de chaque sous-groupe**,
   * faire la **moyenne pondérée**,
   * calculer **IG = H(S) − H\_{\text{après}}**.
3. **Choisir l’attribut au plus grand IG** → noeud.
4. **Répéter** sur chaque branche jusqu’à feuilles **pures** ou plus d’attributs.
5. **Lire les règles** du haut jusqu’à la feuille.


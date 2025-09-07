

# 1) C’est quoi une probabilité ?

**Probabilité = ce que je veux / tout ce qui peut arriver.**
En chiffres : $P(A) = \frac{\text{nombre de cas favorables}}{\text{nombre de cas possibles}}$.

Exemples très concrets

* 3 boules rouges, 2 bleues → tirer rouge : $3/5 = 0{,}6 = 60\%$.
* 10 jours, il a plu 3 jours → $P(\text{pluie}) = 3/10 = 30\%$.

Deux règles qui sauvent des vies en examen

* **Complément** : $P(\text{pas A}) = 1 - P(A)$.
* **Conditionnelle** : $P(A\,|\,B) = \frac{\text{A et B}}{\text{B}}$ (on compte **dans** B).

---

# 2) Appliqué à ton exemple “jouer ou pas”

Tu as 14 lignes (14 situations).

* “oui” = 9 fois → $P(\text{jouer=oui}) = 9/14 \approx 0{,}643$ (64,3 %).
* “non” = 5 fois → $P(\text{jouer=non}) = 5/14 \approx 0{,}357$ (35,7 %).
  (Et vérif : $0{,}643 + 0{,}357 = 1$.)

---

# 3) Pourquoi on parle d’**entropie** dans ID3/C4.5 ?

L’entropie mesure le **mélange** des classes dans un groupe (0 = pur, 1 = très mélangé).
Formule (ne leur fais pas peur, lis-la comme “somme des proportions × log2”) :

$$
H(S)= -\sum_i p_i \log_2(p_i)
$$

Ici :

$$
H(S) = -\Big(\tfrac{9}{14}\log_2\tfrac{9}{14} + \tfrac{5}{14}\log_2\tfrac{5}{14}\Big) \approx 0{,}940
$$

*(Petite correction dans ton texte collé : la 2ᵉ log2 est bien sur **5/14**, pas sur 9/14.)*

**Idée à retenir pour les étudiants** :

* Si un groupe est tout “oui” ou tout “non” → entropie **0** (aucune incertitude).
* Plus c’est moitié-moitié → entropie **haute** (incertitude max ≈ 1).

---

# 4) “Gain d’information” = combien le mélange baisse après une question

On teste un attribut (ex. **temps** = ensoleillé/nuageux/pluvieux).
On calcule l’entropie **dans chaque sous-groupe**, puis on fait la moyenne **pondérée** par la taille des sous-groupes.

Pour **temps**, on obtient (avec tes chiffres) :

* ensoleillé : 2 “oui”, 3 “non” → $H \approx 0{,}971$
* nuageux : 4 “oui”, 0 “non” → $H = 0$
* pluvieux : 3 “oui”, 2 “non” → $H \approx 0{,}971$
  Pondération : $5/14,\ 4/14,\ 5/14$.

Entropie après découpe par **temps** :

$$
H_{\text{après}} \approx \tfrac{5}{14}\cdot0{,}971 + \tfrac{4}{14}\cdot0 + \tfrac{5}{14}\cdot0{,}971 \approx 0{,}694
$$

**Gain d’info** :

$$
\text{IG} = H_{\text{avant}} - H_{\text{après}} \approx 0{,}940 - 0{,}694 \approx 0{,}247
$$

→ c’est pour ça qu’on choisit **temps** en premier (c’est celui qui “démélange” le mieux).

---

# 5) Mini “recette express” à donner aux débutants

1. **Compter** les “oui” et les “non”.
2. **Probabilités** = compte / total.
3. **Entropie d’un groupe** :

   * Si tout pareil → 0.
   * Sinon, appliquer $-\sum p\log_2 p$ (prendre 3 décimales).
4. **Après une question** (un attribut) :

   * Entropie de chaque sous-groupe.
   * **Moyenne pondérée** par la taille des sous-groupes.
5. **Gain d’info** = avant − après.
6. **Choisir l’attribut** au **gain d’info le plus grand**.

---

# 6) Trois mini-exos (avec réponses)

**Ex1.** 8 rouges, 2 bleues. $P(\text{rouge})=?$ → $8/10=0{,}8=80\%$.
**Ex2.** 20 matchs, 7 gagnés. $P(\text{perdre})=?$ → $1-7/20=13/20=0{,}65$.
**Ex3.** Classe : 12 “oui”, 4 “non”. Entropie ?

$$
H = -\big(12/16\log_2(12/16) + 4/16\log_2(4/16)\big) \approx 0{,}811.
$$


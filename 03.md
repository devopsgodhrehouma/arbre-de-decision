

# Cellule 1 — Préparer l’environnement

```python
# Exécutez cette cellule en premier
import numpy as np
import pandas as pd

from sklearn.datasets import load_iris, make_regression
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier, KNeighborsRegressor
from sklearn.metrics import accuracy_score, classification_report, mean_squared_error, r2_score

import matplotlib.pyplot as plt
```

---

# Cellule 2 — Classification K-NN (Iris)

```python
# 1) Charger les données (classification)
iris = load_iris(as_frame=True)
X = iris.data         # 4 features
y = iris.target       # 3 classes

# 2) Séparer train / test
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

# 3) Normaliser (important pour K-NN)
scaler = StandardScaler()
X_train_s = scaler.fit_transform(X_train)
X_test_s  = scaler.transform(X_test)

# 4) Chercher le meilleur k (validation croisée simple)
k_values = range(1, 21)
cv_scores = []
for k in k_values:
    knn = KNeighborsClassifier(n_neighbors=k)
    scores = cross_val_score(knn, X_train_s, y_train, cv=5, scoring="accuracy")
    cv_scores.append(scores.mean())

best_k = k_values[int(np.argmax(cv_scores))]
print("Meilleur k (CV):", best_k)

# 5) Entraîner avec best_k et évaluer
clf = KNeighborsClassifier(n_neighbors=best_k)
clf.fit(X_train_s, y_train)
y_pred = clf.predict(X_test_s)

acc = accuracy_score(y_test, y_pred)
print(f"Accuracy test: {acc:.3f}")
print("\nClassification report:\n", classification_report(y_test, y_pred, target_names=iris.target_names))

# 6) Courbe accuracy vs k
plt.figure()
plt.plot(list(k_values), cv_scores, marker="o")
plt.title("Validation croisée — Accuracy vs k (Iris)")
plt.xlabel("k (nombre de voisins)")
plt.ylabel("Accuracy (CV)")
plt.grid(True)
plt.show()
```

---

# Cellule 3 — Tester une nouvelle observation (classification)

```python
# Saisir une observation (ex. sépales/pétales)
# Format: [sepal length, sepal width, petal length, petal width]
nouveau = np.array([[5.1, 3.4, 1.5, 0.2]])
nouveau_s = scaler.transform(nouveau)
pred = clf.predict(nouveau_s)[0]
print("Classe prédite:", iris.target_names[pred])
```

---

# Cellule 4 — Régression K-NN (jeu synthétique)

```python
# 1) Créer un dataset de régression simple
X_reg, y_reg = make_regression(n_samples=800, n_features=4, noise=15, random_state=42)

# 2) Split train/test
Xr_train, Xr_test, yr_train, yr_test = train_test_split(
    X_reg, y_reg, test_size=0.25, random_state=42
)

# 3) Normalisation
scaler_r = StandardScaler()
Xr_train_s = scaler_r.fit_transform(Xr_train)
Xr_test_s  = scaler_r.transform(Xr_test)

# 4) Choisir k par CV (MSE négatif → on maximise)
k_values = range(1, 21)
cv_scores_r = []
for k in k_values:
    knr = KNeighborsRegressor(n_neighbors=k)
    scores = cross_val_score(knr, Xr_train_s, yr_train, cv=5, scoring="neg_mean_squared_error")
    cv_scores_r.append(scores.mean())

best_k_r = k_values[int(np.argmax(cv_scores_r))]
print("Meilleur k (régression, CV):", best_k_r)

# 5) Entraîner + évaluer
reg = KNeighborsRegressor(n_neighbors=best_k_r)
reg.fit(Xr_train_s, yr_train)
yr_pred = reg.predict(Xr_test_s)

mse = mean_squared_error(yr_test, yr_pred)
r2  = r2_score(yr_test, yr_pred)
print(f"MSE test: {mse:.2f} | R² test: {r2:.3f}")

# 6) Courbe CV (on affiche -MSE pour lire plus facilement : plus haut = mieux)
plt.figure()
plt.plot(list(k_values), [-s for s in cv_scores_r], marker="o")
plt.title("Validation croisée — (-)MSE vs k (Régression)")
plt.xlabel("k (nombre de voisins)")
plt.ylabel("-MSE (CV)  → plus haut = mieux")
plt.grid(True)
plt.show()
```

---

# Cellule 5 — Questions à rendre (réponses courtes)

```python
questions = [
    "Q1. Pourquoi normalise-t-on les features avant K-NN ?",
    "Q2. Quel k avez-vous obtenu pour Iris ? Commentez la courbe CV.",
    "Q3. Donnez l’accuracy test sur Iris et interprétez en une phrase.",
    "Q4. Pour la régression : quel k optimal ? Comparez 1-NN vs k optimal (biais/variance).",
    "Q5. Changez 'metric' en 'manhattan' dans KNeighborsClassifier : l’accuracy change-t-elle ? Expliquez.",
]
for q in questions:
    print(q)
```



## Consignes

* Ouvrir **Google Colab** → Nouveau notebook → coller les cellules **dans l’ordre** et exécuter.
* Répondre aux questions de la **Cellule 5** sous forme d’un petit paragraphe (texte dans une cellule Markdown).
* Bonus (facultatif) :

  * Tester d’autres valeurs de `weights` (`'uniform'` vs `'distance'`) en classification.
  * Visualiser la frontière de décision sur deux features (Iris : petal_length vs petal_width).
  * Pour la régression, tracer `yr_test` vs `yr_pred` (nuage de points) et commenter.



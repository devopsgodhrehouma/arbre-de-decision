```mermaid
flowchart LR
    A["temps ?\nH = 0,940 • IG = 0,247\nP(oui) = 9/14 (0,643)"]

    A -->|nuageux • 4/14| B["FEUILLE\njouer = oui\nP(oui) = 4/4 = 1,00"]
    A -->|ensoleillé • 5/14| C["humidité ?\nH = 0,971 • IG = 0,971\nP(oui) = 2/5 (0,40)"]
    A -->|pluvieux • 5/14|  F['vent ?\nH = 0,971 • IG = 0,971\nP(oui) = 3/5 (0,60)']

    C -->|haute • 3/5|   D["FEUILLE\njouer = non\nP(oui) = 0/3 = 0,00"]
    C -->|normale • 2/5| E["FEUILLE\njouer = oui\nP(oui) = 2/2 = 1,00"]

    F -->|non • 3/5|  G["FEUILLE\njouer = oui\nP(oui) = 3/3 = 1,00"]
    F -->|oui • 2/5|  H["FEUILLE\njouer = non\nP(oui) = 0/2 = 0,00"]

    %% Styles (optionnels)
    classDef yes fill:#e6ffe6,stroke:#2e7d32,stroke-width:1px;
    classDef no  fill:#ffe6e6,stroke:#c62828,stroke-width:1px;
    class B,E,G yes;
    class D,H no;
```

### Comment lire/obtenir ces probabilités

* **Sur les arêtes** : c’est la proportion d’exemples qui suivent ce chemin.
  Exemple racine → *ensoleilé* : $5/14$.
* **Dans un nœud** : $P(\text{oui}) = \#\text{oui du nœud} / \#\text{exemples du nœud}$.
  Exemple nœud *ensoleilé* : $2/5 = 0{,}40$.
* **Dans une feuille** (probabilité prédite) : même calcul, ex. feuille *pluvieux & vent=non* : $3/3=1{,}00$.
* **Entropie H** : mélange des classes du nœud (0 = pur, ≈1 = très mélangé).
* **Gain d’info IG** : baisse d’entropie après la question posée au nœud.

👉 Astuce classe : pour éviter des 0%/100% sur toutes petites feuilles, utilisez le **lissage de Laplace** :

$$
\hat P(\text{oui})=\frac{\#\text{oui}+1}{\#\text{exemples}+2}
$$

Ex. feuille (0 oui / 3 ex.) → $(0+1)/(3+2)=0{,}20$.

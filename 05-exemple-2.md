## Version ASCII de notre arbre de décision

```txt
                             [ temps ? ]
                 H = 0.940 • IG = 0.247 • P(oui) = 9/14 (0.643)
                /                 |                   \
               /                  |                    \
     nuageux 4/14          ensoleillé 5/14            pluvieux 5/14
          |                       |                        |
    [ FEUILLE ]              [ humidité ? ]             [ vent ? ]
    jouer = oui          H = 0.971 • IG = 0.971     H = 0.971 • IG = 0.971
    P(oui) = 4/4 = 1.00  P(oui) = 2/5 (0.40)        P(oui) = 3/5 (0.60)
                               /     \
                        haute 3/5    normale 2/5
                          |              |
                    [ FEUILLE ]      [ FEUILLE ]
                    jouer = non      jouer = oui
                    P(oui) = 0/3     P(oui) = 2/2
                    = 0.00           = 1.00

                                                  /          \
                                            non 3/5          oui 2/5
                                              |                |
                                        [ FEUILLE ]       [ FEUILLE ]
                                        jouer = oui       jouer = non
                                        P(oui) = 3/3      P(oui) = 0/2
                                        = 1.00            = 0.00
```


- Comment lire/obtenir ces probabilités

* **Sur les arêtes** : c’est la proportion d’exemples qui suivent ce chemin.
  Exemple racine → *ensoleilé* : $5/14$.
* **Dans un nœud** : $P(\text{oui}) = \#\text{oui du nœud} / \#\text{exemples du nœud}$.
  Exemple nœud *ensoleilé* : $2/5 = 0{,}40$.
* **Dans une feuille** (probabilité prédite) : même calcul, ex. feuille *pluvieux & vent=non* : $3/3=1{,}00$.
* **Entropie H** : mélange des classes du nœud (0 = pur, ≈1 = très mélangé).
* **Gain d’info IG** : baisse d’entropie après la question posée au nœud.

- Astuce classe : pour éviter des 0%/100% sur toutes petites feuilles, utilisez le **lissage de Laplace** :



* **Dans un nœud** :  
  $$
  P(\text{oui}) = \frac{n_{\text{oui}}}{n_{\text{total}}}
  $$

* **Dans une feuille (Laplace smoothing)** :  
  $$
  \hat{P}(\text{oui}) = \frac{n_{\text{oui}}+1}{n_{\text{total}}+2}
  $$




```dot
digraph Arbre {
    node [shape=box, style="rounded,filled", color=black, fontname="Helvetica"];

    A [label="temps ?\nH=0,940 • IG=0,247\nP(oui)=9/14 (0,643)"];

    B [label="FEUILLE\njouer = oui\nP(oui) = (4+1)/(4+2)=0,83", fillcolor="#e6ffe6"];
    C [label="humidité ?\nH=0,971 • IG=0,971\nP(oui)=2/5 (0,40)"];
    F [label="vent ?\nH=0,971 • IG=0,971\nP(oui)=3/5 (0,60)"];

    D [label="FEUILLE\njouer = non\nP(oui) = (0+1)/(3+2)=0,20", fillcolor="#ffe6e6"];
    E [label="FEUILLE\njouer = oui\nP(oui) = (2+1)/(2+2)=0,75", fillcolor="#e6ffe6"];
    G [label="FEUILLE\njouer = oui\nP(oui) = (3+1)/(3+2)=0,80", fillcolor="#e6ffe6"];
    H [label="FEUILLE\njouer = non\nP(oui) = (0+1)/(2+2)=0,25", fillcolor="#ffe6e6"];

    A -> B [label="nuageux • 4/14"];
    A -> C [label="ensoleillé • 5/14"];
    A -> F [label="pluvieux • 5/14"];

    C -> D [label="haute • 3/5"];
    C -> E [label="normale • 2/5"];

    F -> G [label="non • 3/5"];
    F -> H [label="oui • 2/5"];
}
```

👉 Vous pouvez le coller dans [Graphviz Online](https://dreampuf.github.io/GraphvizOnline) et exporter en **SVG/PNG**.



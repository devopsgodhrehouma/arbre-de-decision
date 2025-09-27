## QCM – Module 4 : Comprendre les algorithmes ML clés (50 questions)

### K-Means (Questions 1-7)

1. K-Means est principalement un algorithme :
   a) De classification supervisée
   b) De clustering non supervisé
   c) De régression
   d) De réseaux de neurones

2. L’objectif principal de K-Means est de :
   a) Minimiser la somme des distances intra-cluster
   b) Maximiser la distance inter-cluster
   c) Minimiser le nombre de variables
   d) Prédire une variable continue

3. Que signifie le “K” dans K-Means ?
   a) Nombre de variables
   b) Nombre de clusters
   c) Nombre d’itérations
   d) Taille d’échantillon

4. Pour utiliser K-Means, il faut :
   a) Connaître le nombre de clusters à l’avance
   b) Connaître les étiquettes de classe
   c) Utiliser un arbre de décision
   d) Définir un seuil d’anomalie

5. K-Means attribue :
   a) Un point à plusieurs clusters
   b) Un point à un seul cluster
   c) Un point à aucun cluster
   d) Un point à tous les clusters

6. K-Means est sensible :
   a) Aux valeurs extrêmes (outliers)
   b) Aux couleurs
   c) Aux textes
   d) À la taille des colonnes

7. K-Means gère bien :
   a) Les clusters sphériques
   b) Les clusters de densité variable
   c) Les données séquentielles
   d) Les séries temporelles

---

### K-NN (Questions 8-15)

8. K-NN est :
   a) Un algorithme supervisé basé sur les exemples
   b) Un algorithme non supervisé
   c) Un algorithme de clustering
   d) Un algorithme de réduction de dimension

9. “K” dans K-NN représente :
   a) Nombre de variables
   b) Nombre de clusters
   c) Nombre de voisins
   d) Nombre d’arbres

10. K-NN est utilisé pour :
    a) La classification seulement
    b) La régression seulement
    c) La classification et la régression
    d) La détection d’anomalies

11. K-NN est un algorithme :
    a) Basé sur un modèle
    b) Basé sur la mémorisation des exemples
    c) Basé sur des arbres
    d) Basé sur le théorème de Bayes

12. La performance de K-NN dépend :
    a) Du choix de la distance
    b) Du nombre de couches
    c) Du type d’activation
    d) De la régularisation

13. K-NN nécessite :
    a) Un long entraînement
    b) Peu ou pas d’entraînement
    c) Un réseau de neurones
    d) Une base relationnelle

14. K-NN fonctionne :
    a) Seulement avec des données catégorielles
    b) Seulement avec des données numériques
    c) Avec données numériques et catégorielles après encodage
    d) Uniquement sur des images

15. Plus K est grand :
    a) Plus le modèle est sensible au bruit
    b) Plus le modèle est robuste au bruit
    c) Plus le modèle est rapide
    d) Plus le modèle surapprend

---

### DBSCAN (Questions 16-22)

16. DBSCAN est un algorithme :
    a) Supervisé
    b) Non supervisé
    c) De régression
    d) De réseaux de neurones

17. DBSCAN signifie :
    a) Density-Based Spatial Clustering of Applications with Noise
    b) Decision-Based Spatial Classification and Normalization
    c) Data-Based Sampling Clustering and Analysis
    d) Distance-Based Similarity Classification Algorithm

18. DBSCAN nécessite :
    a) De fixer le nombre de clusters
    b) De fixer des paramètres de densité (eps et minPts)
    c) De fixer les étiquettes
    d) Un arbre de décision

19. DBSCAN peut :
    a) Détecter des outliers
    b) Ne jamais détecter d’outliers
    c) Prédire des prix
    d) Réduire la dimension

20. DBSCAN est adapté :
    a) Aux clusters de forme quelconque
    b) Aux clusters strictement sphériques
    c) Aux données texte
    d) Aux séries temporelles

21. Les points de faible densité sont :
    a) Considérés comme bruit
    b) Toujours inclus dans un cluster
    c) Étiquetés comme centre
    d) Ignorés sans étiquette

22. DBSCAN est sensible :
    a) À l’ordre des données
    b) À l’échelle et au choix de eps
    c) Aux couleurs
    d) Aux colonnes vides

---

### Isolation Forest (Questions 23-29)

23. Isolation Forest est :
    a) Supervisé
    b) Non supervisé
    c) Basé sur K-Means
    d) Basé sur SVM

24. L’usage principal d’Isolation Forest est :
    a) La classification d’images
    b) La régression linéaire
    c) La détection d’anomalies
    d) La réduction de dimension

25. Isolation Forest repose sur :
    a) Le théorème de Bayes
    b) Des arbres aléatoires qui isolent les points
    c) Les k plus proches voisins
    d) Un réseau de neurones

26. Isolation Forest fournit :
    a) Un label de classe
    b) Un score d’anomalie
    c) Une image
    d) Un cluster

27. Isolation Forest peut :
    a) Détecter des anomalies financières
    b) Réduire la dimension
    c) Prévoir la température
    d) Classer des images

28. Isolation Forest construit :
    a) Des sous-échantillons pour isoler les points
    b) Un seul arbre
    c) Un réseau dense
    d) Des distances de Mahalanobis

29. Isolation Forest est dérivé :
    a) Des forêts aléatoires
    b) Des réseaux de neurones
    c) Des SVM
    d) Des KNN

---

### Naive Bayes (Questions 30-36)

30. Naive Bayes est :
    a) Un algorithme de classification basé sur Bayes
    b) Un algorithme de clustering
    c) Un algorithme de régression
    d) Un réseau de neurones

31. Naive Bayes suppose :
    a) L’indépendance des variables
    b) La dépendance des variables
    c) Un seul cluster
    d) Un grand nombre d’arbres

32. Naive Bayes est particulièrement adapté :
    a) À la classification de texte (spam)
    b) À la détection d’anomalies
    c) À la réduction de dimension
    d) Au clustering d’images

33. Naive Bayes donne :
    a) Des probabilités pour chaque classe
    b) Un seul label fixe
    c) Un cluster
    d) Un score d’anomalie

34. Naive Bayes est :
    a) Supervisé
    b) Non supervisé
    c) Basé sur des arbres
    d) Basé sur K-Means

35. Naive Bayes fonctionne :
    a) Seulement avec données numériques
    b) Bien avec données textuelles après vectorisation
    c) Uniquement sur des images
    d) Jamais avec variables catégorielles

36. Naive Bayes est :
    a) Rapide à entraîner
    b) Lent à entraîner
    c) Très coûteux en mémoire
    d) Toujours non interprétable

---

### Random Forest (Questions 37-43)

37. Random Forest est :
    a) Un ensemble d’arbres de décision
    b) Un algorithme de clustering
    c) Un algorithme de régression linéaire
    d) Un réseau de neurones

38. Random Forest est :
    a) Supervisé
    b) Non supervisé
    c) Basé sur K-Means
    d) Basé sur SVM

39. Random Forest peut être utilisé pour :
    a) La classification seulement
    b) La régression seulement
    c) La classification et la régression
    d) La détection d’anomalies

40. Random Forest effectue :
    a) Un “bagging” des échantillons
    b) Un “boosting” des échantillons
    c) Un clustering
    d) Une PCA

41. Random Forest donne :
    a) L’importance des variables
    b) Des probabilités Bayesiennes
    c) Un réseau dense
    d) Des clusters

42. Random Forest réduit :
    a) La variance et améliore la robustesse
    b) La dimension
    c) Les valeurs extrêmes
    d) Le nombre de variables

43. Random Forest nécessite :
    a) Beaucoup d’hyperparamètres obligatoires
    b) Quelques hyperparamètres simples
    c) Aucun paramètre
    d) Un seul arbre

---

### Comparaisons et généralités (Questions 44-50)

44. K-Means et DBSCAN sont :
    a) Tous deux des algorithmes non supervisés
    b) Tous deux des algorithmes supervisés
    c) Des réseaux de neurones
    d) Des algorithmes de régression

45. K-NN et Random Forest sont :
    a) Des algorithmes supervisés
    b) Des algorithmes non supervisés
    c) Des algorithmes de clustering
    d) Des algorithmes Bayesiens

46. Pour détecter des anomalies, on choisit plutôt :
    a) K-Means
    b) K-NN
    c) Isolation Forest
    d) Naive Bayes

47. Pour des données textuelles, on choisit plutôt :
    a) Naive Bayes
    b) K-Means
    c) Isolation Forest
    d) DBSCAN

48. L’algorithme qui demande de définir le nombre de voisins est :
    a) K-NN
    b) K-Means
    c) DBSCAN
    d) Random Forest

49. L’algorithme qui demande de définir le nombre de clusters est :
    a) K-Means
    b) DBSCAN
    c) Random Forest
    d) Naive Bayes

50. Parmi ces algorithmes, lequel est un **ensemble** d’arbres ?
    a) K-NN
    b) Random Forest
    c) K-Means
    d) Naive Bayes


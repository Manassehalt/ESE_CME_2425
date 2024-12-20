<p align="center"> <img src="IMAGE/logo ENSEA.png" width="30%" height="auto" /> </p>

#                                                            ESE_CEM_2425
JACQUOT NOLAN GUIFFAULT GABRIEL ESE 2024
## I.	Exécution du script tp00.m

https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp00.m

## Objectif de cette étape
L’objectif de cette étape est d’exécuter un script MATLAB/Octave permettant de résoudre numériquement l’équation de Laplace en 2D à l’aide de la méthode des différences finies. Ce script initialise un domaine de calcul de dimensions 40×40 avec des potentiels sources et des conditions aux limites, et produit une visualisation graphique du potentiel V dans tout le domaine.
### Description et analyse du script
### _1. Définition des paramètres et du domaine de calcul_

•	Les dimensions du maillage unitaire  (dx=1, dy=1) ainsi que le nombre de cellules (Nx=40, Ny=40) sont définis pour le domaine.

•	Les conditions aux limites sont fixées à V=0 sur les bords du domaine, simulant un potentiel nul à l’infini.

•	Deux conducteurs sont définis comme sources de potentiel avec :

 o	V1=100 V pour le premier conducteur.
 
 o	V2=−100V pour le second conducteur.
 
### _2. Initialisation et conditions aux limites_

•	Une matrice V est initialisée à zéro pour représenter le potentiel dans tout le domaine.

•	Les conditions aux limites sont appliquées sur les bords du domaine, garantissant V=0 sur les extrémités (x=1, x=40, y=1, y=40).

### _3. Définition des conducteurs_
   
Les conducteurs sont positionnés dans le domaine :

•	Le premier conducteur est représenté par une barre horizontale située entre les cellules (25:28,8:34) avec un potentiel V=100.

•	Le second conducteur est une barre verticale entre les cellules (5:22,20:21)(5:22, 20:21)(5:22,20:21) avec un potentiel V=−100V.

##  Visualisation des résultats
>[!TIP]
>```
>% Equation de calcul
>%i=1;j=1;
>for iter = 1:200
>    i=2:Nx-1;
>    j=2:Ny-1;
>    V(i,j)=0.25*( V(i+1,j) + V(i-1,j) + V(i,j+1) + V(i,j-1) );
>    V(Ny_v1_start:Ny_v1_end,Nx_v1_start:Nx_v1_end) = v1;
>    V(Ny_v2_start:Ny_v2_end,Nx_v2_start:Nx_v2_end) = v2;
>end
>```
>Le fait d'utiliser la vectorisation de matlab evite des problemes d'asymétrie que l'on rencontrerais sinon avec un calcul séquentiel.
Cette représentation graphique est produite :

### 1.	Visualisation des conditions initiales
 Ce graphe montre les conducteurs +100 V (en rouge) et −100V (en bleu) dans un domaine initialisé à V=0 :
 

<p align="center"> <img src="IMAGE/image1.png" width="65%" height="auto" /> </p>

>[!TIP]
> ````
> figure(11);
> colormap("jet");
> pcolor(V);
> ````
> permet d'obtenir le meme schéma de couleur que le sujet, ce qui permet de mieux reperer les erreurs éventuelles.

# II.	Résolution itérative de l’équation de Laplace (200 itérations)

https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp02.m

## Objectif de cette étape
L’objectif est de modifier le script MATLAB/Octave pour résoudre numériquement l’équation de Laplace en 2D à l’aide de la méthode des différences finies (DF) sur un domaine de 40×40. Cette résolution itérative (200 itérations) permet d’obtenir une distribution stable du potentiel V dans tout le domaine.
### Description de la méthode et du script
Dans cette étape, une boucle itérative est ajoutée pour appliquer l’équation discrétisée de Laplace :

V(i,j)=0.25*( V(i+1,j) + V(i-1,j) + V(i,j+1) + V(i,j-1) )

Le potentiel est calculé pour chaque point du domaine interne (excluant les bords) jusqu’à convergence approximative après 200 itérations.

### Modifications apportées au script
1.	Ajout d’une boucle itérative pour mettre à jour les valeurs de V(i,j) selon la méthode DF.
   
3.	Réimposition des potentiels fixes +100V et −100V sur les conducteurs à chaque itération pour garantir leur stabilité.

### Résultat
Ce graphe obtenu montre une distribution stable du potentiel après 200 itérations :

•	Une transition progressive entre les potentiels +100V (en rouge) et −100V (en bleu).

•	Un gradient de potentiel visible dans tout le domaine, illustrant les interactions entre les deux conducteurs.

 <p align="center"> <img src="IMAGE/image2.png" width="65%" height="auto" /> </p>

# III.	Convergence et seuil de convergence
https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp03.m
## Objectif de cette étape
L’objectif est de résoudre l’équation de Laplace en 2D en ajoutant un critère de convergence basé sur la variation maximale du potentiel V entre deux itérations successives. Cette méthode permet d’arrêter la simulation lorsque la solution devient suffisamment stable, évitant ainsi un nombre arbitraire d’itérations.
## Méthode et description du script
### 1. Paramètres de convergence

•	Seuil de convergence (seuil) : Définit la précision souhaitée pour la variation maximale du potentiel entre deux itérations successives. Dans ce script, le seuil est défini à 0.010.010.01, mais d'autres valeurs (0.001,0.0001,…)peuvent être testées.

•	Nombre maximum d’itérations (max_iterations) : Définit une limite pour éviter des boucles infinies si la convergence n'est pas atteinte.

### 2. Boucle de calcul
•	À chaque itération, une copie de l’état précédent du potentiel (V) est conservée dans Vold.

•	La méthode des différences finies est appliquée pour mettre à jour 

V : V(i, j) = 0.25 * (V(i+1, j) + V(i-1, j) + V(i, j+1) + V(i, j-1));

•	Les potentiels des conducteurs (v1 et v2) sont réimposés après chaque mise à jour pour garantir leur stabilité.

•	Le test de convergence compare V et Vold en calculant la variation maximale : diff=max(∥V−Vold∥)

•	La boucle s’arrête si la variation maximale devient inférieure au seuil.

### 3. Visualisation des résultats

•	Le potentiel V final est affiché avec pcolor après la convergence ou après avoir atteint le nombre maximum d’itérations.

## Résultats 
•	Le nombre d’itérations nécessaire pour atteindre la convergence dépend du seuil choisi (0.01,0.001, etc.). Par exemple :

o	Avec seuil=0.01, la convergence est atteinte en environ 284 itérations :
 <p align="center"> <img src="IMAGE/image3.png" width="65%" height="auto" /> </p>
o	Avec seuil=0.001, la convergence est atteinte en environ 491 itérations :

<p align="center"> <img src="IMAGE/image4.png" width="65%" height="auto" /> </p>
 
•	La méthode permet d’éviter des calculs inutiles tout en obtenant une solution stable.

# IV.	Étude de l’influence de la taille du domaine de calcul
https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp04.m
## Objectif de cette étape
Cette étape explore l'effet de la taille du domaine de calcul sur la distribution du potentiel V et la convergence des résultats. En augmentant ou réduisant les dimensions du domaine (Nx×Ny), on évalue l'impact sur la répartition des potentiels et la précision de la solution.
Méthode et description du script
### 1. Dimensions variables

•	Le domaine de calcul est testé pour plusieurs tailles : 10×10, 20×20, 40×40, 80×80, et 100×100.

•	Les proportions des cellules sources (V1=+100V et V2=−100V) sont ajustées proportionnellement à la taille du domaine.
### 2. Boucle de calcul pour chaque taille
Initialisation :

•	Les conditions aux limites (V=0) sont appliquées sur tous les bords du domaine.

•	Une matrice V est initialisée à zéro pour représenter le potentiel dans tout le domaine.

Itérations pour chaque taille :

•	À chaque itération, le potentiel V est mis à jour selon la méthode des différences finies :

V(i, j) = 0.25 * (V(i+1, j) + V(i-1, j) + V(i, j+1) + V(i, j-1))

•	Les potentiels des conducteurs (v1 et v2) sont réimposés pour garantir leur stabilité.

## Critère de convergence :
•	Une copie de V (Vold) est conservée à chaque itération.

•	La variation maximale entre deux itérations est calculée : diff=max(∣V−Vold∣)

•	La boucle s’arrête si diff<seuil (seuil=0.01) ou si le nombre maximum d’itérations (max_iterations=1000) est atteint.

### 3. Visualisation des résultats
•	Une figure est générée pour chaque taille de domaine, montrant la distribution finale du potentiel après convergence.

## Résultats :
 <p align="center"> <img src="IMAGE/image5.png" width="65%" height="auto" /> </p>

# V.	Affichage des lignes équipotentielles
https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp05.m
## Objectif de cette étape :
L’objectif est de visualiser les lignes équipotentielles du potentiel V dans le domaine de calcul.
Ces lignes représentent les zones où le potentiel est constant, offrant une meilleure compréhension
des interactions électrostatiques dans le système.

## Méthode et description

### 1. Initialisation du calcul :
• Les dimensions du domaine sont configurées pour différentes tailles (10 × 10, 20 × 20, 40 × 40, 80 × 80, 100 × 100).

• Les conducteurs sont positionnés proportionnellement à chaque taille, avec des potentiels fixés (+100 V pour le conducteur 1, -100 V pour le conducteur 2).

• Les conditions aux limites (V = 0) sont appliquées sur les bords.

### 2. Résolution de l’équation de Laplace :

• Une résolution itérative est réalisée jusqu’à ce que le critère de convergence soit atteint (seuil = 0.01) ou jusqu’à un maximum de 1000 itérations.

• Le potentiel V est mis à jour à chaque itération, en réimposant les potentiels fixes pour les conducteurs.

### 3. Visualisation des lignes équipotentielles :
• La fonction contour est utilisée pour tracer les lignes équipotentielles superposées à la carte de couleurs du potentiel V.

 <p align="center"> <img src="IMAGE/image6.jpg" width="65%" height="auto" /> </p>
 
• Les figures obtenues montrent les lignes équipotentielles pour chaque taille de domaine, mettant en évidence les transitions entre les zones de potentiel.
 
# VI.	Calcul du champ électrostatique
https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp06.m
## Objectif de cette étape :
L’objectif est de calculer et de représenter les composantes du champ électrique E⃗ dans tout le domaine de calcul.
Le champ électrique est obtenu en calculant le gradient du potentiel V, ce qui permet de visualiser les interactions électrostatiques à travers le domaine.

## Méthode et description

### 1. Initialisation du calcul :
• Les dimensions du domaine sont configurées pour différentes tailles (10 × 10, 20 × 20, 40 × 40, 80 × 80, 100 × 100).

• Les conducteurs (+100 V et -100 V) sont positionnés proportionnellement à chaque taille, en maintenant leurs emplacements relatifs.

• Les conditions aux limites (V = 0) sont appliquées sur les bords du domaine.

### 2. Calcul du potentiel V :
• L’équation de Laplace est résolue par différences finies en itérant jusqu’à ce que le critère de convergence soit atteint (seuil = 0.01) ou jusqu’à un maximum de 1000 itérations.

• Le potentiel V est mis à jour à chaque itération en réimposant les valeurs fixes pour les conducteurs.

### 3. Calcul des composantes du champ électrique :
• Une fois la convergence atteinte, les composantes Ex et Ey du champ électrique sont calculées comme suit :

Ex = −∂V/∂x, Ey = −∂V/∂y

• Ces composantes sont obtenues en utilisant la fonction gradient de MATLAB/Octave, qui calcule le gradient spatial du potentiel.

### 4. Visualisation des résultats :
   
• Une représentation graphique du potentiel V est superposée aux lignes équipotentielles à l’aide de la fonction contour.

• Les vecteurs représentant le champ électrique E⃗ sont affichés avec la fonction quiver, indiquant la direction et la magnitude du champ en chaque point.

<p align="center"> <img src="IMAGE/image7.png" width="65%" height="auto" /> </p>

# VII.	 Calculs de capacités
https://github.com/Manassehalt/ESE_CME_2425/blob/main/tp07.m

## Objectif de cette étape :

L’objectif est de calculer la capacité Cij entre deux conducteurs dans un domaine 2D en utilisant la solution de l’équation de Laplace.
La capacité est obtenue à partir de la charge totale Qi sur un conducteur et de la différence de potentiel entre les deux conducteurs (Vj − Vi).

## Méthode et description

### 1. Initialisation du calcul :
• Le domaine est configuré avec des dimensions variables (10 × 10, 20 × 20, 40 × 40, 80 × 80, 100 × 100).

• Les conducteurs sont positionnés proportionnellement à la taille du domaine, avec des potentiels fixés (+100 V pour le conducteur 1, -100 V pour le conducteur 2).

• Les conditions aux limites (V = 0) sont appliquées sur tous les bords.

### 2. Résolution de l’équation de Laplace :
• Une résolution itérative est réalisée jusqu’à atteindre un critère de convergence (seuil = 0.01) ou un maximum de 1000 itérations.

• Le potentiel V est mis à jour à chaque itération, avec réimposition des potentiels des conducteurs.

### 3. Calcul de la charge Qi :
• Les composantes Ex et Ey du champ électrique sont calculées à partir du gradient du potentiel :

Ex = −∂V/∂x, Ey = −∂V/∂y

• L’intégrale discrète de Gauss est utilisée pour calculer la charge Qi entourant le conducteur 1 :

Qi = ε0 ∮ E⃗ ⋅ dS⃗

• Cette intégrale est approximée comme la somme des contributions sur les quatre côtés du contour entourant le conducteur :

- Bord gauche : Contribution de Ey le long du côté gauche.
  
- Bord droit : Contribution de Ey le long du côté droit.
  
- Bord haut : Contribution de Ex le long du haut.
  
- Bord bas : Contribution de Ex le long du bas.


  
### 4. Calcul de la capacité Cij :

• La capacité entre les conducteurs est obtenue par la relation :

Cij = Qi / (Vj − Vi)

<p align="center"> <img src="IMAGE/image9.png" width="65%" height="auto" /> </p>



# Limitations du modèle :
### 1.	Discrétisation de l'espace :

o	La méthode des différences finies (DF) repose sur une discrétisation du domaine, ce qui introduit des approximations dans les calculs des dérivées. Ces approximations sont d'autant plus significatives que la résolution spatiale (dx, dy) est faible.

o	Pour des géométries complexes, la représentation discrète peut déformer ou simplifier les formes réelles, ce qui altère les résultats.
### 2.	Conditions aux limites :
o	Les conditions aux limites imposées sont simplifiées et rigides. Par exemple, les conducteurs sont définis comme des rectangles discrets, ce qui ne reflète pas leur géométrie réelle.

o	Si le domaine est trop petit, les effets des bords artificiels peuvent perturber les résultats.

### 3.	Hypothèses du modèle :
o	Le modèle suppose que le milieu est homogène, isotrope, et linéaire (ϵ0 constant). Toute variation spatiale ou anisotropie de la permittivité est ignorée.

o	Les charges sont supposées distribuées de manière uniforme sur les conducteurs.

### 4.	Critères de convergence :
o	La méthode itérative peut échouer à converger si le seuil ou le nombre d'itérations maximum est mal paramétré.

o	Même avec une convergence, l'erreur numérique peut subsister en raison des approximations dans les calculs intermédiaires.

## Différences entre valeur théorique et valeur numérique :
### 1.	Erreur de discrétisation :
o	Les différences finies approximent les dérivées par des rapports finis, ce qui introduit une erreur qui dépend de la taille de la grille. Une grille plus fine réduit cette erreur mais augmente le temps de calcul.
### 2.	Effet des bords :
o	Dans le modèle numérique, les bords du domaine peuvent influencer les solutions près des conducteurs. Ces effets n'existent pas dans une solution théorique où le domaine est supposé infini.
### 3.	Interpolation des charges :
o	Dans le modèle numérique, les charges sont placées sur une grille discrète. Cette interpolation peut produire des champs légèrement différents de la solution théorique.
### 4.	Erreurs cumulées :
o	Les erreurs liées à la discrétisation, aux itérations, et aux artefacts numériques s'accumulent, rendant la solution numérique légèrement différente de la solution théorique.



## Calcul de la capacité d'une structure coaxiale

Ce document présente le calcul théorique et numérique de la capacité d'une structure coaxiale. Un câble coaxial est constitué d'un conducteur interne de rayon r1, d'un conducteur externe de rayon r2, et d'un milieu diélectrique homogène de permittivité ε = ε₀·εr entre les deux.
## Calcul théorique
La capacité par unité de longueur pour un câble coaxial est donnée par la formule suivante :
C = (2π·ε·L) / ln(r₂ / r₁)
où :
- r₁ : rayon du conducteur interne
- r₂ : rayon du conducteur externe
- L : longueur du câble
- ε = ε₀·εr : permittivité du matériau entre les conducteurs.
### Calcul numérique
Pour calculer la capacité numériquement, les étapes suivantes sont réalisées :
 1. Discrétisation : Représenter une section transversale du câble coaxial dans une grille discrète.
 2. Résolution de l'équation de Laplace : Utiliser la méthode des différences finies pour obtenir le potentiel V entre r₁ et r₂.
 3. Calcul du champ électrique : À partir du potentiel V, déterminer les composantes du champ électrique Ex et Ey via le gradient.
 4. Calcul de la charge : Intégrer les composantes du champ électrique autour du conducteur interne r₁ pour obtenir la charge totale Q :
Q = ∮(ε·E · dS)
 5. **Capacité** : La capacité est ensuite donnée par :
C = Q / (v₁ - v₂)
## Comparaison entre calcul théorique et numérique
Les résultats théoriques et numériques peuvent être comparés pour valider le modèle numérique. Les écarts entre les deux approches dépendent principalement de :

- La résolution de la grille discrète (plus elle est fine, plus l'approximation est précise).
  
- La qualité de la représentation des conducteurs dans la grille discrète.
  
- Les conditions aux limites imposées dans le modèle.
## Conclusion
La méthode numérique permet d'obtenir une estimation précise de la capacité d'une structure coaxiale, même dans des configurations où la géométrie est complexe ou les conditions aux limites sont irrégulières. Bien que les résultats théoriques soient idéaux, les simulations numériques offrent une flexibilité et une précision adaptées aux applications pratiques.



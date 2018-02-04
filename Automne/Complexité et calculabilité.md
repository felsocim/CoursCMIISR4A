# Complexité et calculabilité

Mise en forme par [Marek Felsoci](mailto:marek.felsoci@etu.unistra.fr).

**L'USAGE DE CE RÉSUMÉ DE COURS NE PEUT ÊTRE QU'ACADÉMIQUE**

## Crédits

Ce résumé s'appuie sur les notes du cours de Complexité et calculabilité dispensé par Christian RONSE à l'Université de Strasbourg.

## Machines de Turing

### Contrôle avec un nombre fini d'états

Les machines de Turing utilisent une bande ordonnée avec un début mais sans fin où on peut lire, écrire et se déplacer.

![Exemple d'une machine de Turing](images/machine_turing_exemple.png)

### Mécanisme

En fonction de l'état du contrôle et du contenu de la case où se trouve la tête de lecture :

* on passe dans un (nouvel) état
  *et simultanément*
* la tête de lecture soit écrit un caractère dans la case et on ne change rien soit se déplace d'une case à droite ou à gauche

L'alphabet fini &Sigma; utilisé par l'automate contient :

* &#9655; : un symbole de début de bande
* &#8852; : un symbole de blanc
* autres symboles

Souvent l'alphabet &Sigma;<sub>0</sub> en entrée/sortie &supe; &Sigma;. Les symboles &rarr; et &larr; codant le déplacement d'une case à droite respectivement à gauche n'appartiennent pas à &Sigma;.

### Définition formelle

Une machine de Turing est un quintuplet (K, &Sigma;, &delta;, s, H) tel que :

* K est un ensemble fini d'états
* s est l'état initial et s &isin; K
* H est l'ensemble d'états d'arrêt et H &sube; K
* &Sigma; est un alphabet fini tel que {&#9655;, &#8852;} &isin; &Sigma; et {&rarr;, &larr;} &notin; &Sigma;
* &delta; est une transition qui correspond à (K - H) &times; &Sigma; &rarr; K &times; (&Sigma; &cup; {&rarr;, &larr;}) se traduisant par (q, a) &#8614; (p, b) où q &isin; (K - H), a &isin; &Sigma;, p &isin; K et b &isin; (&Sigma; &cup; {&rarr;, &larr;})

### Contraintes

Soit &delta; une transition telle que &delta;(q, a) = (p, b).

* Si a = &#9655; alors b = &rarr;.
* Si a &ne; &#9655; alors b &ne; &#9655;.

***Example***

Une machine de Turing effaçant la bande serait définie comme suit :

K = {q<sub>0</sub>, q<sub>1</sub>, h}  
s = q<sub>0</sub>  
H = {h}  
&Sigma; = {&#9655;, &#8852;, a}  
&delta; :

| q | &sigma; | &delta;(q, &sigma;) |
| --- | --- | --- |
| q<sub>0</sub> | &#9655; | (q<sub>0</sub>, &rarr;) |
| q<sub>0</sub> | &#8852; | (h, &#8852;) |
| q<sub>0</sub> | a | (q<sub>1</sub>, &#8852;) |
| q<sub>1</sub> | &#9655; | (q<sub>1</sub>, &rarr;)<sup>n'arrive jamais</sup> |
| q<sub>1</sub> | &#8852; | (q<sub>0</sub>, &rarr;) |
| q<sub>1</sub> | a | (q<sub>0</sub>, a) |

### Configuration

Une configuration représente un couple composé de l'état et le contenu de la bande avec la position de la tête de lecture. Dans la notation (q, &#9655;&#8852; **m** o t) q représente l'état, &laquo; &#9655;&#8852; **m** o t &raquo; est le contenu de la bande jusqu'au dernier symbole différent de blanc et la tête de lecture est positionnée sur **m**.

Il est également possible d'utiliser un triplet (q, w, u) pour décrire une configuration où :

* q &isin; K
* w &isin; &#9655;&Sigma;<sub>&rarr;</sub><sup>\*</sup>
* u &isin; &Sigma;<sub>&rarr;</sub><sup>\*</sup>(&Sigma; - &#8852;)
* la tête de lecture est sur le caractère le plus à droite de *w*
* *u* est le mot à droite de la tête de lecture jusqu'au dernier caractère non-blanc

**Exemples :** (q, &#9655;&#8852; **m** o t) &hArr; (q, &#9655;&#8852; **m**, o t), (q, &#9655;&#8852; m o t &#8852; **&#8852;**) &hArr; (q, &#9655;&#8852; m o t &#8852; &#8852;, &epsilon;)

### Transitions de configuration

Notons (q<sub>1</sub>, w<sub>1</sub>, a<sub>1</sub>, u<sub>1</sub>) &#8866;<sub>M</sub> (q<sub>2</sub>, w<sub>2</sub>, a<sub>2</sub>, u<sub>2</sub>) la transition de configuration entre deux états d'une machine de Turing *M* où q<sub>1</sub> &isin; K - H et &delta;(q<sub>1</sub>, a<sub>1</sub>) = (q<sub>2</sub>, b).

Lorsqu'on veut déterminer *w<sub>2</sub>* et *u<sub>2</sub>* à partir de *b* il y a trois cas possibles :

1. b &isin; &Sigma; alors *w<sub>2</sub>* = *w<sub>1</sub>*, *u<sub>2</sub>* = *u<sub>1</sub>* et *a<sub>2</sub>* = *b*
2. b = &rarr; alors la tête de lecture se déplace d'un cran à droite, *w<sub>2</sub>* = *w<sub>1</sub>a<sub>1</sub>* et :
  * si *u<sub>1</sub>* = &Sigma; alors *a<sub>2</sub>* = &#8852; et *u<sub>2</sub>* = &epsilon;
  * sinon *u<sub>1</sub>* = *a<sub>2</sub>u<sub>2</sub>* &rArr; *a<sub>2</sub>* = tête de *u<sub>1</sub>* et *u<sub>2</sub>* = queue
3. b = &larr; alors la tête de lecture se déplace d'un cran à gauche, *w<sub>1</sub>* = *w<sub>2</sub>a<sub>2</sub>* où *a<sub>2</sub>* est le dernier caractère de *w<sub>1</sub>* et *w<sub>2</sub>* est le mot d'avant,
  * si *a<sub>1</sub>* = &#8852; et *u<sub>1</sub>* = &epsilon; alors *u<sub>2</sub>* = &epsilon;
  * sinon *u<sub>2</sub>* = *a<sub>1</sub>u<sub>1</sub>*

La fermeture transitive de configurations &#8866;<sub>M</sub><sup>+</sup> signifie qu'il est possible de passer de celle de gauche à celle de droite en un nombre *n* &gt; 0 de transitions.

D'autre part la fermeture réflexive et transitive de configurations &#8866;<sub>M</sub><sup>\*</sup> signifie qu'il est possible de passer de celle de gauche à celle de droite en un nombre *n* &ge; 0 de transitions.

### Composition

Une machine de Turing peut être contruite à partir des machines de Turing de base.

#### Machines de Turing de base

Soit M<sub>a</sub> une machine de Turing de base telle que :

* a &isin; &Sigma; &cup; {&rarr;, &larr;}
* K = {s, h}
* &delta;(s,  &#9655;) = (s, &rarr;)
* &delta;(s, x) = (h, a) où x &ne; &#9655;

M<sub>a</sub> est alors une machine qui écrit la lettre *a* sur la bande.

Dans la suite on outilisera les abbréviations M<sub>&rarr;</sub> et M<sub>&larr;</sub> pour noter les machines de Turing qui déplacent la tête de lecture d'un cran à droite respectivement à gauche sur la bande.

#### Combinaisons

***Exemples***

![Combinaison de machines de Turing](images/machine_turing_composee.png)

* On démarre avec M<sub>1</sub>.
* Si M<sub>1</sub> se termine avec *a* sur la tête alors on va sur M<sub>2</sub>.
* Si M<sub>1</sub> se termine avec *b* sur la tête alors on va sur M<sub>3</sub>.

![Différents moyens de notation de machines de Turing](images/notation_mt.png)

K = {s, h} &cup; {qa | q &isin; &Sigma; - {&#9655;, &#8852;}}

|q|&sigma;|&delta;|
|---|---|---|
|s|&#9655;|(s, &rarr;)|
|s|&#8852;|(s, &rarr;)|
|s|a &ne; &#9655;, &#8852;|(q<sub>0</sub>, &rarr;)|
|s|&#9655;|(q<sub>0</sub>, &rarr;)|
|q<sub>0</sub>|x &ne; &#9655;|(h, a)|

![Machines raccourcies](images/raccourcis_mt.png)

***Définition formelle***

On décrit la combinaison des machines de Turing M<sub>1</sub>, ..., M<sub>n</sub> comme suit :

> M<sub>i</sub> = (K<sub>i</sub>, &Sigma;, &delta;<sub>i</sub>, s<sub>i</sub>, H<sub>i</sub>)

> K = &#8899;<sup>n</sup><sub>i = 1</sub> K<sub>i</sub> &cup; {h<sup>\*</sup>}

> s = s<sub>i</sub> pour &gt;M<sub>i</sub>

> H = {h<sup>\*</sup>}

Transitions :

> * celles des machines : &delta;<sub>i</sub> &supe; &#8899;<sup>n</sup><sub>i = 1</sub> &delta;<sub>i</sub>
> * entre les machines :
>   * M<sub>i</sub> &rarr; M<sub>j</sub>, &delta;(h<sub>i</sub>, &sigma;) = (s<sub>j</sub>, &sigma;) &and; &sigma; &isin; &Sigma; - {&#9655;}
>   * M<sub>i</sub> &rarr;<sup>a</sup> M<sub>j</sub>, &delta;(h<sub>i</sub>, a) = (s<sub>j</sub>, a)

> Si aucune flèche ne sort de M<sub>i</sub> avec un *a* dessus alors &delta;(h<sub>i</sub>, a) = (h<sup>\*</sup>, a).

*****

## Décisions et calcul

### Décision

Partons de la configuration initiale (s, &#9655;**&#8852;**w) où *w* est le mot en entrée. Soit H = {y, n}.

Une configuration d'arrêt sur l'état *y* respectivement sur l'état *n* est dite **acceptante** respectivement **refusante** ou **rejetante**. Une machine de Turing *M* accepte l'entrée *w* tel que *w* &isin; (&Sigma; - {&#9655;, &#8852;})<sup>\*</sup> si (s, &#9655;**&#8852;**w) &#8866;<sub>M</sub><sup>\*</sup> c'est-à-dire si on aboutit à une configuration acceptante. De même elle le rejette si on aboutit à une configuration rejetante.

En pratique *w* &isin; &Sigma;<sub>0</sub><sup>\*</sup> &sube; &Sigma; - {&#9655;, &#8852;}.

Soit L &sube; &Sigma;<sub>0</sub><sup>\*</sup> un **langage**. On dit que *M* décide *L* si et seulement si &forall;*w* &isin; L, M accepte *w* et &forall;*w* &isin; L&#773; = &Sigma;<sub>0</sub><sup>\*</sup> \\ L, M rejtte *w*. En d'autres termes si *w* fait partie du langage L la machine aboutit à l'état *y* (oui) sinon elle aboutit à l'état *n* (non).

Un langage est **récursif** s'il existe une machine de Turing qui le décide.

***Exemple***

Soit l'alphabet &Sigma;<sub>0</sub> = {a, b, c} et L un langage récursif tel que {a<sup>n</sup>b<sup>n</sup>c<sup>n</sup> | n &isin; &#8469;}

![Machine de Turing correspondante au langage récursif L](images/langage_recursif_mt.png)

Voici l'évolution du contenu de la bande : *aaabbbccc* &rarr; *$aa$bb$cc* &rarr; *$$a$$b$$c* &rarr; *$$$$$$$$$* &rArr; Y

### Calcul

Soient H = { h } l'ensemble des états d'arrêt, l'alphabet &Sigma;<sub>0</sub> &sube; &Sigma; - {&#9655;, &#8852;} et un mot *w* &isin; &Sigma;<sub>0</sub><sup>\*</sup>.

Si pour la configuration de départ (s, &#9655;**&#8852;**w) la machine de Turing s'arrête sur la configuration (h, &#9655;**&#8852;**u) avec *w* &isin; &Sigma;<sub>0</sub><sup>\*</sup> alors *u* est la sortie de la machine de Turing pour l'entrée *w*. Si la machine de Turing est notée M on écrit *u* = M(*w*). Si M ne s'arrête pas sur l'entrée *w* alors M(*w*) n'est pas défini.

Soit *f* : &Sigma;<sub>0</sub><sup>\*</sup> &rarr; &Sigma;<sub>0</sub><sup>\*</sup> une fonction. Une fonction est **récursive** s'il y a une machine de Turing qui la calcule. Dans le cas des fonctions à plusieurs variables il faut un caractère spécial, un séparateur, pour séparer les mots en entrée.

***Exemple***

Addition entière : &Sigma;<sub>0</sub> = {0, 1, ;}

> 5 + 7 = 12

> 101;111 &rarr; 1100

Une machine de Turing M calcule *f* : &#8469;<sup>k</sup> &rarr; &#8469; si pour l'entrée *codage*(x<sub>1</sub>, ..., x<sub>k</sub>), M s'arrête sur le mot en sortie *codage*(*f*(x<sub>1</sub>, ..., x<sub>k</sub>)).

### Semi-décision

Soient L &sube; &Sigma;<sub>0</sub><sup>\*</sup> tel que &Sigma;<sub>0</sub><sup>\*</sup> &sube; &Sigma; - {&#9655;, &#8852;}.

Une machine de Turing M semi-décide L si et seulement si &forall;*w* &isin; &Sigma;<sub>0</sub><sup>\*</sup>. *w* &isin; L si et seulement si M s'arrête sur l'entrée *w*. En d'autres termes si *w* &isin; L la machine M s'arrête sinon elle boucle.

Un langage est **récursivement énumérable** s'il existe une machine de Turing qui le semi-décide. Voici quelques propriétés de L :

> Si L est récursif alors L&#773; aussi. Il suffit d'inverser les Y et les N à la sortie de la machine de Turing.
> Si L est récursif alors L est récursivement énumérable. Autrement dit, on transforme l'état d'arrêt *n* en un état qui boucel sur lui-même : &delta;(*n*, *n* &ne; &#9655;) = (*n*, *x*).

## Extensions de machines de Turing

### Plusieurs bandes

À chaque instant, en fonction de l'état et des caractères lus sur toutes les bandes on passe dans un nouvel état et on effectue une action sur chacune des bandes. Pour *n* bandes la fonction de transition sera de la forme :

> &delta;(q, a<sub>1</sub>, ..., a<sub>n</sub>) = (p, b<sub>1</sub>, ..., b<sub>n</sub>) où a<sub>i</sub> &isin; &Sigma; et b<sub>i</sub> &isin; &Sigma; &cup; {&rarr;, &larr;}

Notons la configuration comme suit :

> (q', w<sub>1</sub>**a<sub>1</sub>**u<sub>1</sub>, ..., w<sub>n</sub>, **a<sub>n</sub>**, u<sub>n</sub>)

Les mots d'entrée et de sortie se trouvent sur la première bande.

Soit M une machine de Turing à *k* bandes. Il existe une machine de Turing standard M' sur un alphabet &Sigma;' tel que &Sigma; &sube; &Sigma;' avec les mêmes états d'arrêt telle que &forall;*x*,*y* &isin; &Sigma;<sup>\*</sup> M sur l'entrée *x* s'arrêtera sur la sortie *y* si et seulement si M' sur la sortie *x* s'arrêtera sur *y* avec le même état d'arrêt.

#### Compléxité

Si M le fait en *t* étapes, M' le fait en &Theta;(*t*(|*x*| + *t*)).

![Exemple d'une machine de Turing à plusieurs bandes](images/bandes_multiples_mt.png)

Les séries binaires indiquent la position de la première respectivement de la deuxième tête de lecture sur les bandes.

#### Application théorique de deux bandes

Un langage L est récursif si et seulement si L et L&#773; sont tous les deux récursivement énumérables.

***Démonstration***

**&rArr; :** Si L est récursif alors L&#773; l'est aussi. De plus si L et L&#773; sont récursifs alors ils sont également récursivement énumérables.

**&lArr; :** Soient M<sub>1</sub> qui semi-décide L, K<sub>1</sub>, H<sub>1</sub> = {h<sub>1</sub>} et M<sub>2</sub> qui semi-décide L<sub>2</sub>, K<sub>2</sub>, H<sub>2</sub> = {h<sub>2</sub>}.

On démarre avec &#9655;**&#8852;**w sur chaque bande. M est une machine de Turing à deux bandes telle que K = (K<sub>1</sub> &times; K<sub>2</sub>) &cup; {*y*, *n*} et ayant les transitions suivantes :

> Soient deux états non-terminaux q<sub>1</sub> &isin; K<sub>1</sub> - H<sub>1</sub> et q<sub>2</sub> &isin; K<sub>2</sub> - H<sub>2</sub>.

> &delta;(q<sub>1</sub>, a<sub>1</sub>) = (p<sub>1</sub>, b<sub>1</sub>), &delta;(q<sub>2</sub>, a<sub>2</sub>) = (p<sub>2</sub>, b<sub>2</sub>)

> &delta;((q<sub>1</sub>, q<sub>2</sub>), a<sub>1</sub>, a<sub>2</sub>) = ((p<sub>1</sub>, p<sub>2</sub>), b<sub>1</sub>, b<sub>2</sub>)

> &delta;((h<sub>1</sub>, q<sub>2</sub>), a<sub>1</sub>, a<sub>2</sub>) = (y, a<sub>1</sub>, a<sub>2</sub>)

> &delta;((q<sub>1</sub>, h<sub>2</sub>), a<sub>1</sub>, a<sub>2</sub>) = (n, a<sub>1</sub>, a<sub>2</sub>)

### Bande infinie des deux cotés

Dans ce cas il n'y a plus besoin du symbole de début de bande. Il est possible de simuler cette construction par une machine de Turing à 2 bandes finies ayant des numéros de cases dans &#8484;.

![Modélisation d'une bande infinie par deux bandes finies](images/machine_turing_bande_infinie.png)

### Multiples têtes

**Remarque :** Pour remédier au problème lorsque deux têtes de lecture se trouvent sur une seule et même case on peut utiliser des bandes en deux dimensions.

Toute fonction décidée ou semi-décidée par les machines de Turing avec variantes peut l'être par une machine de Turing standard.

De même si la compléxité d'une fonction décidée ou semi-décidée par une machine de Turing avec variantes est polynomiale elle le sera aussi avec une machine de Turing standard étant donné qu'en remplaçant des variables d'un polynôme par des polynômes on obtient toujours un autre polynôme.

## Machines de Turning non-déterministes

La fonction de transition &delta; est remplacée par une relation &Delta; telle que &Delta; &sube; (K - H) &times; &Sigma; &times; K &times; (&Sigma; &cup; {&rarr;, &larr;}) où (K - H) est l'ensemble des états non-terminaux, &Sigma; est le contenu de la bande, K le nouvel état et (&Sigma; &cup; {&rarr;, &larr;}) l'action à effectuer sur la bande.

Alors &Delta;(*q*, *a*, *p*, *b*) signifie que si on est dans l'état *q* &notin; H et qu'on lit *a* sur la bande alors on peut aller dans l'état *p* et faire *b* sur la bande.

|*q*|*a*|(*p*, *b*)|
|---|---|---|
|...|...|...|

Soit &#8866; le symbole de transition alors C &#8866; C' veut dire que l'on peut aller de la configuration C à la configuration C'. À partir d'une configuration intiale on obtient par dérivation une arborescence de configurations :

![Exemple d'arborescence de configuration](images/arbo.png)

*Notations*

* *s* : configuration initiale
* *w* : partie du mot déjà lue
* *v* : partie du mot à droite de la tête de lecture

On note *r* le nombre maximum de choix qu'on peut avoir à chaque configuration tel que *max*(*Card*({(*p*, *b*) &isin; K &times; (&Sigma; &cup; {&rarr;, &larr;} | &Delta;(*q*, *a*, *p*, *b*)}))), (*q*, *a*) &isin; (K - H) &times; &Sigma;.

Soit M une machine de Turing non-déterministe. M accepte le mot *w* si pour un *h* &isin; H et *v*, *u* &isin; &Sigma;<sup>\*</sup>, a &isin; &Sigma; on a (s, &#9655;**&#8852;**w) &#8866; (h, v**a**u). Autrement dit, pour une succession de dérivations on peut à partir d'une configuration initiale aboutir à une configuration terminale.

M décide le langage L si pour tout mot *w* en entrée :

* il existe un *n* &isin; &#8469; fonction de *w* et de M pour lequel il n'y a pas de configuration C avec (*s*, &#9655;**&#8852;** *w*) &#8866;<sup>n</sup> C,
* *w* &isin; L &hArr; &exist; *u*, *v* &isin; &Sigma;<sup>\*</sup>, a &isin; &Sigma;, (s, &#9655;**&#8852;**w) &#8866;<sup>\*</sup> (y, v**a**u) (au moins une dérivation aboutit à *y*, sinon elles sont toutes à *n*).

*Arborescence de configurations*

Si *w* &isin; L alors &exist; une feuille *y*, sinon toutes les feuilles donnent *n*.

![Illustration d'arborescence de configurations](images/arbo2.png)

M semi-décide L pour tout mot *w* en entrée si *w* &isin; L &hArr; &exist; *u*, *v* &isin; &Sigma;<sup>\*</sup>, a &isin; &Sigma;, (s, &#9655;**&#8852;**w) &#8866;<sup>\*</sup> (h, u**a**v) (au moins une dérivation aboutit à l'arrêt).

### Calcul d'une fonction *f*

*Conditions*

1. Comme pour la décision.
2. (*s*, &#9655;**&#8852;**w) &#8866;<sup>\*</sup> (h, u**a**v) &hArr; u**a**v) = &#9655;**&#8852;** *f*(w)

Tout langage décidé ou semi-décidé et toute fonction calculée par une machine de Turing non-déterministe l'est aussi par une machine de Turing déterministe. Si la machine de Turing non-déterministe décide en temps *t*, la machine de Turing déterministe décide en temps O(r<sup>*t*</sup>) (en faisant toujours un parcours en largeur).

L'idée du non-déterminisme est que si on a beaucoup de chance on trouve le résultat en temps polynomiale sinon en temps exponentiel (plus exactement factoriel mais comparable à l'exponentiel via la formule de Sterling) car il y a un long parcours de possibilités à effectuer.

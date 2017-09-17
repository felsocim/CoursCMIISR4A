# Complexité et calculabilité

## À propos

Ce document reprend les notes du cours de Complexité et calculabilité dispensées par [**M Christian RONSE**](https://dpt-info.u-strasbg.fr/~cronse/welcome.html) à *l'Université de Strasbourg*.

Mise en forme par [Marek Felsoci](mailto:marek.felsoci@etu.unistra.fr).

**ATTENTION !** L'USAGE DE CE RÉSUMÉ DE COURS NE PEUT ÊTRE QU'ACADÉMIQUE.

## 1 Machines de Turing

### 1.1 Contrôle avec un nombre fini d'états

Les machines de Turing utilisent une bande ordonnée avec un début mais sans fin où on peut lire, écrire et se déplacer.

![Exemple d'une machine de Turing](images/machine_turing_exemple.png)

### 1.2 Mécanisme

En fonction de l'état du contrôle et du contenu de la case où se trouve la tête de lecture :

* on passe dans un (nouvel) état
  *et simultanément*
* la tête de lecture soit écrit un caractère dans la case et on ne change rien soit se déplace d'une case à droite ou à gauche

L'alphabet fini &Sigma; utilisé par l'automate contient :

* &#9655; : un symbole de début de bande
* &#8852; : un symbole de blanc
* autres symboles

Souvent l'alphabet &Sigma;<sub>0</sub> en entrée/sortie &supe; &Sigma;. Les symboles &rarr; et &larr; codant le déplacement d'une case à droite respectivement à gauche n'appartiennent pas à &Sigma;.

### 1.3 Définition formelle

Une machine de Turing est un quintuplet (K, &Sigma;, &delta;, s, H) tel que :

* K est un ensemble fini d'états
* s est l'état initial et s &isin; K
* H est l'ensemble d'états d'arrêt et H &sube; K
* &Sigma; est un alphabet fini tel que {&#9655;, &#8852;} &isin; &Sigma; et {&rarr;, &larr;} &notin; &Sigma;
* &delta; est une transition qui correspond à (K - H) &times; &Sigma; &rarr; K &times; (&Sigma; &cup; {&rarr;, &larr;}) se traduisant par (q, a) &#8614; (p, b) où q &isin; (K - H), a &isin; &Sigma;, p &isin; K et b &isin; (&Sigma; &cup; {&rarr;, &larr;})

### 1.4 Contraintes

Soit &delta; une transition telle que &delta;(q, a) = (p, b).

* Si a = &#9655; alors b = &rarr;.
* Si a &ne; &#9655; alors b &ne; &#9655;.

### Example

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

### 1.5 Configuration

Une configuration représente un couple composé de l'état et le contenu de la bande avec la position de la tête de lecture. Dans la notation (q, &#9655;&#8852; **m** o t) q représente l'état, &laquo; &#9655;&#8852; **m** o t &raquo; est le contenu de la bande jusqu'au dernier symbole différent de blanc et la tête de lecture est positionnée sur **m**.

Il est également possible d'utiliser un triplet (q, w, u) pour décrire une configuration où :

* q &isin; K
* w &isin; &#9655;&Sigma;<sub>&rarr;</sub><sup>\*</sup>
* u &isin; &Sigma;<sub>&rarr;</sub><sup>\*</sup>(&Sigma; - &#8852;)
* la tête de lecture est sur le caractère le plus à droite de *w*
* *u* est le mot à droite de la tête de lecture jusqu'au dernier caractère non-blanc

**Exemples :** (q, &#9655;&#8852; **m** o t) &hArr; (q, &#9655;&#8852; **m**, o t), (q, &#9655;&#8852; m o t &#8852; **&#8852;**) &hArr; (q, &#9655;&#8852; m o t &#8852; &#8852;, &epsilon;)

### 1.6 Transitions de configuration

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

### 1.7 Composition

Une machine de Turing peut être contruite à partir des machines de Turing de base.

#### 1.7.1 Machines de Turing de base

Soit M<sub>a</sub> une machine de Turing de base telle que :

* a &isin; &Sigma; &cup; {&rarr;, &larr;}
* K = {s, h}
* &delta;(s,  &#9655;) = (s, &rarr;)
* &delta;(s, x) = (h, a) où x &ne; &#9655;

M<sub>a</sub> est alors une machine qui écrit la lettre *a* sur la bande.

Dans la suite on outilisera les abbréviations M<sub>&rarr;</sub> et M<sub>&larr;</sub> pour noter les machines de Turing qui déplacent la tête de lecture d'un cran à droite respectivement à gauche sur la bande.

#### 1.7.2 Combinaisons

##### Exemple

![Combinaison de machines de Turing](images/machine_turing_composee.png)

* On démarre avec M<sub>1</sub>.
* Si M<sub>1</sub> se termine avec *a* sur la tête alors on va sur M<sub>2</sub>.
* Si M<sub>1</sub> se termine avec *b* sur la tête alors on va sur M<sub>3</sub>.

##### Définition formelle

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

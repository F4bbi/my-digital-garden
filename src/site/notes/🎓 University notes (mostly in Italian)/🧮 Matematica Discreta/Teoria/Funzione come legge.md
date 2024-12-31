---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/matematica-discreta/teoria/funzione-come-legge/","created":"2023-01-23T16:26:11.445+01:00","updated":"2023-01-23T16:26:11.445+01:00"}
---

# Funzione come legge
Siano $X$ e $Y$ due insiemi non vuoti.
Una funzione $f_{legge}:X \rightarrow Y$ (come legge, visto come concetto primitivo),
è una legge che ad ogni $x \in X$ associa un unico $y \in Y$
Ad ogni input si può associare un solo output
## Equivalenza tra funzione come relazione e funzione come legge
(Nel caso in cui $X \neq \varnothing$, $Y \neq \varnothing$)
### Definizione
Dati $X$ e $Y$, due insiemi (eventualmente vuoti), definiamo l'insieme $Y^X$ come l'insieme i cui elementi sono tutte e sole le funzioni del tipo $f:X \rightarrow Y$.
$f \subset X$ x $Y$ $\Leftrightarrow \space f \in 2^{X \space x \space Y}$ 
#### Osservazione
```ad-info
title: INSIEME DELLE PARTI
Dato un insieme A, indichiamo con $2^A$  o P(A) l'insieme i cui elementi sono tutti i sottoinsiemi di A, detto INSIEME DELLE PARTI DI A.
```

##### Esempio
$A = \{1,2,3\}$
$2^A = \{\varnothing, \{1\}, \{2\}, \{3\}, \{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\} \}$

#### Osservazione
$Y^X = \{ f \in 2^{X \space x \space Y} | (\forall x \in X, \exists ! \space y \space \space t.c. (x,y) \in f)\}$ 
				  $\hookrightarrow INSIEME$
ASSIOMA DI SEPARAZIONE $\implies$ $Y^X$ è un insieme

##### Esercizio
Dato un insieme $X$ (qualsiasi), calcolare $X^{\varnothing}$ e $\varnothing^X$. 
Suggerimento: qui le funzioni sono relazioni.
$X^\varnothing = \{f \in 2^{\varnothing \ \times \ X} = 2^\varnothing = \{\varnothing\} \ | \ \forall \in \varnothing, \exists \ I \ g \in X$ t.c. $(x,y) \in f\} = \{\varnothing\}$  
$\varnothing^X =$

##### Soluzione
Sia X un insieme $\neq \varnothing$, vale:
$$\varnothing^X = \{f \in 2^{X \times \varnothing} = 2^\varnothing = \{\varnothing\} \ | \ \forall x \in X, \ \exists ! \ y \in \varnothing \ t.c. \ (x,y) \in f \in \varnothing\} = \varnothing$$
##### Esercizio
Siano $X,Y,Z$ tre insiemi e siano $f:X \rightarrow Y$ e $g:Y\rightarrow Z$ due funzioni. Dimostrare che:
1) $f,g$ iniettiva $\implies$ $g \ o \ f$ iniettiva
2) $f,g$ surgettiva $\implies$ $g \ o \ f$ surgettiva
3) $f,g$ bigettiva $\implies$ $g \ o \ f$ bigettiva
4) $f$ bigettiva $\implies$ $f^{-1}$ bigettiva
##### Soluzione
1. Siano $x_1, x_2 \in X$ t.c. $x_1 \neq x_2$
	Vale: $f(x_1) \neq f(x_2)$ in quanto $f$ è iniettiva.
	$g(f(x_1)) \neq g(f(x_2))$ in quanto $g$ è iniettiva.
	$(g \ o \ f)(x_1) \neq (g \ o \ f)(x_2)$
2. Supponiamo che $f$ e $g$ siano surgettive.
	Sia $z \in Z$. Perchè $g$ è surgettiva, 
	$\exists \ y \in Y$ t.c. $g(y) = z$. Anche $f$ è surgettiva, dunque $\exists x \in X$ t.c.$f(x) = y$. Vale:$$(g \ o \ f)(x) = g(f(x)) = g(y) = z$$
3. 1. + 3. bigettività preservata
##### Esercizio 1.11 e 1.12
Ci son 2 errori. Quali?
	
Da qui in poi $f_{legge} = f$

##### Esempio 1.12
Sia $X$ un insieme non vuoto, indichiamo con $id_x:X \rightarrow X$  tale che $id_x(x) = x$, $\forall x \in X$
### Definizione
```ad-info
title: COMPOSIZIONE
Siano $X$, $Y$ e $Z$ tre insiemi non vuoti e $f:X\rightarrow Y$ e $g:Y \rightarrow Z$ due funzioni.
Definiamo la composizione $g \space o \space f:X \rightarrow Z$, detto composizione di f e g, come segue:

$(g \space o \space f)(x):= g(f(x)) \space \forall x \in X$
```
---
### Definizione
```ad-info
title: IMMAGINE
Siano $X$ e $Y$ due insiemi non vuoti, e sia $f:X \rightarrow Y$ una funzione. Dato $A \subset X$ definiamo l'immagine di A tramite $f$ come segue:

$f(A):=\{y \in Y | \space \exists x \in A, \space t.c. \space y = f(x)\}$
$f(x)$ si dice IMMAGINE di $f$.
```
---
### Definizione
```ad-info
title: CONTROIMMAGINE
Dato $B \subset Y$, definiamo la controimmagine di B tramite $f$, ponendo

$f^{-1}(B) := \{x\in X \ | \ f(x) \in B\}$
Se $B = \{y\}$, allora

$f^{-1}(Y) = f^{-1}(\{Y\}) := \{x\in X \ | \ f(x) = y\}$
																				           	$\hookrightarrow$ fibra di $f$ sopra $y$       $\hookrightarrow$ incognita
```

Dato $f:X \rightarrow Y$, si dice che:
- f è <mark style="background: #FF5582A6;">INIETTIVA</mark> se $\forall x_1, x_2, \in X$ t.c. $x_1 \neq x_2$, $f(x_1) \neq f(x_2)$
- f è <mark style="background: #FFB86CA6;">SURGETTIVA</mark> se $f(x) = y$  ovvero $\forall y \in Y, \exists x \in X$ t.c. $f(x) = y$.
- f è <mark style="background: #FFF3A3A6;">BIGETTIVA</mark> se è iniettiva e surgettiva.

##### Esercizio
$\mathbb{R}$ = "insieme dei numeri reali"
$\mathbb{R}^+ = \{t \in \mathbb{R} \ \ | t \geq 0\}$
1) $f_1 : \mathbb{R}, f_1(x)$
---
## Proposizione 1.21
```ad-info
title: FUNZIONE INVERSA
Sia $f:X \rightarrow Y$ una funzione bigettiva tra insiemi non vuoti. Allora è esiste ed è unica una funzione $g:Y \rightarrow X$
$g \ o \ f = id_x$ e $f \ o \ g = id_y$

In questo caso g si dice INVERSA di $f$ e si indica con $f^{-1}$
```
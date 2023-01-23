---
{"dg-publish":true,"permalink":"/university-notes-in-italian/matematica-discreta/teoria/teorema-di-ricorsione/"}
---

# Teorema di ricorsione
Sia $X$ un insieme non vuoto. Sia $h: \mathbb N \times X \rightarrow X$ e sia $c \in X$.
Esiste ed è unica una funzione $f:\mathbb N \rightarrow X$ t.c. 
1) $f(0) = c$
2) $f(succ(n)) = h(n,f(n)) \ \forall n \in \mathbb N$
3) $f(1) = h(0,c)$
4) $f(2) = h(1, f(1)) = h(1,h(0,c))$
 ## Applicazione del teorema di ricorsione
 Sia $n \in \mathbb N$
 Definiamo la "funzione somma sulla sinistra con m" ovvero "$\mathbb N \rightarrow \mathbb N$", $n \rightarrow m+n$.
 Poniamo 
 - $X:=\mathbb N$
 - $c_i=?$ formalizzare il seguente concetto: $c= "f(0) = m+n")$
 - $h:\mathbb N \times \mathbb N \rightarrow \mathbb N$
	  $(a,b) \longmapsto succ(n)$
	$m + succ(n) = h(n, m+n)$
	$m+(n+1)$
	$h(a,b):=succ(b)$
Grazie al teorema di ricorsione esiste unica 
$f:\mathbb N \rightarrow \mathbb N$ t.c. $f(0) = m, f(succ(n)) = h(n,f(n)) = succ(f(n)) \ \forall n \in \mathbb N$
$f(n) = m+n \ \ \ m+0=m$
								$m+succ(n) = succ(m+n) \ \forall n \in \mathbb N$
### Esercizio
Trovare $X(=\mathbb N),c(=0),h$ che sostituiti nel teorema di ricorsione, formalizzino la seguente intuizione: 
$\mathbb N \rightarrow \mathbb N$
$n \rightarrow m \times n \ (m \in \mathbb N \ fissato)$
#### Esercizio 3.1
$0+n=n$
$1+n=n+1$
dove $1:=succ(0)$
### Osservazione
$n+1=succ(n) \ \forall n \in \mathbb N$

## Ordinamento dei numeri naturali
### Definizione
```ad-info
title: ORDINAMENTO PARZIALE
Sia $X$ un insieme non vuoto e sia $R$ una relazione binaria su $X$ (cioè $R \subset X \times X$).
Si dice che $R$ è un ordinamento parziale di $X$ se
1) $x \ R \ x \ \ \forall x \in X$ *(riflessiva)*
2) $(x \ R \ x) \ e \ (y \ R \ x) \implies x = y$ *(antisimmetrica)*
3) $(x \ R \ x) \ e \ (y \ R \ z) \implies x \ R \ z$ *(transitiva)*

Se in aggiunta vale la seguente proprietà *(tricotomica)*, allora si dice che $R$ è un ordinamento totale di $X$.

4) $\forall x \in X, \ x \ R \ y$ vera oppure $y \ R \ X$ vera

Usualmente si utilizza $R = \leq$
Se $\leq$ è un ordinamento parziale di $X$, ($X, \leq$) si dice insieme parzialmente ordinato.
Se $\leq$ è un ordinamento totale di $X$, ($X, \leq$) si dice insieme totalmente ordinato.
```
### Definizione
$\forall n,m \in \mathbb N$, poniamo:
$$n \leq m \ \ se \ \exists \ k \in \mathbb N \ t.c. \ m=n+k$$
## Proposizione 3.5
$(\mathbb N, \leq)$ è un insieme totalmente ordinato

#### Esercizio 3.3
Dimostrare che $n,m,k \in \mathbb N,$
1) $n \leq m$ e $k \leq n \implies n+k \leq m+h$
2) $n \leq m$ e $k \leq n \implies nk \leq mh$
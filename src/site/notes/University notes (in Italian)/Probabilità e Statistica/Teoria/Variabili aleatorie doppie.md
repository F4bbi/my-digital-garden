---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/variabili-aleatorie-doppie/","created":"2022-05-23T08:28:56.232+02:00","updated":"2023-01-23T01:48:39.333+01:00"}
---

# Variabili aleatorie doppie
Con riferimento ad uno spazio campionario $\Omega$, una variabile aleatoria doppia $(X,Y)$ è una coppia di funzioni $X(\omega)$ e $Y(\omega)$ a valori reali che associa ad ogni evento elementare $\omega \in \Omega$ una coppia di numeri reali $(x,y)$.

Si dividono in 
- Variabili aleatorie doppie discrete
- Variabili aleatorie doppie continue

## Distribuzioni condizionali e indipendenza
### V.a. discrete
Due v.a. discrete X e Y sono stocasticamente indipendenti se (e solo se)
$$p_{X,Y}(x,y) = p_X(x) \cdot p_Y(y)$$
questo implica che
$$p_{X,Y}(x,y) = p_{X|Y}(X=x\ | \ Y=y) \cdot p_Y(y) = p_{Y|X}(Y=y\ | \ X=x) \cdot p_X(x) $$
### V.a. continue
Due v.a. continue X e Y sono stocasticamente indipendenti se (e solo se)
$$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y)$$
In caso di indipendenza di variabili continue vale:
$$f_{X|Y}(x|y) = f_X(x)$$
Inoltre
$$f_{X|Y} = \frac {f_{X,Y}(x,y)}{f_Y(y)}$$
### Valore atteso
$\mathbb E(X \ | \ Y=y) = \sum_x x \cdot p_{X|Y}(x|y)$

$\mathbb Var(X \ | \ Y=y) = \sum_x (x - \mathbb E(X \ | \ Y=y))^2 \cdot p_{X|Y}(x|y)$

### Covarianza
$\mathbb Cov(X,Y) = \mathbb E(X \cdot Y) - \mathbb E(X) \cdot \mathbb E(Y)$

### Indice di Pearson
$\rho(X,Y) = \frac {\mathbb Cov(X,Y)}{\sqrt {\mathbb Var(X) \cdot \mathbb Var(Y)}}$
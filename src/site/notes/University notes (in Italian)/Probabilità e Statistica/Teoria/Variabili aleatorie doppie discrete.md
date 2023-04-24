---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/variabili-aleatorie-doppie-discrete/","created":"2022-05-23T08:39:56.942+02:00","updated":"2023-01-23T01:49:18.108+01:00"}
---

# Variabili aleatorie doppie discrete
## Proprietà
- $f_{X,Y}(x,y) \geq 0$

- $\sum_x \sum_y f_{X, Y}(x, y) = 1$

## Probabilità congiunta 
La distribuzione di probabilità congiunta discreta esprime la probabilità che $X$ assuma il valore $x$ e, contemporaneamente, $Y$ assuma il valore $y$, ossia
$$f_{X,Y}(x,y) = P(X=x \cap Y=y)$$

#### Esempio
Data la seguente $f_{X,Y}(x,y)$

| $_X\backslash^Y$ | 0 | ==1== |  |
|------|----------|----------|------|
| ==0== | 0.09     | ==0.66==      | 0.75  |
| 1 | 0.01      | 0.24      | 0.25  ||
|| 0.1     | 0.9     | 1 | |

quanto vale la probabilità congiunta $f_{X,Y}(0,1)$?
$$f_{X,Y}(0,1) = P(X=0 \cap Y=1)=0.66$$

## Probabilità marginale
Le distribuzioni di probabilità marginali discrete rappresentano le probabilità di una delle due variabili, ignorando l’altra.
Si ottengono quindi sommando le probabilità congiunte rispetto all’altra variabile:
$$f_X(x) = P(X=x) = \sum_y f_{X, Y}(x, y)$$
$$f_Y(y) = P(Y=y) = \sum_x f_{X, Y}(x, y)$$
#### Esempio
Data la seguente $f_{X,Y}(x,y)$

| $_X\backslash^Y$ | ==0== | 1 |  |
|------|----------|----------|------|
| 0 | 0.09     | 0.66      | 0.75  |
| 1 | 0.01      | 0.24      | 0.25  ||
|| ==0.1==     | 0.9     | 1 | |

quanto vale la probabilità marginale $f_{Y}(0)$, cioè $P(Y=0)$?
$$f_{Y}(0) = P(Y=0) = \sum_x f_{X, Y}(x, y) = f_{X,Y}(0,0) + f_{X,Y}(1,0) = 0.09 + 0.01 = 0.10$$

## Valore atteso 
$\mathbb{E}(X) = \sum\limits_{x} x \cdot p_X(x)$
$\mathbb{E}(XY) = \sum_x \sum_y \ x \cdot y \cdot p_{X,Y}(x,y)$

## Varianza
$\mathbb Var(X) = E(X^2) - E(X)^2 = \sum\limits_{x} x^2 \cdot p_X(x) + \left(\sum\limits_{x} x \cdot p_X(x)\right)^2$

## Distribuzioni condizionali e indipendenza
Due v.a. discrete X e Y sono stocasticamente indipendenti se (e solo se)
$$p_{X,Y}(x,y) = p_X(x) \cdot p_Y(y)$$
questo implica che
$$p_{X,Y}(x,y) = p_{X|Y}(X=x\ | \ Y=y) \cdot p_Y(y) = p_{Y|X}(Y=y\ | \ X=x) \cdot p_X(x) $$
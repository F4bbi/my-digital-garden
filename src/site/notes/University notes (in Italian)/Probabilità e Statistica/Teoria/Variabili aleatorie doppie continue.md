---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/variabili-aleatorie-doppie-continue/"}
---

# Variabili aleatorie doppie continue
## Proprietà
- $f_{X,Y}(x,y) \geq 0$

- $\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f_{X, Y}(x, y) \ dx \ dy = 1$

## Probabilità congiunta 
La distribuzione di probabilità congiunta continua viene usata per esprimere la probabilità che $X$ assuma un valore nell’intervallo $[a,b]$ e, contemporaneamente, $Y$ assuma un valore nell’intervallo $[c,d]$, ossia
$$P(X \in [a,b] \cap X \in [a,b]) = \int_{a}^{b} \int_{c}^{d} f_{X, Y}(x, y) \ dx \ dy$$

## Probabilità marginale
Si ottengono integrando la densità congiunta rispetto all’altra variabile:
$$f_X(x) =\int_{R_Y} f_{X,Y}(x,y) dy$$
$$f_Y(y) =\int_{R_X} f_{X,Y}(x,y) dx$$

## Valore atteso 
$\mathbb{E}(X) = = \int_{-\infty}^{+\infty} x\cdot f_X(x) dx$

$\mathbb{E}(XY) = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} x \cdot y \cdot f_{X,Y}(x,y) \ dx \ dy$

## Varianza
$\mathbb Var(X) = E(X^2) - E(X)^2$

## Distribuzioni condizionali e indipendenza
Per le v.a. continue vale la stessa cosa:
$$f_{X|Y} = \frac {f_{X,Y}(x,y)}{f_Y(y)}$$
Due v.a. continue X e Y sono stocasticamente indipendenti se (e solo se)
$$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y)$$
In caso di indipendenza di variabili continue vale:
$$f_{X|Y}(x|y) = f_X(x)$$
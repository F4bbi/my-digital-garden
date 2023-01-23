---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/funzione-di-probabilita-continua/"}
---

# Funzione di probabilità continua
Una v.a. $X$ definita su $(\Omega, \mathcal A)$ è detta continua se la sua funzione di ripartizione è continua.

## Proprietà
- $Pr(X \in (a,b]) = Pr(a < X \leq b) = \int_{a}^{b} f(x) \ dx$

- $f(x) > 0$ per ogni $x \in \mathbb R$

- $\int_{-\infty}^{\infty} f(x) \ dx = 1$

- $Pr(X=x) = 0$

### Come si passa da una funzione di densità discreta ad una funzione di distribuzione/ripartizione?
$\Large{F(x) = \int_{-\infty}^{x} f(t) \ dt} = \begin{cases} 0 & \text{ for } t < 0 \\ \int_0^t f(s) ds & 0 \leq t < n \\ 1 & \text{ for } t \geq n \end{cases}$

## Valore atteso
$\mathbb{E}(X) = \int_{-\infty}^{\infty}\ x\cdot f_X(x) \ dx$

## Momento non centrato di ordine r
$\mathbb{E}(X^r) = \int_{-\infty}^{\infty}\ x^r\cdot f_X(x) \ dx$

## Varianza
$Var(X) = E(X^2) - E(X)^2$
- Date due costanti a e b e posto $Y = aX + b$ allora
	$Var(Y) = Var(aX + b) = a^2 \cdot Var(X)$
	
## Funzioni di probabilità continue da sapere:
- **Distribuzione Normale (o di Gauss)**

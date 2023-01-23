---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/formulario/"}
---

# Formulario di Probabilità e Statistica
**$\star$Requisiti algebra**
-   $\Omega$ e $\emptyset \in \mathcal{A}$
-   $\forall A \in \mathcal{A}$ anche $A^c \in \mathcal{A}$
-   l'unione finita di elementi di $\mathcal{A}$ continua a stare in $\mathcal{A}$.

**$\star$Teorema di Bayes**

$P(A_i | H) = \frac{P(A_i)P(H | A_i)}{P(H)} =\frac{P(A_i)P(H | A_i)}{ \sum_{i=1}^n P(A_i)P(H | A_i) }$

**$\star$Proprietà di una funzione di ripartizione**
- $F_X(x) = P(X \leq x)$
- **Non** decrescente
- $0 \leq F_X(x) \leq 1$
- $lim_{x \to +\infty} \ F_X(x_{0})= 1$
- $lim_{x \to -\infty} \ F_X(x_{0})= 0$

Nel caso di una variabile casuale *discreta* si aggiungono le proprietà:
- continua da destra $\forall x \in \mathbb R$, quindi $lim_{x \to x_{0+}}F_X(x)= F_X(x_0)$
{ #1c2ff9}

- $lim_{x \to x_{0-}}F_X(x) \neq F_X(x_0)$

Inoltre:
$\text{Pr}((a,b]) = F(b^+) - F(a^+)$
$\text{Pr}([a,b)) = F(b^-) - F(a^-)$
$\text{Pr}((a,b)) = F(b^-) - F(a^+)$
$\text{Pr}([a,b]) = F(b^+) - F(a^-)$
$\text{Pr}(X = x) = \text{Pr}((x,x])= lim_{x \to x_{0+}}F_X(x) - lim_{x \to x_{0-}}F_X(x)$

**$\star$Valore atteso (momento non centrato)**
$\mathbb{E}(X) = \sum\limits_{x = 0}^n x \cdot p_X(x) = \int_{R_X}\ x\cdot f_X(x) \ dx$
$\mathbb{E}(XY) = \sum_x \sum_y \ x \cdot y \cdot p_{X,Y}(x,y) = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} x \cdot y \cdot f_{X,Y}(x,y) \ dx \ dy$

$\mathbb E(X \ | \ Y=y) = \sum_x x \cdot p_{X|Y}(x|y) = \mathbb{E}(X|Y=y) = \int_{R_X} x \cdot f_{X|Y}(x|y) \ dx = \int_{R_X} x \cdot \frac{f_{X,Y}(x, y)}{f_Y(y)} \ dx$

**$\star$Momento di ordine r**
$\mathbb{E}(X^r) = \sum\limits_{x} x^r \cdot p_X(x) = \int_{-\infty}^{\infty}\ x^r\cdot f_X(x) \ dx$

**$\star$Varianza**
$\mathbb Var(X) = E(X^2) - E(X)^2$
$\mathbb Var(X \ | \ Y=y) = \sum_x (x - \mathbb E(X \ | \ Y=y))^2 \cdot p_{X|Y}(x|y) = \mathbb{E}(X^2 | Y = y) - \left( \mathbb{E}(X | Y = y) \right)^ 2$
$\mathbb{V}ar(a X+b Y+c)=a^{2} \mathbb{Var}(X)+b^{2} \mathbb{Var}(Y)+2 a b \mathbb{\mathbb{Cov}}(X, Y)$

**$\star$Covarianza**
$\mathbb Cov(X,Y) = \mathbb E(X \cdot Y) - \mathbb E(X) \cdot \mathbb E(Y)$

**$\star$Indice di Pearson**
$\rho(X,Y) = \frac {\mathbb Cov(X,Y)}{\sqrt {\mathbb Var(X) \cdot \mathbb Var(Y)}}$

**$\star$Funzione di probabilità discreta**
$p(x) \geq 0$ per ogni $x$ reale e $\sum_{x \in R_X} \ p(x) = 1$
$\newline$
$\newline$
$\newline$

**$\star$Funzione di probabilità continua**
- $Pr(X \in (a,b]) = Pr(a < X \leq b) = \int_{a}^{b} f(x) \ dx$

- $f(x) > 0$ per ogni $x \in \mathbb R$

- $\int_{-\infty}^{\infty} f(x) \ dx = 1$

- $Pr(X=x) = 0$

- $F(x) = \int_{-\infty}^{x} f(t) \ dt = \begin{cases} 0 & \text{ for } t < 0 \\ \int_0^t f(s) ds & 0 \leq t < n \\ 1 & \text{ for } t \geq n \end{cases}$

**$\star$Variabili aleatorie doppie continue**
- $f_{X,Y}(x,y) \geq 0$

- $\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f_{X, Y}(x, y) \ dx \ dy = 1$

- $P(X \in [a,b] \cap X \in [a,b]) = \int_{a}^{b} \int_{c}^{d} f_{X, Y}(x, y) \ dx \ dy$

- $f_X(x) =\int_{R_Y} f_{X,Y}(x,y) dy$

- $f_Y(y) =\int_{R_X} f_{X,Y}(x,y) dx$

**$\star$Variabili aleatorie doppie discrete**
-   $f_{X,Y}(x,y) \geq 0$

-   $\sum_x \sum_y f_{X, Y}(x, y) = 1$

-   $f_{X,Y}(x,y) = P(X=x \cap Y=y)$

-   $f_X(x) = P(X=x) = \sum_y f_{X, Y}(x, y)$

-   $f_Y(y) = P(Y=y) = \sum_x f_{X, Y}(x, y)$

**$\star \star$Distribuzioni condizionali e indipendenza**
$\star$**Variabili discrete**
Due v.a. discrete X e Y sono stocasticamente indipendenti se (e solo se)
$$p_{X,Y}(x,y) = p_X(x) \cdot p_Y(y)$$
questo implica che
$$p_{X,Y}(x,y) = p_{X|Y}(X=x\ | \ Y=y) \cdot p_Y(y) = p_{Y|X}(Y=y\ | \ X=x) \cdot p_X(x) $$
$\star$**Variabili continue**
Due v.a. continue X e Y sono stocasticamente indipendenti se (e solo se)
$$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y)$$
In caso di indipendenza di variabili continue vale:
$$f_{X|Y}(x|y) = f_X(x)$$
Inoltre
$$f_{X|Y} = \frac {f_{X,Y}(x,y)}{f_Y(y)}$$

$\newline$
$\newline$
**$\star$Teorema Limite Centrale**

$\large \frac {X ± 0.5 - \mathbb E[X]}{\sqrt{(\mathbb Var(x))}} \quad$ $+$ con $P(X \leq k), \ -$ con $P(X \geq k)$


**$\star$Disuguaglianza di Markov**

$Pr(Y \geq a) \leq \frac {\mathbb E(Y)}{a}$

**$\star$Disuguaglianza di Chebychev**

$Pr(|Y − \mu| \geq \epsilon) \leq \frac {\sigma^2} {\epsilon^2}$

**$\star$Binomiale**

$$Pr(X=k) = {n \choose k} \ p^k (1-p)^{n - k} \quad k = 0, 1,...,n$$
$$
\mathbb{E}(X) = n\cdot p
\quad \quad
Var(X) = n\cdot p(1-p)
$$
In R:
```js
dbinom(k, n, p) #P(X=x)
pbinom(k, n, p) #P(X≤x)
pbinom(k, n, p, FALSE) #P(X>x)
```

**$\star$Poisson**
$$Pr(X=x) = {\frac {\lambda ^{x}}{x!}}e^{-\lambda } \quad x = 0, 1,... \quad \quad \mathbb{E}(X) = Var(X) = \lambda$$
In R:
```js
dpois(x, lambda) #P(X=x)
ppois(x, lambda) #P(X≤x)
ppois(x, lambda, FALSE) #P(X>x)
```
Se $Y_1$ e $Y_2$  sono due variabili aleatorie indipendenti con distribuzioni di Poisson di parametri $\lambda_1$ e $\lambda_2$ rispettivamente, allora
- la loro somma $Y=Y_{1}+Y_{2}$ segue ancora una distribuzione di Poisson, di parametro $\lambda =\lambda _{1}+\lambda _{2}$;
- la distribuzione di $Y_{1}$ condizionata da $Y=n$ è la distribuzione binomiale di parametri $n$ e $\frac {\lambda_1}{\lambda}$.

**$\star$Geometrica**
$$Pr(X=x) = p(1-p)^{k-1} \quad x = 1, 2,... \quad \quad \mathbb{E}(X) = \frac 1 p \quad \quad Var(X) = \frac{1-p}{p^2}$$
In R:
```js
dgeom(x, prob) #P(X=x)
pgeom(x, prob) #P(X≤x)
pgeom(x, prob, FALSE) #P(X>x)
```

**$\star$Normale**
$$N(µ, \sigma^2 ) = {\frac {1}{\sqrt {2\pi \sigma ^{2}}}}\exp \left\{-{\frac {1}{2}}\left({\frac {x-\mu }{\sigma }}\right)^{2}\right\}$$
$$\mathbb{E}(X) = µ \quad Var(X) = {\sigma^2}$$
In R:
```js
dnorm(x, mean = 0, sd = 1) #f(X=x), mean = µ, sd = σ
pnorm(x, mean = 0, sd = 1) #integral from 0 to x
qnorm(p, mean = 0, sd = 1) #integral from 0 to k = p
```

**$\star$Esponenziale**
$$Exp(\lambda) = \lambda e^{-\lambda x} \quad x>0 $$
$$F(x) = 1- e^{-\lambda x} \quad x\geq 0$$
$$\mathbb{E}(X) = \frac 1 \lambda \quad Var(X) = \frac 1 {\lambda^2}$$
In R:
```js
dexp(x, lambda) #f(X=x), mean = µ, sd = σ
pexp(x, lambda) #integral from 0 to x
qexp(p, lambda) #integral from 0 to k = p
```

**$\star$Integrali in R**
```js
integrand <- function(x) { exp(-x) }
integrate(integrand, 0, +Inf)$value
```
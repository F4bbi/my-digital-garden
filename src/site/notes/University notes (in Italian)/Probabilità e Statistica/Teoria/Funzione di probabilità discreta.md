---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/funzione-di-probabilita-discreta/"}
---

# Funzione di probabilità discreta
Se X è una v.a. discreta con $R_X = \{x_1, x_2, ...\}$ allora $p(x) \geq 0$ per ogni $x$ reale e $\sum_{x \in R_X} \ p(x) = 1$

### Come si passa da una funzione di distribuzione/ripartizione discreta ad una funzione di densità discreta (o viceversa)?
#### Esempio
Sia X una variabile aleatoria discreta con la seguente funzione di distribuzione (o ripartizione)

$
F(x) = \begin{cases}
0 & x < -0.425, \newline\\
0.0531 &  -0.425  \leq  x < 0.474, \newline\\
0.2541 &  0.474  \leq  x < 1.372, \newline\\
0.5713 &  1.372  \leq  x < 2.271, \newline\\
0.8383 &  2.271  \leq  x < 3.17, \newline\\
0.9647 &  3.17  \leq  x < 4.069, \newline\\
0.9966 &  4.069  \leq  x < 4.967, \newline\\
1 &  4.967  \leq  x.
\end{cases}
$

Qual è la funzione di densità discreta $p_X$ della nostra v.a. $X$? 
Ricordiamo che la funzione di ripartizione/distribuzione di una v.a. discreta è costante a tratti con salti negli $x \in R_X$ e che tali salti hanno ampiezza $P(X=x)$. Quindi
$$p_X(x)=P(X=x)=P(X \leq x)-P(X < x)$$
Allora si ha:

$
p_X(x)=
\begin{cases}
0.0531 &  \text{per } x = -0.425, \newline\
0.201 &  \text{per } x = 0.474, \newline\
0.3172 &  \text{per } x = 1.372, \newline\
0.267 &  \text{per } x = 2.271, \newline\
0.1264 &  \text{per } x = 3.17, \newline\
0.0319 &  \text{per } x = 4.069, \newline\
0.0034 &  \text{per } x = 4.967
\end{cases}
$

Infatti:
$$F(x) = P(X \leq x) = P((-\infty, x])) = \sum_{x_i \leq x} p(x_i)$$

#### Esempio

Sia dato lo spazio probabilizzato $(\Omega, \mathcal{A}, P)$ dove, $\Omega=\mathbb{R}, \mathcal{A} = \mathbb{B}(\mathbb{R})$ e P e definita (non nulla) nei seguenti singoletti di $\mathbb{R}$

| x    | 1.00 | 3.00 | 5.00 | 7.00 | 8.00 | 9.00 | 11.00 | 13.00 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----- | ----- |
| p(x) | 0.09 | 0.02 | 0.17 | 0.11 | 0.12 | 0.11 | 0.19  | 0.19 |

La probabilità data è discreta e abbiamo che $P(\{x\}) = p(x)$ è la funzione di densità (discreta) di una variabile aleatoria $X$.

La sua funzione di distribuzione/ripartizione: è la seguente:

$
F(x) = \begin{cases}
0 & x < 1, \newline\\
0.09 &  1  \leq  x < 3, \newline\\
0.11 &  3  \leq  x < 5, \newline\\
0.28 &  5  \leq  x < 7, \newline\\
0.39 &  7  \leq  x < 8, \newline\\
0.51 &  8  \leq  x < 9, \newline\\
0.62 &  9  \leq  x < 11, \newline\\
0.81 &  11  \leq  x < 13, \newline\\
1 &  13  \leq  x.
\end{cases}
$

## Valore atteso
$\mathbb{E}(X) = \sum\limits_{x = 0}^n x \cdot p_X(x)$

## Varianza
$Var(X) = E(X^2) - E(X)^2$
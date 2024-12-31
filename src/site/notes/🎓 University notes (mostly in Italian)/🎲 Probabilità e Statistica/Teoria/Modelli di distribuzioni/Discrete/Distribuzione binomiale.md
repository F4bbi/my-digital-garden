---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/probabilita-e-statistica/teoria/modelli-di-distribuzioni/discrete/distribuzione-binomiale/","created":"2023-01-23T10:43:16.455+01:00","updated":"2023-01-23T10:43:16.455+01:00"}
---

# Distribuzione Binomiale
Questa distribuzione di probabilità regola il numero totale di successi ottenuti in $n$ prove indipendenti con probabilità di successo di ogni prova pari a $p$.

## Funzione di densità 
Si dice che una v.a. $X$ si distribuisce secondo la distribuzione di probabilità (o legge) binomiale di parametri $n \geq 1$ (intero) e $0 \leq p \leq 1$, se
$$
Pr(X=x) =
\begin{cases}
{n \choose k} \ p^k (1-p)^{n - k} \quad &x = 0, 1,...,n \\ \\
0 &altrimenti
\end{cases}
$$
e scriveremo $X \thicksim Bi(n, p)$.

## Valore atteso
$\mathbb{E}(X) = \sum\limits_{x = 0}^n x \cdot p_X(x)=n\cdot p$

## Varianza
$Var(X) = E(X^2) - E(X)^2 = n\cdot p(1-p)$
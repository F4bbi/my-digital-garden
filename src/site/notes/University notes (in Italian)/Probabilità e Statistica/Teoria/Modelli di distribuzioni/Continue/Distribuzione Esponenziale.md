---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/modelli-di-distribuzioni/continue/distribuzione-esponenziale/"}
---

# Distribuzione Esponenziale
## Funzione di densità 
Si dice che una v.a. $X$ ha legge Esponenziale con parametro $\lambda > 0$ se possiede la seguente densità

$$f(x,\lambda) = \begin{cases} \lambda e^{-\lambda x} \quad &x>0 \\ \\ 0 &altrimenti \end{cases}$$

e la indichiamo con $X ∼ Exp(\lambda)$

## Funzione di ripartizione
$$F(x) =
\begin{cases}
1- e^{-\lambda x} \quad &x\geq 0 \\ \\
0 &x<0
\end{cases}$$
## Valore atteso
$\mathbb{E}(X) = \Large{\frac 1 \lambda}$

## Varianza
$Var(X) = \Large{\frac 1 {\lambda^2}}$
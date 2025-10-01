---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/probabilita-statistica-and-knowledge-discovery/teoria/modelli-di-distribuzioni/discrete/distribuzione-geometrica/","created":"2023-01-23T10:44:03.552+01:00","updated":"2023-01-23T10:44:03.552+01:00"}
---

# Distribuzione Geometrica
È la probabilità che il primo successo richieda l'esecuzione di $k$ prove indipendenti, ognuna con probabilità di successo $p$.

## Funzione di densità 
Se la probabilità di successo in ogni prova è $p$, allora la probabilità che alla $k$-esima prova si ottenga il primo successo è:
$$
Pr(X=x) =
\begin{cases}
p(1-p)^{k-1} \quad &x = 1, 2,...\\ \\
0 &altrimenti
\end{cases}
$$
e scriveremo $X \thicksim Ge(p)$.

## Valore atteso
$\mathbb{E}(X) = \Large{\frac 1 p}$

## Varianza
$Var(X) = \Large{\frac{1-p}{p^2}}$
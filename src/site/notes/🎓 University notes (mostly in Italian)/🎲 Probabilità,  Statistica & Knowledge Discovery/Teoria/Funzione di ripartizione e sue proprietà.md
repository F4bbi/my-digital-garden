---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/probabilita-statistica-and-knowledge-discovery/teoria/funzione-di-ripartizione-e-sue-proprieta/","created":"2023-01-23T01:48:19.056+01:00","updated":"2023-01-23T01:48:19.056+01:00"}
---

# Funzione di ripartizione e sue proprietà
Data una variabile casuale $X$, la funzione che fa corrispondere ai valori di $x$, le probabilità cumulate $P(X \leq x)$ viene detta funzione di ripartizione è indicata con $F_X$

## Requisiti di una funzione di ripartizione:
- **Non** decrescente
- $0 \leq F_X(x) \leq 1$
- $lim_{x \to +\infty} \ F_X(x_{0})= 1$
- $lim_{x \to -\infty} \ F_X(x_{0})= 0$

Nel caso di una variabile casuale *discreta* si aggiungono le proprietà:
- continua da destra $\forall x \in \mathbb R$, quindi $lim_{x \to x_{0+}}F_X(x)= F_X(x_0)$
{ #1c2ff9}

- $lim_{x \to x_{0-}}F_X(x) \neq F_X(x_0)$ (la funzione di ripartizione presenta dei punti di discontinuità di 1° specie (o salti))

Inoltre:
$\text{Pr}((a,b]) = F(b^+) - F(a^+)$
$\text{Pr}([a,b)) = F(b^-) - F(a^-)$
$\text{Pr}((a,b)) = F(b^-) - F(a^+)$
$\text{Pr}([a,b]) = F(b^+) - F(a^-)$
$\text{Pr}(X = x) = \text{Pr}((x,x])= lim_{x \to x_{0+}}F_X(x) - lim_{x \to x_{0-}}F_X(x)$

#### Esempio
Data la seguente funzione di ripartizione:

$
\begin{cases}
 1 - \text{exp}(- \frac{x^{2}}{2 \sigma^2}) &\text{ se } x \geq 0 \\ \\
 0 &\text{ altrimenti}  
\end{cases}
$

$\star$ **Qual è la probabilità dell'intervallo $[−1.12,1.12)$?**
$\text{Pr} ((-1.12,1.12]) = F(1.12^-) - F(-1.12^-) = 0.3398201 - 0 = 0.3398201$

$\star$ **Qual è la probabilità dell'intervallo $(0.215,0.712]$?**
$\text{Pr} ((0.215,0.712]) = F(0.712^+) - F(0.215^+) = 0.1393029289592$

$\star$ **Qual è infine la probabilità di $(0.215, 1.58] \cap ( 0.712 , 1.727 ] \cup (1.822, 2.274]$?**
L'intervallo è quindi $(0.712,1.58] \cup (1.822,2.274]$, di conseguenza la probabilità è:
$\text{Pr} ((0.712,1.58] \cup (1.822,2.274]) = F(1.58^+) - F(0.712^+) + F(2.274^+) - F(1.822^+) = 0.5605703$

**Fonte utile:**
[Spiegazione funzione di ripartizione](http://progettomatematica.dm.unibo.it/Prob2/7funzionediripartizione.html#:~:text=La%20probabilit%C3%A0%20che%20la%20variabile%20casuale%20X%20assuma%20un%20valore,intervallo%20%5Ba%2Cx%5D)
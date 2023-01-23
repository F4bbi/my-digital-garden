---
{"dg-publish":true,"permalink":"/university-in-italian/fondamenti-matematici-per-l-informatica/esercizi-sul-principio-di-induzione/appello-10-marzo-2014/"}
---

# Esercizio (Appello 10 Marzo 2014)
Si dimostri per induzione su $n \in \mathbb N$ che $\forall n \geq 2$ la seguente disuguaglianza:
$$\sum_{k=1}^{n} 6k^2 = n(n+1)(2n+1)$$
## Soluzione
Procediamo per induzione su $n \geq 2$
$n = 2$ (*BASE DELL'INDUZIONE*) 
Dobbiamo provare che
$$\sum_{k=1}^{n} 6k^2 = 2(2+1)(2 \cdot 2+1)$$
Vale:
-  $\large{\sum_{k=1}^{2} \normalsize {6k^2} = (6 \cdot 1^2) + (6 \cdot 2^2) = 6 + 24= 30}$

- $2(2+1)(2 \cdot 2+1) = 2 \cdot 3 \cdot 5 = 30$

La base dell'induzione è verificata in quanto
$$\sum_{k=1}^{n} 6k^2 = 30 = 2(2+1)(2 \cdot 2+1)$$

$n \geq 2, \ n \implies n+1$ (*PASSO INDUTTIVO*)
Assumiamo che l'uguaglianza sia vera per un certo $n \geq 2$, cioè che valga 
$$\sum_{k=1}^{n} 6k^2 = n(n+1)(2n+1) \ \ \text{per qualche } n \geq 2 \ \text{(IPOTESI INDUTTIVA)}$$
Dobbiamo provare che vale la stessa uguaglianza con $n+1$ al posto di $n$, ovvero
$$\sum_{k=1}^{n+1} 6k^2 = (n+1)((n+1)+1)(2(n+1)+1) \ \text{(TESI DEL PASSO INDUTTIVO)}$$
Vale:
$$\sum_{k=1}^{n+1} 6k^2 = (n+1)((n+1)+1)(2(n+1)+1)$$
$$\underline{(\sum_{k=1}^{n} 6k^2)}\ + 6(n+1)^2 = (n+1)((n+1)+1)(2(n+1)+1$$
$$\huge {\Updownarrow} \ \normalsize\text{(ipotesi induttiva)}$$
$$\underline{n(n+1)(2n+1)} \ + \ 6(n+1)^2 = (n+1)((n+1)+1)(2(n+1)+1)$$$$(n+1)[n(2n+1)+6(n+1)] = (n+1)(n+2)(2n+3)$$$$\cancel{(n+1)}(2n^2+n+6n+6) = \cancel{(n+1)}(2n^2+3n+4n+6)$$$$(2n^2+7n+6) = (2n^2+7n+6)$$Dunque il passo induttivo è verificato.
Grazie al passo induttivo $P(n)$ è vera $\forall n \geq 2$
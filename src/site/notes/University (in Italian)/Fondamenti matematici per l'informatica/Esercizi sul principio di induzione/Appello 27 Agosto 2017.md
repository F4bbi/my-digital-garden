---
{"dg-publish":true,"permalink":"/university-in-italian/fondamenti-matematici-per-l-informatica/esercizi-sul-principio-di-induzione/appello-27-agosto-2017/"}
---

# Esercizio (Appello 27 Agosto 2017)
Si dimostri per induzione su $n \in \mathbb N$ che $\forall n \geq 2$ la seguente disuguaglianza:
$$\sum_{k=2}^{n} \frac {1}{k(k+1)} = \frac {n-1}{2n+2} \ \ \forall n \geq 2$$
## Soluzione
Procediamo per induzione su $n \geq 2$
$n = 2$ (*BASE DELL'INDUZIONE*) 
Dobbiamo provare che
$$\sum_{k=2}^{n} \frac {1}{k(k+1)} = \frac {2-1}{2 \cdot 2+2}$$
Vale:
-  $\large\sum_{k=2}^{2} \frac {1}{k(k+1)} = \frac {1}{2(2+1)} = \frac 1 6$

- $\Large\frac {2-1}{2 \cdot 2+2} = \frac 1 6$

La base dell'induzione è verificata in quanto
$$\sum_{k=2}^{2} \frac {1}{k(k+1)} = \frac 1 6 = \frac {n-1}{2n+2}$$

$n \geq 2, \ n \implies n+1$ (*PASSO INDUTTIVO*)
Assumiamo che l'uguaglianza sia vera per un certo $n \geq 2$, cioè che valga 
$$\sum_{k=2}^{n} \frac {1}{k(k+1)} = \frac {n-1}{2n+2} \ \ \text{per qualche } n \geq 2 \ \text{(IPOTESI INDUTTIVA)}$$
Dobbiamo provare che vale la stessa uguaglianza con $n+1$ al posto di $n$, ovvero
$$\sum_{k=2}^{n+1} \frac {1}{k(k+1)} = \frac {(n+1)-1}{2(n+1)+2} \ \text{(TESI DEL PASSO INDUTTIVO)}$$

Vale:$$\sum_{k=2}^{n+1} \frac {1}{k(k+1)} = \frac {(n+1)-1}{2(n+1)+2} \huge\Longleftrightarrow$$$$\underline{\sum_{k=2}^{n} \frac {1}{k(k+1)}} + \frac {1}{(n+1)((n+1)+1)} = \frac {n}{2n+4}$$$$\huge {\Updownarrow} \ \normalsize\text{(ipotesi induttiva)}$$$$\underline{\frac {n-1}{2n+2}} \ + \ \frac {1}{(n+1)(n+2)} = \frac {n}{2n+4}$$$$\frac{(n+2)(n-1)+2}{2(n+1)(n+2)} = \frac {n}{2n+4}$$$$\frac{n^2-n+2n-2+2}{\cancel2(n+1)\cancel{(n+2)}} = \frac {n}{\cancel{2(n+2)}}$$$$\huge {\Updownarrow} \ \ \normalsize\text{Moltiplico a destra e a sinistra per } (n+1)$$$$n^2-n+2n\cancel{-2}\cancel{+2} = n(n+1)$$$$n^2+n = n^2+n$$Dunque il passo induttivo è verificato.
Grazie al passo induttivo $P(n)$ è vera $\forall n \geq 2$
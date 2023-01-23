---
{"dg-publish":true,"permalink":"/university-notes-in-italian/matematica-discreta/esercizi-sul-principio-di-induzione/appello-11-febbraio-2016/"}
---

# Esercizio (Appello 11 Febbraio 2016)
Si dimostri per induzione su $n$ la seguente disuguaglianza:
$$\sum_{k=1}^{n} \frac{k}{2^k} = 2 - \frac{n+2}{2^n} \ \ \ \forall n \geq 1$$
## Soluzione in forma compatta
Procediamo per induzione su $n \geq 1$
$n = 1$ (*BASE DELL'INDUZIONE*) 
Dobbiamo provare che 
$$\sum_{k=1}^{n} \frac{k}{2^k} = 2 - \frac{1+2}{2^1}$$Vale:
-  $\Large{\sum_{k=1}^{1} \frac{1}{2^1} = \frac{1}{2}}$

- $\large{2 - \frac{1+2}{2^1} = 2 - \frac 3 2 = \frac {4-3}2 = \frac 1 2}$

La base dell'induzione è verificata, in quanto $\large{\sum_{k=1}^{1} \frac{k}{2^k} = \frac 1 2 = 2 - \frac{1+2}{2}}$

$n \geq 1, \ n \implies n+1$ (*PASSO INDUTTIVO*)
Assumiamo che l'uguaglianza sia vera per un certo $n \geq 1$, cioè che valga 
$$\sum_{k=1}^{n} \frac{k}{2^k} = 2 - \frac{n+2}{2^n} \ \text{per qualche } n \geq 1 \ \text{(IPOTESI INDUTTIVA)}$$
 Dobbiamo provare che vale la stessa uguaglianza con $n+1$ al posto di $n$, ovvero
$$\sum_{k=1}^{n+1} \frac{k}{2^k} = 2 - \frac{(n+1)+2}{2^{n+1}} \ \text{(TESI DEL PASSO INDUTTIVO)}$$
##### Metodo 1
Vale:
$$\sum_{k=1}^{n+1} \frac{k}{2^k} = ({\sum_{k=1}^{n} \frac{k}{2^k}}) + \frac{n+1}{2^{n+1}} = \underline {(2 - \frac{n+2}{2^n}) + \frac{n+1}{2^{n+1}}} =$$
$(...) :$ Ipotesi induttiva
$$= {2 - (\frac{n+2}{2^n} - \frac{n+1}{2^{n} \times 2^1})} = \text{(raccolto il meno)} $$
$$= 2 - \frac{2(n+2) \ - \ (n+1) }{2^n \times 2} = \text{(denominatore comune)}$$
$$= 2 - \frac{2n + 4 - n - 1}{2^{n+1}} =$$
$$= 2 - \frac{n+3}{2^{n+1}} =$$
$$= 2 - \frac{(n+1)+2}{2^{n+1}} =$$
Dunque il passo induttivo è verificato.
Grazie al passo induttivo $P(n)$ è vera $\forall n >= 1$
##### Metodo 2
Vale:
$$\sum_{k=1}^{n+1} \frac{k}{2^k} = 2 - \frac{(n+1)+2}{2^{n+1}}$$
$$({\sum_{k=1}^{n} \frac{k}{2^k}}) + \frac{n+1}{2^{n+1}} = 2 - \frac{(n+1)+2}{2^{n+1}} $$
$$\huge {\Updownarrow} \ \normalsize\text{(ipotesi induttiva)}$$
$$(\cancel 2 - \frac{n+2}{2^n}) + \frac{n+1}{2^{n+1}} = \cancel2 - \frac{(n+1)+2}{2^{n+1}} $$
$$- \frac{n+2}{2^n} + \frac{n+1}{2^{n+1}} = - \ \frac{(n+1)+2}{2^{n+1}}$$
$$\huge {\Updownarrow} \ \ \normalsize\text{Moltiplico a destra e a sinistra per } -1$$
$$\frac{n+2}{2^n} - \frac{n+1}{2^{n+1}} =\frac{(n+1)+2}{2^{n+1}}$$
$$\huge {\Updownarrow} \ \ \normalsize\text{Moltiplico a destra e a sinistra per } 2^{n+1}$$
$$2(n+2) - n - 1 = n+3$$
$$n+3=n+3$$Dunque il passo induttivo è verificato.
Grazie al passo induttivo $P(n)$ è vera $\forall n \geq 1$
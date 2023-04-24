---
{"dg-publish":true,"permalink":"/university-notes-in-italian/matematica-discreta/teoria/principio-di-induzione/","created":"2022-03-18T16:36:41.187+01:00","updated":"2023-01-23T16:28:57.188+01:00"}
---

# Principio di induzione
Sia $\{P(n)\}_{n \in \mathbb N}$ una famiglia di proposizioni, ovvero affermazioni, indicizzata su $\mathbb N$. Supponiamo
1. $P(0)$ è vera. (BASE DELL'INDUZIONE)
2. $\forall n \in \mathbb N, \ \clubsuit P(n) \implies \blacklozenge P(succ(n))$ è vera. 
$\clubsuit :$*IPOTESI INDUTTIVA*
$\blacklozenge :$*PASSO INDUTTIVO*
Allora $P(n)$ è vera $\forall n \in \mathbb N$.
## Dimostrazione
$$A:=\{n \in \mathbb N \ | \ P(n) \ è \ vera\} \ \subset \mathbb N$$
Osserviamo:
1) $\Leftrightarrow \ 0 \in A$
Sia $n \in A$. Vale: $n \in A \Leftrightarrow P(n)$ è vera $\implies P(succ(n))$ è vera $\Leftrightarrow succ(n) \in A$
Grazie all'assioma 2.8 (di induzione), $A = \mathbb N \Leftrightarrow P(n)$ è vera $\forall n \in \mathbb N$

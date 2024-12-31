---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/matematica-discreta/teoria/numeri-naturali/","created":"2023-04-24T18:34:10.862+02:00","updated":"2023-04-24T18:34:10.862+02:00"}
---

# Numeri naturali
## Assiomi di Peano
1. 0 $\in \mathbb N$ detto zero.
2. $∃ \ succ : \mathbb N → \mathbb N$ tale che sia iniettiva.
3. $succ(n) \subset \mathbb N \backslash \{0\})$ ovvero $\forall n \in \mathbb N, succ(n) \neq 0$

## Assioma di induzione
Sia $A$ un sottoinsieme di $\mathbb N$. Supponiamo che:
1. 0 $\in A$ (base dell'induzione)
2. $\forall n \in \mathbb N:(n \in A \implies succ(n) \in A)$ ovvero se n $\in A$ allora anche $succ(n) \in A$ ($\underline{passo \ induttivo}$).
	Allora $A = \mathbb N$
## Proposizione 2.9
$\forall m \in \mathbb N \backslash \{0\}$, esiste unico $n \in \mathbb N$ t.c. $succ(n) = m$.
### Dimostrazione
Se n esiste allora è unico, infatti se $n' \in \mathbb N$  avesse la stessa proprietà allora:
$succ(n) = m = succ(n')$
Grazie all'iniettività assunta dall'assioma (2.), $n = n'$
Supponiamo per assurdo che non sia vero, ossia che esista un $m \neq 0$ che non abbia predecessori, ovvero 
$succ(n) \neq m, \ \forall n \in \mathbb N$
Definiamo quindi $A:=\mathbb N \ \backslash \ \{m\}$.
Poichè $m \neq 0$ e $A = \mathbb N \ \backslash \{m\}$, $0 \in A$.
Sia $n \in A$. Osserviamo che $succ(n) \neq m \implies succ(n) \in A$. Grazie quindi all'assioma di induzione applicato ad $A$, si ha che $A = \mathbb N$. Ma questo è assurdo perchè $A = \mathbb N \ \backslash \{m\}$.
## Corollario 2.9
$\mathbb N$ e $\mathbb N \ \backslash \ \{0\}$ sono equipotenti, ovvero esiste una bigezione fa $\mathbb N$ in $\mathbb N \ \backslash \{0\}$.
### Dimostrazione
Definiamo 
$$f:\mathbb N \rightarrow \mathbb N \ \backslash \ \{0\}$$$$n \rightarrow succ(n)$$
Osserviamo che
- $f(\mathbb N) = succ(\mathbb N) \subset \mathbb N \ \backslash \ \{0\}$
- Prop. 2.9 $\implies f(\mathbb N) = \mathbb N \ \setminus \ \{0\}$
										$f$ è surgettiva
- $f$ è iniettiva grazie all'assioma 2.6
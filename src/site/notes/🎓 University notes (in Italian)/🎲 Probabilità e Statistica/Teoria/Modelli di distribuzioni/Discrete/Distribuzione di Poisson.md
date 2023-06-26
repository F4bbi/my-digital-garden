---
{"dg-publish":true,"permalink":"/university-notes-in-italian/probabilita-e-statistica/teoria/modelli-di-distribuzioni/discrete/distribuzione-di-poisson/","created":"2022-05-21T14:57:31.748+02:00","updated":"2023-01-23T10:43:41.681+01:00"}
---

# Distribuzione di Poisson
In teoria delle probabilità la distribuzione di Poisson è una distribuzione di probabilità discreta che esprime le probabilità per il numero di eventi che si verificano successivamente ed indipendentemente in un dato intervallo di tempo, sapendo che mediamente se ne verifica un numero $\lambda$. Ad esempio, si utilizza una distribuzione di Poisson per misurare il numero di chiamate ricevute in un call-center in un determinato arco temporale, come una mattinata lavorativa.

## Funzione di densità 
La distribuzione di Poisson è una distribuzione di probabilità discreta data da
$$
\mathcal {P}_{\lambda (n)} =
\begin{cases}
{\frac {\lambda ^{n}}{n!}}e^{-\lambda } \quad &n = 0, 1,... \\ \\
0 &altrimenti
\end{cases}
$$

## Valore atteso
$\Large{\lambda}$

## Varianza
$\Large{\lambda}$

### Proprietà particolare
Se $Y_1$ e $Y_2$  sono due variabili aleatorie indipendenti con distribuzioni di Poisson di parametri $\lambda_1$ e $\lambda_2$ rispettivamente, allora
- la loro somma $Y=Y_{1}+Y_{2}$ segue ancora una distribuzione di Poisson, di parametro $\lambda =\lambda _{1}+\lambda _{2}$;
- la distribuzione di $Y_{1}$ condizionata da $Y=n$ è la distribuzione binomiale di parametri $n$ e $\frac {\lambda_1}{\lambda}$.
---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/probabilita-statistica-and-knowledge-discovery/teoria/disuguaglianza-di-markov-e-di-chebychev/","created":"2023-01-23T01:45:29.128+01:00","updated":"2023-01-23T01:45:29.128+01:00"}
---

# Disuguaglianza di Markov
Sia $Y$ una v.c. che assume valori non negativi, allora per ogni numero reale $a > 0$
$$Pr(Y \geq a) \leq \frac {\mathbb E(Y)}{a}$$

# Disuguaglianza di Chebychev
Sia $Y$ una v.c. con valore atteso $\mathbb E(Y) = \mu$ e varianza $\mathbb Var(Y) = \sigma^2$.
Allora
$$Pr(|Y − \mu| \geq \epsilon) \leq \frac {\sigma^2} {\epsilon^2}$$

L’importanza delle due disuguaglianze di Markov e Chebychev è che ci danno dei limiti superiori per delle probabilità senza alcun riferimento alla distribuzione di probabilità della v.a. $Y$. 
Basta conoscerne il valore atteso o il valore atteso e la varianza. 
Ad esempio, si supponga che il numero di clienti in un giorno ad uno sportello sia una v.a. $Y$ con $E(Y) = 130$. Cosa possiamo dire della probabilità che possano arrivare più di $250$ clienti? Grazie alla disuguaglianza di Markov possiamo dire che $Pr(Y \geq 250) \leq \frac {130} {250} = 0.52$. Ovviamente questi limiti possono essere molto grossolani. Se $Y ∼ \mathcal N (0, 1)$ sappiamo che $Pr(|Y | \geq 1.96) = 0.05$, mentre la disuguaglianza di Chebychev afferma che $Pr(|Y| \geq 1.96) \leq \frac 1 {1.96^2} \approx 0.260$.
---
{"dg-publish":true,"permalink":"/university-in-italian/probabilita-e-statistica/teoria/approssimare-la-binomiale-con-la-normale/"}
---

# Approssimare la binomiale con la normale
All’aumentare del numero $n$ di prove, la distribuzione di una variabile casuale binomiale di parametri $n$ e $p$ si “avvicina” sempre di più a quella di una normale con parametri $\mu = np$ e $\sigma^2 = np(1 − p)$.

L’approssimazione è valida quando $n$ è abbastanza grande e $p$ e $1 − p$ non sono vicini a zero.
Regola pratica:
1. si calcola l’intervallo di estremi $np ± 3 \sqrt{np(1 − p)}$
2. se esso è contenuto nell’intervallo $[0, n]$ allora l’approssimazione può ritenersi valida.

Per calcolare il valore standardizzato corrispondente si utilizza la seguente formula:
$$\frac {X ± 0.5 - \mathbb E[X]}{\sqrt{(\mathbb Var(x))}}$$

E successivamente si calcola la funzione di ripartizione della distribuzione normale in quel punto, con $\mu = 0$ e $\sigma = 1$ (chiamata anche Normale Standard).

### Esempio
Vogliamo calcolare $Pr(B \leq 25)$ dove $B ∼ B(30, 0.7)$. L’intervallo $[13.47, 28.53]$ è contenuto nell’intervallo $[0, 30]$ quindi procediamo con l’approssimazione. Calcoliamo il valore standardizzato corretto corrispondente a 25:
$$z_0 = \frac {(25 + 0.5) - 30 \cdot 0.7} {\sqrt{30 \cdot 0.7(1-0.7)}} \approx 1.79$$
A questo punto:
$$Pr(B \leq 25) \approx Pr(Z \leq 1.79) = 0.9633$$
In R è calcolabile nel seguente modo
```js
pnorm(1.79, 0, 1)
```
$Pr(B \leq 25)$ invece risulta $0.9699$, quindi è una buona approssimazione.
Si osservi che nel calcolo di $z_0$ abbiamo aggiunto $0.5$ al valore dato $25$. Si tratta della cosiddetta **correzione per continuità** che migliora l’approssimazione di una v.a. discreta con una continua.

### Correzione per continuità
$$P(X \leq k) = \phi\left(\frac{(k+0.5) - np}{\sqrt{np(1-p)}}\right)$$
$$P(X \geq k) = 1 - \phi\left(\frac{(k-0.5) - np}{\sqrt{np(1-p)}}\right)$$
---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/linguaggi-formali-e-compilatori/riassunto/","created":"2025-12-30T10:00:45.506+01:00","updated":"2026-06-05T18:02:12.870+02:00"}
---

# Riassunto

## Grammatica / Linguaggi liberi (context-free)

- Una grammatica è libera se ogni produzione è del tipo:
    
    $$A \to \alpha$$
    
    dove $A$ è un singolo non-terminale e $\alpha$ è una stringa composta da terminali e/o non-terminali.
    
- Un linguaggio $L$ è libero se e solo se esiste una grammatica $G$ libera tale che $L = \mathcal{L}(G)$.

---

## Pumping lemma per linguaggi liberi

Il negato di questo lemma ci permette di dimostrare che non esiste una determinata grammatica libera che genera un dato linguaggio.

### Enunciato

Sia $L$ un linguaggio libero. Allora:

$$\exists p \in \mathbb{N}^+ \text{ t.c. } \forall z \in L, |z| \ge p, \exists u, v, w, x, y \text{ t.c. }$$

1. $z = uvwxy$
    
2. $|vwx| \le p$
    
3. $|vx| > 0$
    
4. $\forall i \in \mathbb{N} \text{ (anche nullo), } uv^i w x^i y \in L$

---

## Proprietà

I linguaggi liberi sono chiusi per **UNIONE**, **CONCATENAZIONE** ma **NON** per intersezione.

---

## Espressioni regolari

Valgono:

| **Operazione**     | **Sintassi** | **Linguaggio**                                  |
| ------------------ | ------------ | ----------------------------------------------- |
| **ALTERNANZA**     | $r \mid s$   | $L(r) \cup L(s)$                                |
| **CONCATENAZIONE** | $r \cdot s$  | $L(r) L(s)$                                     |
| **MOLTEPLICITÀ**   | $r^*$        | $\{\varepsilon\} \cup \{w_1 \dots w_k \dots \}$ |
| **PRECEDENZA**     | $(r)$        |                                                 |

### Precedenze

$$* \quad > \quad \cdot \quad > \quad \mid$$

Esempio:

$a \mid b^* c \longrightarrow (a \mid ((b^*) \cdot c))$

### Conversioni

Da un'espressione regolare è possibile:

- **Ottenere un NFA** utilizzando la **costruzione di Thompson** $\Rightarrow$ simulare l'NFA.
    
    - **Ottenere un DFA** dato l'NFA utilizzando la **subset construction** $\Rightarrow$ simulare il DFA.
        
        - **Ottenere un DFA minimo** dato un DFA utilizzando il **partition refinement**.

---

## Simulazione di un NFA

- Si parte da $T_0$, che contiene lo **stato iniziale** più quelli a cui possiamo arrivare facendo una **$\varepsilon$-transizione**.
    
- A questo punto prendiamo il **primo carattere** della parola analizzata e facciamo una transizione di quel carattere.
    
- Facciamo pure una **$\varepsilon$-chiusura** e aggiungiamo gli stati raggiunti.
    
- Prendiamo ora il **secondo carattere** e ripartiamo con la procedura.
    
- Se $T_{last}$ (l'ultimo insieme di stati raggiunto) contiene uno **stato finale**, allora la parola $\in L$ (appartiene al linguaggio).

Costo: $O(|w| \cdot (n+m))$

---

## Trasformazione di un NFA in DFA

La procedura segue questi passaggi:

1. **Inizio**: Si parte da $T_0$, che contiene lo stato iniziale più tutti quelli raggiungibili tramite **$\varepsilon$-transizioni**.
    
2. **Costruzione Tabella**: Si crea una tabella dove:
    
    - Sulle **righe** si pongono i vari insiemi di stati $T_n$.
        
    - Sulle **colonne** si pongono tutti i **terminali** dell'alfabeto.
        
3. **Iterazione**:
    
    - Partendo da $T_0$, per ogni terminale nelle colonne si calcola la transizione, creando così un **nuovo stato** del DFA.
        
    - **Nota bene**: Bisogna ricordarsi di calcolare la **$\varepsilon$-chiusura** (o $\varepsilon$-transizione) per ognuno degli stati così ottenuti!

---

## Minimizzare un DFA

Bisogna intanto essere sicuri che il DFA sia **totale**, ovvero che ogni stato abbia una transizione per tutti i terminali. Se non è così, si aggiunge un **SINK** a cui si fanno arrivare tutte le transizioni mancanti.

A questo punto si inseriscono in un set tutti gli **stati non finali** ed in un altro set tutti gli **stati finali**. A questo punto si parte con le **n-equivalenze**.

Due stati appartengono allo stesso set se:

- La transizione di entrambi finisce nello stesso set.
    
- La transizione di entrambi finisce nello stesso stato.

Se queste condizioni non valgono, i due stati non appartengono allo stesso set.

---

## Linguaggi regolari

Un linguaggio $L$ si dice **regolare** se e solo se:

- $\iff$ esiste un'**espressione regolare** $r$ t.c. $L = \mathcal{L}(r)$
    
- $\iff$ esiste un **NFA** $N$ t.c. $L = \mathcal{L}(N)$
    
- $\iff$ esiste un **DFA** $D$ t.c. $L = \mathcal{L}(D)$
    
- $\iff$ esiste una **grammatica regolare** $G$ t.c. $L = \mathcal{L}(G)$
    

> [!NOTE] Gerarchia delle Grammatiche
> 
> Ricordiamo che ogni grammatica regolare è anche una grammatica libera (context-free).

### Proprietà di Chiusura

I linguaggi regolari sono chiusi rispetto alle seguenti operazioni:

- **UNIONE**
    
- **CONCATENAZIONE**
    
- **COMPLEMENTAZIONE**
    
- **INTERSEZIONE**

### Pumping lemma per linguaggi regolari

Sia $L$ un linguaggio regolare. Allora:

$$\exists p \in \mathbb{N}^+ \text{ t.c. } \forall z \in L : |z| \ge p$$

$$\exists u, v, w \text{ t.c. } z = uvw \quad \& \quad |uv| \le p \quad \& \quad |v| > 0$$

$$\& \quad \forall i \ge 0, \quad uv^i w \in L$$

---

## FIRST

In linea generica, il **FIRST** di una stringa (di terminali / non-terminali) è il primo carattere che compare in ogni produzione di quella stringa.

Dunque:

1. Per una produzione del tipo $X \to \varepsilon$:
    
    $$First(X) = \{\varepsilon\}$$
    
2. Per ogni produzione con inizio di un terminale ($X \to a \dots$):
    
    $$First(X) = \{a\}$$
    
3. **Per ogni produzione del tipo** $X \to Y_1 Y_2 \dots Y_n$ con $Y_i$ terminali o non-terminali:
    
    - Se $\varepsilon \notin First(Y_1) \implies First(X) = First(Y_1)$
        
    - Se $\varepsilon \in First(Y_1) \implies First(X) = \{First(Y_1) - \varepsilon\} \cup First(Y_2 \dots Y_n)$
        

Per calcolare $First(Y_2 Y_3 \dots Y_n)$ si rieseguono i passaggi del punto 3 ricorsivamente finché non si incontra un $First$ che non contenga $\varepsilon$.

Se $\varepsilon \in First(Y_n)$ (ovvero tutti i componenti della produzione possono annullarsi), allora:

$$First(X) = First(X) \cup \{\varepsilon\}$$

## FOLLOW

In linea generica il **FOLLOW** di una stringa (di non-terminali) è l'insieme dei terminali che seguono la stringa in qualche derivazione.

### Procedura di calcolo

1. **Inizio**: Come primo passo si aggiunge il simbolo **$** (fine input) ai follow dello **start symbol**.
    
2. **Iterazione**: Per ogni produzione del tipo $B \to A \beta$ (dove $A$ è un non-terminale e $\beta$ è la stringa che segue):
    
    - Se $\beta \neq \varepsilon$:
        
        $$Follow(A) \text{.add } (First(\beta) \setminus \{\varepsilon\})$$
        
    - Se $\beta = \varepsilon \lor \varepsilon \in First(\beta)$:
        
        $$Follow(A) \text{.add } (Follow(B))$$

Nello schema sopra, A rappresenta un non-terminale e $\beta$ è la stringa che lo segue all'interno di una produzione.

---

## Parsing Top-Down: LL(1) parser

Sta per **"Left-to-Right Leftmost"** parser. Prende in input una parola e la _parsing table_ e restituisce in output la derivazione _leftmost_ per ottenere la parola (se $\in L(G)$), altrimenti restituisce un errore.

### Procedimento

1. **Calcolare First e Follow** di ogni non-terminale.
    
2. **Costruire la tabella di parsing**.
    
3. **Riconoscimento della parola** usando uno **stack** e la _parsing table_.

Il punto 1 l'abbiamo già esplorato in precedenza.

## (2) Costruzione della tabella di parsing

Innanzitutto si crea una tabella con:

- I **NON terminali** sulle righe.
    
- I **terminali** sulle colonne.

Si prende poi ogni produzione della grammatica e si applica la seguente logica:

**Algoritmo di riempimento:**

foreach $(A \to \alpha)$:

  foreach (terminale $b \in First(\alpha)$):

    inserire $A \to \alpha$ nella entry $[A, b]$ della tabella

  if $\varepsilon \in First(\alpha)$:

    foreach (terminale $x \in Follow(A)$):

      inserire $A \to \alpha$ nella entry $[A, x]$

## (3) Grammatica LL(1)

Se la tabella di parsing non ha più di una produzione per una certa cella (non ha "**multiple defined entries**"), allora la grammatica è **LL(1)**.

### Riconoscimento della parola

In questa fase ci servono:

- La **parola $w$** (a cui attacchiamo alla fine il simbolo **$\$$**).
    
- Uno **stack** (in cui pushiamo subito **$\$S$**).

### Procedimento

L'algoritmo legge la parola e utilizza lo stack per verificare la correttezza della derivazione:

```cpp
stack s;
s.push($); 
s.push(S); // S è lo start symbol
x = s.top();

while (x != $) {
    b = read(); // legge un carattere dalla parola

    if (x == b) {
        s.pop();
    } 
    else if (x is terminal || M[x, b] is error) {
        error();
    } 
    else if (M[x, b] == A -> Y1 ... Yn) {
        s.pop();
        s.push(Y1 ... Yn); // Push dei simboli della produzione
    }
    
    x = s.top();
}

// Se il ciclo termina correttamente: w ∈ L
```

> [!TIP] Nota sulla funzione read()
> 
> La funzione read() legge un carattere alla volta dalla parola in input.

---

### Tips per riconoscere se una grammatica è LL(1)

Data una grammatica $G$ con produzioni nel formato $A \to \alpha \mid \beta$, allora:

- Se $First(\alpha) \cap First(\beta) = \emptyset \implies G$ è $LL(1)$
    
- Se $\varepsilon \in First(\alpha)$, allora $First(\beta) \cap Follow(A) = \emptyset \implies G$ è $LL(1)$
    
- Se $\varepsilon \in First(\beta)$, allora $First(\alpha) \cap Follow(A) = \emptyset \implies G$ è $LL(1)$
    

### Casi di non-LL(1):

- Le grammatiche **ricorsive sinistre** **NON** sono $LL(1)$
    
- Le grammatiche **fattorizzabili a sinistra** **NON** sono $LL(1)$
    
- Le grammatiche **ambigue** **NON** sono $LL(1)$

### Rimuovere la ricorsione a sinistra

Generalmente, dato lo schema:

$$A \to A\alpha_1 \mid \dots \mid A\alpha_n \mid \beta_1 \mid \dots \mid \beta_k$$

con $\alpha_j \neq \varepsilon$ e $\beta_i \neq A\gamma$

Si trasforma in:

$$A \to \beta_1 A' \mid \dots \mid \beta_k A'$$

$$A' \to \alpha_1 A' \mid \dots \mid \alpha_n A' \mid \varepsilon$$

> [!TIP] In breve
> 
> Le produzioni non affette da ricorsione le tieni, ma ci attacchi $A'$. Le altre le sposti in $A'$ ma senza la $A$ ricorsiva iniziale.

### Ricorsione sinistra non immediata

In caso di ricorsione sinistra **non immediata**, si sostituiscono le produzioni del 1° NON-terminale nel 2° NON-terminale.

**Esempio:**

Dati i seguenti passaggi:

$$A \to Ba \mid b$$

$$B \to Bc \mid Ad \mid b$$

In questo caso, abbiamo una ricorsione sinistra su $A$ (indiretta): $A \Rightarrow Ba \Rightarrow AdA$.

#### 1. Procediamo per sostituzione

Sostituiamo le produzioni di $A$ all'interno di $B$ dove compare il simbolo non-terminale $A$.

La produzione $B \to Ad$ diventa quindi:

$$B \to Bad \mid bd$$

Otteniamo quindi la grammatica aggiornata:

$$A \to Ba \mid b$$

$$B \to Bc \mid Bad \mid bd \mid b$$

#### 2. Togliamo ora la ricorsione (immediata)

Applichiamo l'algoritmo standard per eliminare la ricorsione sinistra su $B$:

$$A \to Ba \mid b$$

$$B \to bd B' \mid b B'$$

$$B' \to c B' \mid ad B' \mid \varepsilon$$

### Fattorizzazione a sinistra

Data una grammatica del tipo:

$$A \to \alpha \beta_1 \mid \dots \mid \alpha \beta_n \mid \gamma_1 \mid \dots \mid \gamma_k$$

Questa si trasforma in:

$$A \to \alpha A' \mid \gamma_1 \mid \dots \mid \gamma_k$$

$$A' \to \beta_1 \mid \dots \mid \beta_n$$

E si continua iterativamente finché la grammatica non diventa **LL(1)**.

### Rimuovere le ambiguità

Non esiste un vero e proprio algoritmo universale, ma generalmente si segue questa regola logica:
 
Si cerca di tenere nelle prime derivazioni (quelle più vicine allo start symbol) i caratteri/operatori con minore priorità, e nelle ultime derivazioni quelli con maggiore priorità. In questo modo, nell'albero di derivazione, gli operatori con maggiore priorità si troveranno più in basso (e verranno quindi valutati per primi).

#### Esempio pratico

Data la grammatica ambigua:

$$S \to S a S \mid S b S \mid S c S \mid id$$

Con priorità degli operatori: $c > b > a$.

**La grammatica trasformata diventa:**

1. **Livello priorità bassa ($a$):** $S \to S a E \mid E$
    
2. **Livello priorità media ($b$):** $E \to E b T \mid T$
    
3. **Livello priorità alta ($c$):** $T \to T c Q \mid Q$
    
4. $Q \to id$

## SLR(1)

Sta per **Simple Left-to-right Rightmost parser**. Siamo nel **Bottom-Up parsing**.

### Procedimento

1. **Costruzione dell'automa**
    
2. **Costruzione della tabella di parsing**
    
3. **Riconoscimento della parola**

## 1. Costruzione dell'automa

- Si parte dallo **stato 0** che contiene $S' \to \bullet S$ e si aggiungono tutte le produzioni del tipo $S \to \bullet \dots$.
    
- A questo punto, per tutti i caratteri che si trovano dopo il punto, creiamo uno stato.
    
- Dobbiamo poi visitare ognuno di quelli, spostando in avanti il **$\bullet$** (per quella relativa produzione).
    
- Quando il **$\bullet$** arriva in fondo alla produzione, lo stato che contiene quella produzione è **"SPECIALE"**.
    
- Lo stato con la produzione $S' \to S \bullet$ è detto **ACCETTATO**.

## 2. Costruzione della tabella di parsing

- **Creiamo una tabella** che ha nelle righe **tutti gli stati** (da $0$ a $n$) e nelle colonne tutti i **terminali** + **$** + i **non terminali**.
    
- Ci calcoliamo i **First** e i **Follow** dei non terminali.

Ci sono essenzialmente **4 casi** per il riempimento:

- **Caso 1**: Se abbiamo una transizione del tipo $T(n_1, \gamma) = n_2$ dove $\gamma$ è un **terminale**:
    
    - Inseriamo in $M[n_1, \gamma] = s n_2$ (ovvero **shift** allo stato $n_2$).
        
- **Caso 2**: Se abbiamo una transizione del tipo $T(n_1, Y) = n_2$ dove $Y$ è un **non terminale**:
    
    - $M[n_1, Y] = n_2$ (ovvero **go-to** allo stato $n_2$).
        
- **Caso 3**: Nella cella $M[\text{stato Accettato}, \$]$ inseriamo **Acc**.
    
- **Caso 4**: Mancano ora gli stati speciali. Per ognuno di questi eseguiamo i semplici passi:
  Per ogni produzione che presenta il **$\bullet$ alla fine** (stato speciale):
	1. Si individua il **non-terminale driver** (quello a sinistra della produzione, es. $A$ in $A \to \alpha \bullet$).
	    
	2. Si recupera l'insieme **$Follow(driver)$**.
	    
	3. Si riempie la tabella come segue:
    
    $$M[\text{stato speciale}, \forall \text{follow}(\text{driver})] = \text{produzione}$$

## 3. Riconoscimento della parola

L'algoritmo vero e proprio serve per ottenere il parsing di una parola $w$. Se $w \in \mathcal{L}(G)$ l'algoritmo ha successo, altrimenti restituisce `error()`.

**Configurazione iniziale:**

- Stringa di input: $w$ seguita dal simbolo **$\$$**.
    
- **stSt**: Stack per gli **stati**.
    
- **symSt**: Stack per i **simboli**.
    

### Algoritmo

```cpp
b = first_symbol_in_the_input_buffer;
stSt.push(0); // Inizializza lo stack degli stati con lo stato 0

while (true) {
    S = stSt.top();

    if (M[S, b] == shift T) {
        symSt.push(b);      // Push del simbolo letto
        stSt.push(T);       // Push del nuovo stato
        b = next_symbol();  // Avanza nell'input
    }
    else if (M[S, b] == reduce A -> beta) {
        pop |beta| symbol off symSt; // Rimuove simboli pari alla lunghezza di beta
        symSt.push(A);               // Push del non-terminale driver
        pop |beta| state off stSt;   // Rimuove gli stati corrispondenti
        stSt.push(M[stSt.top(), symSt.top()]); // Esegue il GOTO
		print(A -> beta);
    }
    else if (M[S, b] == Accept) {
	    return; // Parsing completato con successo
	}
	else {
	    error(); // Errore di sintassi
	}
}
```

## Generazione del codice intermedio

Questa fase si occupa di tradurre le istruzioni della grammatica in un linguaggio intermedio (solitamente simile all'assembly o a tre indirizzi) utilizzando degli attributi sintetizzati o ereditati.

### Produzione $P \to S$

- **Logica:** Si crea una nuova etichetta per segnare la fine dello statement.
    
- **Regole:**
    
    - $S.next = newlabel()$
        
    - $P.code = S.code \blacktriangleright label(S.next)$
        

### Produzione $S \to if (B) S_1$

- **Logica:** Se la condizione $B$ è vera, si salta al codice di $S_1$.
    
- **Regole:**
    
    - $B.true = newlabel()$
        
    - $B.false = S_1.next = S.next$
        
    - $S.code = B.code \blacktriangleright label(B.true) \blacktriangleright S_1.code$
        

### Produzione $S \to if (B) S_1 \text{ else } S_2$

- **Logica:** Vengono create etichette separate per i rami _true_ e _false_. Al termine del ramo _true_ è necessario un salto incondizionato alla fine dello statement.
    
- **Regole:**
    
    - $B.false = newlabel()$
        
    - $B.true = newlabel()$
        
    - $S_1.next = S.next$
        
    - $S_2.next = S.next$
        
    - $S.code = B.code \blacktriangleright label(B.true) \blacktriangleright S_1.code \blacktriangleright gen(goto \text{ } S.next) \blacktriangleright label(B.false) \blacktriangleright S_2.code$

### Produzione $S \to \text{while } (B) S_1$

- **Logica:** Viene definita un'etichetta iniziale per consentire il salto all'indietro alla fine del corpo del ciclo. La condizione $B$ determina se eseguire $S_1$ o saltare alla fine dello statement.
    
- **Regole:**
    
    - $\text{begin} = \text{new label()}$
        
    - $B.\text{true} = \text{new label()}$
        
    - $B.\text{false} = S.\text{next}$
        
    - $S_1.\text{next} = \text{begin}$
        
	- $S.\text{code} = \text{label(begin)} \blacktriangleright B.\text{code} \blacktriangleright \text{label}(B.\text{true}) \blacktriangleright S_1.\text{code} \blacktriangleright \text{gen}(\text{goto begin})$
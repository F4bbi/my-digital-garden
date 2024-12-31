---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/matematica-discreta/teoria/introduzione/","created":"2024-09-13T09:27:55.284+02:00","updated":"2024-09-13T09:27:55.284+02:00"}
---

# Introduzione
## Argomenti del corso
1) Aritmetica modulare e crittografia RSA
2) Grafi

## Cenni di teoria degli insiemi
- La teoria degli insiemi può essere formalizzata da 2 concetti primitivi:
1) *Elemento*
2) *Insieme*

La teoria può essere sviluppata dal concetto di <mark style="background: #FFF3A3A6;">appartenenza</mark> 
Scriveremo $x \in A$, e si legge "x è un elemento dell'insieme A" oppure "x appartiene ad A".
Oppure $A \ni x$, e si legge "L'insieme A contiene x come uno dei suoi elementi".

Scriveremo anche $x \notin A$ oppure $x \not\ni A$, e si legge "x non appartiene ad A".

La proprietà fondamentale che si richiede per poter parlare di un insieme A è la seguente:
- Per ogni elemento x, devo essere in grado di stabilire se $x \in A$ o $x \notin A$

### Osservazione

Ci chiediamo se  $\{ x | x \notin x \}$ è un insieme?

Supponiamo lo sia, allora A è l'insieme di tutti gli insiemi x tali che x visto come elemento non appartiene a x come insieme.
Poichè A è un insieme (per nostra ipotesi), è anche un elemento, dunque una delle seguenti condizioni deve valere: 

1.  $A \in A \Rightarrow A \notin A$   ASSURDO
2.  $A \notin A \Rightarrow A \in A$   ASSURDO

Quindi $\{ x | x \notin x \}$ non è un insieme $\rightarrow$ <mark style="background: #FFB8EBA6;">Paradosso di Russel</mark> 

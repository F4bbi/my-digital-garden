---
{"dg-publish":true,"permalink":"/university-notes-in-italian/sistemi-operativi/laboratorio/lezione-3/"}
---

### Perché C?
- Struttura minimale
- Poche parole chiave (con i suoi pro e contro!)
- Unix compliant, alla base di Unix, nato per scrivere Unix
- Organizzato a passi, con sorgente, file intermedi ed eseguibile finale
- Disponibilità di librerie conosciute e standard
- Efficiente perché di basso livello
- Pieno controllo del programma e delle sue risorse
- Ottimo per interagire con il sistema operativo

### Direttive e istruzioni fondamentali
- \#include … / \#define … /
- char / int / … / enum (v. esempio seguente)
- for ( initialization ; test; increment ) { … ; }
- break / continue
- switch (expression){ case val: … \[break;\] \[default: …\] }
- while (expression) { … } / do { … } while (expression)
- if (expression) { … } \[else { … }\]
- struct / union

### Tipi e Casting
C è un linguaggio debolmente tipizzato che utilizza 8 tipi fondamentali. È possibile fare il casting tra tipi differenti:
```c
float a = 3.5;
int b = (int)a;
```
La grandezza delle variabili è dipendente dall’architettura di riferimento, i valori massimi per ogni tipo cambiano a seconda se la variabile è _signed_ o _unsigned_.

- void (0 byte)
- char (1 byte)
- short (2 bytes)
- int (4 bytes)
- float (4 bytes)
- long (8 bytes)
- double (8 bytes)
- long double (8 bytes)

NB: non esiste il tipo boolean, ma viene spesso emulato con un char.

### sizeof (operatore)
```c
sizeof (type) / sizeof expression
```
Si tratta di un operatore che elabora il tipo passato come argomento (tra parentesi) o quello dell’espressione e restituisce il numero di bytes occupati in memoria.
```c
#include <stdio.h>
int main() 
{
	int x = 10;
	printf("variable x : %lu\n", sizeof x);
	printf("expression 1/2 : %lu\n", sizeof 1/2);
	printf("int type : %lu\n", sizeof(int));
	printf("char type : %lu\n", sizeof(char));
	printf("float type : %lu\n", sizeof(float));
	printf("double type : %lu\n", sizeof(double));
	return 0;
}
```

### Puntatori di variabili 1
C si evolve attorno all’uso di puntatori, ovvero degli alias per zone di memorie condivise tra diverse variabili/funzioni. L’uso di puntatori è abilitato da due operatori: '<span class="code">*</span>' ed '<span class="code">&</span>'.

<span class="code">*</span> ha significati diversi a seconda se usato in una dichiarazione o in un’assegnazione:
```c
int *punt; // → crea un puntatore ad intero
int valore = *(punt); // → ottiene valore puntato, ==a seconda del tipo del puntatore, gcc sa come interpretarlo==
```

<span class="code">&</span> ottiene l’indirizzo di memoria in cui è collocata una certa variabile.
```c
long whereIsValore = &valore;
```

Esempi
```c
float pie = 3.4;
float *pPie = &pie;
pie *= 2;
float pie4 = *pPie * 2;
char *array = "str";
*array = 's';
array[1] = 't';
*(array + 2) = 'r';
```

### Puntatori di variabili 2
```c
int i = 42;
int * punt = &i;
int b = *(punt);
```

<div class="grid-container">
  <table class="left-table">
    <tr>
      <td>Tipo</td>
      <td>Nome</td>
      <td>Valore</td>
      <td>Indirizzo</td>
    </tr>
    <tr>
      <td>int</td>
      <td>i</td>
      <td>42</td>
      <td>0xaaaabbbb</td>
    </tr>
    <tr>
      <td>int*</td>
      <td>punt</td>
      <td>0xaaaabbbb</td>
      <td>0xccccdddd</td>
    </tr>
    <tr>
      <td>int</td>
      <td>b</td>
      <td>42</td>
      <td>0x11112222</td>
    </tr>
  </table>

  <div class="center-text">i=20<br>&#x27A1;</div>

  <table class="right-table">
    <tr>
      <td>Nome</td>
      <td>Valore</td>
    </tr>
    <tr>
      <td>i</td>
      <td>20</td>
    </tr>
    <tr>
      <td>punt</td>
      <td>0xaaaabbbb</td>
    </tr>
    <tr>
      <td>b</td>
      <td>?</td>
    </tr>
  </table>
</div>

### Puntatori di funzioni
```c
#include <stdio.h>
float xdiv(float a, float b) 
{
    return a/b;
}
float xmul(float a, float b) 
{
    return a*b;
}

int main() 
{
    float (*punt)(float,float);
    punt = xdiv;
    float res = punt(10,10);
    punt = &xmul; //& opzionale
    res = (*punt)(10,10);
    printf("%f\n", res);
    return 0;
}
```
C consente anche di creare dei puntatori a delle funzioni: puntatori che possono contenere l’indirizzo di funzioni differenti. Sintassi simile ma diversa!
```c
float (*punt)(float,float);
ret_type (* pntName)(argType,argType,...)
```

### main.c
A parte casi particolari (es. sviluppo moduli per kernel) l’applicazione deve avere una funzione “**main**” che è utilizzata come punto di ingresso.
- Il valore di ritorno è int, un intero che rappresenta il codice di uscita dell’applicazione (variabile **$?** in bash) ed è 0 di default se omesso. Può essere usato anche void, ma non è standard.
- Quando la funzione è invocata riceve normalmente in input il numero di argomenti (**int argc**), con incluso il nome dell’eseguibile, e la lista degli argomenti come “vettore di stringhe” (**char \* argv[]**) (*)

(*) in C una stringa è in effetti un vettore di caratteri, quindi un vettore di stringhe è un vettore di vettori di caratteri, inoltre i vettori in C sono sostanzialmente puntatori (al primo elemento del vettore) → lista di argomenti spesso indicata con “**char \*\* argv**”.
Utile riferimento generale: https://www.gnu.org/software/gnu-c-manual/gnu-c-manual.html

### Esecuzione - esempio
```c
#include <stdio.h>
int main(int argc, char **argv) 
{
    printf("%d\n", argc);
    printf("%s\n", argv[0]);
    return 0;
}
```
Compilazione:
```bash
gcc main.c -o main
```
Esecuzione:
```bash
./main arg1 arg2
```

In output si ha “3” (numero argomenti incluso il file eseguito) e “<span class="code">./main</span>” (primo degli argomenti). In generale quindi <span class="code">argc</span> è sempre maggiore di zero.


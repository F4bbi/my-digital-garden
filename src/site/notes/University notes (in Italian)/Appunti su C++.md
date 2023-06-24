---
{"dg-publish":true,"permalink":"/university-notes-in-italian/appunti-su-c/","created":"2022-06-16T10:44:11.895+02:00","updated":"2023-06-03T17:11:55.957+02:00"}
---

# Appunti su C++
## Appunto 1)
```c
struct node {
	int data;
	node *next;
};

int main(int argc, char * argv[])
{
	node * risultato, * temp = new node;
	temp->data = 118;
	temp->next = NULL;
	risultato = temp;
	temp = temp->next;
	temp = new node;
	temp->data = 69;
	temp->next = NULL;
	temp = temp->next;
}
```

Questo pezzo di codice non funziona come previsto.
Il problema è che quando creo un nuovo nodo mentre temp è NULL, il suo indirizzo cambia e il nodo risultato perde il riferimento a temp.
Bisogna pertanto fare nel seguente modo:
```c
struct node {
	int data;
	node *next;
};

int main(int argc, char * argv[])
{
	node * risultato, * temp = new node;
	temp->data = 118;
	temp->next = NULL;
	risultato = temp;
	temp->next = new node;
	temp = temp->next;
	temp->data = 69;
	temp->next = NULL;
	temp = temp->next;
}
```
oppure utilizzare una funzione e passargli i `next`.

---
## Appunto 2)
Quando si utilizza un for con una lista, molte volte capita che la lista risultante abbia un nodo in eccesso, in particolare l'ultimo. 
**Esempio:**
```c
struct node
{
	int info;
	node *next;
};

void stampa_lista(node *& head)
{
    if(head == NULL) 
        return;
    
	cout << head->info << " ";
	stampa_lista(head->next);
}

node* crea_lista_v1()
{
    node * head = new node;
    node * current = head;
    for(int i = 1; i < 5; i++) {
        current->info = i;
        current->next = new node;
        current = current->next;
    }
    return head;
}

node* crea_lista_v2()
{
    node * head = new node;
    node * current = head;
    for(int i = 1; i < 5; i++) {
        current->next = new node;
        current->next->info = i;
        current = current->next;
    }
    node * tmp = head; //Per evitare memory-leaks
    head = head->next;
    delete tmp;
    return head;
}

int main()
{
    node * lista = crea_lista_v1(); 
    stampa_lista(lista); //Output: 1 2 3 4 0
    node * lista2 = crea_lista_v2();
    stampa_lista(lista2); //Output: 1 2 3 4
}
```
Come si può notare, nel for di _crea_lista_v1_ viene assegnato il valore al primo nodo, ne creo un altro e sposto il puntatore su quello. Così facendo però all'ultima iterazione si creerà un nodo vuoto in più.
In _crea_lista_v2_ quello vuoto è il primo nodo e la vera lista inizia dal secondo nodo. 

---
## Appunto 3)
Sempre nel codice precedente, si noti che passando _head->next_ a  _stampa_lista_, seppure accetti un node \*&, quando si esce dalla funzione, la lista punta al primo nodo e non all'ultimo come si potrebbe pensare (è come se la funzione prendesse un node \*). Questo perchè non vengono modificati i puntatori tra i nodi.

```c
void stampa_lista(node *& head)
{
    if(head == NULL) 
        return;
    
	cout << head->info << " ";
	head = head->next;
	stampa_lista(head);
}
```
In questa funzione invece, visto che stiamo modificando _head_, all'uscita sarà null.

---
## Appunto 4)
Per spostarsi avanti e indietro in un array senza preoccuparsi di un OutOfBounds:
```c
nextElement = (element + 1 (oppure -1) + length) % length
```

---
## Appunto 5)
Per compilare un file con C++17 e vari flag di aiuto:
```
g++ -std=c++17 -Wall -g file.cc
```

Per dare i privilegi in caso di necessità:
```
sudo chown -R fabbi ~/coding
```

File make per compilare un file C++
```bash
CC = g++
TARGET = canial
SOURCE = main.cpp
FLAGS = -Wall -Wextra -Werror -g -std=c++17
DEST ?= .

# Regola per generare il binario nella cartella specificata tramite la variabile DEST
all:
	@if [ -d $(DEST) ]; then \
		$(CC) $(FLAGS) $(SOURCE) -o $(DEST)/$(TARGET); \
	else \
		echo "ERROR" >& 2; \
	fi

# Regola per pulire i file oggetto e i file senza estensione
clean:
	rm -f $(DEST)/*.o $(DEST)/*.out $(DEST)/*
```

---
## Appunto 6)
```cpp
Node() = default;
```
istanzia le sue variabili a default (a 0, a NULL...), è come se non fosse scritto 

```cpp
Node() {}
```
non fa proprio niente, dentro le variabili di istanza ci può essere di tutto

----
## Appunto 7)
Per inizializzare una matrice dinamica usando `std::vector`:
```cpp
vector<vector<int>> DP(n, vector<int>(m));
```

## Appunto 8)
Copy-paste per stampare una matrice
```cpp
for(int i = 0; i < n; i++) {
	for(int j = 0; j < m; j++) {
	    cout << "[" << DP[i][j] << "]" << " ";
    }
    cout << endl;
}
```
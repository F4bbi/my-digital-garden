---
{"dg-publish":true,"permalink":"/coding/data-structures/binary-search-trees-in-italian/"}
---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/university-notes-in-italian/algoritmi-e-strutture-dati/3-3-alberi-binari-di-ricerca/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




# Alberi binari di ricerca
## Indice
- [[Coding/Data Structures/Binary Search Trees (in italian)#๐ Definizione\|๐ Definizione]]
- [[Coding/Data Structures/Binary Search Trees (in italian)#โ Operazioni\|โ Operazioni]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#๐ Ricerca\|๐ Ricerca]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#โ Inserimento\|โ Inserimento]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#โ๏ธ Minimo\|โ๏ธ Minimo]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#โถ๏ธ Massimo\|โถ๏ธ Massimo]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#Successore di un nodo\|Successore di un nodo]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#Predecessore di un nodo\|Predecessore di un nodo]]
	- [[Coding/Data Structures/Binary Search Trees (in italian)#โ Rimozione\|โ Rimozione]]
## ๐ Definizione
Un albero binario di ricerca รจ un albero binario nel quale ogni nodo rispetta una specifica proprietร  di ordinamento:
`figli nel sottoalbero sinistro <= nodo < figli nel sottoalbero destro`. Questo deve essere vero per ogni nodo.

## โ Operazioni
### ๐ Ricerca
Permette di ricercare un determinato valore all'interno dell'albero. 

**Restituisce** 
- il nodo dellโalbero $T$ che contiene la chiave $k$, se presente 
- NULL, se non presente

**L'idea** รจ la seguente:
1. Iniziamo dalla radice.ย 
2.  Compariamo se l'elemento cercato con la radice, se minore visitiamo il sottoalbero sinistro, altrimenti visitiamo il sottoalbero destro.ย 
3.  Se troviamo l'elemento cercato restituiamo vero (oppure il nodo), altrimenti restituiamo falso (o NULL).

#### Implementazione ricorsiva
```cpp
Tree search(Tree root, Item key) {
	if(root == NULL || root.key == key)
		return root;
	else if(key < root.key)
		return search(root.left, key);
	else
		return search(root.right, key);	
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(h)$ (se l'albero non รจ bilanciato, nel caso peggiore รจ praticamente una lista) 
#### Implementazione iterativa
```cpp
Tree search(Tree root, Item key) {
	Tree current = root;
	while(current != NULL && current.key != key) {
		if(key < root.key)
			current = current.left;
		else
			current = current.right;
	}
	return current;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$

###  โ Inserimento
Permette di inserire un nuovo nodo all'interno dell'albero binario di ricerca.

**L'idea** รจ la stessa della ricerca, cerchiamo quindi la chiave del nodo da aggiungere dalla radice fino in fondo all'albero, ossia quando arriveremo ad un nodo nullo. Lรฌ inseriremo il nostro nodo. Il problema รจ solo fare `new node` dentro un nodo  nullo. Per questo ci terremo un puntatore al nodo visitato precedentemente, cosรฌ da fare `prev->left (o prev->right) = new node`. 

Nell'implementazione sotto assumiamo che ogni nodo abbia il riferimento ai due figli. Se ogni nodo avesse anche un riferimento al padre, una volta inserito il nodo dovremmo anche fargli puntatore il padre.
#### Implementazione iterativa
```cpp
Tree insert(Tree root, Item key) {
	Tree newNode = new Tree(key);
    if(root == NULL) {
        root = newNode;
        return newNode;
    }
    Tree current = root;
    Tree prev = NULL;
    while(current != NULL) {
        prev = current;
        if(key < current.key)
            current = current.left;
        else
            current = current.right;
    }
    if(key < prev.key) 
        prev.left = newNode;
    else 
        prev.right = newNode;
    return newNode;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$ 
#### Implementazione ricorsiva
```cpp
Tree insert(Tree root, Item key) {
{
    if(root == NULL) {
        root = new Tree(key);
        return root;
    }
    if(key < root.key)
        return insert(root.left, key);
    else
        return insert(root.right, key);
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(h)$
### โ๏ธ Minimo
Scelto un nodo, restituisce il minimo rispetto a quel nodo. 

**L'idea** รจ la seguente:
- ร sufficiente visitare ricorsivamente il figlio sinistro, visto che sarร  minore del nodo corrente.

#### Implementazione iterativa
```cpp
Tree min(Tree root) {
		Tree current = root;
		while(current.left != NULL) {
			current = current.left;
		}
		return current;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$

#### Implementazione ricorsiva
```cpp
Tree min(Tree root) {
	if(root.left == NULL)
		return root;
	return min(root.left);	
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(h)$

### โถ๏ธ Massimo
Scelto un nodo, restituisce il massimo rispetto a quel nodo. 

**L'idea** รจ la seguente:
- ร sufficiente visitare ricorsivamente il figlio destro, visto che sarร  maggiore del nodo corrente.

#### Implementazione iterativa
```cpp
Tree max(Tree root) {
	Tree current = root;
	while(current.right != NULL) {
		current = current.right;
	}
	return current;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$

#### Implementazione ricorsiva
```cpp
Tree max(Tree root) {
	if(root.right == NULL)
		return root;
	return max(root.right);	
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(h)$

### Successore di un nodo
Il successore di un nodo $u$ รจ il piรน piccolo nodo maggiore di $u$.

Ci sono essenzialmente 4 casi che possono capitare quando si cerca il successore di un nodo.
In tutti gli esempi il nodo blu รจ il nodo $u$ scelto, il nodo rosso il successore.
#### Caso 1
```mermaid
	graph TD
		A((6))
		B((3))
	    C((8))
	    D((1))
	    E((4))
	    F((NULL))
	    G((12))
	    H((NULL))
	    I((5))
	    J((9))
	    K((15))

		A-->B
		A-->C
		B-->D
		B-->E
		C-->F
		C-->G
		E-->H
		E-->I
		G-->J
		G-->K
		style C fill:#00758f
		style J fill:#ff0000
```
Il caso piรน semplice รจ quando il nodo $u$ ha un sottoalbero destro. In questo caso il successore รจ il minimo del sottoalbero destro di $u$. Semplicemente, dal nodo di partenza visito il figlio destro e poi vado piรน a sinistra che posso.

#### Caso 2
```mermaid
	graph TD
		A((6))
		B((3))
	    C((8))
	    D((1))
	    E((4))
	    F((NULL))
	    G((12))
	    H((NULL))
	    I((5))
	    J((9))
	    K((15))

		A-->B
		A-->C
		B-->D
		B-->E
		C-->F
		C-->G
		E-->H
		E-->I
		G-->J
		G-->K
		style J fill:#00758f
		style G fill:#ff0000
```
Da questo caso in poi vogliamo trovare il successore di un nodo foglia. In questo specifico caso, $u$ รจ figlio sinistro del nodo padre (12) e proprio per questo 12 รจ per forza il suo successore. 
Pensiamo infatti ai possibili padri di 12:
- O il padre di 12 รจ piรน piccolo di $u$, come in questo caso (se cosรฌ non fosse 9 sarebbe un figlio sinistro del padre di 12)
- O il padre di 12 รจ piรน grande di 12, quindi non sarebbe il suo successore.
In conclusione, se $u$ รจ figlio sinistro il suo successore รจ il padre.

#### Caso 3
```mermaid
	graph TD
		A((6))
		B((3))
	    C((8))
	    D((1))
	    E((4))
	    F((NULL))
	    G((12))
	    H((NULL))
	    I((5))
	    J((9))
	    K((15))

		A-->B
		A-->C
		B-->D
		B-->E
		C-->F
		C-->G
		E-->H
		E-->I
		G-->J
		G-->K
		style I fill:#00758f
		style A fill:#ff0000
```
In questo caso, $u$ รจ figlio destro del nodo padre. A questo punto l'idea รจ continuare a risalire l'albero finchรฉ non troviamo un figlio che รจ sinistro rispetto al padre (ricongiungendosi cosรฌ al caso 2). A quel punto il successore sarร  il nodo padre di quel figlio.

#### Caso 4
```mermaid
	graph TD
		A((6))
		B((3))
	    C((8))
	    D((1))
	    E((4))
	    F((NULL))
	    G((12))
	    H((NULL))
	    I((5))
	    J((9))
	    K((15))

		A-->B
		A-->C
		B-->D
		B-->E
		C-->F
		C-->G
		E-->H
		E-->I
		G-->J
		G-->K
		style K fill:#00758f
```
Il caso รจ lo stesso del precedente, dovremmo continuare a risalire l'albero nella speranza di trovare un figlio che รจ sinistro rispetto al padre. Il problema รจ che in questo caso $u$ รจ il massimo dell'intero albero. Arriveremo quindi a visitare il padre della radice, che รจ NULL, e restituiremo quello.

#### Implementazione iterativa
**Attenzione:** In questa implementazione dobbiamo per forza avere un campo `parent`all'interno del nodo, altrimenti non possiamo risalire l'albero! 
```cpp
Tree successor(Tree t) {
	if(t == NULL)
		return t;
	/* Caso 1*/
	Tree current = t;
	if(current.right != NULL)
		return min(current.right)
	else {
		Tree p = current.parent;
		/* Se p.right != t finiamo nel caso 2-3 */
		/* Se p == NULL finiamo nel caso 4 */
		while(p != NULL && p.right == current) {
			current = p;
			p = p.parent;
		}
		return p;
	}	
}
```
- Time complexity: $O(n)$ (_n_ รจ il numero di elementi)
- Space complexity: $O(1)$
### Predecessore di un nodo
Il predecessore di un nodo $u$ รจ il piรน grande nodo minore di $u$.

Potrei scrivere "Vedi [[Coding/Data Structures/Binary Search Trees (in italian)#Successore di un nodo\|#Successore di un nodo]]" visto che รจ la stessa cosa, ma visto che fortunatamente esiste il copia-incolla lo descriverรฒ lo stesso.

Ci sono essenzialmente 4 casi che possono capitare quando si cerca il predecessore di un nodo.
In tutti gli esempi il nodo blu รจ il nodo $u$ scelto, il nodo rosso il predecessore.
#### Caso 1
```mermaid
graph TB;
    A((6))-->B((3))
    A-->C((8))
    B-->E((1))
    B-->F((4))
    C-->H((NULL))
    C-->I((12))
    F-->J((NULL))
    F-->K((5))
    I-->L((9))
    I-->M((15))
style A fill:#00758f
style K fill:#ff0000
```
Il caso piรน semplice รจ quando il nodo $u$ ha un sottoalbero sinistro. In questo caso il successore รจ il massimo del sottoalbero sinistro di $u$. Semplicemente, dal nodo di partenza visito il figlio sinistro e poi vado piรน a destra che posso.

#### Caso 2
```mermaid
graph TB;
    A((6))-->B((3))
    A-->C((8))
    B-->E((1))
    B-->F((4))
    C-->H((NULL))
    C-->I((12))
    F-->J((NULL))
    F-->K((5))
    I-->L((9))
    I-->M((15))
style K fill:#00758f
style F fill:#ff0000
```
Da questo caso in poi vogliamo trovare il predecessore di un nodo foglia. In questo specifico caso, $u$ รจ figlio destro del nodo padre (5) e proprio per questo 5 รจ per forza il suo predecessore. 
Pensiamo infatti ai possibili padri di 5:
- O il padre di 5 รจ piรน piccolo di $u$, come in questo caso (se cosรฌ non fosse 5 sarebbe un figlio sinistro del padre di 4)
- O il padre di 12 รจ piรน grande di 12, quindi non sarebbe il suo successore.
In conclusione, se $u$ รจ figlio destro il suo predecessore รจ il padre.

#### Caso 3
```mermaid
graph TB;
    A((6))-->B((3))
    A-->C((8))
    B-->E((1))
    B-->F((4))
    C-->H((NULL))
    C-->I((12))
    F-->J((NULL))
    F-->K((5))
    I-->L((9))
    I-->M((15))
style L fill:#00758f
style C fill:#ff0000
```
In questo caso, $u$ รจ figlio sinistro del nodo padre. A questo punto l'idea รจ continuare a risalire l'albero finchรฉ non troviamo un figlio che รจ destro rispetto al padre (ricongiungendosi cosรฌ al caso 2). A quel punto il predecessore sarร  il nodo padre di quel figlio.

#### Caso 4
```mermaid
graph TB;
    A((6))-->B((3))
    A-->C((8))
    B-->E((1))
    B-->F((4))
    C-->H((NULL))
    C-->I((12))
    F-->J((NULL))
    F-->K((5))
    I-->L((9))
    I-->M((15))
style E fill:#00758f
```
Il caso รจ lo stesso del precedente, dovremmo continuare a risalire l'albero nella speranza di trovare un figlio che รจ sinistro rispetto al padre. Il problema รจ che in questo caso $u$ รจ il minimo dell'intero albero. Arriveremo quindi a visitare il padre della radice, che รจ NULL, e restituiremo quello.

#### Implementazione iterativa
**Attenzione:** In questa implementazione dobbiamo per forza avere un campo `parent`all'interno del nodo, altrimenti non possiamo risalire l'albero! 
```cpp
Tree predecessor(Tree t) {
	if(t == NULL)
		return t;
	/* Caso 1*/
	Tree current = t;
	if(current.left != NULL)
		return max(current.left)
	else {
		Tree p = current.parent;
		/* Se p.left != t finiamo nel caso 2-3 */
		/* Se p == NULL finiamo nel caso 4 */
		while(p != NULL && p.left == current) {
			current = p;
			p = p.parent;
		}
		return p;
	}	
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$
### โ Rimozione
Rimuove il nodo contenente la chiave $k$ dallโalbero $T$ e restituisce la radice dellโalbero (potenzialmente cambiata).

Ci sono essenzialmente 3 casi che possono capitare quando si rimuove un nodo.

#### Caso 1
Il caso piรน semplice รจ quando il nodo da eliminare $u$ non ha figli. Semplicemente si elimina!

**Esempio**
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->J((6))
    F-->K((10))
style J fill:#ff0000
```

**Risultato**
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->J((NULL))
    F-->K((10))
```

#### Caso 2
Il nodo da eliminare $u$ ha un solo figlio. In questo caso si elimina $u$ e si attacca il figlio al padre di $u$.

**Esempio**
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->J((6))
    F-->K((10))
    J-->L((NULL))
    J-->M((7))
    M-->N((NULL))
    M-->O((8))
style J fill:#ff0000
```

**Risultato**
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->J((7))
    F-->K((10))
    J-->L((NULL))
    J-->M((8))
style J fill:#ff0000
```

#### Caso 3
Il piรน complicato, quando il nodo da eliminare $u$ ha due figli.

In questo caso si procede nel seguente modo:
1. Si cerca il successore del nodo da eliminare(o il predecessore, in base a com'รจ fatto l'albero conviene di piรน rimuovere uno che l'altro, vedi qui)
2. Il successore puรฒ avere
	1. Zero figli, allora si stacca il successore e lo si sostituisce al nodo da eliminare, collegandogli i figli di $u$.
	2. Un figlio (destro, non puรฒ essere sinistro altrimenti sarebbe proprio lui il successore), allora si stacca il successore e pure il suo figlio. Il successore lo si sostituisce al nodo da eliminare (collegandogli i vecchi figli di $u$), mentre il figlio del successore si attacca al padre del successore. 

**Esempio**
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->J((6))
    F-->K((10))
    J-->L((NULL))
    J-->M((7))
    M-->N((NULL))
    M-->O((8))
style B fill:#ff0000
```

**Risultato**
Passo intermedio:
```mermaid
graph TB;
    A((11))-->B((4))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->P((NULL))
    F-->K((10))
    J((6))-->M((7))
    M-->O((8))
style B fill:#ff0000
style J fill:#3b19fc
style M fill:#0b6b30
style O fill:#6b4c0b
```

Risultato finale:
```mermaid
graph TB;
    A((11))-->B((6))
    A-->C((NULL))
    B-->E((1))
    B-->F((9))
    F-->M((7))
    F-->K((10))
    M-->N((NULL))
    M-->O((8))
style B fill:#3b19fc
style M fill:#0b6b30
style O fill:#6b4c0b
```

#### Implementazione iterativa
```cpp
Tree remove(Tree root, Item k) {
	Tree p = NULL;
	/* individuo il nodo da rimuovere */
	Tree u = getParent(root, key, p);
	/* se il nodo da rimuovere รจ presente nellโalbero... */
	if(u == NULL) return;
	/* e non ha figli... */
	if(u.left == NULL && u.right == NULL) {
		if(u != root)
			unlink(p, u);
		else
			root = NULL;
	}
	/* e ha due figli... */
	else if(u.left != NULL && u.right != NULL) {
		Tree successor = u.right;
		Tree succParent = u;
		/* determiniamo il successore di u e teniamo un puntatore a suo padre*/
		while(successor.left != NULL) {
			succParent = successor;
			successor = successor.left;
		}
		if(succParent == u) {
			successor.left = u.left;
		}
		else {
			/* il padre del successore ora punta al figlio destro del successore */
			succParent.left = successor.right;
			/* il successore viene sostituito al nodo da eliminare*/
			successor.left = u.left;
			successor.right = u.right;
		} 
		if(u == p.left)
			p.left = successor;
		else
			p.right = successor;
	}
	/* e ha un solo figlio... */
	else {
		/* determiniamo se il figlio รจ destro o sinistro*/
		Tree childToLink = (u.left != NULL) ? u.left : u.right;
		/* verifichiamo se il nodo da eliminare รจ radice */
		if(u != root) {
			/* se u รจ figlio sinistro attachiamo al padre il nuovo figlio sinistro */
			if(u == p.left)
				p.left = childToLink;
			/* se u รจ figlio destro attachiamo al padre il nuovo figlio destro */
			else
				p.right = childToLink;
		}
		else
			root = childToLink;
	}
	delete u;		
}

/* Cerca un nodo e restituisce il puntatore ad esso e a suo padre */
Tree search(Tree root, Item key, Tree parent) {
    Tree current = root;
    while(current != NULL && current.key != key) {
		parent = current;
		if(current.key < key)
			current = current.left;
		else
			current = current.right;
	}
	return current;
}

/* Scollega il padre dal figlio */
void unlink(Tree p, Tree u) {
	if(u == p.left)
		p.left = NULL;
	else
		p.right = NULL;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(1)$
#### Implementazione ricorsiva
```cpp
Tree remove(Tree root, Item k) {
	if(k < root.key) {
		root.left = remove(root.left, k);
	else if(k > root.key)
		root.right = remove(root.right, k);
	else {
		Tree toDelete = root;
		/* Copre sia il caso di 0 figli, sia il caso di 1 figlio */
		if(root.left == NULL || root.right == NULL)
			return (root.left != NULL) ? root.left : root.right;
		else {
			Tree *succ = root.right;
			Tree *succParent = root;  
			while(succ.left) {
				succParent = succ;
				succ = succ.left;  
			}
			/* corner case, avviene quando vogliamo eliminare la radice ed รจ proprio il padre del successore */
			if(succParent == root) {
				/* basta collegare il sottoalbero sinistro al successore e farlo diventare radice */
				succ.left = root.left;
			}
			else {
				/* colleghiamo il padre del successore al figlio del successore */
				succParent.left = succ.right;
				/* per non fare root.key = succ.key */
				succ.left = root.left;  
				succ.right = root.right;  
			}  
			root = succ;
		}
		delete toDelete;	
	}
	return root;
}
```
- Time complexity: $O(h)$ (_h_ รจ l'altezza dell'albero)
	(Worst case Time Complexity : $O(n)$)
- Space complexity: $O(h)$ 

</div></div>

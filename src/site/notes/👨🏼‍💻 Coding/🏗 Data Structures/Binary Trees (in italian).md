---
{"dg-publish":true,"permalink":"/coding/data-structures/binary-trees-in-italian/","created":"2023-01-24T23:36:20.957+01:00","updated":"2023-01-25T00:09:10.531+01:00"}
---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/university-notes-in-italian/algoritmi-e-strutture-dati/3-2-alberi-binari/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




# Alberi binari
## Indice
- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#📝Definizione\|📝Definizione]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#🔎 Visita di un albero\|🔎 Visita di un albero]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Depth-First Search (DFS)\|Depth-First Search (DFS)]]
		- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Pre-Order Traversal\|Pre-Order Traversal]]
		- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#In-Order Traversal\|In-Order Traversal]]
		- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Post-Order Traversal\|Post-Order Traversal]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Breadth First Search (BFS)\|Breadth First Search (BFS)]]
		- [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Level Order Traversal\|Level Order Traversal]]
## 📝Definizione
Un albero binario è un albero in cui ogni nodo ha al massimo due figli, identificati come figlio sinistro e figlio destro.

**Nota**: Due alberi $T$ e $U$ che hanno gli stessi nodi, gli stessi figli per ogni nodo e la stessa radice, sono distinti qualora un nodo $u$ sia designato come figlio sinistro in $T$ e come figlio destro in $U$.
**Per esempio, i seguenti due alberi binari sono diversi:**

![AlberoBinario.png|500](/img/user/%F0%9F%8E%93%20University%20notes%20(in%20Italian)/%E2%9A%99%EF%B8%8F%20Algoritmi%20e%20Strutture%20Dati/_images/AlberoBinario.png)
## 🔎 Visita di un albero
Due strategie per analizzare (visitare) tutti i nodi di un albero:

| Visita in profondità [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Depth-First Search (DFS)\|Depth-First Search (DFS)]]                                                | Visita in ampiezza [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Breadth First Search (BFS)\|Breadth First Search (BFS)]] | 
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Per visitare un albero, si visita ricorsivamente ognuno dei suoi **sottoalberi**                                            | Ogni **livello** dell’albero viene visitato, uno dopo l’altro                  |
| Tre varianti: [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Pre-Order Traversal\|Pre-Order]], [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#In-Order Traversal\|In-Order]], [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Post-Order Traversal\|Post-Order]] | [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Level Order Traversal\|Level Order Traversal]]                               |
| Richiede uno **stack**                                                                                                      | Richiede una **queue**                                                         |

### Depth-First Search (DFS)
Visto che si visita ricorsivamente ognuno dei sottoalberi dell'albero, in base al momento in cui decidiamo di stampare il nodo, la visita si suddivide in 3 modi. Qualunque metodo si scelga, il costo di una visita di un albero contenente $n$ nodi è $\Theta(n)$, in quanto ogni nodo viene visitato al massimo una volta.
- Time complexity: $O(n)$ (_n_ è il numero di elementi)
- Space complexity: $O(h)$ (_h_ è l'altezza dell'albero) 
#### Pre-Order Traversal
Nella visita in pre-order prima si stampa il nodo corrente, poi si visitano i figli. Nella visita in pre-order la radice è sempre il nodo visitato per primo.

```cpp
void preOrder(TreeNode t) {
	if (t != NULL) {
		print(t);
		preOrder(t.left);
		preOrder(t.right);
	}
}
```

##### Esempio
```mermaid
	graph TD
		A((A))
		B((B))
	    C((C))
	    D((D))
	    E((E))
	    F((F))
	    G((G))
	    
	    A-->B
	    A-->E
	    B-->C
	    B-->D
	    E-->F
	    E-->G
```
**Sequenza:** A B C D E F G

#### In-Order Traversal
Nella visita in in-order prima si stampa il ramo sinistro, poi il nodo corrente e infine il ramo destro. Quando eseguita in un albero binario di ricerca, la visita in in-order visita i nodi in ordine crescente (da qui il nome "in-order").

```cpp
void inOrder(TreeNode t) {
	if (t != NULL) {
		inOrder(t.left);
		print(t);
		inOrder(t.right);
	}
}
```

##### Esempio
```mermaid
	graph TD
		A((A))
		B((B))
	    C((C))
	    D((D))
	    E((E))
	    F((F))
	    G((G))
	    
	    A-->B
	    A-->E
	    B-->C
	    B-->D
	    E-->F
	    E-->G
```
**Sequenza:** C B D A F E G
#### Post-Order Traversal
Nella visita in post-order vengono visitati prima i figli, poi il nodo corrente. Nella visita in post-order la radice è sempre il nodo visitato per ultimo.

```cpp
void postOrder(TreeNode t) {
	if (t != NULL) {
		postOrder(t.left);
		postOrder(t.right);
		print(t);
	}
}
```

##### Esempio
```mermaid
	graph TD
		A((A))
		B((B))
	    C((C))
	    D((D))
	    E((E))
	    F((F))
	    G((G))
	    
	    A-->B
	    A-->E
	    B-->C
	    B-->D
	    E-->F
	    E-->G
```
**Sequenza:** C D B F G E A

### Breadth First Search (BFS)
Partendo dalla radice si visitano tutti i nodi livello per livello. La visita è chiamata Level Order Traversal.
Possiamo implementarlo sia ricorsivamente sia iterativamente.

#### Level Order Traversal
##### Esempio
```mermaid
	graph TD
		A((A))
		B((B))
	    C((C))
	    D((D))
	    E((E))
	    F((F))
	    G((G))
	    
	    A-->B
	    A-->E
	    B-->C
	    B-->D
	    E-->F
	    E-->G
```
**Sequenza:** A B E C D F G
##### Implementazione ricorsiva
L'idea è la seguente:
- Usiamo un for che va da $1$ a  $h$ (l'altezza dell'albero)
- Ad ogni iterazione, utilizziamo il [[👨🏼‍💻 Coding/🏗 Data Structures/Binary Trees (in italian)#Depth-First Search (DFS)\|Depth-First Search (DFS)]] per attraversare l'albero, mantenendoci l'altezza del nodo corrente.
	- Se il nodo è nullo -> `return`
	- Se il livello è $1$ siamo arrivati al livello desiderato, quindi stampiamo `tree->data`
	- Se il livello è maggiore di $1$, allora
		- Ricorsivamente, visitiamo il figlio sinistro, decrementando il livello di $1$
		- Ricorsivamente, visitiamo il figlio destro, decrementando il livello di $1$

```cpp
/* Function to print level order traversal a tree */
void levelOrderTraversal(Tree root)
{
    int h = height(root);
	for(int level = 1; level < h + 1; ++level)
        printCurrentLevel(root, level);
}

/* Print nodes at a current level */
void printCurrentLevel(Tree root, int level)
{
	if(root == NULL)
        return;
    else if(level == 1)
        cout << root.data << " ";
    else {
        printCurrentLevel(root.left, level - 1);
        printCurrentLevel(root.right, level - 1);
    }
}

/* Compute the "height" of a tree -- the number of 	nodes along the longest path from the root node
	down to the farthest leaf node. */
int height(Tree node)
{
    if (node == NULL)
        return 0;

    int leftHeight = height(node.left);
    int rightHeight = height(node.right);
    return max(leftHeight, rightHeight) + 1;
}
```
- Time complexity: $O(n^2)$ (_n_ è il numero di elementi)
	- Questo perchè `levelOrderTraversal()` è $O(n) + O(n-1) + O(n-2) + ... + O(1)$, cioè $O(n^2)$
- Space complexity: 
	- $O(n)$ (se l'albero non è bilanciato, nel caso peggiore è praticamente una lista) 
	- $O(\log n)$ (se l'albero è bilanciato, nel caso pessimo non devi tenere tutti gli elementi nello stack)

##### Implementazione iterativa (usando una coda)
L'idea è la seguente:
- Per ogni nodo, prima stampiamo il suo valore, poi lo rimuoviamo dalla coda e successivamente aggiungiamo i suoi figli nella coda. 
```cpp
void levelOrderTraversal(Tree root) {
    /* Base case */
    if (root == NULL)
        return;
    
    /* Create an empty queue for level order traversal */
    queue<Tree> queue;

    /* Enqueue root */
    queue.push(root);

    while(!queue.empty()) {
        /* Print front of queue and remove it from queue */
        Tree node = queue.front();
        cout << node.data << " ";
        queue.pop();

        /* Enqueue left child */
        if(node.left != NULL)
            queue.push(node.left);

        /* Enqueue right child */
        if(node.right != NULL)
                queue.push(node.right);
    }
}
```
- Time complexity: $O(n)$ (_n_ è il numero di elementi)
- Space complexity: $O(w)$ (_w_ è la massima larghezza dell'albero)

</div></div>

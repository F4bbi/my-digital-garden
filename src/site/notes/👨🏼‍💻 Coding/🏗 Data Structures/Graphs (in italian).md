---
{"dg-publish":true,"permalink":"/coding/data-structures/graphs-in-italian/","created":"2023-01-25T00:09:24.849+01:00","updated":"2023-01-25T00:09:24.849+01:00"}
---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/university-notes-mostly-in-italian/algoritmi-e-strutture-dati/4-grafi/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




# Grafi
## Indice
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#➡️ Grafo orientato (directed)\|➡️ Grafo orientato (directed)]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#↔️ Grafo non orientato (undirected)\|↔️ Grafo non orientato (undirected)]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#⚙️ Proprietà dei grafi\|⚙️ Proprietà dei grafi]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#⌨️ Implementazione\|⌨️ Implementazione]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Matrici di adiacenza\|Matrici di adiacenza]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Liste di adiacenza\|Liste di adiacenza]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#🔎 Visita di un grafo\|🔎 Visita di un grafo]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Visita in ampiezza Breadth First Search (BFS)\|Visita in ampiezza Breadth First Search (BFS)]]
		- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#🔢 Numero di Erdos\|🔢 Numero di Erdos]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Visita in profondità Depth-First Search (DFS)\|Visita in profondità Depth-First Search (DFS)]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#📝 Definizioni\|📝 Definizioni]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#🔄 Cicli\|🔄 Cicli]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#📶 Ordinamento topologico\|📶 Ordinamento topologico]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#🔗 Componenti connesse e fortemente connesse\|🔗 Componenti connesse e fortemente connesse]]
## ➡️ Grafo orientato (directed)
È una coppia $G = (V, E)$ dove: 
- $V$ è un insieme di nodi (node) o vertici (vertex) 
- $E$ è un insieme di coppie ordinate di nodi $(u, v)$ dette archi o lati (edge)

### Esempio
_Input:_

$V = \{ a,b,c,d,e,f \}$
$E = \{ (a,b), (a,d), (b,c), (d,a), (d,c), (d,e), (e,c) \}$

_Output:_
```mermaid
    graph TB
        A((a))
        B((b))
        C((c))
        D((d))
        E((e))
        F((f))
        
        A-->B
        A-->D
        D-->A
        B-->C
        D-->C
        D-->E
        E-->C
        F
```
## ↔️ Grafo non orientato (undirected)
È una coppia $G = (V, E)$ dove: 
- $V$ è un insieme di nodi (node) o vertici (vertex) 
- $E$ è un insieme di coppie non ordinate di nodi $(u, v)$ dette archi o lati (edge)

### Esempio
_Input:_

$V = \{ a,b,c,d,e,f \}$
$E = \{ (a,b), (a,d), (b,c), (d,a), (d,c), (d,e), (e,c) \}$

_Output:_
```mermaid
    graph TB
        A((a))
        B((b))
        C((c))
        D((d))
        E((e))
        F((f))

        A---B
        A---D
        D---A
        B---C
        D---C
        D---E
        E---C
        F
```

## ⚙️ Proprietà dei grafi
- $n = |V|$: numero di nodi 
- $m = |E|$: numero di archi 

Alcune relazioni fra $n$ e $m$:
- in un grafo non orientato, $m \leq \frac {n(n−1)} {2} = O(n^2)$ 
- in un grafo orientato, $m ≤ n^2 − n = O(n^2)$
- in un grafo orientato aciclico, il numero massimo di archi che possono essere contenuti senza creare un ciclo è $\sum_{0}^{n-1} \ i = \frac{n(n-1)}{2}$

Complessità di algoritmi su grafi:
- La complessità è espressa in termini sia di $n$ che di $m$ (ad es. $O(n + m)$)

## ⌨️ Implementazione
### Matrici di adiacenza
Utilizziamo una matrice per implementare il grafo, ogni cella ha la seguente regola:
$
m_{uv} =
\begin{cases}
1 &(u,v) \in E \\ \\
0 &(u,v) \notin E 
\end{cases}
$
#### Grafi orientati
Utilizziamo l'intera matrice, lo spazio utilizzato sarà $n^2$ bit.

#### Grafi non orientati
Utilizziamo metà matrice, lo spazio utilizzato sarà comunque $n^2$ bit, ma quello realmente utilizzato è di $\frac {n(n-1)} {2}$ bit.

#### Grafi pesati
- Gli archi possono avere un peso (costo, profitto, etc.) 
- Se non esiste arco fra due vertici, il peso assume un valore che dipende dal problema - e.g. $w(u, v) = 0$ oppure $+\infty$
- Al posto di $1$ nella matrice si mette il valore del peso

### Liste di adiacenza
Utilizziamo un vettore di liste per implementare il grafo, ogni cella del vettore corrisponde ad un nodo del grafo, che a sua volta contiene la lista di nodi adiacenti. Quindi $v[u] = G.adj(u) = \{ v \ | \ (u,v) \in E\}$.

#### Grafi orientati
Dobbiamo inserire gli archi tra i nodi una solta volta, lo spazio utilizzato sarà di $an + bm$ bit.

#### Grafi non orientati
Dobbiamo inserire gli archi tra i nodi due volte, lo spazio utilizzato sarà di $an + 2 \cdot bm$ bit.

#### Grafi pesati
- Gli archi possono avere un peso (costo, profitto, etc.) 
- Se non esiste arco fra due vertici, il peso assume un valore che dipende dal problema - e.g. $w(u, v) = 0$ oppure $+\infty$
- Nella lista di nodi adiacenti avremo anche un campo _peso_ in ogni nodo.

**Per riassumere**

| Matrici di adiacenza                                  | Liste di adiacenza                                        |
| ----------------------------------------------------- | --------------------------------------------------------- |
| Spazio richiesto: $O(n^2)$                            | Spazio richiesto: $O(n+m)$                                |
| Verificare se u è adiacente a v richiede tempo $O(1)$ | Verificare se $u$ è adiacente a $v$ richiede tempo $O(n)$ |
| Iterare su tutti gli archi richiede tempo $O(n^2)$    | Iterare su tutti gli archi richiede tempo $O(n + m)$      |
| Ideale per grafi densi                                | Ideale per grafi sparsi                                   |

## 🔎 Visita di un grafo
Dato un grafo $G = (V, E)$ e un vertice $r ∈ V$ (radice, sorgente), visitare una e una volta sola tutti i nodi del grafo che possono essere raggiunti da $r$.

| Visita in profondità Depth-First Search (DFS)                                                                     | Visita in ampiezza Breadth First Search (BFS)                                           |     |
| ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --- |
| Per esplorare un intero grafo, non solo i nodi raggiungibili da una singola sorgente                              | Per esplorare tutti i nodi raggiungibili da una singola sorgente                        |     |
| Per ogni nodo adiacente, si visita ricorsivamente tale nodo, visitando ricorsivamente i suoi nodi adiacenti, etc. | Prima si visita la radice, poi i nodi a distanza 1 dalla radice, poi a distanza 2, etc. |     |
| Utilizzato per l'ordinamento topologico, componente connesse, componenti fortemente connesse                      | Utilizzato per calcolare i cammini più brevi da una singola sorgente                    |     |

### Visita in ampiezza Breadth First Search (BFS)
Obiettivi:
- Visitare i nodi a distanze crescenti dalla sorgente 
- Calcolare il cammino più breve da $r$ a tutti gli altri nodi

#### Implementazione
Utilizziamo una coda per inserire i nodi che scopriamo ad ogni iterazione. Visto che è una coda, quando inseriamo i figli di un nodo, finiscono in fondo alla coda. In questo modo vengono analizzati prima tutti i nodi a distanza $k$ dal nodo di partenza. 
```cpp
void BFS(vector<vector<int>>& graph, int root) {
    queue<int> queue;
    vector<bool> visited(graph.size(), false);
    visited.at(root) = true;
    queue.push(root);
    while(!queue.empty()) {
        int v = queue.front();
	    queue.pop();
	    cout << v << " "; 
        for(int adj : graph.at(v)) {
            if(!visited.at(adj)) {
                visited.at(adj) = true;
                queue.push(adj);
            }
        }
    }
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi[^1])
- Space complexity: $O(n)$ 
#### 🔢 Numero di Erdos
> Il numero di Erdős è un modo per descrivere la "distanza" tra una persona e il matematico ungherese Paul Erdős in termini di collaborazione in pubblicazioni matematiche. È stato creato dagli amici di Erdős come tributo scherzoso all'enorme numero di pubblicazioni da lui scritte in collaborazione con un gran numero di matematici diversi.

Nel nostro caso calcoliamo la distanze tra un nodo e tutti gli altri nodi e li memorizziamo in un array.

```cpp
vector<int> distance(vector<vector<int>>& graph, int node) {
    vector<int> distance(graph.size(), -1);
    queue<int> queue;
    distance.at(node) = 0;
    queue.push(node);
    while(!queue.empty()) {
        int v = queue.front();
        queue.pop();
        for(int adj : graph.at(v)) {
            if(distance.at(adj) == -1) {
                distance.at(adj) = distance.at(v) + 1;
                queue.push(adj);
            }
        }
    }
    return distance;
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

#### Cammino tra 2 nodi
Modifichiamo leggermente la funzione precedente e aggiungiamo un vettore per memorizzare il padre di ogni nodo. In questo modo possiamo ottenere direttamente il cammino tra 2 nodi "risalendo" tutti i padri.
```cpp
vector<int> distance(vector<vector<int>>& graph, int node, vector<int>& parent) {
    vector<int> distance(graph.size(), -1);
    queue<int> queue;
    distance.at(node) = 0;
    parent.at(node) = node;
    queue.push(node);
    while(!queue.empty()) {
        int v = queue.front();
        queue.pop();
        for(int adj : graph.at(v)) {
            if(distance.at(adj) == -1) {
                distance.at(adj) = distance.at(v) + 1;
                parent.at(adj) = v;
                queue.push(adj);
            }
        }
    }
    return distance;
}

void printPathHelper(int from, int to, vector<int>& parent) {
    if(from == to) {
        cout << from << " "; 
        return;
    }
    else if(parent.at(to) == -1) {
        cout << "The node " << to << " is unreachable" << endl; 
        return;
    }
    else {
        printPathHelper(from, parent.at(to), parent);
        cout << to << " ";
    }
}

void printPath(vector<vector<int>>& graph, int from, int to) {
    vector<int> parent(graph.size(), -1);
    distance(graph, from, parent);
    printPathHelper(from, to, parent);
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

### Visita in profondità Depth-First Search (DFS)
Come detto in precedenza, è utilizzata per esplorare un intero grafo, non solo i nodi raggiungibili da una singola sorgente.

#### Implementazione ricorsiva
```cpp
void DFS(vector<vector<int>>& graph) {
    vector<bool> visited(graph.size(), false);
    for(int i = 0; i < graph.size(); i++) {
        if(!visited.at(i))
            DFSHelper(graph, i, visited);
    }
    cout << endl;
}

void DFSHelper(vector<vector<int>>& graph, int root, vector<bool>& visited) {
    visited.at(root) = true;
    cout << root << " ";
    for(int adj : graph.at(root)) {
        if(!visited.at(adj))
            DFSHelper(graph, adj, visited);
    }
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

Tuttavia, eseguire una DFS basata su chiamate ricorsive può essere rischioso in grafi molto grandi e connessi. È possibile che la profondità raggiunta sia troppo grande per la dimensione dello stack del linguaggio. In tali casi, si preferisce utilizzare una BFS oppure una DFS basata su stack esplicito.

#### Implementazione iterativa
Il concetto è simile al BFS ma ora inseriamo i nodi in uno stack. Inoltre controlliamo se un nodo è già stato visitato all’estrazione di esso, non all’inserimento, altrimenti torneremmo su nodi già visitati.
```cpp
void DFS(vector<vector<int>>& graph) {
    vector<bool> visited(graph.size(), false);
    stack<int> stack;

    for(int i = 0; i < graph.size(); i++) {
        stack.push(i);
        while(!stack.empty()) {
            int node = stack.top();
            stack.pop();
            if(!visited.at(node)) {
                cout << node << " ";
                visited.at(node) = true;
                for(int adj : graph.at(node)) {  
                    stack.push(adj);
                }
            }
        }
    }
    cout << endl;
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

## 📝 Definizioni
- **Sottografo:** $G'$ è un sottografo di $G$ $(G' ⊆ G) ⇔ V' ⊆ V$ e $E' ⊆ E$
- **Massimale:** $G'$ è massimale $⇔ \nexists$ un altro sottografo $G''$ di $G$ tale che $G''$ è connesso e più grande di $G'$ (i.e. $G' ⊆ G'' ⊆ G$)

&nbsp;
- Un grafo $G = (V, E)$
	- **non orientato** è **connesso** ⇔ ogni suo nodo è raggiungibile da ogni altro suo nodo
	- **orientato** è **fortemente connesso** ⇔ ogni suo nodo è raggiungibile da ogni altro suo nodo

&nbsp;
- Un grafo $G' = (V', E')$
	- è una **componente connessa** di $G$ (**non orientato**) ⇔ $G'$ è un sottografo connesso e massimale di $G$
	- è una **componente fortemente connessa** di $G$ (**orientato**) ⇔ $G'$ è un sottografo connesso e massimale di $G$

&nbsp;
- **Ciclo:** In un grafo orientato $G = (V, E)$, un ciclo $C$ è una sequenza di nodi $u_0, u_1, ... , u_k$ tale che $(u_i , u_{i+1}) ∈ E$ per $0 ≤ i ≤ k − 1$ e $u_0 = u_k$
	- in un grafo **non orientato** un ciclo ha lunghezza $k  >  2$
	- in un grafo **orientato** un ciclo ha lunghezza $k  \geq  2$

&nbsp;
- **Classificazione degli archi** in un grafo:

| Tipo di arco            | Definizione                                                                 | Implementazione                    |
| ----------------------- | --------------------------------------------------------------------------- | ---------------------------------- |
| Arco dell'albero        | Ogni volta che si esamina un arco da un nodo marcato ad un nodo non marcato | $dt[v] == 0$                       |
| Arco in avanti          | Se, dato un arco $(u, v)$, $u$ è un antenato di $v$                         | $dt[u] < dt[v]$ and $ft[v] \neq 0$ |
| Arco all'indietro       | Se, dato un arco $(u, v)$, $u$ è un discendente di $v$                      | $dt[u] > dt[v]$ and $ft[v] == 0$   |
| Arco di attraversamento | Un arco dell'albero tra 2 nodi già marcati                                  | Tutti gli altri casi               |

&nbsp;

- **Ordinamento topologico** di un grafo:
	-  Dato un **grafo orientato aciclico** (senza cicli) $G$, un ordinamento topologico di $G$ è un ordinamento lineare dei suoi nodi tale che se $(u, v) ∈ E$, allora $u$ appare prima di $v$ nell’ordinamento. 
		**Nota**: 
		- Esistono più ordinamenti topologici
		- Se il grafo contiene un ciclo, non esiste un ordinamento topologico.

_Esempio:_

![TopologicalSort.png|700](/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%E2%9A%99%EF%B8%8F%20Algoritmi%20e%20Strutture%20Dati/_images/TopologicalSort.png)

&nbsp;
- **Grafo trasposto:**
	- Dato un grafo orientato $G = (V, E)$, il grafo trasposto $G^T = (V, E^T)$ ha gli stessi nodi e gli archi orientati in senso opposto: $E^T = \{(u, v) | (v, u) ∈ E\}$

## 🔄 Cicli
Dopo aver dato una [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#📝 Definizioni\|definizione di ciclo]], vediamo come implementare una funzione per determinarne se è presente un ciclo in un determinato grafo.

### Grafi non orientati
Per i grafi non orientati è sufficiente eseguire un [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Visita in profondità Depth-First Search DFS\|DFS]] facendo attenzione a non visitare nuovamente il nodo da cui si era arrivati, altrimenti verrebbe generato un falso positivo.
#### Implementazione
```cpp
bool hasCycle(vector<vector<int> >& undirected_graph) {
    vector<bool> visited(undirected_graph.size(), false);
    for(int node = 0; node < undirected_graph.size(); node++) {
        /* In case that graph has more than 1 connected component */
        if(!visited.at(node))
            if(hasCycleHelper(undirected_graph, node, -1, visited))
                return true;
    }
    return false;
}

bool hasCycleHelper(vector<vector<int> >& undirected_graph, int node, int parent, vector<bool>& visited) {
    visited.at(node) = true;
    for(int adj : undirected_graph.at(node)) {
        if(adj != parent) {
            if(visited.at(adj))
                return true;
            else if(hasCycleHelper(undirected_graph, adj, node, visited))
                return true;
        }
    }
    return false;
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

### Grafi orientati
Per i grafi orientati invece è più complicato, visto che se troviamo un nodo già visitato non è detto che il grafo contenga un ciclo.

**Esempio:**
```mermaid
    graph TB
        A((a))
        B((b))
        C((c))
        
        A-->B
        B-->C
        A-->C
```
Se questo grafo non fosse orientato conterrebbe un ciclo!
Dobbiamo allora trovare un altro sistema, classificando cioè gli archi del grafo analizzato.
Se troviamo un arco all'indietro il grafo contiene un ciclo.
#### Implementazione
Quando viene individuato un arco all’indietro, questo causa la restituzione di true in una chiamata e la conseguente restituzione di true da parte di tutte le chiamate ricorsive precedenti.

```cpp
bool hasCycle(vector<vector<int> >& directed_graph) {
    /* discovery time of every node */
    vector<int> dt(directed_graph.size(), 0);
    /* finish time of every node */
    vector<int> ft(directed_graph.size(), 0);
    int time = 0;
    for(int node = 0; node < directed_graph.size(); node++) {
        /* in case that graph is a forest */
        if(dt.at(node) == 0)
            if(hasCycleHelper(directed_graph, node, time, dt, ft))
                return true;
    }
    return false;
}

bool hasCycleHelper(vector<vector<int> >& directed_graph, int node, int& time, vector<int>& dt, vector<int>& ft) {
    ++time;
    dt.at(node) = time;
    for(int adj : directed_graph.at(node)) {
        if(dt.at(adj) == 0)
            if(hasCycleHelper(directed_graph, adj, time, dt, ft))
                return true;
        /* back edge found */
        else if(dt.at(adj) < dt.at(node) && ft.at(adj) == 0)
            return true;
    }
    ++time;
    ft.at(node) = time;
    return false;
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

## 📶 Ordinamento topologico
Dopo aver dato una  [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#📝 Definizioni\|definizione di ordinamento topologico]], vediamo come implementare una funzione per determinare l'ordinamento topologico di un grafo.

### Grafi orientati
L'output sarà una sequenza dei nodi, ordinati per tempo decrescente di fine. Ricordiamo che se il grafo contiene un ciclo, non esiste un ordinamento topologico.

#### Implementazione
- Quando un nodo è "finito", tutti i suoi discendenti sono stati scoperti e aggiunti alla lista. 
- Aggiungendolo in testa alla lista, il nodo è in ordine corretto.

```cpp
stack<int> topologicalSort(vector<vector<int> >& directed_graph) {
    vector<bool> visited(directed_graph.size(), false);
    stack<int> stack;

    for(int node = 0; node < directed_graph.size(); node++) {
        if(!visited.at(node))
            ts_DFS(directed_graph, node, stack, visited);
    }
    return stack;
}

void ts_DFS(vector<vector<int> >& directed_graph, int node, stack<int>& stack, vector<bool>& visited) {
    visited.at(node) = true;
    for(int adj : directed_graph.at(node)) {
        if(!visited.at(adj))
            ts_DFS(directed_graph, adj, stack, visited);
    }
    stack.push(node);
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

## 🔗 Componenti connesse e fortemente connesse
Dopo aver dato una  [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#📝 Definizioni\|definizione di componente connessa e fortemente connessa]], vediamo come implementare una funzione per determinarne il numero in un grafo.

### Grafi non orientati
#### Implementazione
Effettuiamo la [[👨🏼‍💻 Coding/🏗 Data Structures/Graphs (in italian)#Visita in profondità Depth-First Search DFS\|DFS]] di ogni nodo ma quando un nodo non era stato scoperto incrementiamo il contatore. Alla fine esso sarà il numero di componenti connesse. Per ogni nodo il vettore `id` restituisce il numero di componente connessa a cui il nodo appartiene.

```cpp
int connectedComponents(vector<vector<int> >& undirected_graph) {
    /* id will map every node with his connected component */
    vector<int> id(undirected_graph.size(), 0);
    int counter = 0;
    for(int node = 0; node < undirected_graph.size(); node++) {
        if(id.at(node) == 0) {
            ++counter;
            DFS(undirected_graph, node, id, counter);
        }
    }
    return counter;
}

void DFS(vector<vector<int> >& undirected_graph, int node, vector<int> & id, int counter) {
    id.at(node) = counter;
    for(int adj : undirected_graph.at(node)) {
        if(id.at(adj) == 0)
            DFS(undirected_graph, adj, id, counter);
    }
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$ 

### Grafi orientati
Per i grafi orientati invece è più complicato, visto che se utilizzassimo un semplice DFS il numero di componenti connesse dipenderebbe dal nodo di partenza. Dobbiamo allora trovare un altro sistema, usando cioè l'algoritmo di Kosaraju. 

#### Implementazione
L'algoritmo di Kosaraju si divide in 3 parti:
1. Effettuare un ordinamento topologico del grafo
2. Calcolare il grafo trasposto del grafo di partenza
3. A questo punto si può utilizzare lo stesso procedimento utilizzato nei grafi non orientati per calcolare il numero di componenti connesse. Attenzione però, invece di esaminare i nodi in ordine arbitrario, questa versione li esamina nell’ordine _LIFO_ memorizzato nello stack

```cpp
int connectedComponents(vector<vector<int> >& directed_graph) {
    stack<int> stack = topologicalSort(directed_graph);
    vector<vector<int> > transposed_graph = transpose(directed_graph);
    return cc_helper(transposed_graph, stack);
}

stack<int> topologicalSort(vector<vector<int> >& directed_graph) {
    vector<bool> visited(directed_graph.size(), false);
    stack<int> stack;

    for(int node = 0; node < directed_graph.size(); node++) {
        if(!visited.at(node))
            ts_DFS(directed_graph, node, stack, visited);
    }
    return stack;
}

void ts_DFS(vector<vector<int> >& directed_graph, int node, stack<int>& stack, vector<bool>& visited) {
    visited.at(node) = true;
    for(int adj : directed_graph.at(node)) {
        if(!visited.at(adj))
            ts_DFS(directed_graph, adj, stack, visited);
    }
    stack.push(node);
}

vector<vector<int> > transpose(vector<vector<int> >& directed_graph) {
    vector<vector<int> > transposed_graph(directed_graph.size());
    for(int node = 0; node < directed_graph.size(); node++) {
        for(int adj : directed_graph.at(node)) {
            transposed_graph.at(adj).push_back(node);
        }
    }
    return transposed_graph;
}

int cc_helper(vector<vector<int> >& directed_graph, stack<int> stack) {
    vector<int> id(directed_graph.size(), 0);
    int counter = 0;
    while(!stack.empty()) {
        int node = stack.top();
        stack.pop();
        if(id.at(node) == 0) {
            ++counter;
            DFS_cc(directed_graph, node, id, counter);
        }
    }
    return counter;
}

void DFS_cc(vector<vector<int> >& graph, int node, vector<int> & id, int counter) {
    id.at(node) = counter;
    for(int adj : graph.at(node)) {
        if(id.at(adj) == 0)
            DFS_cc(graph, adj, id, counter);
    }
}
```
- Time complexity: $O(m + n)$ (_n_ è il numero di nodi, _m_ il numero di archi)
- Space complexity: $O(n)$

[^1]: dove  $m = \sum_{u \in V} \ d_{out}(u)$ ($d_{out}$ è l’out-degree del nodo $u$)

</div></div>

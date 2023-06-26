---
{"dg-publish":true,"permalink":"/coding/data-structures/trees-in-italian/","created":"2023-01-24T23:34:58.410+01:00","updated":"2023-01-25T00:09:27.359+01:00"}
---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/university-notes-in-italian/algoritmi-e-strutture-dati/3-1-alberi/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




# Alberi
## Indice
- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#📝 Definizione 1\|📝 Definizione 1]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#📝 Definizione 2\|📝 Definizione 2]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#📚 Terminologia\|📚 Terminologia]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Profondità di un nodo (depth)\|Profondità di un nodo (depth)]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Livello (level)\|Livello (level)]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Altezza albero (height)\|Altezza albero (height)]]
- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#⌨️ Implementazione\|⌨️ Implementazione]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Realizzazione con vettore dei figli\|Realizzazione con vettore dei figli]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Realizzazione primo figlio, prossimo fratello\|Realizzazione primo figlio, prossimo fratello]]
	- [[👨🏼‍💻 Coding/🏗 Data Structures/Trees (in italian)#Realizzazione con vettore dei padri\|Realizzazione con vettore dei padri]]
## 📝 Definizione 1 
Un albero consiste di un insieme di nodi e un insieme di archi orientati che connettono coppie di nodi, con le seguenti proprietà: 
- Un nodo dell’albero è designato come nodo radice
- Ogni nodo n, a parte la radice, ha esattamente un arco entrante
- Esiste un cammino unico dalla radice ad ogni nodo
- L’albero è connesso

## 📝 Definizione 2
Un albero è dato da: 
- Un insieme vuoto, oppure 
- Un nodo radice e zero o più sottoalberi, ognuno dei quali è un albero; la radice è connessa alla radice di ogni sottoalbero con un arco orientato

## 📚 Terminologia
### Profondità di un nodo (depth)
La lunghezza del percorso dalla radice al nodo (cioè il numero di archi tra la radice e il nodo).

### Livello (level) 
L’insieme di nodi alla stessa profondità.

### Altezza albero (height) 
La profondità massima della sue foglie.

## ⌨️ Implementazione
Esistono diversi modi per memorizzare un albero, più o meno indicati a seconda del numero massimo e medio di figli presenti.

### Realizzazione con vettore dei figli 
![ImplVettoreFigli.png|500](/img/user/%F0%9F%8E%93%20University%20notes%20(in%20Italian)/%E2%9A%99%EF%B8%8F%20Algoritmi%20e%20Strutture%20Dati/_images/ImplVettoreFigli.png)

**Campi memorizzati nei nodi:** 
- _parent_: reference al nodo padre 
- _vettore dei figli_: a seconda del numero di figli, può comportare una discreta quantità di spazio sprecato
### Realizzazione primo figlio, prossimo fratello 
Implementato come una lista di fratelli

![ImplListaFigli.png|500](/img/user/%F0%9F%8E%93%20University%20notes%20(in%20Italian)/%E2%9A%99%EF%B8%8F%20Algoritmi%20e%20Strutture%20Dati/_images/ImplListaFigli.png)
### Realizzazione con vettore dei padri
L’albero è rappresentato da un vettore i cui elementi contengono il valore associato al nodo e l’indice della posizione del padre nel vettore.

![ImplVettorePadri.png|500](/img/user/%F0%9F%8E%93%20University%20notes%20(in%20Italian)/%E2%9A%99%EF%B8%8F%20Algoritmi%20e%20Strutture%20Dati/_images/ImplVettorePadri.png)

Per ogni nodo in posizione $i$ nel vettore:
- Il figlio sinistro si trova nella posizione $2 \cdot i + 1$
- Il figlio destro si trova nella posizione $2 \cdot i + 2$

</div></div>

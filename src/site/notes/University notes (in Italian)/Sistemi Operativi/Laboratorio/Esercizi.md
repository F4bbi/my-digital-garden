---
{"dg-publish":true,"permalink":"/university-notes-in-italian/sistemi-operativi/laboratorio/esercizi/","created":"2023-05-30T08:26:43.425+02:00","updated":"2023-06-24T14:45:11.332+02:00"}
---

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Laboratorio/1. Terminale & Bash\|1. Terminale & Bash]]
## Esercizio 1
Scrivere delle sequenze di comandi (singola riga da eseguire tutta in blocco) che utilizzano come “input” il valore della variabile DATA per:
1. Stampa “T” (per True) o “F” (per False) a seconda che il valore rappresenti un ﬁle o cartella esistente
2. Stampa “file”, “cartella” o “?” a seconda che il valore rappresenti un ﬁle (esistente), una cartella (esistente) o una voce non presente nel ﬁle-system
3. Stampa il risultato di una semplice operazione aritmetica (es: ‘1 < 2’) contenuta nel ﬁle indicato dal valore di DATA, oppure “?” se il ﬁle non esiste

### Per assegnare un valore alla variabile DATA, il comando bash da eseguire è:
```bash
DATA=nomefile.cpp #Per assegnare alla variabile data il nome di un file esistente (o tutto il percorso)
DATA=mydir #Per assegnare alla variabile data il nome di una cartella esistente (o tutto il percorso)
DATA=$((1>2)) #Per assegnare il risultato di un'operazione aritmetica
```

### Per assegnare un valore alla variabile DATA tramite uno script bisogna inserire il codice seguente all'interno di un file (in questo caso il file si chiama script.sh):
```bash
#!/bin/bash
DATA=prova.cpp
export DATA
```
Per rendere la variabile DATA disponibile nella shell corrente bisogna eseguire lo script con il comando 
```bash
source script.sh
```
Se utilizziamo il comando
```bash
chmod +x ./script.sh
```
Ci permetterà di eseguire lo script utilizzando il comando
```bash
./script.sh
```

### Risposta 1
```bash
[[ -e $DATA ]] && echo T || echo F
```

### Risposta 2
```bash
[[ -f $DATA ]] && echo "file" || ([[ -d $DATA ]] && echo "cartella" || echo "?")
```

### Risposta 3
```bash
[[ -f $DATA ]] && echo $(( $(cat $DATA) )) || echo "?" #cat $DATA stamperebbe il contenuto del file indicato dalla variabile DATA. Utilizzando il comando all'interno della subshell $(cat $DATA) viene restituito il contenuto del file indicato dal valore di DATA come argomento del comando echo
```

## Esercizio 2
1. Scrivere uno script che dato un qualunque numero di argomenti li restituisca in output in ordine inverso.
2. Scrivere uno script che mostri il contenuto della cartella corrente in ordine inverso rispetto all’output generato da “ls” (che si può usare ma senza opzioni). Per semplicità, assumere che tutti i ﬁle e le cartelle non abbiano spazi nel nome.

### Risposta 1
```bash
#!/usr/bin/env bash
nargs=$#
for (( i=nargs; i>0; i-- )); do
	echo ${!i} # stampa il valore dell'argomento corrente
done
```

### Risposta 2
```bash
#!/bin/bash
ls | awk '{a[i++]=$0} END {for (j=i-1; j>=0;) print a[j--] }' #’output di ls viene instradato nel comando awk che legge ogni riga (cioè ogni file) e la memorizza in un array. Il blocco END stampa l'array in ordine inverso
```

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Laboratorio/2. GCC & Make\|2. GCC & Make]]
## Esercizio 1
Creare un makeﬁle con una regola help di default che mostri una nota informativa, una regola backup che crei un backup di una cartella appendendo “.bak” al nome e una restore che ripristini il contenuto originale. Per deﬁnire la cartella sorgente passarne il nome come variabile, ad esempio:

make -f mf-backup FOLDER=...

(la variabile FOLDER è disponibile dentro il makeﬁle)

### Risposta 1
```bash
FOLDER = #Definisce una variabile vuota che serve per specifiare da linea di comando la directory

.PHONY: help backup restore #Specifica che questi target non sono nomi di file ma sono struzioni associate a questi target devono essere sempre eseguite quando si specifica il target, indipendentemente dall’esistenza di un file con lo stesso nome

help: #Definisce il target, in questo caso è il primo
	@echo "Uso: make [comando]"
	@echo "Comandi disponibili:"
	@echo "  help    - mostra questa nota informativa"
	@echo "  backup  - crea un backup della cartella specificata"
	@echo "  restore - ripristina il contenuto originale della cartella specificata"

backup: #Secondo target
	@if [ -z "$(FOLDER)" ]; then \
		echo "Errore: la variabile FOLDER non è stata specificata"; \
	else \
		tar -czf $(FOLDER).bak $(FOLDER); \
	fi

restore: #Terzo target
	@if [ -z "$(FOLDER)" ]; then \
		echo "Errore: la variabile FOLDER non è stata specificata"; \
	else \
		tar -xzf $(FOLDER).bak; \
	fi
```

- -z è un'opzione del comando di test \[\] che verifica se la lunghezza della stringa fornita come argomento è zero. In questo caso, l’argomento fornito è \$(FOLDER), che rappresenta il valore della variabile FOLDER. Quindi, \[ -z "\$(FOLDER)" \] restituisce vero se la variabile FOLDER è vuota e falso altrimenti.
- tar è un comando che crea un archivio compresso della cartella specificata dalla variabile FOLDER. Le opzioni utilizzate sono:
	- **-c**: indica a tar di creare un nuovo archivio
	- **-x**: indica a tar di estrarre i file dall'archivio
	- **-z**: indica a tar di comprimere (o decomprimere nel caso dell'estrazione) l'archivio utilizzando il programma gzip
	- **-f**: specifica il nome del file dell’archivio da creare o da gestire. Specifica anche il nome del file dell'archivio da cui estrarre i file nel caso si stia estraendo. Quando si utilizza questa opzione, il nome del file deve essere fornito come argomento successivo all’opzione -f. In questo caso $(FOLDER).bak specifica il nome del file dell'archivio da creare, mentre il secondo $(FOLDER) specifica quale cartella deve essere compressa e archiviata nel file (che in questo caso si chiamerà $(FOLDER).bak, odve $(FOLDER) sarà il nome passato da linea di comando.

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Laboratorio/3. C\|3. C]]
## Esercizio 1
1. Scrivere un’applicazione che data una stringa come argomento ne stampa a video la lunghezza, ad esempio:
		./lengthof "Questa frase ha 28 caratteri" deve restituire a video il numero 28.
2. Scrivere un’applicazione che deﬁnisce una lista di argomenti validi e legge quelli passati alla chiamata veriﬁcandoli e memorizzando le opzioni corrette, restituendo un errore in caso di un’opzione non valida.
3. Realizzare funzioni per stringhe char \*stringrev(char \* str) (inverte ordine caratteri) e int stringpos(char \* str, char chr) (cerca chr in str e restituisce la posizione)

### Soluzione 1
```c
#include <stdio.h>
#include <string.h>

int main(int argc, char * argv[])
{
	int len=0;
	for(int i=1; i<argc; i++)
	{
		len += strlen(argv[i]);
	}
	printf("%d\n", len);
	
	return 0;
}
```

### Soluzione 2
```c
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) 
{
    const char *valid_args[] = {"arg1", "arg2", "arg3"};
    int num_valid_args = sizeof(valid_args) / sizeof(valid_args[0]);
    int valid_options[argc];
    int num_valid_options = 0;

    for (int i = 1; i < argc; i++) 
	{
        int is_valid = 0;
        for (int j = 0; j < num_valid_args; j++) 
		{
            if (strcmp(argv[i], valid_args[j]) == 0) 
			{
                is_valid = 1;
                break;
            }
        }
        if (is_valid) 
		{
            valid_options[num_valid_options++] = i;
        } else {
            printf("Errore: opzione non valida: %s\n", argv[i]);
            return 1;
        }
    }

    printf("Opzioni valide: ");
    for (int i = 0; i < num_valid_options; i++) 
	{
        printf("%s ", argv[valid_options[i]]);
    }
    printf("\n");

    return 0;
}
```

### Soluzione 3
```c
#include <stdio.h>
#include <string.h>

char *stringrev(char *str)
{
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++)
	{
        char temp = str[i];
        str[i] = str[len - i - 1];
        str[len - i - 1] = temp;
    }
    return str;
}

int stringpos(char *str, char chr) {
    for (int i = 0; str[i] != '\0'; i++)
	{
        if (str[i] == chr)
		{
            return i;
        }
    }
    return -1;
}

int main()
{
    char str[] = "hello";
    printf("Original string: %s\n", str);
    printf("Reversed string: %s\n", stringrev(str));
    printf("Position of 'l' in string: %d\n", stringpos(str, 'l'));
    return 0;
}
```

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Teoria/14. Sistema di I-O\|14. Sistema di I-O]]
## Esercizio 1
Crea un’applicazione che copia il contenuto di un ﬁle, leggendolo con i ﬁle streams e scrivendolo con i ﬁle descriptors. Crea poi un programma che fa il contrario. Prova a copiare carattere per carattere e linea per linea.

### Soluzione 1
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>

int main(int argc, char *argv[])
{
    if (argc != 3)
	{
        printf("Uso: %s <file_input> <file_output>\n", argv[0]);
        return 1;
    }

    FILE *input = fopen(argv[1], "r");
    if (input == NULL) 
	{
        perror("Errore nell'apertura del file di input");
        return 1;
    }

    int output = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC , S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH);
    if (output == -1)
	{
        perror("Errore nell'apertura del file di output");
        fclose(input);
        return 1;
    }

    char buffer[BUFSIZ]; //BUFSIZ e' una costante predefinita che rappresenta la dimensione del buffer. La sua dimensione esatta dipende dal sistema, ma di solito è abbastanza grande 
    size_t nread; // Dichiarazione della variabile nread per memorizzare il numero di byte letti dalla funzione fread
    while ((nread = fread(buffer, 1, sizeof(buffer), input)) > 0) // Lettura dei dati dal file di input finché ci sono dati da leggere
	{
        if (write(output, buffer, nread) == -1) // Scrittura dei dati letti nel file di output e controllo se la scrittura è andata a buon fine
		{
            perror("Errore nella scrittura del file di output");
            fclose(input);
            close(output);
            return 1;
        }
    }

    fclose(input);
    close(output);

    return 0;
}
```

### Soluzione 2
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>

int main(int argc, char *argv[]) 
{
    if (argc != 3) 
	{
        printf("Uso: %s <file_input> <file_output>\n", argv[0]);
        return 1;
    }

    int input = open(argv[1], O_RDONLY);
    if (input == -1) 
	{
        perror("Errore nell'apertura del file di input");
        return 1;
    }

    FILE *output = fopen(argv[2], "w");
    if (output == NULL)
	{
        perror("Errore nell'apertura del file di output");
        close(input);
        return 1;
    }

    char buffer[BUFSIZ];
    ssize_t nread;
    while ((nread = read(input, buffer, sizeof(buffer))) > 0) 
	{
        if (fwrite(buffer, 1, nread, output) != nread) 
		{
            perror("Errore nella scrittura del file di output");
            close(input);
            fclose(output);
            return 1;
        }
    }

    close(input);
    fclose(output);

    return 0;
}
```

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Laboratorio/5. Kernel & System Calls\|5. Kernel & System Calls]]
## Esercizio 1
1. Avendo come argomenti dei “binari”, si eseguono con exec ciascuno in un sottoprocesso
2. idem punto 1 ma in più salvando i ﬂussi di stdout e stderr in un unico ﬁle
3. Dati due eseguibili come argomenti del tipo ls e wc si eseguono in due processi distinti: il primo deve generare uno stdout redirezionato su un ﬁle temporaneo, mentre il secondo deve essere lanciato solo quando il primo ha ﬁnito leggendo lo stesso ﬁle come stdin. Ad esempio ./main ls wc deve avere lo stesso effetto di ls | wc.

### Risposta 1
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) 
{
    if (argc < 2) 
	{
        printf("Inserire almeno un binario come argomento.\n");
        return 1;
    }

    for (int i = 1; i < argc; i++)
	{
        pid_t pid = fork(); //Per ogni argomento viene creato un processo figlio
        if (pid == -1)
		{
            perror("fork");
            exit(1);
        } else if (pid == 0) //SE la chiamata a fork ha successo il codice viene eseguito dal processo figlio
		{
            execl(argv[i], argv[i], NULL); //Esegue il programma specificato come primo argomento, mentre come secondo argomento è specificato il primo argomento da passare al programma che viene eseguito. In genere, il primo argomento passato a un programma è il nome del programma stesso.
            perror("execl");
            exit(1);
        }
    }

    for (int i = 1; i < argc; i++)
	{
        wait(NULL);
    }

    return 0;
}
```

### Risposta 2
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/wait.h>
#include <sys/stat.h>

int main(int argc, char *argv[]) 
{
	int outfile = open("output.txt", O_RDWR | O_CREAT, S_IRUSR | S_IWUSR);
	dup2(outfile, 1);
	dup2(outfile, 2);

    if (argc < 2) 
	{
        printf("Inserire almeno un binario come argomento.\n");
        return 1;
    }

    for (int i = 1; i < argc; i++)
	{
        pid_t pid = fork();
        if (pid == -1)
		{
            perror("fork");
            exit(1);
        } else if (pid == 0)
		{
            execl(argv[i], argv[i], NULL); //Esegue il programma specificato come primo argomento, mentre come secondo argomento è specificato il primo argomento da passare al programma che viene eseguito. In genere, il primo argomento passato a un programma è il nome del programma stesso.
            perror("execl");
            exit(1);
        }
    }

    for (int i = 1; i < argc; i++)
	{
        wait(NULL);
    }

    return 0;
}
```

### Risposta 3
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/wait.h>

#define TEMP_FILE_NAME ".temp"
#define BUF_SIZE 1024

int main(int argc, char **argv)
{
    if (argc != 3)
    {
        printf("Wrong usage, needs 2 arguments, eg. ls wc\n");
    }
    else
    {
        int temp_file = open(TEMP_FILE_NAME, O_CREAT | O_RDWR | O_TRUNC, S_IRUSR | S_IWUSR); //Il programma apre (o crea se non esiste) un file temporaneo con il nome specificato nella costante TEMP_FILE_NAME in modalità lettura/scrittura e tronca il suo contenuto a zero byte
        if (fork() == 0) //Parte eseguita dal figlio
        {
            dup2(temp_file, STDOUT_FILENO); //Tutto ciò che viene scritto sullo standard output verrà invece scritto sul file temporaneo
            execlp(argv[1], argv[1], NULL); //Cerca il file eseguibile che può essere il comando ls ad esempio, oppure gli si passa un eseguibile personale
        }
        else //Parte eseguita dal padre
        {
            int status;
            wait(&status); //Attende che termini il processo figlio (nel caso di più figli attende la terminazione di uno qualsiasi)
            dup2(temp_file, STDIN_FILENO); //Tutto ciò che viene letto dallo standard input verrà invece letto dal dal temporaneo
            lseek(STDIN_FILENO, 0, SEEK_SET); //Imposta il puntatore di lettura/scrittura, in questo caso le successive operazioni di lettura dallo standard input leggeranno i dati dall’inizio del file associato al descrittore di file standard input
            if (fork() == 0) //Crea un altro processo figlio
            {
                execlp(argv[2], argv[2], NULL); //ESegue il secondo comando specificato come secondo argomento sulla riga di comando
            }
            else
            {
                wait(&status); //Attende che termini il nuovo processo figlio
                remove(TEMP_FILE_NAME); //Rimuove il file specificato (in questo caso quello temporaneo)
            }
        }
    }
    return 0;
}
```

# Esercizi riguardanti [[8. Segnali\|8. Segnali]]
## Esercizio 1
Impostare una comunicazione bidirezionale tra due processi con due livelli di complessità:
- Alternando almeno due scambi (P1 → P2, P2 → P1, P1 → P2, P2 → P1)
- Estendendo il caso a mo’ di “ping-pong”, ﬁno a un messaggio convenzionale di "ﬁne comunicazione"

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h> 
#include <string.h>
#define READ 0 
#define WRITE 1 //bi.c
int main()
{
    int pipe1[2], pipe2[2]; char buf[50];
    pipe(pipe1); pipe(pipe2); //Crea due pipe senza nome
    int p2 = !fork(); //p2 vale 1 nel processo figlio e 0 nel processo padre
    if(p2)
    {
        close(pipe1[WRITE]); close(pipe2[READ]); //Il processo figlio P2 chiude il lato di scrittura di pipe1 e il lato lettura di pipe2
        while(1) {
            int r = read(pipe1[READ],&buf,50); //Legge un messaggio da pipe1
            printf("P2 received: '%s'\n",buf); //Stampa il messaggio ricevuto
            if(strcmp(buf, "end") == 0) break; //Se il messaggio è "end", interrompe il ciclo
            write(pipe2[WRITE],"Msg from p2",12); //Scrive un messaggio su pipe2
        }
        close(pipe1[READ]); close(pipe2[WRITE]);
    }else
    {
        close(pipe1[READ]); close(pipe2[WRITE]); //Il processo padre P1 chiude il lato di lettura di pipe1 e il lato scrittura di pipe2
        int count = 0;
        while(1) {
            if(count < 2) {
                write(pipe1[WRITE],"Msg from p1",12); //Scrive un messaggio su pipe1
            } else {
                write(pipe1[WRITE],"end",4); //Scrive "end" su pipe1
            }
            int r = read(pipe2[READ],&buf,50); //Legge un messaggio da pipe2
            printf("P1 received: '%s'\n",buf); //Stampa il messaggio ricevuto
            if(strcmp(buf, "end") == 0) break; //Se il messaggio è "end", interrompe il ciclo
            count++;
        }
        close(pipe1[WRITE]); close(pipe2[READ]);
    }
    while(wait(NULL)>0);
}
```

## Esercizio 2
Creare un programma che prenda come argomento 'n' il numero di ﬁgli da generare. Ogni ﬁglio creato comunicherà al genitore (tramite pipe) un numero casuale e il genitore calcolerà la somma di tutti questi numeri, inviandola a stdout.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h> 
#include <string.h>
#include <stdlib.h>
#include <time.h>

#define READ 0 
#define WRITE 1
#define RANDMAX 10

int main(int argc, char **argv)
{
    int rnd=0;

    if (argc != 2) //Controlli nel caso sia necessario specificare gli argomenti
    {
        printf("Sintassi: %s n\n", argv[0]);
        return 1;
    }
    else
    {
        int pipe1[2];
        char buf[50];
        pipe(pipe1);

        int n = atoi(argv[1]);

        for(int i=0; i<n; i++)
        {
            pid_t pid = fork();
            if (pid == -1)
            {
                perror("fork");
            } else if (pid == 0) //Parte eseguita dal figlio
            {
                close(pipe1[READ]); //Il processo figlio P2 chiude il lato lettura di pipe1 perché deve solo scrivere
                srand(time(NULL)+i); //Inizializza il generatore di numeri casuali 
                rnd = rand() % RANDMAX;
                sprintf(buf, "%d", rnd); //Scrive i dati formattati in una stringa. In questo caso scrive il valore della variabile rnd nella stringa buf utilizzando il formato %d, che indica che il valore deve essere formattato come un intero decimale. La funzione restituisce il numero di caratteri scritti nella stringa di destinazione, escluso il carattere nullo finale.
                write(pipe1[WRITE], buf, strlen(buf) + 1);
                exit(0);
            }
        }
        close(pipe1[WRITE]); //Il processo padre P1 chiude il lato di scrittura di pipe1 perché deve solo leggere
        int sum = 0;
        int value;
        while(read(pipe1[READ], buf, sizeof(buf)) > 0) //Legge un messaggio da pipe2
        {
            value = atoi(buf);
            printf("P1 received: '%d'\n", value);
            sum += value;
        }
        printf("Sum: '%d'\n", sum); //Stampa il messaggio ricevuto

        close(pipe1[READ]);

        while(wait(NULL)>0);
    }
    
}
```

# Esercizi riguardanti [[University notes (in Italian)/Sistemi Operativi/Teoria/9. Gestione della memoria\|9. Gestione della memoria]]
## Esercizio 1
Implementare in C una coda di messaggi basata sulla priorità, che consenta di aggiungere messaggi con livelli di priorità diversi e di eliminarli in base al loro livello di priorità.

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>
#include <errno.h>

struct msg_buffer
{
	long mtype;
	char mtext[100];
} msg_snd, msg_rcv;

int main()
{
    remove("/tmp/unique"); //Rimuove il file chiamato /tmp/unique se esiste
    creat("/tmp/unique", 0777); //Crea il file (0777 -> S_IRWXU |	S_IRWXG | S_IRWXO)
    key_t queue1Key = ftok("/tmp/unique", 1); //Prova ad ottenere una chiave univoca. Dato che la chiamata ha successo viene restituita una chiave univoca
    int queueId = msgget(queue1Key , 0777 | IPC_CREAT); //Crea una coda di messaggi con la chiave generata
    char scelta=' ';
    char temp=' ';

    do
    {
        fflush(NULL);
        printf("Selezionare l'operazione che si vuole eseguire:\ns - Aggiungere un nuovo messaggio alla coda\nr - Rimuovere un nuovo messaggio dalla coda\nq - Uscire dal programma\n\nInput: ");
        scanf(" %c", &scelta);

        switch(scelta)
        {
            case 's':
                printf("\nInserire il messaggio da inviare: ");
                scanf("%c", &temp);
                scanf("%[^\n]", msg_snd.mtext);

                printf("Inserire la priorità del messaggio: ");
                scanf("%ld", &msg_snd.mtype);

                if(msgsnd(queueId, &msg_snd, sizeof(msg_snd.mtext), 0) == -1)
                {
                    perror("Errore nell'invio del messaggio");
                    exit(EXIT_FAILURE);
                } else {
                    printf("\nInvio il messaggio alla coda: %s\nPriorità del messaggio: %ld\n\n", msg_snd.mtext, msg_snd.mtype);
                }
                break;
            
            case 'r':
                printf("\nInserire la priorità del messaggio che si vuole rimuovere: ");
                scanf("%ld", &msg_rcv.mtype);
                if(msgrcv(queueId, &msg_rcv, sizeof(msg_snd.mtext), msg_rcv.mtype, IPC_NOWAIT) == -1)
                {
                    if (errno == ENOMSG) 
                    {
                        printf("\nNessun messaggio disponibile nella coda con priorità %ld\n\n", msg_rcv.mtype);
                    } else {
                        perror("Errore durante la ricezione del messaggio");
                    }
                } else {
                    printf("\nIl messaggio %s, con priorità %ld, è stato eliminato dalla coda\n\n", msg_rcv.mtext, msg_rcv.mtype);
                }
                break;
            
            case 'q':
                break;
            
            default:
                printf("Selezione non valida, riprovare\n");
                break;
        }
        
    }while(scelta != 'q');

    msgctl(queueId,IPC_RMID,NULL); //Rimuove correttamente una coda di messaggi
    return 0;
}
```


## Simulazione 1
```c
#include <stdio.h> //printf
#include <stdlib.h> //atoi
#include <sys/types.h> //key_t
#include <sys/ipc.h> //ftok
#include <fcntl.h> //creat e costanti simboliche
#include <sys/stat.h> //creat e costanti simboliche
#include <sys/msg.h> //msgget
#include <errno.h> //errno
#include <signal.h> //SIGUSR1 e SIGUSR2
#include <string.h> //strlen() strcmp

struct msg_buffer
{
    long mtype;
    char mtext[100];
} msg;

int createQueue(char *argv, int pid);
void putQueue(char **argv, int queueId, int pid);
void getQueue(int queueId, int pid);
void deleteQueue(char *argv, int pid);
void emptyQueue(int queueId, int pid);
int moveQueue(int queueId, int queue2Id, int pid);

int main(int argc, char **argv)
{
    if(argc < 4) //Controlli nel caso sia necessario specificare gli argomenti
    {
        printf("Sintassi: %s <name> <action> [<value>] <pid>\n", argv[0]);
        exit(1);
    }

    int pid = 0;

    if(strcmp(argv[2], "new") == 0)
    {
        pid = atoi(argv[3]);
        if(argc != 4)
        {
            printf("Sintassi: %s <name> new <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        { 
            int queueId = createQueue(argv[1], pid);
            printf("ID coda di messaggi: %d\n", queueId);
            kill(pid, SIGUSR1);
        }
    } 
    else if(strcmp(argv[2], "put") == 0)
    {
        if(argc != 5)
        {
            pid = atoi(argv[3]);
            printf("Sintassi: %s <name> put <value> <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        {
            pid = atoi(argv[4]);
            int queueId = createQueue(argv[1], pid);
            putQueue(argv, queueId, pid);
            kill(pid, SIGUSR1);
        }
    } 
    else if(strcmp(argv[2], "get") == 0)
    {
        pid = atoi(argv[3]);
        if(argc != 4)
        {
            printf("Sintassi: %s <name> get <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        {
            int queueId = createQueue(argv[1], pid);
            getQueue(queueId, pid);
            kill(pid, SIGUSR1);
        }
    } 
    else if(strcmp(argv[2], "del") == 0)
    {
        pid = atoi(argv[3]);
        if(argc != 4)
        {
            printf("Sintassi: %s <name> del <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        {
            deleteQueue(argv[1], pid);
            kill(pid, SIGUSR1);
        }
    } 
    else if(strcmp(argv[2], "emp") == 0)
    {
        pid = atoi(argv[3]);
        if(argc != 4)
        {
            printf("Sintassi: %s <name> emp <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        { 
            int queueId = createQueue(argv[1], pid);
            emptyQueue(queueId, pid);
            kill(pid, SIGUSR1);
        }
    } 
    else if(strcmp(argv[2], "mov") == 0)
    {
        if(argc != 5)
        {
            printf("Sintassi: %s <name> new <pid>\n", argv[0]);
            kill(pid, SIGUSR2);
            exit(1);
        }
        else
        { 
            pid = atoi(argv[4]);
            key_t queueKey = ftok(argv[1], 1);
            if (queueKey == -1) //Se il file non esiste o non è accessibile la funzione ftok resituirà -1
            {
                perror("ftok");
                kill(pid, SIGUSR2);
                exit(1);
            }
            else
            {
                int queueId = msgget(queueKey, 0777);
                if (queueId == -1) 
                {
                    perror("msgget");
                    kill(pid, SIGUSR2);
                    exit(1);
                }
                else
                {
                    int queue2Id = createQueue(argv[3], pid);
                    int count = moveQueue(queueId, queue2Id, pid);
                    deleteQueue(argv[1], pid);
                    printf("Messaggi spostati: %d\n", count);
                    kill(pid, SIGUSR1);
                }
            }
        }
    }

    return 0;
}

int createQueue(char *argv, int pid) //Crea una nuova coda se non esiste, altrimenti ritorna l'ID della coda esistente
{
    key_t queueKey = ftok(argv, 1);
    if (queueKey == -1) //Se il file non esiste o non è accessibile la funzione ftok resituirà -1
    {
        if(errno == 2) //No such file or directory
        {
            creat(argv, 0777);
            queueKey = ftok(argv, 1);
        }
        if (queueKey == -1)
        {
            perror("ftok");
            kill(pid, SIGUSR2);
            exit(1);
        }
    }

    int queueId = msgget(queueKey, 0777 | IPC_CREAT);
    if (queueId == -1) 
    {
        perror("msgget");
        kill(pid, SIGUSR2);
        exit(1);
    }

    return queueId;
}

void putQueue(char **argv, int queueId, int pid)
{
    strcpy(msg.mtext, argv[3]);
    msg.mtype = 1;
    if(msgsnd(queueId, &msg, sizeof(msg.mtext), 0) == -1)
    {
        perror("Errore nell'invio del messaggio");
        kill(pid, SIGUSR2);
        exit(1);
    } 
    else
    {
        kill(pid, SIGUSR1);
    }
}

void getQueue(int queueId, int pid)
{
    if(msgrcv(queueId, &msg, sizeof(msg.mtext), 0, IPC_NOWAIT) == -1) //IPC_NOWAIT fa fallire la syscall se non sono presenti messaggi
    {
        if(errno == ENOMSG)
        {
            printf("Nessun messaggio disponibile nella coda\n");
            kill(pid, SIGUSR1);
        } 
        else
        {
            perror("Errore durante la ricezione del messaggio");
            kill(pid, SIGUSR2);
            exit(1);
        }
    } 
    else
    {
        printf("Ricevuto il messaggio: %s\n", msg.mtext);
        kill(pid, SIGUSR1);
    }
}

void deleteQueue(char *argv, int pid)
{
    key_t queueKey = ftok(argv, 1);
    if (queueKey == -1) //Se il file non esiste o non è accessibile la funzione ftok resituirà -1
    {
        kill(pid, SIGUSR1);
    }
    else
    {
        int queueId = msgget(queueKey, 0777); //Non serve mettere il flag per crare una coda perché abbiamo già verificato che esiste
        if (queueId == -1) 
        {
            if(errno == ENOENT)
            {
                printf("Nessuna coda con quella chiave da cancellare\n");
                kill(pid, SIGUSR1);
            } 
            else
            {
                perror("Errore durante l'eliminazione della coda");
                kill(pid, SIGUSR2);
                exit(1);
            }
        }
        else
        {
            msgctl(queueId,IPC_RMID,NULL); //Cancella la coda
        }
    }
}

void emptyQueue(int queueId, int pid)
{
    while(msgrcv(queueId, &msg, sizeof(msg.mtext), 0, IPC_NOWAIT) != -1)
    {
        printf("Ricevuto il messaggio: %s\n", msg.mtext);
    }
    kill(pid, SIGUSR1);
}

int moveQueue(int queueId, int queue2Id, int pid)
{
    int count = 0;
    while(msgrcv(queueId, &msg, sizeof(msg.mtext), 0, IPC_NOWAIT) != -1)
    {
        msg.mtype = 1;
        if(msgsnd(queue2Id, &msg, sizeof(msg.mtext), 0) == -1)
        {
            perror("Errore nell'invio del messaggio");
            kill(pid, SIGUSR2);
            exit(1);
        } 
        else
        {
            printf("Spostato il messaggio: %s\n", msg.mtext);
            count++;
        }
    }
    return count;
}
```
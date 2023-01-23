---
{"dg-publish":true,"permalink":"/university-notes-in-italian/appunti-su-java/"}
---

# Appunti su Java
## Appunto 1)
> [!warning] IMPORTANTE
> Un metodo statico non può accedere ai campi di istanza della stessa classe.
```java
public class B {  
    int x = 2;  
    B(){ System.out.print("B");}   
    public static void main(String[] args) {  
        this.x++;  //<- Errore a compile time
    }  
}
```

---
## Appunto 2)
> [!warning] IMPORTANTE
Non si può fare il cast dall'istanza della superclasse ad una sua classe figlia.
```java
Koala koala = new Koala();  
Animale animale = (Animale) koala;
```
Questo si può fare.

---
```java
Animale animale = new Koala();  
Animale animale2 = (Animale) animale;
```
Questo si può fare.

---
```java
Animale animale = new Animale();  
Koala koala = (Koala) animale; //<- Errore a runtime
```
Questo **non** si può fare. È come fare Koala koala = new Animale().
_Exception in thread "main": Animale cannot be cast to class Koala_

## Appunto 3)
> [!warning] IMPORTANTE
Non può esserci più di una classe pubblica in uno stesso file.
```java
public class Test {  
    public static void main(String[] args) {
      
    }  
}  
class A {  
    String s;  
    A(String s) { this.s = s; }  
    public String m() { }  
}  
public class B extends A{  //<- Errore a compile time
    B(String s) { super(s); }  
    public String m() {}  
}
```
## Appunto 4)
> [!info] Un po' di ereditarietà
> Generalmente quando passo una classe ad una funzione, guardo se la super-classe contiene quel metodo con quella classe. Se sì a posto, altrimenti devo fare un up cast.
> Eccezione: se passo una classe del tipo SuperC c = new SubC() viene sempre fatto un up-cast.

#### Esempio 1
```java
public class Test {  
    public static void main(String[] args) {  
        A obj = new B();  
        D obj2 = new D();  
        obj.m(obj2);  
    }  
}  
class A {  
    void m(C c) { System.out.println("A constructor with C class"); }  
    void m(D d) { System.out.println("A constructor with D class"); }  
}  
class B extends A {  
    void m(C c) { System.out.println("B constructor with C"); }  
    void m(D c) { System.out.println("B constructor with D"); }  
}  
class C { }  
class D extends C { }
```
**Output:** B constructor with D

#### Esempio 2
```java
public class Test {  
    public static void main(String[] args) {  
        A obj = new B();  
        D obj2 = new D();  
        obj.m(obj2);  
    }  
}  
class A {  
    void m(C c) { System.out.println("A constructor with C class"); }    
}  
class B extends A {  
    void m(C c) { System.out.println("B constructor with C class"); }  
    void m(D c) { System.out.println("B constructor with D class"); }  
}  
class C { }  
class D extends C { }
```
**Output:** B constructor with C

#### Esempio 3
```java
public class Test {  
    public static void main(String[] args) {  
        A obj = new B();  
        C obj2 = new D();  
        obj.m(obj2);  
    }  
}  
class A {  
    void m(C c) { System.out.println("A constructor with C class"); }  
    void m(D c) { System.out.println("A constructor with D class"); }  
  
}  
class B extends A {  
    void m(C c) { System.out.println("B constructor with C class"); }  
    void m(D c) { System.out.println("B constructor with D class"); }  
}  
class C { }  
class D extends C { }
```
**Output:** B constructor with C

#### Bonus
```java
public class Test {  
    public static void main(String[] args) {  
        B obj = new B();  
        E obj2 = new E();  
        obj.m(obj2);  
    }  
}  
class A {  
    void m(C c) { System.out.println("A constructor with C class"); }  
    void m(D c) { System.out.println("A constructor with D class"); }  
}  
class B extends A {  
    void m(C c) { System.out.println("B constructor with C class"); }  
  
}  
class C { }  
class D extends C { }  
class E extends D { }
```
**Output:** A constructor with D class

## Appunto 5)
> [!info] Costruttori
> Generalmente quando inizializzo una sotto-classe, questa invoca automaticamente il costruttore di default della sua super-classe.
> Il problema quindi si presenta quando la super-classe non ha il costruttore di default. 
> In questo caso, nella sotto-classe si deve obbligatoriamente invocare uno dei costruttori disponibili della super-classe, utilizzando la chiamata super() con i parametri adeguati, altrimenti si ottiene un errore a compile time.
> Ultimo caso, se la super-classe non ha costruttori, la sotto-classe chiama automaticamente un costruttore di default vuoto della sua super-classe.

#### Esempio 1
```java
public class Test {  
    public static void main(String[] args) {  
        NamedPoint namedPoint = new NamedPoint("Canial");
        //namedPoint.x = 6  
        //namedPoint.y = 9  
        //namedPoint.name = "Canial"   
    }  
}  
class Point {  
    public int x;  
    public int y;  
  
    public Point() {  
        this.x = 6;  
        this.y = 9;  
    }  
}  
class NamedPoint extends Point {  
    String name;  
    public NamedPoint(String name) {  
        this.name = name;  
    }  
}
```

#### Esempio 2
```java
public class Test {  
    public static void main(String[] args) {  
        NamedPoint namedPoint = new NamedPoint(6,9,"Canial");  
    }  
}  
class Point {  
    public int x;  
    public int y;  
  
    public Point(int x, int y) {  
        this.x = x;  
        this.y = y;  
    }  
}  
class NamedPoint extends Point {  
    String name;  
    public NamedPoint(int x, int y, String name) {  
        super(x,y); //<- Se lo rimuoviamo otteniamo un errore a compile time  
        this.name = name;  
    }  
}
```
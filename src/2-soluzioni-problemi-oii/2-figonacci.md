# Numeri di Figonacci

[ois_figonacci](https://training.olinfo.it/task/ois_figonacci) · OIS 2014/15

La successione $G$ è definita dai valori iniziali $G_0 = -1$, $G_1 = 0$ e, per ogni $n \ge 2$, dalla relazione

$$G_n = \sum_{i=0}^{n-2} \left(G_{n-1} - G_i\right)$$

Dati due interi $N$ e $M$, si vuole calcolare $G_N$ modulo $M$: il risultato deve essere sempre un intero in $[0, M)$.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Due interi `N M` separati da uno spazio.

```text
5 9
```

::right::

**Output**

Un intero: il valore $G_N \bmod M$.

```text
6
```

::bottom::

> **Altri esempi** — `3 1000 → 3` (infatti $G_2 = 1$ e $G_3 = (1)\,(1-0) + 1 = 3$); `5 9 → 6` (infatti $G_5 = 33$ e $33 \bmod 9 = 6$).

---
layout: two-cols-header
---

# Spiegazione

Partiamo dalla definizione e isoliamo la somma $S = \sum_{i=0}^{n-2} G_i$:

::left::

$$G_n = (n-1)\,G_{n-1} - S$$

Applicando la stessa relazione a $n-1$:

$$G_{n-1} = (n-2)\,G_{n-2} - \sum_{i=0}^{n-3} G_i$$

da cui

$$\sum_{i=0}^{n-3} G_i = (n-2)\,G_{n-2} - G_{n-1}$$

::right::

Aggiungendo $G_{n-2}$ a entrambi i membri ottengo $S$:

$$S = (n-1)\,G_{n-2} - G_{n-1}$$

che, sostituito sopra, dà la ricorrenza finale a **due termini**:

$$G_n = n\,G_{n-1} - (n-1)\,G_{n-2}$$

**Complessità:** $O(N)$ tempo, $O(1)$ memoria.

::bottom::

> **Attenzione** — con i numeri negativi il resto di C++ non è quello matematico: `-12 % 9 == -3`, ma a noi serve `6`.

---
layout: default
---

# Codice

```cpp
inline int mod(int a, int b) {
  int r = a % b;
  return r + (r < 0) * b; // rende sempre il resto positivo
}

int enumera(int N, int M) {
  int a = -1; // G_0
  int b = 0;  // G_1

  for (int i = 2; i <= N; i++) {
    // G_i = (G_{i-1} - G_{i-2}) * (i - 1) + G_{i-1}
    int c = mod((b - a) * (i - 1) + b, M);
    a = b;
    b = c;
  }
  return b;
}
```

> Applicando il modulo a ogni passo i valori non crescono mai, quindi non c'è rischio di overflow.

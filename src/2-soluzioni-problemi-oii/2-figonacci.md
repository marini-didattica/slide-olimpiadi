---
layout: two-cols-header
---

# Numeri di Figonacci

[ois_figonacci](https://training.olinfo.it/task/ois_figonacci) · OIS 2014/15

La successione $G$ è definita da $G_0 = -1$, $G_1 = 0$ e, per ogni $n \ge 2$:

$$G_n = \sum_{i=0}^{n-2} \left(G_{n-1} - G_i\right)$$

Dati $N$ e $M$, stampare $G_N \bmod M$.

::left::

**Input** · due interi `N M`

**Output** · un intero, $G_N \bmod M$

::right::

**Esempio**

| Input   | Output |
| ------- | ------ |
| `5 9`   | `6`    |

> La sommatoria si "srotola" in una ricorrenza a **due termini**: bastano gli ultimi due valori.

---
layout: two-cols-header
---

# Srotoliamo la sommatoria

Partiamo dalla definizione e isoliamo la somma $S = \sum_{i=0}^{n-2} G_i$:

::left::

$$G_n = (n-1)\,G_{n-1} - S$$

Applichiamo la stessa relazione a $n-1$:

$$G_{n-1} = (n-2)\,G_{n-2} - \sum_{i=0}^{n-3} G_i$$

da cui

$$\sum_{i=0}^{n-3} G_i = (n-2)\,G_{n-2} - G_{n-1}$$

::right::

Aggiungendo $G_{n-2}$ a entrambi i membri ottengo $S$:

$$S = (n-1)\,G_{n-2} - G_{n-1}$$

che, sostituito sopra, dà la ricorrenza finale:

$$G_n = n\,G_{n-1} - (n-1)\,G_{n-2}$$

**Complessità:** $O(N)$ tempo, $O(1)$ memoria.

::bottom::

> **Attenzione** — con i numeri negativi il resto di C++ non è quello matematico: `-12 % 9 == -3`, ma ci serve `6`. Aggiungere $M$ quando il resto è negativo.

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

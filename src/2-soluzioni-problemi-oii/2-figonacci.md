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
layout: default
---

# Idea · il girotolo

La definizione di $G_i$ è una sommatoria; **srotoliamola** per vedere cosa contiene:

$$G_i = \sum_{j=0}^{i-2}\big(G_{i-1} - G_j\big) = (i-1)\,G_{i-1} - \sum_{j=0}^{i-2} G_j$$

Il termine $G_{i-1}$ compare $i-1$ volte, mentre ogni valore precedente viene sottratto una volta sola. Tutti questi valori finiscono così nel **"girotolo"** $\sum_{j=0}^{i-2} G_j$: un nastro che si allunga di un termine ad ogni passo.

<div style="display:flex; justify-content:center;">
  <svg viewBox="0 0 570 150" style="width:min(100%, 560px); height:auto;" role="img" aria-label="Nastro dei termini G; da G_{i-1} partono le frecce verso tutti i termini da sottrarre per calcolare G_i">
    <defs>
      <marker id="gir-arrow" viewBox="0 0 10 10" refX="8.5" refY="5" markerWidth="7" markerHeight="7" orient="auto">
        <path d="M0,0 L10,5 L0,10 Z" style="fill: var(--c-text-muted); stroke: none;" />
      </marker>
    </defs>
    <g style="fill:none; stroke: var(--c-text-muted); stroke-width:1.5;">
      <path d="M429,80 Q381,18 333,80" marker-end="url(#gir-arrow)" />
      <path d="M429,80 Q333,18 237,80" marker-end="url(#gir-arrow)" />
      <path d="M429,80 Q285,18 141,80" marker-end="url(#gir-arrow)" />
      <path d="M429,80 Q237,18 45,80" marker-end="url(#gir-arrow)" />
      <circle cx="429" cy="80" r="3" style="fill: var(--c-text-muted); stroke: none;" />
    </g>
    <g style="font-family: var(--slidev-code-font-family); font-size:20px; text-anchor:middle;">
      <rect x="0" y="80" width="90" height="45" rx="5" style="fill: var(--c-surface); stroke: var(--c-border-strong); stroke-width:1.5;" />
      <text x="45" y="110" style="fill: var(--c-text-strong);">G<tspan dy="5" style="font-size:12px">0</tspan></text>
      <rect x="96" y="80" width="90" height="45" rx="5" style="fill: var(--c-surface); stroke: var(--c-border-strong); stroke-width:1.5;" />
      <text x="141" y="110" style="fill: var(--c-text-strong);">G<tspan dy="5" style="font-size:12px">1</tspan></text>
      <rect x="192" y="80" width="90" height="45" rx="5" style="fill: var(--c-surface); stroke: var(--c-border-strong); stroke-width:1.5;" />
      <text x="237" y="108" style="fill: var(--c-text-muted);">⋯</text>
      <rect x="288" y="80" width="90" height="45" rx="5" style="fill: var(--c-surface); stroke: var(--c-border-strong); stroke-width:1.5;" />
      <text x="333" y="110" style="fill: var(--c-text-strong);">G<tspan dy="5" style="font-size:12px">i−2</tspan></text>
      <rect x="384" y="80" width="90" height="45" rx="5" style="fill: var(--c-surface); stroke: var(--c-border-strong); stroke-width:1.5;" />
      <text x="429" y="110" style="fill: var(--c-text-strong);">G<tspan dy="5" style="font-size:12px">i−1</tspan></text>
      <rect x="480" y="80" width="90" height="45" rx="5" style="fill: var(--slidev-theme-primary); stroke: var(--slidev-theme-primary); stroke-width:1.5;" />
      <text x="525" y="110" style="fill: var(--c-on-primary); font-weight:600;">G<tspan dy="5" style="font-size:12px">i</tspan></text>
    </g>
  </svg>
</div>


---
layout: default
---

# Spiegazione · isoliamo la somma

Dal girotolo sappiamo che

$$G_n = (n-1)\,G_{n-1} - S, \qquad S = \sum_{i=0}^{n-2} G_i$$

Vogliamo eliminare la somma $S$. Applichiamo la stessa relazione a $n-1$:

$$G_{n-1} = (n-2)\,G_{n-2} - \sum_{i=0}^{n-3} G_i$$

da cui possiamo isolare la somma fino a $n-3$:

$$\sum_{i=0}^{n-3} G_i = (n-2)\,G_{n-2} - G_{n-1}$$

---
layout: default
---

# Spiegazione · la ricorrenza

Aggiungendo $G_{n-2}$ a entrambi i membri, la somma precedente diventa $S$:

$$S = \sum_{i=0}^{n-2} G_i = (n-1)\,G_{n-2} - G_{n-1}$$

Sostituendola in $G_n = (n-1)\,G_{n-1} - S$:

$$G_n = (n-1)\,G_{n-1} - \big((n-1)\,G_{n-2} - G_{n-1}\big) = n\,G_{n-1} - (n-1)\,G_{n-2}$$

Abbiamo così una ricorrenza a **due termini**.

**Complessità:** $O(N)$ tempo, $O(1)$ memoria.

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

> **Attenzione** — in C++ `-12 % 9 == -3`: la funzione `mod` riporta sempre il resto in $[0, M)$ e, applicata a ogni passo, evita l'overflow.

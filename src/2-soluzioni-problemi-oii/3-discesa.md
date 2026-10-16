# Discesa massima

[discesa](https://training.olinfo.it/task/discesa)

Abbiamo un **triangolo di numeri** con $n$ righe:

```text
      1
     2 9
    3 7 5
   8 4 11 6
```

Una **discesa** parte dalla cella in cima e scende di riga in riga. Ad ogni passo, dalla cella `(i, j)` si può passare solo a una delle due celle adiacenti della riga successiva, cioè `(i+1, j)` oppure `(i+1, j+1)`.

Il **peso** di una discesa è la somma dei valori che incontra. Trovare il peso massimo possibile.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga l'intero $n$; nelle righe successive le righe del triangolo.

```text
4
1
2 9
3 7 5
8 4 11 6
```

::right::

**Output**

Un intero: il peso massimo di una discesa.

```text
28
```

::bottom::

> **Altri esempi** — con un triangolo di una sola cella (`1` / `5`) la discesa vale `5`; nel triangolo qui sopra la discesa migliore è $1 \to 9 \to 7 \to 11 = 28$.

---
layout: default
---

# Spiegazione

Salvo in `dp[i][j]` il **peso massimo di una discesa che termina** nella cella `(i, j)`.

Per arrivare in `(i, j)` posso venire solo da `(i-1, j-1)` o da `(i-1, j)`:

$$dp[i][j] = w[i][j] + \max\big(dp[i-1][j-1],\; dp[i-1][j]\big)$$

I due **bordi** del triangolo hanno un solo predecessore:

$$dp[i][0] = dp[i-1][0] + w[i][0], \qquad dp[i][i] = dp[i-1][i-1] + w[i][i]$$

La risposta è il massimo dell'ultima riga.

**Complessità:** $O(n^2)$ tempo; lo spazio può scendere a $O(n)$ tenendo una sola riga.

---
layout: two-cols-header
clicks: 10
---

# Esempio · riempiamo `dp`

A ogni clic si calcola una cella: a **sinistra** si evidenzia la cella di `w` in esame, a **destra** `dp` si riempie man mano.

::left::

**Triangolo `w`**

<div class="ds-stack">
  <div v-for="(row, i) in [[1], [2, 9], [3, 7, 5], [8, 4, 11, 6]]" :key="i" class="ds-row" :style="{ width: (i + 1) * 25 + '%', gridTemplateColumns: 'repeat(' + (i + 1) + ', minmax(0, 1fr))' }">
    <div v-for="(val, j) in row" :key="j" class="ds-cell" :class="{ 'ds-cell--cur': $clicks === 1 + i * (i + 1) / 2 + j }">{{ val }}</div>
  </div>
</div>

::right::

**Triangolo `dp`**

<div class="ds-stack">
  <div v-for="(row, i) in [[1], [3, 10], [6, 17, 15], [14, 21, 28, 21]]" :key="i" class="ds-row" :style="{ width: (i + 1) * 25 + '%', gridTemplateColumns: 'repeat(' + (i + 1) + ', minmax(0, 1fr))' }">
    <div
      v-for="(val, j) in row"
      :key="j"
      class="ds-cell"
      :class="{
        'ds-cell--cur': $clicks === 1 + i * (i + 1) / 2 + j,
        'ds-cell--above': i < 3 && ($clicks === 1 + (i + 1) * (i + 2) / 2 + j || $clicks === 2 + (i + 1) * (i + 2) / 2 + j),
      }"
    >{{ $clicks >= 1 + i * (i + 1) / 2 + j ? val : '/' }}</div>
  </div>
</div>

::bottom::

> Clic per avanzare: la cella corrente (in **blu**) è calcolata dalla cella di `w` in esame e dalle due celle qui **sopra** (in azzurro) già pronte. Prima di essere calcolata ogni cella mostra `/`.

<style>
.ds-stack { display: flex; flex-direction: column; gap: 0.35rem; margin-top: 1rem; }
.ds-row { display: grid; margin: 0 auto; }
.ds-cell {
  padding: 0.45rem 0.5rem;
  border: 1px solid var(--c-border-strong);
  border-right: 0;
  background: var(--c-surface);
  color: var(--c-text-strong);
  font-family: var(--slidev-code-font-family);
  text-align: center;
  transition: background var(--tracker-transition, 220ms) ease, color var(--tracker-transition, 220ms) ease;
}
.ds-row .ds-cell:first-child { border-top-left-radius: var(--c-radius); border-bottom-left-radius: var(--c-radius); }
.ds-row .ds-cell:last-child { border-right: 1px solid var(--c-border-strong); border-top-right-radius: var(--c-radius); border-bottom-right-radius: var(--c-radius); }
.ds-cell--above { background: var(--tracker-active-bg, rgba(122, 162, 255, 0.14)); }
.ds-cell--cur { background: var(--slidev-theme-primary); color: var(--c-on-primary); }
</style>

---
layout: default
---

# Codice

```cpp
for (int i = 1; i < n; i++) {
  in >> tmp;
  dp[i][0] = dp[i - 1][0] + tmp; // bordo sinistro
  for (int j = 1; j < i; j++) {
    in >> tmp;
    dp[i][j] = tmp + max(dp[i - 1][j - 1], dp[i - 1][j]);
  }
  in >> tmp;
  dp[i][i] = dp[i - 1][i - 1] + tmp; // bordo destro
}

out << *max_element(dp[n - 1], dp[n - 1] + n) << endl;
```

> Le celle vengono calcolate riga per riga: quando arrivo alla riga `i`, tutte le celle della riga `i-1` sono già pronte.

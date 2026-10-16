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

A ogni clic si calcola una cella: a **sinistra** si evidenziano i due antenati in `w`, a **destra** si popola `dp`.

::left::

**Triangolo `w`**

<div style="display:flex; flex-direction:column; gap:0.35rem; margin-top:var(--flow);">
  <div style="width:25%; margin:0 auto;">
    <ListTracker :items="[1]" :trackers="{ p: { 2: 0, 3: 0, 4: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:50%; margin:0 auto;">
    <ListTracker :items="[2, 9]" :trackers="{ l: { 4: 0, 5: 0, 6: 1, 7: null }, r: { 4: null, 5: 1, 6: null, 7: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:75%; margin:0 auto;">
    <ListTracker :items="[3, 7, 5]" :trackers="{ l: { 7: 0, 8: 0, 9: 1, 10: 2, 11: null }, r: { 7: null, 8: 1, 9: 2, 10: null, 11: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:100%; margin:0 auto;">
    <ListTracker :items="[8, 4, 11, 6]" :trackers="{}" highlight :show-labels="false" :show-indexes="false" />
  </div>
</div>

::right::

**Triangolo `dp`**

<div style="display:flex; flex-direction:column; gap:0.35rem; margin-top:var(--flow);">
  <div style="width:25%; margin:0 auto;">
    <ListTracker :items="[1]" :trackers="{ a: { 1: 0 } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:50%; margin:0 auto;">
    <ListTracker :items="[3, 10]" :trackers="{ a: { 2: 0 }, b: { 3: 1 } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:75%; margin:0 auto;">
    <ListTracker :items="[6, 17, 15]" :trackers="{ a: { 4: 0 }, b: { 5: 1 }, c: { 6: 2 } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:100%; margin:0 auto;">
    <ListTracker :items="[14, 21, 28, 21]" :trackers="{ a: { 7: 0 }, b: { 8: 1 }, c: { 9: 2 }, d: { 10: 3 } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
</div>

::bottom::

> Clic per avanzare: `dp` si riempie riga per riga. La risposta è il massimo dell'ultima riga (`28`).

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

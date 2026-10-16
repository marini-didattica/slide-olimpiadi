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

> **Complessità:** $O(n^2)$ tempo; lo spazio può scendere a $O(n)$ tenendo una sola riga.

---
layout: two-cols-header
clicks: 10
---

# Esempio · riempiamo `dp`

A ogni clic si calcola una cella: a **sinistra** si evidenzia la cella di `w` in esame, a **destra** `dp` si riempie man mano.

::left::

**Triangolo `w`**

<div style="display:flex; flex-direction:column; gap:0.35rem; margin-top:1rem;">
  <div style="width:25%; margin:0 auto;">
    <ListTracker :items="[1]" :trackers="{ cur: { 1: 0, 2: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:50%; margin:0 auto;">
    <ListTracker :items="[2, 9]" :trackers="{ cur: { 2: 0, 3: 1, 4: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:75%; margin:0 auto;">
    <ListTracker :items="[3, 7, 5]" :trackers="{ cur: { 4: 0, 5: 1, 6: 2, 7: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:100%; margin:0 auto;">
    <ListTracker :items="[8, 4, 11, 6]" :trackers="{ cur: { 7: 0, 8: 1, 9: 2, 10: 3, 11: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
</div>

::right::

**Triangolo `dp`**

<div style="display:flex; flex-direction:column; gap:0.35rem; margin-top:1rem;">
  <div style="width:25%; margin:0 auto;">
    <ListTracker :items="['/']" :updates="{ 0: { 1: 1 } }" :trackers="{ aL: { 2: null, 3: 0, 4: null }, aR: { 2: 0, 3: null, 4: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:50%; margin:0 auto;">
    <ListTracker :items="['/', '/']" :updates="{ 0: { 2: 3 }, 1: { 3: 10 } }" :trackers="{ aL: { 4: null, 5: 0, 6: 1, 7: null }, aR: { 4: 0, 5: 1, 6: null, 7: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:75%; margin:0 auto;">
    <ListTracker :items="['/', '/', '/']" :updates="{ 0: { 4: 6 }, 1: { 5: 17 }, 2: { 6: 15 } }" :trackers="{ aL: { 7: null, 8: 0, 9: 1, 10: 2, 11: null }, aR: { 7: 0, 8: 1, 9: 2, 10: null, 11: null } }" highlight :show-labels="false" :show-indexes="false" />
  </div>
  <div style="width:100%; margin:0 auto;">
    <ListTracker :items="['/', '/', '/', '/']" :updates="{ 0: { 7: 14 }, 1: { 8: 21 }, 2: { 9: 28 }, 3: { 10: 21 } }" :trackers="{}" highlight :show-labels="false" :show-indexes="false" />
  </div>
</div>

::bottom::

> Clic per avanzare: la cella corrente di `dp` **lampeggia** mentre il suo valore (da `/` al numero) viene scritto; le due celle **sopra** da cui deriva restano evidenziate. A sinistra si evidenzia la cella di `w` in esame.

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

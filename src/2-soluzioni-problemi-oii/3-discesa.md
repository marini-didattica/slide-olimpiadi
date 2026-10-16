---
layout: two-cols-header
---

# Discesa massima

[discesa](https://training.olinfo.it/task/discesa)

Abbiamo un triangolo di numeri con $n$ righe. Una **discesa** parte dalla cima e scende di riga in riga, spostandosi ogni volta nella cella adiacente (in basso a sinistra o in basso a destra). Trovare il **peso massimo** di una discesa.

::left::

**Input**

Sulla prima riga $n$; nelle righe successive il triangolo.

```text
4
1
2 9
3 7 5
8 4 11 6
```

::right::

**Output**

Il peso massimo di una discesa: `28`.

$$1 \to 9 \to 7 \to 11 = 28$$

::bottom::

> **Nota bene** — è lo stesso problema della *torre di numeri*: la scelta "migliore" locale non basta, serve la DP.

---
layout: two-cols-header
---

# Idea

Salvo in `dp[i][j]` il **peso massimo di una discesa che termina** nella cella `(i, j)`.

Per arrivare in `(i, j)` posso venire solo da `(i-1, j-1)` o da `(i-1, j)`:

$$dp[i][j] = w[i][j] + \max\big(dp[i-1][j-1],\; dp[i-1][j]\big)$$

::left::

I due **bordi** hanno un solo predecessore:

$$dp[i][0] = dp[i-1][0] + w[i][0]$$
$$dp[i][i] = dp[i-1][i-1] + w[i][i]$$

La risposta è il massimo dell'ultima riga.

**Complessità:** $O(n^2)$ tempo; lo spazio può scendere a $O(n)$ tenendo una sola riga.

::right::

**Esempio**

```text
       1                1
      2 9    ----\     3 10
     3 7 5   ----/    6 17 15
    8 4 11 6         14 21 28 21
        w                  dp
```

L'ultima riga di `dp` ha massimo `28`.

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

---
layout: two-cols-header
---

# Spiedini di frutta

[oii_spiedini](https://training.olinfo.it/task/oii_spiedini) · OII 2012/13

Uno spiedino è una sequenza di $N$ bocconi. Il valore `0` indica una **fragola**, un valore positivo indica una **verdura** che provoca un dispiacere pari al suo valore. Si mangia lo spiedino **dalle due estremità**: il dispiacere totale dei bocconi mangiati non può superare $K$. Massimizzare il numero di **fragole** mangiate.

::left::

**Input**

Sulla prima riga `N K`; sulla successiva gli $N$ valori.

```text
11 9
1 0 2 8 0 5 1 6 0 0 3
```

::right::

**Output**

Il massimo numero di fragole: `3`.

Mangiando il prefisso `1 0 2` e il suffisso `0 0 3` ottengo 3 fragole con dispiacere $1+2+3 = 6 \le 9$.

::bottom::

> **Osservazione** — la parte *non* mangiata è sempre un **intervallo contiguo** centrale. Aumentare da sinistra può obbligare a rinunciare a destra: si gestisce con due puntatori.

---
layout: two-cols-header
---

# Idea · due puntatori

I due puntatori `l` e `r` delimitano i bocconi **ancora da mangiare**. Tengo traccia di fragole e dispiacere **già mangiati** (prefisso `[0, l)` + suffisso `(r, N-1]`).

::left::

1. Mangio da **sinistra** finché il dispiacere resta $\le K$.
2. Torno indietro fino all'ultima fragola: mangiare una verdura in più riduce le fragole possibili.
3. Completo mangiando da **destra**, finché il dispiacere lo permette.
4. Arretro di una fragola e riallargo da destra, aggiornando il massimo; ripeto finché `l` non esaurisce lo spiedino.

::right::

**Perché è lineare**

Ogni puntatore si muove **solo in avanti** (o `l` indietro solo quando "restituisco" bocconi già contati). In totale ogni boccone viene attraversato un numero costante di volte.

**Complessità:** $O(N)$ tempo, $O(1)$ memoria aggiuntiva.

::bottom::

> Non serve ricalcolare la somma da capo a ogni tentativo: aggiorno il dispiacere **incrementando** e **decrementando** i valori.

---
layout: default
---

# Codice · estrazione iniziale

```cpp
int solve(int N, int K, int* S) {
  int straw = 0, pain = 0, l = 0, r = N - 1;

  // 1. mangio da sinistra finché il dispiacere lo permette
  while (pain <= K && l < N) {
    if (S[l] == 0) straw++; else pain += S[l];
    l++;
  }
  if (l == N) return straw; // ho mangiato tutto lo spiedino

  // 2. torno indietro fino all'ultima fragola
  l--;
  while (l >= 0 && S[l] != 0) { pain -= S[l]; l--; }

  // 3. completo mangiando da destra
  while (pain <= K) {
    if (S[r] == 0) straw++; else pain += S[r];
    r--;
  }
```

---

# Codice · il passo "riprova"

```cpp
  int best = straw;
  // 4. arretro di una fragola e riallargo da destra
  while (l >= 0) {
    l--;
    while (l >= 0 && S[l] != 0) { pain -= S[l]; l--; }
    straw--;

    while (pain <= K) {
      if (S[r] == 0) straw++; else pain += S[r];
      r--;
    }
    if (straw > best) best = straw;
  }
  return best;
}
```

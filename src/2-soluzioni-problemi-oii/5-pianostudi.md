# Piano degli studi

[ois_pianostudi](https://training.olinfo.it/task/ois_pianostudi) · OIS 2014/15

Uno studente deve comporre il proprio piano di studi scegliendo alcuni dei corsi disponibili. Ci sono $N$ corsi: il corso $i$ si tiene nell'intervallo di tempo $[da_i, a_i]$ e, se scelto, frutta $crediti_i$ crediti.

Due corsi scelti **non possono sovrapporsi**: non è possibile seguire due corsi nello stesso istante, nemmeno su un singolo punto estremo.

Si vuole massimizzare il **totale dei crediti** ottenibili.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga $N$; seguono $N$ terne `da a crediti`.

```text
2
5 8 1
1 5 2
```

::right::

**Output**

Un intero: il massimo credito ottenibile.

```text
2
```

::bottom::

> **Altro esempio** — `1` / `0 10 7` dà `7`: con un solo corso conviene prenderlo. Nell'esempio principale i due corsi si toccano nell'istante $5$, quindi ne scelgo uno solo.

---
layout: two-cols-header
---

# Spiegazione

Ordino i corsi per **istante di fine** $a_i$ e uso `dp[i]` = massimo credito ottenibile usando i primi `i` corsi ordinati.

::left::

Per il corso `i` ho due opzioni:

- **non lo prendo:** `dp[i] = dp[i-1]`;
- **lo prendo:** ai suoi crediti sommo il migliore che termina **prima** che inizi. Detto `pred(i)` il numero di corsi con fine $< da_i$:

$$dp[i] = \max\Big(dp[i-1],\; crediti_i + dp\big(pred(i)\big)\Big)$$

::right::

**Trovare `pred(i)`**

Poiché i corsi sono ordinati per fine, gli istanti $a_j$ sono crescenti: una **ricerca binaria** (`lower_bound` su $da_i$) conta quanti corsi terminano strettamente prima dell'inizio di `i`.

**Complessità:** $O(N \log N)$ — dominata dall'ordinamento e dalle ricerche binarie.

::bottom::

> È il classico problema degli **intervalli pesati**: la scelta golosa (*"prendi sempre quello che finisce prima"*) non funziona, perché un corso molto redditizio può valere più di diversi corsi corti.

---
layout: default
---

# Codice

```cpp
struct Corso { int da, a, crediti; };

int pianifica(int N, int da[], int a[], int crediti[]) {
  vector<Corso> c(N);
  for (int i = 0; i < N; i++)
    c[i] = {da[i], a[i], crediti[i]};

  // ordino i corsi per istante di fine
  sort(c.begin(), c.end(),
       [](const Corso &x, const Corso &y) { return x.a < y.a; });

  vector<int> fine(N);
  for (int i = 0; i < N; i++)
    fine[i] = c[i].a;

  vector<int> dp(N + 1, 0); // dp[i] = top con i primi i corsi
  for (int i = 0; i < N; i++) {
    // corsi compatibili: quelli con fine < inizio del corso i
    int pred = lower_bound(fine.begin(), fine.begin() + i, c[i].da)
               - fine.begin();
    dp[i + 1] = max(dp[i], c[i].crediti + dp[pred]);
  }
  return dp[N];
}
```

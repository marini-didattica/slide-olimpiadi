---
layout: two-cols-header
---

# Piano degli studi

[ois_pianostudi](https://training.olinfo.it/task/ois_pianostudi) · OIS 2014/15

Ci sono $N$ corsi tra cui scegliere. Il corso $i$ si tiene nell'intervallo $[da_i, a_i]$ e vale $crediti_i$ crediti. Due corsi scelti **non possono sovrapporsi** (nemmeno su un solo istante). Massimizzare il totale dei crediti.

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

Il massimo credito ottenibile: `2`.

I due corsi si sovrappongono nell'istante $5$, quindi ne posso scegliere uno solo: il migliore vale $2$.

::bottom::

> È il classico problema degli **intervalli pesati**: la scelta golosa (*"prendi sempre quello che finisce prima"*) non funziona, perché un corso molto redditizio può valere più di diversi corsi corti.

---
layout: two-cols-header
---

# Idea · intervalli pesati

Ordino i corsi per **istante di fine** $a_i$. In `dp[i]` salvo il massimo credito ottenibile usando i primi `i` corsi ordinati.

::left::

Per il corso `i` ho due opzioni:

- **non lo prendo:** `dp[i] = dp[i-1]`;
- **lo prendo:** ai suoi crediti sommo il migliore che termina **prima** che inizi. Detto `pred(i)` il numero di corsi con fine $< da_i$:

$$dp[i] = crediti_i + dp\big(pred(i)\big)$$

$$dp[i] = \max\Big(dp[i-1],\; crediti_i + dp\big(pred(i)\big)\Big)$$

::right::

**Trovare `pred(i)`**

Poiché i corsi sono ordinati per fine, gli istanti $a_j$ sono crescenti: una **ricerca binaria** (`lower_bound` su $da_i$) trova quanti corsi terminano strettamente prima dell'inizio di `i`.

**Complessità:** $O(N \log N)$ — dominata dall'ordinamento e dalle ricerche binarie.

::bottom::

> Confronta con la teoria degli **intervalli pesati**: `dp[i]` dipende da un unico sottoproblema compatibile, non da tutti i precedenti.

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

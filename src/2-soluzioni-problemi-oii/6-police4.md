---
layout: two-cols-header
---

# Police 4

[ois_police4](https://training.olinfo.it/task/ois_police4) · OIS 2021/22

Come *Police 3*, ma ora il veicolo può **saltare al più $R$ semafori**. I semafori si trovano alle posizioni crescenti $X_0, \ldots, X_{N-1}$ e la destinazione è $L$. Ogni semaforo ha periodo $T$: chi vi arriva con `tempo % (2T) < T` passa, altrimenti attende che diventi verde. Trovare il **tempo minimo** di arrivo.

::left::

**Input**

Sulla prima riga `N R T L`; sulla successiva le posizioni $X_i$.

```text
3 1 3 10
1 5 9
```

::right::

**Output**

Il tempo minimo: `11`.

**Altro esempio**

```text
1 0 5 10     ->  15
5
```

::bottom::

> Rispetto a Police 3 il vincolo "due di fila" sparisce: al suo posto c'è un **budget di salti** $R$.

---
layout: two-cols-header
---

# Idea · DP a due dimensioni

`dp[i][j]` = tempo minimo per raggiungere il semaforo $i$ **avendo saltato al più $j$ semafori**.

::left::

Per arrivare a $i$ ho due opzioni:

- **saltare il semaforo $i-1$**, spendendo uno dei $j$ salti:
  $$t_1 = dp[i-1][j-1]$$
- **passare dal semaforo $i-1$** (eventualmente aspettando):
  $$t_2 = dp[i-1][j] + \text{attesa}$$

$$dp[i][j] = \min(t_1, t_2) + \big(X_i - X_{i-1}\big)$$

::right::

**L'attesa**

Se `dp[i-1][j] % (2T) < T` il semaforo è verde e l'attesa è `0`, altrimenti vale `T - dp[i-1][j] % T`.

**Casi base**

`dp[0][j] = X_0` per ogni $j$ (nessun salto ancora usato).
Risposta: `dp[N][R]` con $X_N = L$.

::bottom::

**Complessità:** $O(NR)$ tempo. In memoria bastano **due righe** → $O(R)$ spazio.

---
layout: default
---

# Codice

```cpp
for (int i = 0; i <= R; i++)
  dp[0][i] = X[0];

for (int i = 1; i <= N; i++) {
  for (int j = 0; j <= R; j++) {
    // Opzione 1: salto il semaforo i-1 (uso un salto)
    unsigned int t1 = (j == 0) ? UINT_MAX : dp[i - 1][j - 1];

    // Opzione 2: passo dal semaforo i-1, con eventuale attesa
    bool can_pass = dp[i - 1][j] % (2 * T) < T;
    unsigned int wait = can_pass ? 0 : T - dp[i - 1][j] % T;
    unsigned int t2 = dp[i - 1][j] + wait;

    dp[i][j] = min(t1, t2) + (X[i] - X[i - 1]);
  }
}

cout << dp[N][R] << endl; // X[N] è posto uguale a L
```

> `UINT_MAX` rappresenta "salto non disponibile": usato come neutro dentro il `min`.

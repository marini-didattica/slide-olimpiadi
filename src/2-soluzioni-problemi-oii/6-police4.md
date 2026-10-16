# Police 4

[ois_police4](https://training.olinfo.it/task/ois_police4) · OIS 2021/22

Come in *Police 3*, un veicolo della polizia deve percorrere una strada rettilinea su cui sono posti $N$ semafori, alle posizioni crescenti $X_0 < X_1 < \dots < X_{N-1}$. Il veicolo parte dalla posizione $0$ e deve raggiungere la destinazione $L$.

La differenza rispetto a Police 3 è che ora il veicolo dispone di un **budget di salti**: può attraversare senza fermarsi **al più $R$ semafori** in tutta la traversata.

Oltre ai semafori e ai loro salti, ogni spostamento lungo la strada costa tempo pari alla distanza percorsa.

---

# Police 4 · i semafori

Ogni semaforo è caratterizzato da un **periodo** $T$: è verde per $T$ unità di tempo e rosso per le $T$ successive, in modo ciclico.

Se il veicolo arriva al semaforo all'istante $t$:

- se $t \bmod 2T < T$ il semaforo è **verde** e si passa subito;
- altrimenti è **rosso** e bisogna attendere fino al prossimo verde, cioè $T - (t \bmod T)$ unità di tempo.

Saltare un semaforo significa attraversarlo senza attendere, anche se rosso, consumando uno dei salti disponibili.

Trovare il **tempo minimo** con cui si può arrivare a $L$.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga `N R T L`; sulla riga successiva le posizioni $X_i$.

```text
3 1 3 10
1 5 9
```

::right::

**Output**

Un intero: il tempo minimo di arrivo a $L$.

```text
11
```

::bottom::

> **Altro esempio** — `1 0 5 10` / `5` dà `15`: con $R = 0$ non ci sono salti, quindi al semaforo in $5$ si attende.

---
layout: two-cols-header
---

# Spiegazione

`dp[i][j]` = tempo minimo per raggiungere il semaforo $i$ **avendo saltato al più $j$ semafori**.

::left::

Per arrivare a $i$ ho due opzioni:

- **saltare il semaforo $i-1$**, spendendo uno dei $j$ salti:
  $$t_1 = dp[i-1][j-1]$$
- **passare dal semaforo $i-1$** (eventualmente attendendo):
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

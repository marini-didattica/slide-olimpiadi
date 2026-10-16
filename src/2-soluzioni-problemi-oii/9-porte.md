# Ma chi è quel mona

[roiti_porte](https://training.olinfo.it/task/roiti_porte)

Gaetano sbatte una sequenza di porte. Parte **sbattendo la porta $0$** e deve **terminare sbattendo la porta $N-1$**.

Dalla porta $i$ può passare alla porta $i - 2^j$, per ogni potenza di $2^j$ non superiore a $i$ (quindi $i-1,\ i-2,\ i-4,\ i-8,\ \dots$). Il passaggio è ammesso solo se la somma delle "frequenze" delle due porte è divisibile per la distanza $2^j$:

$$\left(A_i + A_{i-2^j}\right) \bmod 2^j = 0$$

Contare il numero di **sequenze armoniche** distinte che partono dalla porta $0$ e arrivano alla porta $N-1$, modulo $10^9 + 7$.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga $N$; sulla riga successiva le frequenze $A_0 \ldots A_{N-1}$.

```text
4
1 3 1 5
```

::right::

**Output**

Un intero: il numero di sequenze, modulo $10^9 + 7$.

```text
3
```

::bottom::

> **Altro esempio** — `8` / `2 2 2 2 2 2 2 2` dà `31`: qui la condizione è sempre soddisfatta, perché ogni somma vale $4$ ed è divisibile per $1, 2$ e $4$.

---
layout: two-cols-header
---

# Spiegazione

`dp[i]` = numero di sequenze armoniche che usano le prime `i` porte e **terminano sulla porta `i`**.

::left::

Per arrivare a `i` posso saltare da ogni porta `i - 2^j` con $2^j \le i$, purché valga la condizione:

$$\left(A_i + A_{i-2^j}\right) \bmod 2^j = 0 \implies dp[i] \mathrel{+}= dp[i - 2^j]$$

In altre parole: sommo `dp[i - 2^j]` per ogni potenza di $2$ ammessa.

::right::

**Caso base:** `dp[0] = 1` (una sola sequenza con la sola porta 0).

**Risposta:** `dp[N-1] mod 1e9+7`.

Per ogni `i` il ciclo interno esamina solo le potenze di $2$ non superiori a `i`, cioè $O(\log N)$ iterazioni. Applico il modulo a ogni somma per evitare overflow.

**Complessità:** $O(N \log N)$ tempo, $O(N)$ memoria.

::bottom::

> Il "trucco" è calcolare `pow *= 2` a ogni passo invece di ricalcolare $2^j$ con `pow(2, j)`.

---
layout: default
---

# Codice

```cpp
#define MOD 1000000007
#define MAXN 400001
int N, A[MAXN], dp[MAXN];

memset(dp, 0, sizeof(dp));
dp[0] = 1;

for (int i = 1; i < N; i++) {
  int pow = 1;
  while (pow <= i) {                    // 1, 2, 4, 8, ...
    if ((A[i] + A[i - pow]) % pow == 0) {
      dp[i] += dp[i - pow];
      dp[i] %= MOD;
    }
    pow *= 2;
  }
}

out << dp[N - 1] % MOD << endl; // l'ultima porta è sempre sbattuta
```

> Le porte sono indicizzate da `0` a `N-1`: la risposta sta in `dp[N-1]`.

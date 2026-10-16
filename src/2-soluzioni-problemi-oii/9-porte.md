---
layout: two-cols-header
---

# Ma chi è quel mona

[roiti_porte](https://training.olinfo.it/task/roiti_porte)

Gaetano sbatte una sequenza di porte: parte dalla porta $0$ e deve **terminare sulla porta $N-1$**. Dalla porta $i$ può saltare alla porta $i - 2^j$ (per ogni potenza di $2$) solo se la somma delle "frequenze" è divisibile per la distanza:

$$\big(A_i + A_{i-2^j}\big) \bmod 2^j = 0$$

Contare le **sequenze armoniche** distinte, modulo $10^9 + 7$.

::left::

**Input** · `N`, poi le frequenze $A_i$

```text
4
1 3 1 5
```

::right::

**Output** · il numero di sequenze: `3`

::bottom::

> **Assunzione chiave del testo** — l'ultima porta *deve* essere sbattuta: la risposta è proprio `dp[N-1]`.

---
layout: two-cols-header
---

# Idea

`dp[i]` = numero di sequenze armoniche che usano le prime `i` porte e **terminano sulla porta `i`**.

::left::

Per arrivare a `i` posso saltare da ogni porta `i - 2^j` con $2^j \le i$, purché valga la condizione:

$$\big(A_i + A_{i-2^j}\big) \bmod 2^j = 0 \implies dp[i] \mathrel{+}= dp[i - 2^j]$$

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

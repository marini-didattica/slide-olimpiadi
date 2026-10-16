# Police 3

[ois_police3](https://training.olinfo.it/task/ois_police3)

Un veicolo della polizia deve attraversare $N$ semafori in sequenza. Al semaforo $i$ ha due possibilità:

- **fermarsi**, impiegando $T_i$ unità di tempo (l'attesa al rosso);
- **passare** senza fermarsi, operazione che richiede tempo nullo.

Passare senza fermarsi è però consentito **solo** se al semaforo immediatamente precedente il veicolo si è fermato: non è possibile "saltare" due semafori consecutivi.

Trovare il **tempo minimo** per superare tutti i semafori.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga $N$; sulla riga successiva i valori $T_0 \ldots T_{N-1}$.

```text
6
3 2 2 6 2 3
```

::right::

**Output**

Un intero: il tempo minimo per superare tutti i semafori.

```text
6
```

::bottom::

> **Altro esempio** — `2` / `4 7` dà `4`: conviene fermarsi al primo semaforo e passare al secondo.

---
layout: default
---

# Spiegazione

Uso due vettori di DP, uno per ciascuna scelta fatta all'ultimo semaforo considerato.

- `ts[i]` — tempo minimo per superare il semaforo $i$ **fermandosi**;
- `tr[i]` — tempo minimo per superare il semaforo $i$ **senza fermarsi**.

$$ts[i] = T_i + \min\big(tr[i-1],\; ts[i-1]\big)$$

$$tr[i] = ts[i-1]$$

Se **non** mi fermo a $i$, l'unico modo lecito è essermi fermato a $i-1$.

---
layout: default
---

# Spiegazione · applicazione all'esempio

| `i`  | 0 | 1 | 2 | 3 | 4 | 5 |
| ---- | - | - | - | - | - | - |
| `ts` | 3 | 2 | 4 | 8 | 6 | 9 |
| `tr` | 0 | 3 | 2 | 4 | 8 | 6 |

Casi base: $ts[0] = T_0$, $tr[0] = 0$. Risposta: $\min(9, 6) = 6$.

> **Complessità:** $O(N)$ tempo e memoria — in realtà bastano due variabili, perché ogni stato dipende solo dal precedente.

---
layout: default
---

# Codice

```cpp
vector<int> ts(N); // se mi fermo
vector<int> tr(N); // se non mi fermo

ts[0] = T[0];
tr[0] = 0;

for (int i = 1; i < N; i++) {
  ts[i] = T[i] + min(tr[i - 1], ts[i - 1]);
  tr[i] = ts[i - 1];
}

cout << min(ts[N - 1], tr[N - 1]) << endl;
```

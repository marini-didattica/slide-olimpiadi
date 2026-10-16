---
layout: two-cols-header
---

# K-step ancestor

[ois_treeancestor](https://training.olinfo.it/task/ois_treeancestor)

Dato un albero con $N$ nodi radicato in $0$, per ogni nodo trovare il suo **antenato a distanza $K$**, cioè il nodo che si incontra risalendo $K$ archi verso la radice. Se un nodo non ha un tale antenato, stampare `-1`.

::left::

**Input**

Sulla prima riga `N K` (es. `11 2`); seguono gli $N-1$ archi.

`0 3 · 1 2 · 0 4 · 0 1 · 2 6 · 4 5 · 5 7 · 2 8 · 8 9 · 8 10`

::right::

**Output**

Gli $N$ antenati, in ordine di nodo.

```text
-1 -1 0 -1 -1 0 1 4 1 2 2
```

(il nodo $2$ ha come 2-antenato il nodo $0$; il nodo $6$ ha il nodo $1$; ecc.)

::bottom::

> Un albero ha un **cammino unico** dalla radice a ogni nodo: è proprio ciò che rende semplice questo problema.

---
layout: two-cols-header
---

# Idea · DFS con il percorso

Esploro l'albero in profondità tenendo due informazioni globali:

- `depth` — la profondità del nodo corrente;
- `path[d]` — il nodo visitato alla profondità `d` lungo il cammino dalla radice.

::left::

Quando visito un nodo a profondità `depth`, il suo antenato a distanza $K$ è semplicemente il nodo che stava $K$ livelli più in alto:

```cpp
if (depth >= K)
  ancestor[node] = path[depth - K];
else
  ancestor[node] = -1;
```

::right::

`path` funziona come una **pila implicita**: aggiorno `path[depth] = node` scendendo e non serve "pulire" risalendo, perché i livelli vengono sovrascritti quando si esplorano altri rami.

**Complessità:** $O(N)$ — una sola visita dell'albero.

::bottom::

> Alternativa avanzata: *binary lifting* ($O(N \log N)$ di preprocessing), ma qui **non** serve perché $K$ è fisso e noto in anticipo.

---
layout: default
---

# Codice

```cpp
void explore(vector<vector<int>>& graph, int node, int depth) {
  visited[node] = true;
  path[depth] = node;

  if (depth >= K)
    ans[node] = path[depth - K];
  else
    ans[node] = -1;

  for (int next : graph[node])
    if (!visited[next])
      explore(graph, next, depth + 1);
}
```

> `path`, `ans` e `visited` sono vettori globali; la DFS parte da `explore(graph, 0, 0)`.

# K-step ancestor

[ois_treeancestor](https://training.olinfo.it/task/ois_treeancestor)

Viene dato un albero con $N$ nodi, numerati da $0$ a $N-1$, radicato nel nodo $0$.

Per ogni nodo si vuole determinare il suo **antenato a distanza $K$**: il nodo che si incontra risalendo esattamente $K$ archi verso la radice. Se un nodo è troppo vicino alla radice e un tale antenato non esiste, il risultato deve essere `-1`.

---
layout: two-cols-header
---

# Input / Output

::left::

**Input**

Sulla prima riga `N K`; seguono gli $N-1$ archi, ciascuno dato da due nodi.

Esempio ($N = 11$, $K = 2$):

`0 3 · 1 2 · 0 4 · 0 1 · 2 6 · 4 5 · 5 7 · 2 8 · 8 9 · 8 10`

::right::

**Output**

Gli $N$ antenati, nell'ordine dei nodi da $0$ a $N-1$.

```text
-1 -1 0 -1 -1 0 1 4 1 2 2
```

::bottom::

**Discussione dell'esempio**

Con $K = 2$: il nodo $2$ ha come 2-antenato il nodo $0$, il nodo $6$ ha il nodo $1$, mentre il nodo $0$ (radice) risponde `-1` perché non ha antenati.

---
layout: two-cols-header
---

# Spiegazione

Esploro l'albero in profondità (DFS), tenendo due informazioni globali:

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

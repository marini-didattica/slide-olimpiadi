# I tre ingredienti

La dp funziona quando il problema ha tutte e tre queste proprietà. Ognuna risponde a una domanda diversa.

| Ingrediente | La domanda a cui risponde |
| --- | --- |
| **Sottoproblemi sovrapponibili** | Lo stesso sottoproblema ricompare più volte? |
| **Sottostruttura ottimale** | La soluzione ottima si costruisce da soluzioni ottime più piccole? |
| **Memoizzazione** | Come conservo i risultati e li riuso? |

> **In sintesi** — un problema di DP può essere scomposto in sottoproblemi più piccoli, i cui risultati possono essere salvati e riutilizzati per costrutire la soluzione globale.

---

# Sottoproblemi sovrapponibili

Un problema ha **sottoproblemi sovrapponibili** quando la ricorsione ingenua calcola più volte lo **stesso** sottoproblema.

<div style="display:flex; justify-content:center;">
  <svg viewBox="0 0 620 260" style="width:min(100%, 510px); height:auto;" role="img" aria-label="Albero delle chiamate di Fibonacci: F(3) e F(2) compaiono più volte">
    <g style="fill:none; stroke:var(--c-border-strong); stroke-width:1.5;">
      <path d="M300,47 L170,78" />
      <path d="M300,47 L430,78" />
      <path d="M170,112 L90,143" />
      <path d="M170,112 L250,143" />
      <path d="M430,112 L370,143" />
      <path d="M430,112 L500,143" />
      <path d="M90,177 L40,208" />
      <path d="M90,177 L140,208" />
      <path d="M250,177 L210,208" />
      <path d="M250,177 L280,208" />
    </g>
    <g style="font-family:var(--slidev-code-font-family); font-size:16px; text-anchor:middle;">
      <g>
        <rect x="272" y="13" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="300" y="35" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">5</tspan></text>
      </g>
      <g>
        <rect x="142" y="78" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="170" y="100" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">4</tspan></text>
      </g>
      <g>
        <rect x="402" y="78" width="56" height="34" rx="5" style="fill:var(--slidev-theme-primary); stroke:var(--slidev-theme-primary); stroke-width:1.5;" />
        <text x="430" y="100" style="fill:var(--c-on-primary); font-weight:600;">F<tspan dy="4" style="font-size:11px">3</tspan></text>
      </g>
      <g>
        <rect x="62" y="143" width="56" height="34" rx="5" style="fill:var(--slidev-theme-primary); stroke:var(--slidev-theme-primary); stroke-width:1.5;" />
        <text x="90" y="165" style="fill:var(--c-on-primary); font-weight:600;">F<tspan dy="4" style="font-size:11px">3</tspan></text>
      </g>
      <g>
        <rect x="222" y="143" width="56" height="34" rx="5" style="fill:var(--slidev-theme-primary); stroke:var(--slidev-theme-primary); stroke-width:1.5;" />
        <text x="250" y="165" style="fill:var(--c-on-primary); font-weight:600;">F<tspan dy="4" style="font-size:11px">2</tspan></text>
      </g>
      <g>
        <rect x="342" y="143" width="56" height="34" rx="5" style="fill:var(--slidev-theme-primary); stroke:var(--slidev-theme-primary); stroke-width:1.5;" />
        <text x="370" y="165" style="fill:var(--c-on-primary); font-weight:600;">F<tspan dy="4" style="font-size:11px">2</tspan></text>
      </g>
      <g>
        <rect x="472" y="143" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="500" y="165" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">1</tspan></text>
      </g>
      <g>
        <rect x="12" y="208" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="40" y="230" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">1</tspan></text>
      </g>
      <g>
        <rect x="112" y="208" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="140" y="230" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">0</tspan></text>
      </g>
      <g>
        <rect x="182" y="208" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="210" y="230" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">1</tspan></text>
      </g>
      <g>
        <rect x="252" y="208" width="56" height="34" rx="5" style="fill:var(--c-surface); stroke:var(--c-border-strong); stroke-width:1.5;" />
        <text x="280" y="230" style="fill:var(--c-text-strong);">F<tspan dy="4" style="font-size:11px">0</tspan></text>
      </g>
    </g>
  </svg>
</div>

I nodi colorati calcolano lo stesso valore: $F_3$ e $F_2$ compaiono già due volte. Per $n=5$: **15 chiamate** per **6 valori distinti**.

> **Sovrapposizione, non divide et impera** — in merge sort i sottoproblemi sono disgiunti: ogni elemento finisce in un solo ramo. Senza sovrapposizione la dp non porta vantaggi.

---

# Sottostruttura ottimale


Un problema ha **sottostruttura ottimale** quando la soluzione ottima si ottiene combinando soluzioni ottime dei suoi sottoproblemi. Supponiamo ad esempio di voler impilare $n$ scatoloni, ciascuno con dimensioni $a,b,c$:

- **Sottoproblema** $x$: calcolo l'altezza massima che posso ottenere con $x$ scatole
- **Sottostruttura ottimale**: se so l'altezza massima che posso ottenere con le prime $x$ scatole, allora posso calcolare quella ottenibile con le prime $x+1$

$$
S_{x+1} = \operatorname{max}(s_a, s_b, s_c) + S_{x}
$$

<!-- La parte più complessa sta nel capire come sfruttare le soluzioni precedenti per calcolare quella corrente -->

> **Attenzione** - la difficoltà sta nel capire **come** combinare le soluzioni che abbiamo già calcolato per sintetizzare quella corrente

---
layout: two-cols-header
---

# Memoizzazione

La memoizzazione è una **cache**: quando risolvo un sottoproblema scrivo il risultato in una tabella; quando lo incontro di nuovo lo leggo, invece di ricalcolarlo. Non cambia la ricorrenza: cambia **quante volte** la esegui.

::left::

```cpp
vector<long long> memo;

long long fib(int n) {
  if (n <= 1) return n;

  long long &r = memo[n];
  if (r != -1) return r;   // già risolto

  return r = fib(n - 1) + fib(n - 2);
}

int main() {
  int n; cin >> n;
  memo.assign(n + 1, -1);
  cout << fib(n) << '\n';
}
```

::right::

- Ogni stato viene calcolato **una sola volta**.
- Tempo: numero di stati × costo per stato. Fibonacci passa da $O(2^n)$ a $O(n)$.
- Memoria: $O(n)$ per la tabella, più $O(n)$ per la ricorsione.

**Top-down (memoizzazione)** — ricorsione + cache: calcolo solo gli stati che servono.

**Bottom-up (tabulazione)** — tabella dai casi base, niente ricorsione; per Fibonacci bastano due variabili ($O(1)$ di memoria).

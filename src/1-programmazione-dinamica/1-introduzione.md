---
layout: cover
---

# Programmazione dinamica

### Quando un problema si ripete, non calcolarlo due volte

<div class="pt-12">
  <span class="px-2 py-1 rounded bg-primary text-[#141418]">Marini Mattia - Informatica</span>
</div>

---

# Lo scopo

Molti problemi si possono risolvere provando tutte le possibilità. Il guaio è che il numero di possibilità cresce molto in fretta: la **programmazione dinamica** (dp) serve a non ripetere lo stesso lavoro.

> **Idea fondamentale**: Barattare un po' di **memoria** per risparmiare molto **tempo**.

In pratica la dp fa tre cose:

- **scompone** il problema in sottoproblemi più piccoli;
- **riusa** la soluzione di ogni sottoproblema invece di ricalcolarla;
- **combina** i risultati per ottenere la risposta finale.

> **Non è un algoritmo** — è un modo di organizzare la soluzione. Lo stesso schema risolve problemi di conteggio, di ottimizzazione (minimo o massimo) e di decisione.

---

# Il costo di non usarla

La ricorsione ingenua di Fibonacci ricalcola gli stessi valori un numero enorme di volte.

| $n$ | chiamate ricorsive | valori distinti |
| --: | -----------------: | --------------: |
| 10  | 177                | 11              |
| 20  | 21 891             | 21              |
| 40  | ~331 000 000       | 41              |

Il numero di chiamate cresce in modo **esponenziale**, mentre i valori da calcolare sono soltanto $n+1$. La dp elimina tutta questa ripetizione: è il passaggio da $O(2^n)$ a $O(n)$.

> **La domanda da fare** — «Sto ricalcolando qualcosa che ho già calcolato?» Se la risposta è sì, c'è spazio per la programmazione dinamica.

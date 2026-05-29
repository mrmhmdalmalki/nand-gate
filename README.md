# NAND Gate

A NAND gate outputs `0` **only when both inputs are `1`**; otherwise it outputs `1`. It is
the inverse of AND, and is the other *universal* gate — any logic circuit can be built from
NAND gates alone.

### Symbol

The AND shape with a **bubble** on the output (the bubble = inversion).

<img src="images/symbol.png" width="400">

### Truth table

| `A` | `B` | `Y` |
|:---:|:---:|:---:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | **0** |

`Y = A NAND B = NOT(A AND B)`

---

## What `0` and `1` really mean

`0` is **not** an empty wire — it is the output **actively connected to ground (0 V)**
through conducting transistors. `1` is the output connected to **+5 V**. A wire connected to
*nothing* is a separate, undefined **floating** state, which we always avoid.

---

## How it is built

Two NPN transistors in **series** (stacked): the top transistor's emitter feeds the bottom
transistor's collector, the bottom emitter goes to ground, and a single pull-up resistor `R_C`
connects the top collector (the output) to `+5 V`. Each input drives one base.

<img src="images/circuit.png" width="640">

How it works — current can only reach ground if **both** transistors conduct:

- **Both A and B are `1`:** both transistors ON, the chain completes, and the output is
  **pulled down to ground** → `0`.
- **Either input `0`:** that transistor is OFF, the chain is broken, so `R_C` **pulls the
  output up to +5 V** → `1`.

Series transistors give the AND condition; the pull-up output makes it inverting, so
`Y = NOT(A AND B)`.

---

## Components

### Transistors — 2N3904  (×2: Q1, Q2)

- **Type:** **NPN** *bipolar junction transistor* (BJT) — a current-controlled switch: a
  small current into the **base** lets a much larger current flow from **collector** to
  **emitter**. Here each transistor is used fully on/off, as a switch.
- **Package:** TO-92 (small black half-cylinder of plastic with 3 legs).
- **Pinout:** hold it with the **flat face toward you and the legs pointing down** — the pins
  are **E – B – C** (Emitter, Base, Collector) from left to right.
- **Key ratings:** V_CE ≈ **40 V** max, I_C ≈ **200 mA** max, current gain *hFE* ≈ **100–300**.
- **Why NPN (not PNP)?** The transistors are stacked from the output down to **ground**, and a
  HIGH (+5 V) on a base turns that transistor ON. Only when **both** are on does the chain
  reach ground and pull the output low. A PNP would need the circuit re-wired upside-down.
- **Substitutes:** 2N2222, PN2222, BC547 — any general-purpose NPN. **Re-check the pinout.**

### Resistors

| Ref | Value | Job |
|:---:|:-----:|:----|
| R_B1, R_B2 | **10 kΩ** | **Base resistors** — one per input; limit base current while switching the transistor fully on. |
| R_C | **1 kΩ**  | **Collector pull-up** — provides the HIGH (+5 V) level and limits current when both transistors pull the output low. |

### Power

- A **+5 V** supply rail and a common **GND** (0 V) reference.

---

## Regenerating the diagrams

```bash
pdflatex circuit.tex
pdflatex symbol.tex
pdftoppm -png -r 600 circuit.pdf images/circuit   # -> images/circuit-1.png
pdftoppm -png -r 600 symbol.pdf  images/symbol     # -> images/symbol-1.png
```

> Use `pdftoppm`, not `pdftocairo` — at high DPI the Cairo backend can garble the fonts.

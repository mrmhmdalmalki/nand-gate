# NAND Gate

A NAND gate outputs `0` **only when both inputs are `1`**; otherwise it outputs `1`. It is
the inverse of AND, and is the other *universal* gate: any logic circuit can be built from
NAND gates alone.

### Symbol

The AND shape with a **bubble** on the output (the bubble = inversion).

<img src="images/symbol.png" width="460">

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

`0` is **not** an empty wire; it is the output **actively connected to ground (0 V)**
through conducting transistors. `1` is the output connected to **+5 V**. A wire connected to
*nothing* is a separate, undefined **floating** state, which we always avoid.

---

## How it is built

Two NPN transistors in **series** (stacked): the top transistor's emitter feeds the bottom
transistor's collector, the bottom emitter goes to ground, and a single pull-up resistor `R_C`
connects the top collector (the output) to `+5 V`. Each input drives one base.

<img src="images/circuit.png" width="900">

How it works: current can only reach ground if **both** transistors conduct:

- **Both A and B are `1`:** both transistors ON, the chain completes, and the output is
  **pulled down to ground** → `0`.
- **Either input `0`:** that transistor is OFF, the chain is broken, so `R_C` **pulls the
  output up to +5 V** → `1`.

Series transistors give the AND condition; the pull-up output makes it inverting, so
`Y = NOT(A AND B)`.

---

## Building it on a breadboard

Two transistors stacked in series (Q1 on top of Q2). Identify each 2N3904's legs with the pinout (flat face toward you, legs down, **E B C** left to right), then wire as in the pin-labeled schematic above.

<img src="images/pinout.png" width="360">

The wiring picture below is the same circuit drawn the way the parts physically sit on the board (each TO-92 package with its legs pointing down), so each leg maps straight to where its wire goes:

<img src="images/wiring.png" width="900">

Connect each 2N3904 as follows:

| Transistor | E (emitter) | B (base) | C (collector) |
|:-----------|:------------|:---------|:--------------|
| **Q1 (top)** | joined to Q2's collector | through R_B1 (10 kΩ) to Input A | through R_C (1 kΩ) to +5 V; this node is Output Y |
| **Q2 (bottom)** | GND | through R_B2 (10 kΩ) to Input B | joined to Q1's emitter |

Reminder: `+5 V` and `GND` are **nodes** (named connections), not physical positions, so the +5 V rail can be the top or the bottom rail of your board. If a result is wrong, the usual cause is a transistor's legs in the wrong holes, so re-check **E B C** against the pinout.

Quick test: Output is GND only when both inputs are +5 V; otherwise it is +5 V.

---

## Components

### Transistors: 2N3904  (×2: Q1, Q2)

- **Type:** **NPN** *bipolar junction transistor* (BJT), a current-controlled switch: a
  small current into the **base** lets a much larger current flow from **collector** to
  **emitter**. Here each transistor is used fully on/off, as a switch.
- **Package:** TO-92 (small black half-cylinder of plastic with 3 legs).
- **Pinout:** hold it with the **flat face toward you and the legs pointing down**, and the pins
  are **E, B, C** (Emitter, Base, Collector) from left to right.
- **Key ratings:** V_CE ≈ **40 V** max, I_C ≈ **200 mA** max, current gain *hFE* ≈ **100–300**.
- **Why NPN (not PNP)?** The transistors are stacked from the output down to **ground**, and a
  HIGH (+5 V) on a base turns that transistor ON. Only when **both** are on does the chain
  reach ground and pull the output low. A PNP would need the circuit re-wired upside-down.
- **Substitutes:** 2N2222, PN2222, BC547, or any general-purpose NPN. **Re-check the pinout.**

### Resistors

| Ref | Value | Job |
|:---:|:-----:|:----|
| R_B1, R_B2 | **10 kΩ** | **Base resistors**, one per input; limit base current while switching the transistor fully on. |
| R_C | **1 kΩ**  | **Collector pull-up**, provides the HIGH (+5 V) level and limits current when both transistors pull the output low. |

### Power

- A **+5 V** supply rail and a common **GND** (0 V) reference.

---

## Standards and references

**Gate symbol.** The distinctive-shape symbol follows the ANSI/IEEE standard for logic graphic symbols:

- IEEE Std 91-1984 and 91a-1991, *Graphic Symbols for Logic Functions* ([standards.ieee.org](https://standards.ieee.org/ieee/91_91a/241/)). The distinctive shapes originate from US MIL-STD-806; the international equivalent is IEC 60617-12.
- Free explainer: Texas Instruments, *Overview of IEEE Standard 91-1984* (PDF) ([ti.com](https://www.ti.com/lit/ml/sdyz001a/sdyz001a.pdf)).
- Symbols and truth tables overview: *Logic gate*, Wikipedia ([wikipedia.org](https://en.wikipedia.org/wiki/Logic_gate)).

**Transistor circuit.** This NAND gate is two transistor switches in series (series = AND) with a pull-up output that inverts the result to NAND. It follows standard transistor switch logic:

- *Resistor-Transistor Logic (RTL)*, Wikipedia ([wikipedia.org](https://en.wikipedia.org/wiki/Resistor%E2%80%93transistor_logic)).
- *NOR and NAND gates using transistor*, TheoryCircuit ([theorycircuit.com](https://theorycircuit.com/digital-electronics/nor-and-nand-gates-using-transistor/)).
- *Logic Gates using Transistors*, Electronics Tutorials ([electronics-tutorials.ws](https://www.electronics-tutorials.ws/logic/logic-gates-using-transistors.html)).
- P. Horowitz and W. Hill, *The Art of Electronics*, 3rd ed., Cambridge University Press, 2015 (the BJT used as a switch).
- A. S. Sedra and K. C. Smith, *Microelectronic Circuits*, Oxford University Press (BJT switch and the logic NOT gate).
- T. L. Floyd, *Digital Fundamentals*, Pearson (logic-gate symbols and truth tables).

**Transistor part.** 2N3904 NPN, onsemi datasheet ([PDF](https://www.onsemi.com/pdf/datasheet/2n3904-d.pdf)), product page ([onsemi.com](https://www.onsemi.com/products/discrete-power-modules/general-purpose-and-low-vcesat-transistors/2n3904)).

**Highlighted source (additional).** The exact building block this design uses, scroll-to-text highlighted on the Wikipedia RTL page: [“a common-emitter stage with a base resistor”](https://en.wikipedia.org/wiki/Resistor%E2%80%93transistor_logic#:~:text=common-emitter%20stage%20with%20a%20base%20resistor).

---

## Regenerating the diagrams

```bash
pdflatex circuit.tex
pdflatex symbol.tex
pdftoppm -png -r 600 circuit.pdf images/circuit   # -> images/circuit-1.png
pdftoppm -png -r 600 symbol.pdf  images/symbol     # -> images/symbol-1.png
```

> Use `pdftoppm`, not `pdftocairo`, at high DPI the Cairo backend can garble the fonts.

# 4-Bit Binary Adder GUI Simulator

**CO_MINI_PROJECT_GROUP_4** · Author: Soumik Jana, Subhankar Ojha, Dibit Sha

An interactive, single-file web simulator of a 4-bit ripple-carry binary adder built from basic logic gates (XOR, AND, OR). It mirrors the layout of the original [CircuitVerse project](https://circuitverse.org/users/480922/projects/co_mini_project_group_4-57e55942-2f4c-4b3f-b5f0-150f411ad7eb) and adds live visualization, gate-level signal inspection, and an automated truth-table verifier.

---

## Features

- **Live circuit diagram**: SVG schematic of all four full-adder stages. Wires and gates glow and animate when carrying a HIGH (1) signal.
- **Clickable inputs**: Toggle any of `A3..A0` and `B3..B0` directly on the control deck or on the diagram.
- **Increment / decrement**: `+` / `-` buttons step A or B through 0–15.
- **Live outputs**: `CARRY, S3, S2, S1, S0` LEDs, plus the 5-bit total, 4-bit sum, signed (two's complement) result and overflow flag.
- **Live equation**: e.g. `8 (1000₂) + 15 (1111₂) = 23 (10111₂)`.
- **Per-stage focus**: Zoom into a single bit slice (Bit 0 LSB … Bit 3 MSB) to inspect its gates.
- **Ripple animation**: Steps through the stages one at a time to show how the carry propagates.
- **Automated truth-table verifier**: Feeds input vectors, reads outputs, and checks them against expected arithmetic, with pass/fail counts and a progress bar.
- **Bit-slice signal table**: Internal values per stage (A, B, P, G, T, carry-in, sum, carry-out).
- **Manual snapshot log**: Record the current state, clear it, or copy the log.
- **Customization**: Five active-signal colors, light/dark canvas toggle, and optional sound effects.

---

## Getting Started

No build step or install is required.

1. Download `4_bit_binary_adder_simulator.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).

> **Note:** The page loads Tailwind CSS (`cdn.tailwindcss.com`) and Google Fonts (Inter, JetBrains Mono) from CDNs, so an internet connection is needed for full styling.

---

## How It Works

Each bit slice is a **full adder** made of five gates:

| Signal | Gate | Expression |
|--------|------|------------|
| `P_i` (propagate) | XOR | `A_i ⊕ B_i` |
| `S_i` (sum) | XOR | `P_i ⊕ C_i` |
| `G_i` (generate) | AND | `A_i · B_i` |
| `T_i` | AND | `P_i · C_i` |
| `C_(i+1)` (carry out) | OR | `G_i + T_i` |

The four slices are chained so each stage's carry-out feeds the next stage's carry-in:

```
C0 = 0  →  [Bit 0] →C1→ [Bit 1] →C2→ [Bit 2] →C3→ [Bit 3] →C4 = CARRY
```

- The first carry-in `C0` is fixed at 0 (addition only; there is no subtract mode).
- **Overflow** (signed): `V = C3 ⊕ C4`.
- **Result**: `{CARRY, S3, S2, S1, S0}` is the 5-bit sum, range 0–30.

---

## Using the Simulator

### Setting inputs
Click a bit button (`A3`…`A0`, `B3`…`B0`) to flip it between 0 and 1, or use the `+` / `-` buttons. **🎲 Randomize** picks random values and **↺ Reset** sets everything to 0.

### Toolbar
| Control | Action |
|---------|--------|
| ⚡ Active Color | Change the HIGH-signal color (orange, cyan, crimson, magenta, gold) |
| 🎨 Canvas | Switch the schematic between light and dark |
| 🔊 Sound | Toggle sound effects |
| ▶ Animate Ripple | Highlight each stage in turn (≈650 ms per stage) |
| CircuitVerse ↗ | Open the original project |

### Schematic Focus
Use the tabs (**All 4 Stages**, **Bit 0**–**Bit 3**) to switch between the full diagram and a zoomed single-stage view.

### Automated Truth-Table Verifier
1. Choose a **Truth Table Scope**:
   - *Representative (16 key vectors)*: default, a hand-picked set including edge cases like `0+0` and `15+15`.
   - *Full (256 inputs)*: every combination of A and B from 0–15.
2. Set the **Time Delay** between vectors (default **200 s**; also available in ms, with presets for 200 s, 200 ms, 1 s and 50 ms).
3. Click one of:
   - **Start Auto-Verify** to run with the configured delay.
   - **Step Next Input Now** to skip the countdown and apply the next vector.
   - **Verify All Instantly** to check every vector at once.
4. Watch the results table, the `PASS: x / y` counter and the progress bar.

---

## Console Automation Script

The **📜 View / Copy Automation Script** button reveals a standalone script that can also be pasted into a browser console (`F12 → Console`). The simulator exposes two helpers on `window` for this:

```js
// Set inputs (A and B are 0–15)
setCircuitInputs(A, B, 0);

// Read outputs
readCircuitOutputs();
// → { CARRY, S3, S2, S1, S0, valS, val5Bit }
```

Example:

```js
setCircuitInputs(9, 6, 0);
readCircuitOutputs(); // CARRY: 0, S3..S0 = 1111, valS: 15, val5Bit: 15
```

---

## Tech Stack

- HTML, CSS and vanilla JavaScript in one file
- [Tailwind CSS](https://tailwindcss.com/) 3.4.17 (CDN)
- Inline SVG for the schematics
- Web Audio API for sound effects
- Google Fonts: Inter and JetBrains Mono

---

## Project Structure

```
4_bit_binary_adder_simulator.html   # entire app (UI, logic, styles)
README.md                           # this file
```

---

## Credits

Developed as part of **CO Mini Project, Group 4** (Computer Organization) by **Soumik Jana**. Original circuit design on [CircuitVerse](https://circuitverse.org/users/480922/projects/co_mini_project_group_4-57e55942-2f4c-4b3f-b5f0-150f411ad7eb).

# 📘 Day 5: Conditional Logic, Inferred Latches, and Looping Constructs

This document summarizes how to use Verilog's conditional (`if`/`case`) and looping (`for`/`generate`) constructs correctly to avoid synthesis issues and create scalable hardware.

---

## 1. `if` vs. `case` and the Latch Problem

### 🛑 The Latch Problem

In combinational logic (`always @ (*)`), a **latch** is unintentionally created when an output register (`reg`) is **not assigned a value** under all possible input conditions. This forces the synthesis tool to create memory to hold the previous value, which is generally inefficient.

| Pitfall | Cause | Fix |
| :--- | :--- | :--- |
| **Incomplete `if`** | Missing an **`else`** branch. | **Always add an `else`** to assign the output for the false condition. |
| **Incomplete `case`** | Missing the **`default`** branch. | **Always include a `default`** to assign the output for unlisted cases (e.g., `default: y = 1'b0;`). |

### 🧠 Logic & Ambiguity

* **`if` statements:** Create **priority logic**.
* **`case` statements:** Create **parallel logic**.

| Pitfall | Synthesis Issue |
| :--- | :--- |
| **Partial Assignment** | If one output is assigned in one branch but **missed** in another, a **latch is inferred** for the missed output. |
| **Overlapping `case`** | Conditions (e.g., `2'b10` and `2'b1?`) that match the same input. This causes **ambiguous hardware** and can lead to simulation vs. synthesis mismatches. |

---

## 2. Looping Constructs for Scalable Hardware

Verilog loops allow designers to write concise code that generates large, repetitive structures.

### 2.1. `for` Loop (Combinational/Sequential Logic)

A `for` loop used inside an `always` block simplifies complex, repetitive logic that operates on bus widths.

* **Use Case:** Writing the scalable logic for a large **Multiplexer (Mux)** or decoder.
* **Mechanism:** The tool **unrolls** the loop during synthesis, resulting in dedicated gates for each iteration.

```verilog
// Used for scalable Mux logic
always @ (*) begin
    for(k = 0; k < 4; k=k+1) begin
        if(k == sel)
            y = i_int[k];
    end
end

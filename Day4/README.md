
## 📂 Repository Structure

```

├── src/                 # RTL and Testbench code
│   ├── ternary_operator_mux.v
│   ├── mux_tb.v
│   └── ...
├── netlist/             # Synthesized gate-level netlists
│   └── ternary_operator_mux_net.v
├── sims/                # Simulation outputs (VCD files, logs)
├── Makefile             # Optional automation for sim/synth/gls
└── README.md            # Documentation

```

<img width="1546" height="708" alt="Screenshot from 2025-09-27 22-58-21" src="https://github.com/user-attachments/assets/4d483738-ae83-46fb-b13e-a5613a3bcb8a" />

<img width="1546" height="708" alt="Screenshot from 2025-09-27 22-57-53" src="https://github.com/user-attachments/assets/4dfdcd8a-1486-4d90-9b75-53409de024b6" />

## 🔄 Verification Flow: RTL → GLS

| Step | Output | Purpose |
| :--- | :--- | :--- |
| **RTL Simulation** | Waveform (`.vcd`) | Verify initial RTL logic functionality |
| **Synthesis (Yosys)** | Gate-Level Netlist (`*_net.v`) | Convert RTL into standard cells |
| **GLS** | Waveform (`.vcd`) | Verify gate-level behavior matches RTL |
| **Final Check** | Compare RTL vs. GLS | Catch silent coding errors fixed by synthesis |

---

## ⚙️ Commands

### 🔹 RTL Simulation

```bash
iverilog -o rtl_sim src/ternary_operator_mux.v src/mux_tb.v
vvp rtl_sim
gtkwave dump.vcd
````


<img width="1546" height="708" alt="image" src="https://github.com/user-attachments/assets/9a11c0aa-3570-484d-ba0c-98c8423d5d72" />

![Uploading image.png…]()


### 🔹 Synthesis (Yosys)

```bash
yosys
read_verilog src/ternary_operator_mux.v
synth -top ternary_operator_mux
write_verilog -noattr netlist/ternary_operator_mux_net.v
```

### 🔹 Gate-Level Simulation (GLS)

```bash
iverilog -o gls_sim /path/to/primitives.v /path/to/sky130_fd_sc_hd.v \
    netlist/ternary_operator_mux_net.v src/mux_tb.v
vvp gls_sim
gtkwave dump.vcd
```

---

## 🧩 Example: Clean 2:1 MUX

A **ternary operator (`?:`)** ensures consistent RTL and GLS behavior.

### ✅ RTL Code

```verilog
assign y = sel ? i1 : i0;
```

✅ **Result:** RTL and GLS waveforms match exactly.

---

## ⚠️ Common Simulation–Synthesis Mismatches

### 1. 🛑 Missing Sensitivity in `always`

Using incomplete sensitivity lists creates mismatches.

| RTL Problem                                                | Simulation (WRONG)           | Synthesis (CORRECT)         |
| :--------------------------------------------------------- | :--------------------------- | :-------------------------- |
| `always @(sel)` (ignores `i0`, `i1`)                       | Output acts like a **latch** | Correct MUX inferred in GLS |
| **Fix:** Always use `always @(*)` for combinational logic. |                              |                             |

---

### 2. 🔄 Blocking Assignment Order

Wrong assignment order makes simulation lag.

#### ❌ Problem RTL

```verilog
always @(*) begin
    d = x & c;   // uses OLD value of x
    x = a | b;   // updates x
end
```

#### ✅ Fixed RTL

```verilog
always @(*) begin
    x = a | b;
    d = x & c;
end
```

| Problem                 | RTL Simulation (WRONG) | GLS Simulation (CORRECT)                       |
| :---------------------- | :--------------------- | :--------------------------------------------- |
| Order-dependent updates | `d` lags by 1 cycle    | Hardware is **combinational**, works instantly |

---

## ✅ Key Takeaways

* Run **GLS** to verify correctness after synthesis.
* Always use **`always @(*)`** in combinational blocks.
* Carefully order **blocking assignments**.
* These practices ensure RTL matches the synthesized netlist.

---

## 📚 Tools Used

* [Yosys](https://yosyshq.net/yosys/) – Open-source synthesis tool
* [Icarus Verilog](http://iverilog.icarus.com/) – RTL & GLS simulation
* [GTKWave](http://gtkwave.sourceforge.net/) – Waveform viewer

---


---

```




markdown
# ⚡ Logic Optimization with Yosys

This README demonstrates how to use **`opt_clean -purge`** in Yosys for logic optimization.  
It shows how redundant or unused logic is eliminated in:

1. Combinational Logic  
2. Sequential Logic  
3. Unused Logic

---

## 📑 Table of Contents

- [Combinational Logic Optimization](#combinational-logic-optimization)
- [Sequential Logic Optimization](#sequential-logic-optimization)
- [Unused Logic Optimization](#unused-logic-optimization)
- [Optimization Flow](#optimization-flow)
- [Summary Table](#summary-table)

---



<img width="1546" height="708" alt="image" src="https://github.com/user-attachments/assets/a4971d8b-17a9-4d9c-8b71-0920f0e7afa8" />


## 🔹 Combinational Logic Optimization

**Module:** `opt_check.v`

```verilog
module opt_check (input a , input b , output y);
    assign y = a ? b : 0;
endmodule
````

<img width="1546" height="708" alt="image" src="https://github.com/user-attachments/assets/4a9cd48d-1ee7-4466-b634-4733b3a64563" />


✅ Yosys simplifies this by removing redundant logic.

**Commands:**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog opt_check_before_purge.v
opt_clean -purge
write_verilog opt_check_after_purge.v
show
```


---

## 🔹 Sequential Logic Optimization

**Module:** `dff_const1.v`

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset) begin
    if (reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
endmodule
```
![Uploading image.png…]()


✅ Since `q` always resolves to a constant, optimization simplifies the flip-flop.

**Commands:**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog dff_const1_before_purge.v
opt_clean -purge
write_verilog dff_const1_after_purge.v
show
```

---

## 🔹 Unused Logic Optimization

**Module:** `counter_opt.v`

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk , posedge reset) begin
    if (reset)
        count <= 3'b000;
    else
        count <= count + 1;
end
endmodule
```

<img width="1546" height="708" alt="image" src="https://github.com/user-attachments/assets/467f930b-e61f-45ab-8bb4-09e43346766a" />


✅ Only `count[0]` is used → higher bits are removed by `opt_clean -purge`.

**Commands:**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog counter_opt_before_purge.v
opt_clean -purge
write_verilog counter_opt_after_purge.v
show
```

**Before Optimization:**


**After Optimization:**



---

## ⚙️ Optimization Flow

```
RTL → Synthesis → Netlist → opt_clean -purge → Optimized Netlist
```

---

## 📊 Summary Table

| Optimization Type | Example       | What Gets Optimized               |
| ----------------- | ------------- | --------------------------------- |
| Combinational     | `opt_check`   | Removes redundant expressions     |
| Sequential        | `dff_const1`  | Simplifies constant DFF behavior  |
| Unused Logic      | `counter_opt` | Removes logic not driving outputs |

---

## ✅ Conclusion

* **`opt_clean -purge`** is a key step in Yosys optimization.
* It removes **unused or redundant logic**, reduces area, and simplifies designs.
* Helps create **cleaner, more efficient netlists** for backend processing.

---

```

Would you like me to also **add a section with Makefile targets** (e.g., `make comb`, `make seq`, `make unused`) so you can automate these Yosys runs directly in GitHub projects?
```

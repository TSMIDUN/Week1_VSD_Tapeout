# Video-12,13,14
## Open the .lib File

To open the sky130_fd_sc_hd__tt_025C_1v80.lib file:

1. **Install a text editor:**
   ```shell
   sudo apt install gedit
   ```
2. **Open the file:**
   ```shell
   gedit sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
   ## P V T = Process,Voltage,Temperature
   - varitaion while fabrication
## Cell
- which says about poer and timing information
-  It is a fixed-height, fixed-function logic element (like a 2-input NAND gate, an inverter, or a D flip-flop) that has been characterized for its precise timing, power, and area properties.


<img width="537" height="659" alt="image" src="https://github.com/user-attachments/assets/52bc690a-1539-444e-84bd-84fc9688eb00" />


| Characteristic | Optimization Goal | Achieved By (Cell Type) | The Trade-off |
| :--- | :--- | :--- | :--- |
| **Low Delay** 🚀 | Maximize Speed/Performance | **Fast Cells** (Wider Transistors) | **Area** and **Power** increase. |
| **Low Area** 📏 | Minimize Chip Size | **Slow Cells** (Narrower Transistors) | **Delay** increases (slower circuit). |
| **Low Power** 🔋 | Maximize Battery Life/Efficiency | **Slow Cells** (Narrower Transistors) | **Delay** increases (slower circuit). |

# Video-15,16
## Openning Multiple Module

### Hierarchical vs. Flat Synthesis

This comparison shows how Yosys handles a design containing multiple modules.

### 🟢 2.1. Hierarchical Synthesis (Preserving Structure)

**Definition:** Maps logic to gates while **retaining the boundaries** of your original Verilog modules.

| Aspect | Feature | Yosys Command Key |
| :--- | :--- | :--- |
| **Structure** | **Blocks kept separate** (Multiple modules). | **`synth -top [module]`** (The `flatten` command is OMITTED). |
| **Advantage** | **Easy Debugging** 🔍 and **IP Reuse**. | |
| **Trade-off** | Optimizations are limited to inside module boundaries. | |




<img width="593" height="539" alt="image" src="https://github.com/user-attachments/assets/529f8043-c87c-440f-984f-c6c6cb964657" />


<img width="889" height="435" alt="image" src="https://github.com/user-attachments/assets/3890788b-ffde-40f2-83d6-05913f34db82" />

### 🟢 2.2. Flat Synthesis (Collapsing Structure)

**Definition:** **Merges** all logic from submodules into a single top module, eliminating the original structure.

| Aspect | Feature | Yosys Command Key |
| :--- | :--- | :--- |
| **Structure** | **One big circuit** (Hierarchy eliminated). | **`synth -top [module]`** followed by **`flatten`**. |
| **Advantage** | **Maximum Optimization** ✨ for speed/area. | |
| **Trade-off** | **Harder to Debug** because the original Verilog structure is lost. | |



<img width="1920" height="1067" alt="image" src="https://github.com/user-attachments/assets/d6c83035-a798-4a1c-99d6-13f50376bf80" />


<img width="1919" height="384" alt="image" src="https://github.com/user-attachments/assets/81381ecc-3c3c-45a4-820a-dd38c00935b2" />














The key to these special case optimizations is that multiplication by a constant can be converted into simpler, more efficient bit-shift and addition operations.

1. Multiplication by Powers of 2 (Bit-Shift)
The expression assign y = a * 2; results in zero additional gates because multiplying any binary number by a power of two is equivalent to a Left Bit-Shift operation.

Optimization: The synthesis tool realizes y=a×2=a≪1.

Hardware: A bit-shift is implemented simply by wiring the input bits to the output at shifted positions. It requires no logic gates; the tool only uses concatenation to implement the shift.

Optimized Verilog Construct: assign y = { a, 1'b0 }; (The input a is concatenated with a constant '0' at the LSB).


**Asynchronous Set Flop (`dff_async_set`)**

```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

<img width="986" height="457" alt="image" src="https://github.com/user-attachments/assets/5a56302a-27cc-437a-97f3-78e848e93fbe" />


2. Decomposition for Constant Multiplication
For a non-power-of-two constant like 9, the synthesis tool decomposes the multiplication into a combination of bit-shifts and additions, which is much cheaper than a full multiplier.

<img width="677" height="188" alt="image" src="https://github.com/user-attachments/assets/ca1ac37a-007a-4f84-9379-0196d4fb021a" />


Decomposition: The tool rewrites the multiplication based on powers of two:

y=a×9=a×(8+1)=(a×8)+(a×1)
Simplified Logic: Using bit-shifts, this becomes:


<img width="758" height="163" alt="image" src="https://github.com/user-attachments/assets/42a7ab3e-798b-4678-a478-5b87f3408862" />


y=(a≪3)+a
Resulting Hardware: The final optimized netlist uses a Concatenation-based Structure (effectively wiring and a small adder) instead of a large multiplier. For simple cases like this, the optimized output may be represented as a concatenation: assign y = { a, a }; (which implicitly represents the addition and shifting).


<img width="986" height="457" alt="image" src="https://github.com/user-attachments/assets/86283928-a7e0-4ec3-98bc-87fe7a729175" />






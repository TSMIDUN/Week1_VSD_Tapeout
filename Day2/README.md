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


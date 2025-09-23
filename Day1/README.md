# 2-Video
## Simulator:
- A software tool for testing a digital circuit's functionality by applying inputs and observing outputs.
+ It lets us verify a design before building it.


## Design:
- The actual Verilog code that describes the hardware logic. 
- It is the blueprint of the circuit.
<img width="522" height="259" alt="image" src="https://github.com/user-attachments/assets/4ec7a178-60ca-42e9-befe-94a5a01b43ea" />


## Testbench:
- A non-synthesizable Verilog file used only for simulation.
- It applies inputs (stimulus) to the design and checks for correct outputs.
<img width="729" height="222" alt="image" src="https://github.com/user-attachments/assets/037ae1b5-3bdf-426b-921a-d9555564eb09" />


## Iverilog & GTKWave Flow:
A process to check a design. 
- First, compile the design and testbench with iverilog to create an executable. 
- Second, run the executable to generate a .vcd waveform file.
- Finally, use GTKWave to view the waveforms and visually debug the design's behavior.
- <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2b5889e9-4bea-4931-878d-0892a12f4655" />


## Synthesizer
### RTL to Gates:
A synthesizer is a tool that automatically converts your behavioral Verilog code (RTL) into a physical circuit blueprint.

### Netlist Output:
The output is called a netlist, which is a list of standard logic gates (like AND, OR, NOT) and how they are all wired together.

### Yosys: 
In our workshop, Yosys is the open-source tool we use for this process.

# 3-Video
## 🔧 Initial Lab Setup :
so , before starting we got some instruction to setup 
### Step 1: Create a folder to work & clonning GitHub
```
midlock@midlock-LOQ-15ARP9:~$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
Cloning into 'sky130RTLDesignAndSynthesisWorkshop'...
remote: Enumerating objects: 417, done.
remote: Counting objects: 100% (69/69), done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 417 (delta 19), reused 47 (delta 12), pack-reused 348 (from 1)
Receiving objects: 100% (417/417), 7.79 MiB | 10.44 MiB/s, done.
Resolving deltas: 100% (242/242), done.
midlock@midlock-LOQ-15ARP9:~$ 
```
### Step2 : cheching out the git 
<img width="1413" height="483" alt="Screenshot from 2025-09-21 15-06-08" src="https://github.com/user-attachments/assets/98360c19-de36-49df-9bd7-994733754bc6" />

# 4-Video 
## Introduction iverilog gtkwave part1
### Design File: good_mux.v

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
  always @(*)
  begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```



















































# Standard Cell Library (.lib)
## Gate Menu: 
The .lib file is like a menu of all the available logic gates that the synthesizer can use.

## Physical Information: 
For each gate, the library contains important details like its size (area), how fast it is (timing), and how much power it consumes.

## Technology Mapping:
The synthesizer uses this .lib file to choose the best gates to map your Verilog logic to, ensuring the final circuit meets all the design requirements.

# ⚙️ Verilog Simulation Flow

This guide breaks down the standard process of simulating a Verilog design using Icarus Verilog and GTKWave, formatted for a GitHub-style document.

## 📂 The Design File: `good_mux.v`

This file is the **blueprint** of our hardware. It describes the actual circuit we want to build.

```verilog
// This module is our circuit, a 2-to-1 Multiplexer.
module good_mux (
  input i0,          // First data input
  input i1,          // Second data input
  input sel,         // The select line to choose between i0 and i1
  output reg y       // The output
);

  // The 'always @(*)' block is the circuit's brain.
  // It tells the simulator to re-evaluate the output anytime an input changes.
  always @(*)
  begin
    // This is the core logic: if 'sel' is 1, connect 'i1' to 'y';
    // otherwise, connect 'i0' to 'y'.
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end

endmodule
```


## 🧪 The Testbench File: tb_good_mux.v
This file is our test environment. It's not part of the final hardware; its only job is to apply inputs and check if the circuit works correctly during simulation.

```Verilog

// Set the time units for simulation (1ns precision).
`timescale 1ns/1ps

module tb_good_mux;

  // These signals are controlled by our testbench.
  reg i0, i1, sel;
  // This signal observes the output from our design.
  wire y;

  // This is where we "plug in" our design.
  // We're instantiating the 'good_mux' module and giving it a name 'uut' (Unit Under Test).
  good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
  );

  // The 'initial' block runs once at the beginning of the simulation.
  initial begin
    // These commands set up the waveform viewer file.
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0, tb_good_mux);

    // Initialize all our inputs to a known state.
    sel = 0; i0 = 0; i1 = 0;

    // This command tells the simulation to stop after 300 nanoseconds.
    #300 $finish;
  end

  // These 'always' blocks are our "stimulus generators."
  // They automatically create the test signals throughout the simulation.
  always #75 sel = ~sel; // Flip 'sel' every 75ns.
  always #10 i0 = ~i0;   // Flip 'i0' every 10ns.
  always #55 i1 = ~i1;   // Flip 'i1' every 55ns.

endmodule
```
▶️ Running the Simulation
Here is the three-step process to run your simulation from the command line.

Shell

# Step 1: Compile the Verilog files.
 This command takes your design and testbench and creates a simulation executable named 'a.out'.
iverilog good_mux.v tb_good_mux.v

# Step 2: Run the simulation executable.
This runs the test and generates the waveform data file ('tb_good_mux.vcd').
./a.out

# Step 3: View the waveform results.
This opens the GTKWave tool and displays the signal activity over time,
allowing you to visually verify your circuit's behavior.
gtkwave tb_good_mux.vcd



# 📖 Yosys Command Index

| Command | Purpose | Notes |
| --- | --- | --- |
| `yosys` | Start Yosys interactive shell | Must be run first |
| `read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load standard cell library (.lib) | `-lib` treats it as library cells, not design |
| `read_verilog good_mux.v` | Load the Verilog RTL design | Reads your design into Yosys |
| `synth -top good_mux` | Run synthesis | `-top` specifies the top-level module |
| `show` | Visualize design | First `show`: before tech mapping; second `show`: after ABC mapping |
| `abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Map design to actual standard cells | Uses ABC optimization & cell mapping |
| `write_verilog good_mux_netlist.v` | Export synthesized netlist (with attributes) | Includes Yosys-specific info |
| `write_verilog -noattr good_mux_netlist.v` | Export clean netlist | Attributes removed → better readability |
| `!vi good_mux_netlist.v` | Inspect netlist in terminal editor | Any editor can be used (`vi`, `nano`, `gedit`) |

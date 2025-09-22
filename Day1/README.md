# Simulator:
A software tool for testing a digital circuit's functionality by applying inputs and observing outputs. It lets us verify a design before building it.

# Design:
The actual Verilog code that describes the hardware logic. It is the blueprint of the circuit.

# Testbench:
A non-synthesizable Verilog file used only for simulation. It applies inputs (stimulus) to the design and checks for correct outputs.

# iverilog & GTKWave Flow:
A process to check a design. First, compile the design and testbench with iverilog to create an executable. Second, run the executable to generate a .vcd waveform file. Finally, use GTKWave to view the waveforms and visually debug the design's behavior.

# Synthesizer
## RTL to Gates:
A synthesizer is a tool that automatically converts your behavioral Verilog code (RTL) into a physical circuit blueprint.

## Netlist Output:
The output is called a netlist, which is a list of standard logic gates (like AND, OR, NOT) and how they are all wired together.

## Yosys: 
In our workshop, Yosys is the open-source tool we use for this process.

# Standard Cell Library (.lib)
## Gate Menu: 
The .lib file is like a menu of all the available logic gates that the synthesizer can use.

## Physical Information: 
For each gate, the library contains important details like its size (area), how fast it is (timing), and how much power it consumes.

## Technology Mapping:
The synthesizer uses this .lib file to choose the best gates to map your Verilog logic to, ensuring the final circuit meets all the design requirements.

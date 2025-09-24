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
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2b5889e9-4bea-4931-878d-0892a12f4655" />


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
<img width="782" height="572" alt="Screenshot from 2025-09-20 18-36-29" src="https://github.com/user-attachments/assets/27662471-80da-44fe-b5e7-df9f113193cf" />

### Running Good Mux
- Open Verilof files And get the good mux files
 ```
idlock@midlock-LOQ-15ARP9:~$ cd sky130RTLDesignAndSynthesisWorkshop/
midlock@midlock-LOQ-15ARP9:~/sky130RTLDesignAndSynthesisWorkshop$ ls
DC_WORKSHOP  lib  my_lib  README.md  verilog_files  yosys_run.sh
midlock@midlock-LOQ-15ARP9:~/sky130RTLDesignAndSynthesisWorkshop$ cd verilog_files/
midlock@midlock-LOQ-15ARP9:~/sky130RTLDesignAndSynthesisWorkshop/verilog_files$ iverilog good_mux.v tb_good_mux.v 
midlock@midlock-LOQ-15ARP9:~/sky130RTLDesignAndSynthesisWorkshop/verilog_files$ ./a.out
VCD info: dumpfile tb_good_mux.vcd opened for output.
midlock@midlock-LOQ-15ARP9:~/sky130RTLDesignAndSynthesisWorkshop/verilog_files$ gtkwave tb_good_mux.vcd
Gtk-Message: 14:43:05.389: Failed to load module "canberra-gtk-module"

GTKWave Analyzer v3.3.104 (w)1999-2020 BSI

[0] start time.
[300000] end time.

 ```


<img width="1148" height="262" alt="Screenshot from 2025-09-21 15-08-38" src="https://github.com/user-attachments/assets/7edfe715-f229-4399-bad9-f453b6822111" />

###  Output :

<img width="999" height="633" alt="Screenshot from 2025-09-21 15-21-34" src="https://github.com/user-attachments/assets/ccefa031-185a-449b-a60e-dacbbb8cf0a6" />


# 5-Video 
## Design File: good_mux.v

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
## Testbench File: tb_good_mux.v

```verilog
`timescale 1ns/1ps
module tb_good_mux;
  // Inputs
  reg i0, i1, sel;
  // Output
  wire y;

  // Instantiate Unit Under Test (UUT)
  good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
  );

  initial begin
    $dumpfile("tb_good_mux.vcd");  // waveform dump file
    $dumpvars(0, tb_good_mux);     // dump all signals
    // Initialize Inputs
    sel = 0; i0 = 0; i1 = 0;
    #300 $finish;                  // stop simulation
  end

  // Stimulus generators
  always #75 sel = ~sel;
  always #10 i0 = ~i0;
  always #55 i1 = ~i1;
endmodule
```
<img width="706" height="651" alt="image" src="https://github.com/user-attachments/assets/e98e545d-a7ec-4043-8564-91f91493b077" />

# 6-Video
## Yosus Setup 
<img width="1349" height="751" alt="image" src="https://github.com/user-attachments/assets/6256fc11-5890-4967-a23f-2fa0884bd2a6" />
## Synthesis Verify 
- **Synthesizer**: A tool that converts **RTL design** into a **netlist** (collection of gates + interconnections).  
- In this course, we use **Yosys** as the synthesizer.  

<img width="1377" height="767" alt="image" src="https://github.com/user-attachments/assets/5faede16-d7b5-464f-8773-4b3404b1a49b" />
<img width="1161" height="124" alt="image" src="https://github.com/user-attachments/assets/b01acc77-a721-43ca-a18f-13032a76e0cc" />

# 7-Video

<img width="675" height="709" alt="image" src="https://github.com/user-attachments/assets/162951e9-7fff-41ee-ac6b-456092f93850" />
## .LIB File 
- A **.lib** (or **Liberty**) file is a text-based format that serves as a **datasheet for standard logic gates**, containing information about their area, timing, and power, which a synthesizer uses to build a circuit.


# 8-Video 
## Slow cells 
- Slower Speed: A slow cell takes a longer time for a signal to pass through it.
- Lower Power & Area: Because they have slower switching speeds, they typically consume less power and have a smaller physical size on the chip.
<img width="1033" height="769" alt="image" src="https://github.com/user-attachments/assets/437ab06d-d5f3-44cd-bdf6-5994de0f5ae5" />

# 9-Video 
## Yosys Control Flow
### 1.Open Yosys 
<img width="801" height="538" alt="image" src="https://github.com/user-attachments/assets/cb35eb34-97a3-42d2-ba04-58eaa7e78aad" />
### 2.Reading Library & verilog file(Good Mux)
- This part took my time long enoughf i cant find my location because of duplicate folders then used help of ai to get my path
- First opend the lib file where is sky.lib file is there
- Then used pwd to get path and then got the out
  <img width="955" height="645" alt="image" src="https://github.com/user-attachments/assets/25187f7c-2c11-45fd-8592-011e780350ba" />
  <img width="722" height="163" alt="image" src="https://github.com/user-attachments/assets/9534f303-6dd0-4524-8b3a-573ae6c11c8c" />
### 3.TOP 
-The -top flag in the Yosys synth command is used to specify the name of the top-level module of your design.
-You use it because your Verilog project can have multiple modules, but only one of them is the main one that you want to synthesize into a complete circuit.

<img width="1012" height="929" alt="Screenshot from 2025-09-24 09-13-37" src="https://github.com/user-attachments/assets/305ab471-edfe-4d23-b1f6-79badb00a109" />
<img width="1012" height="855" alt="image" src="https://github.com/user-attachments/assets/4364b257-bf3b-4a2e-9f41-5326fb76cc39" />

### 4.Generate Netlist 
- We used this to generate the netlist 

```
yosys> abc -liberty /home/midlock/vlsiflow/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

<img width="1210" height="746" alt="image" src="https://github.com/user-attachments/assets/2ecebe60-55e3-4a03-b7bb-be7357484601" />ow the visual repres
### 5.Show the visual representation 
- Shows no of inputs and outputs (in netlist generation)
- show -> shows the visual representation
<img width="1210" height="302" alt="image" src="https://github.com/user-attachments/assets/969a3655-150c-4e41-824b-36f82372226d" />

<img width="607" height="262" alt="image" src="https://github.com/user-attachments/assets/c1b86472-131c-411d-9cb3-abcac30a11c3" />

# 10-Video
































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

# DFT Open-Source Tools
## 1) Fault Tool v0.9.4
Fault is a complete open source design for testing (DFT) solution. It includes automatic test pattern generation (ATPG), scan stitching, static test vector compaction, and JTAG insertion. Fault generates test vectors pseudo-randomly and performs fault simulations using the stuck-at-fault model.
### Fault is composed of five main options:
### 1) Synth (Uses inbuilt Yosys Synthesis Tool v0.53 instead of the latest v0.55)
Synth is a Yosys-based synthesis script that synthesizes and maps Verilog RTL into a flatten netlist using a standard cell library. Since Fault is compatible with any flatten netlist, this option could be skipped if the user wants to run their own synthesis script.
### 2) Cut
Cut removes the flip flops from the flatten netlist and converts it into a pure combinational design.
### 3) ATPG
ATPG is the main event. It runs Fault simulations for a generated test vector set using the stuck-at fault model. The test vector set could be supplied to the simulator externally or it could be internally generated using a pseudo-random number generator or by using Atalanta.
Fault supports two random number generators Swift system default generator and a linear feedback shift register (LFSR).
ATPG also optimizes the test vector set by eliminating redundant vectors.
### 4) Chain
Chain performs scan-chain stitching. Using Pyverilog, a boundary scan chain is constructed through a netlist’s input and output ports. An internal register chain is also constructed through the netlist’s D-flip-flops.
### 5) Tap
Tap adds the JTAG interface to a chained netlist. This is accomplished by adding its five namesake test access ports: serial test data in (TDI), test mode select (TMS) which navigates an internal finite state machine, serial test data out (TDO), test clock (TCK), and an active low test reset signal (TRST).The interface has been extended to support a custom instruction (ScanIn) to select the internal register scan chain.

# Performance-Modelling---RISC-V-processor
As part of the Computer Systems Architecture Course(ECE-GY 6913) Project I implement cycle-accurate simulator of a 32-bit RISC-V processor in 
Python.</br>
The simulator takes in two files as inputs: imem.text and dmem.txt files</br>
The simulator  gives out the following:</br>
● cycle by cycle state of the register file (RFOutput.txt)</br>
● Cycle by cycle microarchitectural state of the machine (StateResult.txt)</br>
● Resulting dmem data after the execution of the program (DmemResult.txt)</br>
The imem.txt file is used to initialize the instruction memory and the dmem.txt file is used to initialize the
data memory of the processor. Each line in the files contain a byte of data on the instruction or the data
memory and both the instruction and data memory are byte addressable. This means that for a 32 bit
processor, 4 lines in the imem.txt file makes one instruction. Both instruction and data memory are in
“Big-Endian” format (the most significant byte is stored in the smallest address).</br>
The instructions to be supported by the processor, different types of instructions and the instruction encoding are all mentioned in the RISCV-Card.</br>
The simulator has the following five stages in its pipeline:</br>
● Instruction Fetch: Fetches instruction from the instruction memory using PC value as address.</br>
● Instruction Decode/ Register Read: Decodes the instruction using the format in the RISCV-Card</br>
and generates control signals and data signals after reading from the register file.</br>
● Execute: Perform operations on the data as directed by the control signals.</br>
● Load/ Store: Perform memory related operations.</br>
● Writeback: Write the result back into the destination register. </br>
Each stage is preceded by a group of flip-flops to store the data to be passed on to the next stage in the
next cycle. Each stage contains a nop bit to represent if the stage should be inactive in the following
cycle.</br>
The simulator deals with two types of hazards.</br>
1. RAW Hazards: RAW hazards are dealt with using either only forwarding (if possible) or, if not,
using stalling + forwarding. Use EX-ID forwarding and MEM-ID forwarding appropriately.</br>
2. Control Flow Hazards: The branch conditions are resolved in the ID/RF stage of the pipeline</br>
The simulator deals with branch instructions as follows:</br>
1. Branches are always assumed to be NOT TAKEN. That is, when a beq is fetched in the IF stage, the
PC is speculatively updated as PC+4.</br>
2. Branch conditions are resolved in the ID/RF stage</br>
3. If the branch is determined to be not taken in the ID/RF stage (as predicted), then the pipeline
proceeds without disruptions. If the branch is determined to be taken, then the speculatively
fetched instruction is discarded and the nop bit is set for the ID/RR stage for the next cycle. Then
the new instruction is fetched in the next cycle using the new branch PC address.</br>

I Measured and reported average CPI, Total execution cycles, and Instructions per cycle for both these cores(single stage core and 5 stage pipelined core)
by adding performance monitors to the code.

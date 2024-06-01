# VSDSquadraonMini_Research_intern

Repository for documenting and accounting the progression of the internship.

## TASK - I

(Executed in VMware Ubuntu 18.04 environment)

### Objective

To execute a simple C program (`prog.c`), compile it, and verify the number of instructions using the RISC-V compiler. Refer to the screenshot below:

![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20211122.png)
   
The above program adds all the numbers from 1 to `n` and produces the output.

### Steps to Convert the C Program to Assembly Code

1. **Compile the Program:**

   ```sh
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o prog.o prog.c
   ls -ltr prog.o
   riscv64-unknown-elf-objdump -d prog.o
   ```

   Or use:

   ```sh
   riscv64-unknown-elf-objdump -d prog.o | less
   ```

2. **Explanation:**

   - `-march=rv64i`: Specifies the target architecture (RV64I - RISC-V 64-bit integer base ISA).
   - `-o prog.o`: Specifies the output file name (`prog.o`).
   - `prog.c`: The input C source file.

3. **Output:**

   The output file `prog.o` is created and displayed as shown below:

   ![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210910.png)

4. **Inspecting the Assembly Code:**

   Search for the `main` function using `/main` and `n` consecutively to navigate through the output:

   ![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210910.png)

   - Observe that the `main` function ends at `100b0` and starts at the instruction `10184`.
   - Subtract the start address from the end address: `100b0 - 10184 = 2C` in hexadecimal.
   - Convert `2C` to decimal and divide by 4 to get the number of instructions: `2C (hex) = 44 (dec) / 4 = 11 instructions`.

5. **Optimized Compilation:**

   Repeat the above process with the `-Ofast` optimization flag:

   ```sh
   riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o prog.o prog.c
   riscv64-unknown-elf-objdump -d prog.o | less
   ```

   The output is as follows:

   ![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210916.png)

   ![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210905.png)

   The result shows `11` instructions in the `main` function.

---

By following these steps, you can verify the number of instructions in the compiled RISC-V assembly code.

## TASK-II

MAIN AIM OF THIS TASK IS TO UNDERSTAND THE VARIOUS RISC-V INSTRUCTIONS AND WHERE IT IS BEING USED.

The documentation used is the official RISC-V spec sheet and the Green card (the card with all instructions).

The documentation used is referred [here](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/tree/main/RISC-V%20learning%20resources).

My previous encounters with these processors are documented [HERE](https://github.com/ARX-0/RISC-V_single_cycle_processor); it's a single cycle processor with a limited instruction set.

Sure! Here are the stages as bullet points:

- **IF**: Instruction Fetch
- **ID**: Instruction Decode
- **EX**: Execute
- **MEM**: Memory Access
- **WB**: Register Write Back
  

   
### **IF** (Instruction Fetch)

The PC or the program counter increments the value of the address-pointer (address) by four for every clock cycle, from the existing (current) instruction to the next instruction, thereby effectively fetching information.
This gives us how the IF (Instruction Fetch) takes place.
The below is the representation of RV32i 31 general-purpose registers x1–x31, which hold integer values. Register x0 is hardcoded to the constant 0. Here the x registers are 32 bits wide.


![the register orientation](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Screenshot%202024-05-28%20081157.png)

### **ID** (Instruction Decode)

In the (Instruction Decode) ID, it decodes the current instruction given by the current address via the PC (program counter). They are categorised into the following R, I, S, B, U, J
(types of instruction sets) following the RV32I base integer instruction set.

THE CATEGORIES THAT THE INSTRUCTION SETS ARE CATEGORISED INTO ARE REPRESENTED BELOW

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Screenshot%202024-05-28%20081711.png" alt="I type jump insrt JALR" width="665">


after DECODING into these perticular instruction sets the 32 bit instruction is then executed

### EX (Execute)
 #### R Type Instruction

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/R%20type%20instruction.png" alt="I type jump insrt JALR" width="665">


   
Here in the R type instruction. All operations read the [19:15] rs1 and [24:20] rs2 registers (the source registers)
as source operands and write the result into register [11:7] rd (the target register) .
The [31:25] funct7 and [14:12] funct3 fields select the type of operation finally the opcode for R-type instruction is [6:0].

ADD and SUB perform addition and subtraction respectively. 

SLT and SLTU perform signed and unsigned compares respectively.

{That is rd becomes (1) if rs1 < rs2 ,otherwise it is made zero (0)}.
{In the operation of SLTU the registers rd,x0,rs2 sets rd to (1) one if and only if rs2 is not equal to zero.
Otherwise rd is set to zero (0).}

AND, OR, and XOR perform bitwise logical operations.
SLL, SRL, and SRA perform logical left, logical right, and arithmetic right shifts on the value in register rs1 by the shift amount held in the lower 5 bits of register rs2.

examples:-
1. **ADD r6, r2, r1**
   - Opcode: `0110011`
   - Funct3: `000`
   - Funct7: `0000000`
   - rd: `00110`   // represent as 5'b6
   - rs1: `00001`  // represent as 5'b1
   - rs2: `00010`  // represent as 5'b2
   - Instruction Code: `0000000 00010 00001 000 00110 0110011`
     
2. **SUB r7, r1, r2**
   - Opcode: `0110011`
   - Funct3: `000`
   - Funct7: `0100000`
   - rd: `00111`
   - rs1: `00001`
   - rs2: `00010`
   - Instruction Code: `0100000 00010 00001 000 00111 0110011`
     
3. **AND r8, r1, r3**
   - Opcode: `0110011`
   - Funct3: `111`
   - Funct7: `0000000`
   - rd: `01000`
   - rs1: `00001`
   - rs2: `00011`
   - Instruction Code: `0000000 00011 00001 111 01000 0110011`
     
4. **OR r9, r2, r5**
   - Opcode: `0110011`
   - Funct3: `110`
   - Funct7: `0000000`
   - rd: `01001`
   - rs1: `00010`
   - rs2: `00101`
   - Instruction Code: `0000000 00101 00010 110 01001 0110011`
     
5. **XOR r10, r1, r4**
   - Opcode: `0110011`
   - Funct3: `100`
   - Funct7: `0000000`
   - rd: `01010`
   - rs1: `00001`
   - rs2: `00100`
   - Instruction Code: `0000000 00100 00001 100 01010 0110011`
     
6. **SLT r11, r2, r4**
   - Opcode: `0110011`
   - Funct3: `010`
   - Funct7: `0000000`
   - rd: `01011`
   - rs1: `00010`
   - rs2: `00100`
   - Instruction Code: `0000000 00100 00010 010 01011 0110011`
     
7. **SRL r16, r14, r2**
   - Opcode: `0110011`
   - Funct3: `101`
   - Funct7: `0000000`
   - rd: `10000`
   - rs1: `01110`
   - rs2: `00010`
   - Instruction Code: `0000000 00010 01110 101 10000 0110011
     
8. **SLL r15, r1, r2**
   - Opcode: `0110011`
   - Funct3: `001`
   - Funct7: `0000000`
   - rd: `01111`
   - rs1: `00001`
   - rs2: `00010`
   - Instruction Code: `0000000 00010 00001 001 01111 0110011`
     
     
#### I Type Instruction 

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/i%20type%20instruction%20decoding%20.png" alt="I type jump insrt JALR" width="665">


Here in the I type instruction we have [31:20] as the 11'b immidiate exteder value (with a sign extend) ,[19:15] the source register adress ,[14:12] function3 (which denotes the type of the function used say ADDI/SLTI/XORI etc) ,[11:7] has the adress of the source register ,[6:0] has the opcode of the immmidiate instruction set.

ADDI(increment/decrement function) adds the sign-extended 12-bit immediate to register rs1. Arithmetic overflow is ignored
and the result is simply the lower \*width of an x register\* bits of the result.

SLTI (set less than immediate) places the value 1 in register rd {if register rs1 is less than the signextended immediate when both are treated as signed numbers}, else 0 is written to rd.

 SLTIU is similar but compares the values as unsigned numbers
Its function is rd, rs1 sets rd to 1 if rs1 equals zero, otherwise sets rd to 0

ANDI, ORI, XORI are logical operations that perform bitwise AND, OR, and XOR on register rs1
and the sign-extended 12-bit immediate and place the result in rd. Note an example, XORI rd, rs1, -1 performs
a bitwise logical inversion of register rs1

rd = rs & imm ,rd = rs | imm ,rd = rs ^ imm are some examples respectively.

examples:-
1. **ADDI r12, r4, 5**
   - Opcode: `0010011`
   - Funct3: `000`
   - rd: `01100`
   - rs1: `00100`
   - imm: `000000000101`
   - Instruction Code: `0000000 000101 00100 000 01100 0010011` 

2. **LW r13, r1, 2**
   - Opcode: `0000011`
   - Funct3: `010`
   - rd: `01101`
   - rs1: `00001`
   - imm: `000000000010`
   - Instruction Code: `0000000 000010 00001 010 01101 0000011`

#### S Type Instruction

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/S%20type%20instructions%20.png" alt="I type jump insrt JALR" width="665">

The S or shift type instruction is a type of I type instruction but with the immidiate extender value bits split into two 
imm[11:5] as [31:25] and the imm[4:0] as the [24:20] instead of the conventional [31:20] being the immidiate extender.

1. **SW r3, r1, 2**
   - Opcode: `0100011`
   - Funct3: `010`
   - rs1: `00001`
   - rs2: `00011`
   - imm: `000000000010` (split as 5-bit imm[11:5] and 7-bit imm[4:0])
   - Instruction Code: `0000000 00011 00001 010 00010 0100011`

 The operand to be shiftedis in rs1, and the shift amount is encoded in the lower 5 bits of the I-immediate field. The right shift type is encoded in a high bit of the I-immediate. 

SLLI is a logical left shift (zeros are shifted into the lower bits).

SRLI is a logical right shift (zeros are shifted into the upper bits).

SRAI is an arithmetic right shift (the original sign bit is copied into the vacated upper bits).
 
#### B Type Instruction 

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/B%20type%20instruction%20.png" alt="I type jump insrt JALR" width="665">

The 12-bit B-immediate encodes signed offsets in multiples of 2, and is added to the current pc (current adress) to give the target address.

Branch instructions compare two registers. 
BEQ and BNE take the branch if registers [19:15] rs1 and [24:20] rs2 are equal or unequal respectively. 

BLT and BLTU take the branch if rs1 is less than rs2, using signed and unsigned comparison respectively.

BGE and BGEU take the branch if rs1 is greater than or equal to rs2, using signed and unsigned comparison respectively.

Note, BGT, BGTU, BLE, and BLEU can be synthesized by reversing the operands to BLT, BLTU, BGE, and BGEU, respectively.

example:-

1. **BNE r0, r1, 20**
   - Opcode: `1100011`
   - Funct3: `001`
   - rs1: `00000`
   - rs2: `00001`
   - imm: `00000000010100` (split as imm[12], imm[10:5], imm[4:1], imm[11])
   - Instruction Code: `0000000 00101 00000 001 00001 1100011`

2. **BEQ r0, r0, 15**
   - Opcode: `1100011`
   - Funct3: `000`
   - rs1: `00000`
   - rs2: `00000`
   - imm: `00000000001111` (split as imm[12], imm[10:5], imm[4:1], imm[11])
   - Instruction Code: `0000000 01111 00000 000 00000 1100011`


#### J Type Instruction

<img src="https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/j%20type%20instruction.png" alt="J Type Instruction" width="665" height="102">

In the J type instruction is of two types unconditional jumps and conditional branches.

JAL or the jump and link instruction uses the J-type format, where the J-immediate encodes a
signed offset in multiples of 2 bytes. The offset is sign-extended and added to the pc to form the
jump target address. Jumps can therefore target a ±1 MiB range. JAL stores the address of the
instruction following the jump (pc+4){the increment in pc} into register rd.We use the x1 as the return address register and x5 as an alternate link register.

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/unbrch.png" alt="I type jump insrt JALR" width="665" height="102">

JALR or the jump and link register (is an indirect jump instruction but here we use I type encoding instead of the J type encoding)

The target address is obtained by adding the 12-bit signed I-immediate to the register [19:15] rs1, then setting the
least-significant bit of the result to zero. The [14:12] function3 is set to all zeros .The address of the instruction following the jump (pc+4)
is written to register [11:7] rd. (Optional case :-Register x0 can be used as the destination if the result is not required.) the pop and push that comes along with it are out of the scope of this repo refer the resources pg 17 of the 
riscv-spec-v2.2.pdf attached above in the same repo :) 


#### U Type Instruction 

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/U%20type%20instruction.png" alt="I type jump insrt JALR" width="665">


The U type instruction has [31:12]immidiate extender ,[11:7]destination register (rd) , [6:0] opcode for the U type instruction LUI or the AUIPC .

LUI (load upper immediate) is used to build 32-bit constants,LUI places the U-immediate value in the top 20 bits of the destination register rd, filling in the lowest
12 bits with zeros.

AUIPC (add upper immediate to pc) is used to build pc-relative addresses. AUIPC forms a 32-bit offset from the 20-bit U-immediate, filling in the lowest 12 bits with zeros, adds this offset to the pc, then places the result in register rd.
   

### MEM (Memory Access)

#### Load Word Instruction & Store worrd instruction

<img src = "https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Load%20and%20Store%20instruction%20.png" alt="I type jump insrt JALR" width="665">

It basically dose read data or writes data to memory. Consisting of the above instructions LW and SW load word and store word respectively, load and store instructions transfer a value between the registers and memory. (register ---> memory)

(LOAD is encoded in I type format) where as (the STORE is encoded in S type instruction format) , 

Load is encoded in the I-type format. The effective byte address is obtained by adding register
rs1 to the sign-extended 12-bit offset. Loads copy a value from memory to register rd. Stores copy
the value in register rs2 to memoryor simply (rs2,rd = rs1 + 12'b offset imm ).

1. **LW r13, r1, 2**
   - Opcode: `0000011`
   - Funct3: `010`
   - rd: `01101`
   - rs1: `00001`
   - imm: `000000000010`
   - Instruction Code: `0000000 000010 00001 010 01101 0000011`

STORE (SW) instruction reads the lower 4 bytes of your source register rs1 and stores them into memory at the address given in the destination operand

### **WB**: Register Write Back

 The Register Write Back (WB) stage is the final step in the execution of an instruction in the RISC-V pipeline. During this stage, the result of the instruction's execution is written back to the register file, making the result available for future instructions. The WB stage is essential for ensuring that the computations performed by the processor are correctly stored and accessible.

Once the data is selected, it is written to the specified destination register (rd). The register file is updated to reflect the new value.
The control signals ensure that the correct register is updated with the correct data.

Example: ADD rd, rs1, rs2
In the WB stage, the result from the ALU is written back to the destination register rd.

Example: LW rd, offset(rs1)
In the WB stage, the data read from memory is written to the destination register rd.

## TASK III

### Installation of required softwares GTKwave,iverilog.

 To install iverilog
                      
      git clone https://github.com/steveicarus/iverilog.git
      cd iverilog
      sh autoconf.sh //To configure the iverilog in your local repository run the following command. 

Autoconf in root…// shown succesfull else try the following command and repeat the above steps

    sudo apt-get install autoconf

Then configure the files

      ./configure

Finally install GTKwave and iverilog
    

         //sudo apt-get install package_name in ubuntu

       sudo dnf install flex 
       sudo dnf install gcc
       sudo dnf install bison
       sudo dnf install g++
       sudo su
       make install
       sudo dnf install iverilog
       sudo dnf install gtkwave 

then we are set to go 
 
use to clone the RISC-V core repository 

     git clone https://github.com/vinayrayapati/iiitb_rv32i
     cd iiitb_rv32i

now in the codes of the RISC-V repo we get to see the memory (the instructions in the memory)

![](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/waveforms_outputs/codes.png)

it is as given in the below HTML TABLE 

<table>
  <tr>
    <th>Instruction</th>
    <th>Hexadecimal</th>
    <th>Binary</th>
  </tr>
  <tr>
    <td>add r6, r1, r2</td>
    <td>32'h02208300</td>
    <td>0000_0010_0010_0000_1000_0011_0000_0000</td>
  </tr>
  <tr>
    <td>sub r7, r1, r2</td>
    <td>32'h02209380</td>
    <td>0000_0010_0010_0000_1001_0011_1000_0000</td>
  </tr>
  <tr>
    <td>and r8, r1, r3</td>
    <td>32'h0230a400</td>
    <td>0000_0010_0011_0000_1010_0100_0000_0000</td>
  </tr>
  <tr>
    <td>or r9, r2, r5</td>
    <td>32'h02513480</td>
    <td>0000_0010_0101_0001_0011_0100_1000_0000</td>
  </tr>
  <tr>
    <td>xor r10, r1, r4</td>
    <td>32'h0240c500</td>
    <td>0000_0010_0100_0000_1100_0101_0000_0000</td>
  </tr>
  <tr>
    <td>slt r11, r2, r4</td>
    <td>32'h02415580</td>
    <td>0000_0010_0100_0001_0101_0101_1000_0000</td>
  </tr>
  <tr>
    <td>addi r12, r4, 5</td>
    <td>32'h00520600</td>
    <td>0000_0000_0101_0010_0000_0110_0000_0000</td>
  </tr>
  <tr>
    <td>sw r3, r1, 2</td>
    <td>32'h00209181</td>
    <td>0000_0000_0010_0000_1001_0001_1000_0001</td>
  </tr>
  <tr>
    <td>lw r13, r1, 2</td>
    <td>32'h00208681</td>
    <td>0000_0000_0010_0000_1000_0110_1000_0001</td>
  </tr>
  <tr>
    <td>beq r0, r0, 15</td>
    <td>32'h00f00002</td>
    <td>0000_1111_0000_0000_0000_0000_0000_0010</td>
  </tr>
  <tr>
    <td>add r14, r2, r2</td>
    <td>32'h00210700</td>
    <td>0000_0000_0010_0001_0000_0111_0000_0000</td>
  </tr>
  <tr>
    <td>bne r0, r1, 20</td>
    <td>32'h01409002</td>
    <td>0001_0100_0000_1001_0000_0000_0000_0010</td>
  </tr>
</table>


   ##### THE OVERALL waveform for the above table is given below 

   ![](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/waveforms_outputs/Screenshot%20from%202024-06-01%2019-59-34.png)

for the waveform marked in purple 0->ADD,1->SUB,2->AND,3->OR,4->XOR,5->SLT,0->ADDI,1->LW,0->SW,0->BEQ


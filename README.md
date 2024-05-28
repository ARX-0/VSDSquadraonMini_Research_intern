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

   
![ALL BASIC INSTRUCTION SETS IN RISC-V ](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Screenshot%202024-05-28%20081711.png)


after DECODING into these perticular instruction sets the 32 bit instruction is then executed

### EX (Execute)
 #### R Type Instruction

   
![R type instruction](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/R%20type%20instruction.png)

   
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

#### I Type Instruction 

![ I Type Instruction](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/i%20type%20instruction%20decoding%20.png)

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

#### U Type Instruction 

![](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/U%20type%20instruction.png)

The U type instruction has [31:12]immidiate extender ,[11:7]destination register (rd) , [6:0] opcode for the U type instruction LUI or the AUIPC .

LUI (load upper immediate) is used to build 32-bit constants,LUI places the U-immediate value in the top 20 bits of the destination register rd, filling in the lowest
12 bits with zeros.

AUIPC (add upper immediate to pc) is used to build pc-relative addresses. AUIPC forms a 32-bit offset from the 20-bit U-immediate, filling in the lowest 12 bits with zeros, adds this offset to the pc, then places the result in register rd.


   




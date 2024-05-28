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
  
<div align = "center" >
   
#### **IF** Instruction Fetch

The PC or the program counter increments the value of the address-pointer (address) by four for every clock cycle, from the existing (current) instruction to the next instruction, thereby effectively fetching information.
This gives us how the IF (Instruction Fetch) takes place.

#### **ID** Instruction Decode

In the (Instruction Decode) ID, it decodes the current instruction given by the current address via the PC (program counter). They are categorised into the following R, I, S, B, U, J
(types of instruction sets) following the RV32I base integer instruction set.

THE CATEGORIES THAT THE INSTRUCTION SETS ARE CATEGORISED INTO ARE REPRESENTED BELOW

![ALL BASIC INSTRUCTION SETS IN RISC-V ](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Screenshot%202024-05-28%20081711.png)

after DECODING into these perticular instruction sets the 32 bit instruction is then executed

#### EX (Execute)

</div>

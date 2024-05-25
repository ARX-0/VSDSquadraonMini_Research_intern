# VSDSquadraonMini_Research_intern
VSDSquadraonMini_Research_intern_repo for documenting/accounting the progression of the internship

# TASK - I 

Here we execute a simple C program prog.c, compile it and verify the number of instructions using the RISC-V compiler
refer the below screenshot:- 

![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20211122.png)

the above programme adds all the numbers form 1 to n and produces the output
Now we aim on converting the C programme to assembly code, the process goes as follows:

    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o prog.o prog.c
    ls -ltr prog.o
    riscv64-unknown-elf-objdump -d prog.o
or use " riscv64-unknown-elf-objdump -d prog.o | less "
### Explaination:

"-march=rv64i" : Specifies the target architecture, in this case, RV64I (RISC-V 64-bit integer base ISA).

"-o prog.o" : Specifies the output file name, prog.o in this case.

"prog.c" : The input C source file.

The ouput folder prog.o is created and gets opened as shown below:

![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210910.png)

search for the main using /main and n consecutively with the below output: 

![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210910.png)

now we observe that the main ends at 100b0 and starts at the instruction 10184 
end - start gives us the value of 2C in the programer mode of calculator at HEX mode which is divided by 4 to give the number of instructions which is 11.

similarly when using the (Ofast instead of O1)  
      
         riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o prog.o prog.c
         riscv64-unknown-elf-objdump -d prog.o | less
and repeating the above process we get the following output 
![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210916.png)

![Annotated Image](https://github.com/ARX-0/VSDSquadraonMini_Research_intern/blob/main/images/Annotation%202024-05-25%20210905.png)

it gives the ouput 11 in DEC which is the number of instructions observed in the <main>






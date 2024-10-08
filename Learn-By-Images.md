# Priority
1) Mal Dev
2) Rev Eng
3) Mal Ana

## Tools:
- Listed later ....


---
![mal1](https://github.com/user-attachments/assets/b05fc2e3-1b30-4cd9-8b1c-f716a08ccd0c)

Description: 
This image illustrates the relationship between high-level code, machine code, and low-level assembly code, showing how a malware author and a malware analyst interact with different representations of code.

1. **Malware Author/Developer (High-Level Language)**:
   - On the left side, the malware author writes code in a high-level programming language, like C. An example code snippet is shown:
     ```c
     int c;
     printf("Hello.\n");
     exit(0);
     ```
   - This code is easier for humans to write and understand.

2. **Compiler**:
   - The compiler converts the high-level code into machine code (binary instructions) that the CPU can execute. 
   - The machine code is represented in hexadecimal format as shown: `55`, `8B EC`, `8B EC 40`. These are CPU instructions specific to the architecture.

3. **CPU (Machine Code)**:
   - The CPU processes the machine code, which is the binary equivalent of the high-level instructions.

4. **Malware Analyst (Low-Level Language)**:
   - On the right side, the malware analyst uses a disassembler to convert the machine code back into assembly language (low-level language).
   - The disassembler output is shown as:
     ```
     push ebp
     move ebp, esp
     sub esp, 0x40
     ```
   - This representation is closer to the CPU's operations but still readable to humans with knowledge of assembly language.

This diagram emphasizes the workflow of malware development and analysis, demonstrating how high-level code is transformed into machine code and then analyzed at the assembly level.


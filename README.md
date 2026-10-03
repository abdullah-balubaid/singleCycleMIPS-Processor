# singleCycleMIPS-Processor

The goal of this project is to create a single cycle processor based on the MIPS architecture that handles all the Instruction Set Architecture.



The circuit is divided into multiple sub-circuits:

* Main:
    It contains all the sub-circuits and handle the external input/output connection

* ALU (Arithmetic and Logical Unit)
    This unit purpose is to operate the arithmetic operations like adding, subtracting, and the logical operations like anding, oring, noring, xoring, setting the output 1 is first input is less than the second input (slt), as well as other opeations like shifting right or left whether it was arithmetically or logically.

    What drives this unit is a MUX controlled by the 4-bit input "ALU control" which dictates which operation is chosen by the ROM instruction code
    ADD 0000,

    SUB 0001,

    AND 0010,

    OR 0011,

    NOR 0100,

    SLT 0101,

    XOR 0110,

    SLL 0111,

    SRL 1000,

    SRA 1001.

    And this MUX of course is taking two variable input (the operation differ) to produce the output based on the operation chosen.

    Also, this unit produces special flags such as Zero, Negative, Carry, and Overflow flag that would become very handy in other instructions.


* Register File
    This unit contains all the 32 x 32-bit registers, it is mainly focus on handing the input and output delivery among the 32 registers after each edge cycle (raising edge in this case) where Register 0 is hardwired to 0 to be helpful in assigning values instructions and other functions. The output value is controlled by the Register Write signal which will determine the exact register that will be written as there must be only one register written in one cycle. The register's output is wired to both the first input and the second input to deliver the read the data coming from each register, but based on the reading signal of each input.

* Data Memory (RAM: Random Access Memory)
  Focuses on storing the data from the register to the RAM'S address.
  As well as, on loading the data from the RAM'S address to the register.

* Instruction Mmeory and PC

   * Instruction Memory (ROM: Read Only Memory)
      As the name implies, the purpose of using the instruction memory is to store and operate the instruction code and it is controlled by the PC (Program counter) so when the counter runs over an instruction (when it is pointing to this instruction and the clk pulses up) the instruction is performed, and will produce a value for each parameter based on the MIPS architecture that will later on drive each parameter using the control unit.

  * PC (Program Counter)
    Its job is very simple, it is controlled by a MUX signal (JUMP:BRANCH)

    00 -> PC = PC + 1 (Normal case)

    01 -> PC = PC + 1 + Immediate value (case of BRANCH)

    10 -> PC = Immediate value (case of JUMP)

    11 -> unused

    then send it to the ROM.

* Branch Unit
  Based on the flags: Zero and Negative flags and which branch the user chose, perform the branch as either

  Assuming A=First input, B=Second input
  
  BEQ: Branch if equal (A-B =0 --> A=B) 

  BNE: Branch if not equal (A-B ~=0 --> A ~= B) 

  BLTZ: Branch if less than 0 (A-B < 0 --> A < B)

  BLEZ: Branch if less than OR equal 0 (A-B < 0 OR A-B = 0 --> A < B OR A = B)

  BGTZ: Branch if greater than 0 (A-B > 0 --> A > B)

  BGEZ: Branch if greater than OR equal 0 (A-B > 0 OR A-B = 0 --> A > B OR A=B)

* Control Unit
  Mainly focuses on generating the control signals (converting the MIPS parameters to the circuit normal parameters)


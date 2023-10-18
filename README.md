## MYTH RISC-V Workshop
Navigate to [Makerchip](https://www.makerchip.com/sandbox/)  IDE
Within this repository, you'll discover comprehensive materials pertaining to the MYTH (Microprocessor for You in Thirty Hours) Workshop, a 5-day program focused on RISC-V CPU Core Design. This workshop is a collaborative effort between VLSI System Design (VSD) and Redwood EDA. Over the course of just five days, participants delve into the fundamentals of the RISC-V ISA and go on to construct a simplified RISC-V core featuring the core instruction set. The RISC-V CPU Core is meticulously crafted using Transaction Level Verilog (TL-Verilog) in conjunction with the Makerchip IDE Platform. Detailed information is provided below for your reference.



Makerchip provides free and instant access to the latest tools directly from your browser and from your desktop. This includes open-source tools and proprietary ones. Turning the tables for the open-source community, Redwood EDA, LLC's commercial capabilities are often available for open-source development here first--before they are available commercially.

- **SystemVerilog Support**: Makerchip allows users to write and simulate digital designs using the SystemVerilog hardware description language, making it suitable for both     beginners and experienced designers.
- **RISC-V and Custom Microprocessor Design**: It provides tools for designing custom RISC-V processors or creating entirely new microprocessor architectures.
- **Code Simulation**: Users can simulate their designs to test functionality and identify potential issues before moving on to the actual hardware implementation.
- **Collaboration**: Makerchip supports collaborative work, enabling multiple users to work on the same project simultaneously.
- **Online Platform**: As a web-based tool, Makerchip eliminates the need for users to install and maintain specialized software, making it accessible from various   devices with an internet connection.

<details>
<summary>DAY-3 : Digital Logic with TL-Verilog in Makerchip IDE</summary>
<br>

#### Task-2 : Lab - Makerchip
To use Makerchip IDE, you need to visit makerchip website at [http://makerchip.com/](http://makerchip.com/) and launch Makerchip IDE
To access a specific example, please follow these steps:
1) **Navigate to the 'Learn' section**
2) **Click on 'Examples'**
3) **Load 'FGPA Multiplier' Example**

**B) XOR Gate**
```
$out = ! $in;
$out1 = ($in1 ^ $in2);
```
![B](https://github.com/dsingla54/dhruv_riscv/assets/139515749/c004943a-8f90-4950-919f-1efa3b23cc8d)




**C) Vectors**
```
$out[4:0] = $in1[3:0] + $in2[3:0];
```
![C](https://github.com/dsingla54/dhruv_riscv/assets/139515749/00d34687-209d-43cc-b384-5a342c2b9aa5)


**D) Mux without vector & with vectors**

```
$out = $sel ? $in1 : $in2;
```
![D](https://github.com/dsingla54/dhruv_riscv/assets/139515749/a07bc1bb-739e-4b60-b11d-3801c37dadc9)


**E) Simple Claculator**

```
$val1[31:0] = $rand1[3:0]; 
$val2[31:0] = $rand2[3:0];
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum);
```
![E](https://github.com/dsingla54/dhruv_riscv/assets/139515749/dc895ba9-ed68-49ff-9f94-f1fbdeae5cba)


#### Task-4 : Sequential logic 
```
$fib[31:0] = $reset ? 1 : (>>1$fib + >>2$fib); 
```
![F](https://github.com/dsingla54/dhruv_riscv/assets/139515749/d6bc873b-40cb-4060-b6ca-388146b76539)


**B) Up-Counter**

```
$num[2:0] = $reset ? 0 : (>>1$num + 1); 
```


![G](https://github.com/dsingla54/dhruv_riscv/assets/139515749/f9a74994-f7ed-4387-9938-02a75b23576a)


**C) Sequential Calculator**

```
$val1[31:0] = (>>1$out); 
$val2[31:0] = $rand2[3:0]; 
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum); 
```


![H](https://github.com/dsingla54/dhruv_riscv/assets/139515749/8d62ce88-b3b8-43f2-a617-15a44d9862d1)



#### Task-5 : Pipelined logic

```
`include "sqrt32.v"
|calc
      @1
         $aa_sq[31:0] = $aa[3:0] * $aa;
         $bb_sq[31:0] = $bb[3:0] * $bb;
      @2
         $cc_sq[31:0] = $aa_sq + $bb_sq;
      @3
         $cc[31:0] = sqrt($cc_sq);
```


![I](https://github.com/dsingla54/dhruv_riscv/assets/139515749/18cce82a-976c-4131-8092-5cf3210087d7)



**Pipeline Implementation**

```
|comp
      @1
         $err1 = $bad_input || $illegal_op;
      @2
         $err2 = $err1 || $over_flow;
      @3
         $err3 = $div_by_zero || $err2;
```


![J](https://github.com/dsingla54/dhruv_riscv/assets/139515749/e018173f-f70a-4e29-9c31-2cbaf9122797)



#### Task-6 : Validity
+ Easier debug
+ Cleaner design
+ Better error checking
+ Automated clock gating

**2 cycle calculator with validity**

```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $out [31:0] = $reset ? 32'b0 :
                      ($op[1:0] == 2'b00) ? $sum :
                      ($op[1:0] == 2'b01) ? $diff :
                      ($op[1:0] == 2'b10) ? $prod :
                                              $qut ;
```

![K](https://github.com/dsingla54/dhruv_riscv/assets/139515749/cc5c90db-1f07-43a8-98ef-a8c609ce4db7)



**Distance Calculator**
```
|calc
      @1
         $reset = *reset;
         
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;;
         @3
            $cc[31:0] = sqrt($cc_sq);
      @4
         $total_distance[63:0] =
            $reset ? 0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
```
![L](https://github.com/dsingla54/dhruv_riscv/assets/139515749/9d13e505-37bf-466e-ae3b-c71af2f4bbe4)


**Calulator Memory**
```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $mem[31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
         
         $out [31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b000) ? $sum :
                      ($op[2:0] == 3'b001) ? $diff :
                      ($op[2:0] == 3'b010) ? $prod :
                      ($op[2:0] == 3'b011) ? $qut  :
                      ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
```


![M](https://github.com/dsingla54/dhruv_riscv/assets/139515749/caf22bf8-c235-41d4-8751-13d4387b75e5)

</details>

<details>

<summary>DAY 4 : Basic RISC-V CPU Micro Architecture</summary>
<br>

## Overview
This RISC-V Architecture Block Diagram illustrates the fundamental components and their interactions within a computer system based on the RISC-V instruction set architecture. RISC-V is a modular and customizable architecture, providing a versatile framework for designing processors tailored to specific application requirements.

## Components
1. **CPU (Central Processing Unit)**
   - *Description*: The CPU serves as the core of the RISC-V processor, responsible for executing instructions. It includes multiple stages:
     - Instruction Fetch (IF): Fetches instructions from memory.
     - Instruction Decode (ID): Decodes the fetched instructions.
     - Execution (EX): Performs arithmetic and logic operations.
     - Memory (MEM): Manages data memory access.
     - Write Back (WB): Writes results back to registers.

2. **Instruction Memory**
   - *Description*: This memory component stores the program's instructions that the CPU fetches and executes. It's essential for the program's proper execution.

3. **Data Memory**
   - *Description*: Data Memory stores data used by the CPU during program execution. It is crucial for data manipulation and storage.

4. **Registers**
   - *Description*: Registers are a set of general-purpose storage units used for temporary data storage and manipulation by the CPU. They play a pivotal role in instruction execution.

5. **Control Unit**
   - *Description*: The Control Unit manages control signals and coordinates the activities of the CPU's components, ensuring the proper execution of instructions.

6. **ALU (Arithmetic Logic Unit)**
   - *Description*: The ALU performs arithmetic and logic operations as directed by the CPU's instructions. It is the computational workhorse of the processor.

7. **Instruction Decoder**
   - *Description*: The Instruction Decoder interprets and decodes instructions fetched from memory. It translates instructions into actions for the CPU to execute.

8. **Cache Memory**
   - *Description*: Cache Memory provides fast access to frequently used instructions and data. It helps improve the system's overall performance by reducing memory access times.

9. **Bus Interface**
   - *Description*: The Bus Interface facilitates data transfer between the CPU, memory, and peripherals. It ensures efficient communication within the system.

10. **Peripherals**
    - *Description*: Peripherals are external devices such as input/output controllers, timers, and more. They connect to the CPU, enhancing the system's functionality by allowing interaction with the outside world.

For the consecutive labs, we will use the "RISC-V lab starting point code" from https://github.com/stevehoover/RISC-V_MYTH_Workshop.

Use the following links : [Link for the starter code](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Frisc-v_shell.tlv#)

#### Task-1 : Program Counter
![1](https://github.com/dsingla54/dhruv_riscv/assets/139515749/538ebaf9-7ee4-4e4c-9d2a-6739cf90752f)




#### Task-2 : Instruction Fetch

![2](https://github.com/dsingla54/dhruv_riscv/assets/139515749/630949bb-5da3-48b2-9daf-5b0ca432a489)



#### Task-3 : Instruction Decode

![3](https://github.com/dsingla54/dhruv_riscv/assets/139515749/94bc6f2b-49f6-45a0-971a-9c4794d5a346)



#### Task-4 : Instruction Decode with validity

![4](https://github.com/dsingla54/dhruv_riscv/assets/139515749/e99d0c4e-c901-46ff-abc5-a300045d5ca0)



#### Task-5 : Individual Instruction decode

![5](https://github.com/dsingla54/dhruv_riscv/assets/139515749/ae871545-330f-4c40-8f76-b4cbb30617ab)



#### Task-6 : Register File Read

![6](https://github.com/dsingla54/dhruv_riscv/assets/139515749/49b2fe71-d75a-4a60-818f-63f2fc9f85b7)



#### Task-7 : ALU

![7](https://github.com/dsingla54/dhruv_riscv/assets/139515749/38acb290-b4ad-4ffb-9949-98ad3fb64f53)



#### Task-8 : Register File Write

![8](https://github.com/dsingla54/dhruv_riscv/assets/139515749/e58daf40-d20b-4e17-acdf-41669bc1fc4b)



#### Task-9 : Branch Instructions

![9](https://github.com/dsingla54/dhruv_riscv/assets/139515749/2700a34b-e388-4a46-8bab-24767948995d)



#### Task-10 : Testbench to check functionality
![10](https://github.com/dsingla54/dhruv_riscv/assets/139515749/e5ad0afb-41f7-4f20-9561-56738021ac10)

</details>

<details>
<summary>DAY 5 : Complete Pipelined RISC-V Micro Architecture </summary>
<br>

- Pipelining helps improve the operating frequency by breaking down the micro-arch ito substages that consume lesser time.But this process intriduces some hazards and dependencies such as
  - Data Hazards
  - Structural Hazards
  - Control Hazards
  - Name Dependence
  - Anti Dependence
  - Output Dependence

 ## 3-cycle RISC-V

 - A simple pipeline approach where we divide the arch into 3 stages ie., PC, Decode to ALU, Reg write.
 - This requires a valid signal that is generated every 3 clock cylces

### Generation of 3 cycle valid signal

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/781aab6c-a300-4b0a-b133-1780624399ed)



- There might be some invalid cycles (invalid operation when valid is on) being encountered in this proccess. SO we have to take care of them.

### 3-cycle RISC-V To take care of invalid signals

- Avoid writing into register file for invalid operations
- Avoid redirecting PC for nvalid instructions(branch)

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/71d1a8c4-f1e7-4277-8f82-a3161e18251a)



### Introduce 5 stage pipeline 

- 5 stage pipeline : PC, Decode, Reg Rd, ALU, Reg write

## Solutions to Pipeline Hazards

1. Register file bypass

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/f6fb6960-b14c-4352-a1db-f5583bacbdb2)



2. Correct the branch target path

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/151870cf-39ce-42db-8a4c-fb5d5df02ed1)



3. Complete Instruction Decode


![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/2ed6bacb-225c-4b49-920e-7f3bac75c39d)


4. Complete ALU

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/ed162772-93e0-499e-8e59-e005037ca0c4)



## Completing RISC-V CPU with final touch of Load/Store Instructions

1. Redirecting Loads


![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/6db9c874-60a6-46a2-8d2f-e977b809a6fd)


2. Load Data from Memory to register file


![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/4ca4df40-697f-4503-af40-2ac144608374)


3. Instantiate Data memory to CPU

![image](https://github.com/dsingla54/dhruv_riscv/assets/139515749/638b3928-b4b6-4c9f-b368-ffc9ae03bbc3)



4. Add loads and stores to test the program

```
m4_asm(SW r0, r10, 100)
m4_asm(LW r15, r0, 100)
```
5. Lab for jump instructions

![10](https://github.com/dsingla54/dhruv_riscv/assets/139515749/81cdb5ff-b287-4c1a-9b18-f7d0cbf69442)



</details>

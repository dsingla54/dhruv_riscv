## MYTH RISC-V Workshop
Navigate to [Makerchip](https://www.makerchip.com/sandbox/)  IDE
Within this repository, you'll discover comprehensive materials pertaining to the MYTH (Microprocessor for You in Thirty Hours) Workshop, a 5-day program focused on RISC-V CPU Core Design. This workshop is a collaborative effort between VLSI System Design (VSD) and Redwood EDA. Over the course of just five days, participants delve into the fundamentals of the RISC-V ISA and go on to construct a simplified RISC-V core featuring the core instruction set. The RISC-V CPU Core is meticulously crafted using Transaction Level Verilog (TL-Verilog) in conjunction with the Makerchip IDE Platform. Detailed information is provided below for your reference.



Makerchip provides free and instant access to the latest tools directly from your browser and from your desktop. This includes open-source tools and proprietary ones. Turning the tables for the open-source community, Redwood EDA, LLC's commercial capabilities are often available for open-source development here first--before they are available commercially.

- **SystemVerilog Support**: Makerchip allows users to write and simulate digital designs using the SystemVerilog hardware description language, making it suitable for both     beginners and experienced designers.
- **RISC-V and Custom Microprocessor Design**: It provides tools for designing custom RISC-V processors or creating entirely new microprocessor architectures.
- **Code Simulation**: Users can simulate their designs to test functionality and identify potential issues before moving on to the actual hardware implementation.
- **Collaboration**: Makerchip supports collaborative work, enabling multiple users to work on the same project simultaneously.
- **Online Platform**: As a web-based tool, Makerchip eliminates the need for users to install and maintain specialized software, making it accessible from various   devices with an internet connection.


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
![B](https://github.com/ramdev604/ramdev_riscv/assets/43489027/3e441e19-8f0f-431c-9b02-e6d01f06eab8)



**C) Vectors**
```
$out[4:0] = $in1[3:0] + $in2[3:0];
```
![C](https://github.com/ramdev604/ramdev_riscv/assets/43489027/c50cfb6b-c373-4c78-8399-9925f91f9695)

**D) Mux without vector & with vectors**

```
$out = $sel ? $in1 : $in2;
```
![D](https://github.com/ramdev604/ramdev_riscv/assets/43489027/c37ffbd6-c070-4077-83af-6fe5416e2932)


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
![E](https://github.com/ramdev604/ramdev_riscv/assets/43489027/0cd4df26-c7a7-49d6-9265-f5d7c6a25b31)

#### Task-4 : Sequential logic 
```
$fib[31:0] = $reset ? 1 : (>>1$fib + >>2$fib); 
```
![F](https://github.com/ramdev604/ramdev_riscv/assets/43489027/91cc0b90-5233-488c-9e07-90ff26243579)

**B) Up-Counter**

```
$num[2:0] = $reset ? 0 : (>>1$num + 1); 
```


![G](https://github.com/ramdev604/ramdev_riscv/assets/43489027/eff2bd80-dd0e-4090-a220-0331e5b8b5ab)

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


![H](https://github.com/ramdev604/ramdev_riscv/assets/43489027/ef7c26e6-a1c3-45d9-8860-26471b65a572)


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


![I](https://github.com/ramdev604/ramdev_riscv/assets/43489027/c0504f06-eb5a-4875-ab2f-f7bea40e07ef)


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


![J](https://github.com/ramdev604/ramdev_riscv/assets/43489027/4967a2af-bf07-4566-bfb6-9922952174e0)


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

![K](https://github.com/ramdev604/ramdev_riscv/assets/43489027/d7ae3d73-f6d0-49ee-b21f-a00526f193ce)


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
![L](https://github.com/ramdev604/ramdev_riscv/assets/43489027/e03dbae4-05ed-4cd5-a7c6-ec5580c48d6f)

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


![M](https://github.com/ramdev604/ramdev_riscv/assets/43489027/c93d95d8-7754-4a91-a12d-090f305c293b)
</details>

<details>

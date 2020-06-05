# MIPS-multi-cycle
MIPS multi cycle Verilog implementation based on Computer Organization and Design by David A. Patterson and John L. Hennessy

##Overview
The implementation supports multiple cycle per instruction add, sub, lw, sw, beq and slt.
The memory is structured in 32-bit words.

Branches are finished in 3 cycles, R-type (ALU) instructions in 4, Stores (sw) in 4, Loads (lw) in 5.

There is only one memory which contains both data and instruction memory. 
The starting address of the instruction memory is PC_START = 128.
```
add $t0, $zero, $zero
add $t6, $zero, $zero

lw $t1, 64($t0)
lw $t2, 68($t0)
lw $t3, 72($t0)
sw $zero, 76($t0) #the sum will be at this location [76]
loop:
lw $t4, 0($t0)
lw $t5, 76($t6)
add $t5, $t5, $t4
sw $t5, 76($t6)
sub $t1, $t1, $t2
add $t0, $t0, $t3
beq $t1, $zero, done
beq $t1, $t1, loop #actually jump (because $t1 = $t1)
done:
#end
```
The program computes the sum of the first 16 values from the data memory. 
The result will be 5 and will be located in the data memory.

#Tools
Modelsim was used for simulation. 

Check Report.pdf for more explanation

#examples

To add all the numbers from 5 to 1 (Sigma 5)
```
20090005 
200A0001
200B0000
01695820
012A4822
11200001
08000083
```
```
ADDI $t1 $zero 0x0005
ADDI $t2 $zero 0x0001
ADDI $t3 $zero 0x0000  
ADD $t3 $t3 $t1	gh	//83
SUB $t1 $t1 $t2
BEQ $t1 $zero 0x0001
J 0x0000083
```
Infinate Fibonacci series

```
20090000
200A0001
200B0000
200C0000
01495820
AD8B0020
01604820
218C0001
01495820
AD8B0020
01605020
218C0001
08000084
```
```
ADDI $t1 $zero 0x0000
ADDI $t2 $zero 0x0001
ADDI $t3 $zero 0x0000  
ADDI $t4 $zero 0x0000  
ADD $t3 $t2 $t1		//84
sw t3 0x20(t4)
ADD $t1 $t3 $zero
addi t4 t4 1
ADD $t3 $t2 $t1	
sw t3 0x20(t4)	
ADD $t2 $t3 $zero
addi t4 t4 1
J 0x0000084
```
Sort the numbers decreasing order:-
```
20170060
20100000
20160003
20110000
8E280060
8E290061
0109582A
11600002
AE290060
AE280061
22310001
02D0A822
12350001
08000084
22100001
20110000
12160001
08000084
```
```
	addi s7 zero 60
	addi $s0,zero 0
	addi $s6,zero 9 #N-1
	addi $s1,zero 0 #j
84:

	lw $t0, 60($s1)  #$t0 is A[j]	
	lw $t1, 61($s1)  #$t1 is A[j+1]	
	slt t3 t0 t1
	beq t3 zero 0x2
	sw $t1, 60($s1)  #$t1 is A[j]	
	sw $t0, 61($s1)  #$t0 is A[j+1]	
90:
	addi $s1, $s1, 1
	sub $s5, $s6, $s0 #$s5 is N-i-1
	beq s1 s5 0x1
	J 84
	addi $s0, $s0, 0x1 
	addi $s1, zero,0x0 #j
	beq s0 s6 0x1
	J 84
```	

Sort in ascending order

```
20170060
20100000
20160003
20110000
8E280060
8E290061
0128582A
11600002
AE290060
AE280061
22310001
02D0A822
12350001
08000084
22100001
20110000
12160001
08000084
```
Reverse an array 


```
20100003 
20130002
20110000
20140001
02309020
8E280060
8E490060
AE290060
AE480060
02749822
02348820
02549022
12600001
08000085
```

N-1
N/2

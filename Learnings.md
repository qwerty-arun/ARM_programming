# Day-1
- Each hex value -> 4 bits of data
- 8 of them -> 32 bits -> 1 word
- Memory layout too works in HEX
- Introduction to SWI - Software interrupt

- First Program:
```
MOV R0,#30
MOV R7,#1 ; telling the program to terminate here 
SWI 0
```
- Observations: '1e' will be stored in R0 and PC also changes from 04 to 08 and so on. CPSR and SPSR's final values are '01d3'. </br>

# Day-2
- Actual function of LDR: Load data from stacks onto registers whereas 'MOV' just assigns direct values.
- `LDR R0,[PC,#4]` , `LDR R1,[R0]` AND `LDR R2,[R0,#4]!` where '!' is fro pre-increment option.
- Second Program:
```
MOV R0,#5
MOV R1,#7
SUB R2,R0,R1 ;R2=R0-R1
```
- R2 will hold the value `ffff fffe` which in decimal is -2.
- If R1 was 6, the value would be `ffff ffff`
- If R1 was 8, the value would be `ffff fffd`
- CSPR was `1d3` for all the cases.
- If R0 would be `ffff ffff` and R1 be `1`, then the value of R2 would be `ffff fffe`
- The question arises: Is it a negative number or a very large number.
- Now, to visualize this, we need CPSR register. </br>

# Day-3
- Arithmetic and CPSR flags: ADDS, SUBS, ADC (r2=r0+r1+carry and carry equals zero if flag is zero and equals one if flag is one.
- Logical operations:
```
MOV R0,#0xff
	MOV R1,#22
	AND R2,R0,R1 // answer will be the same as 22
```
- OR operation: ORR is the command
- XOR : EOR
- Negate: MVN (move negative)
```
MOV R0,#0xff
	MVN R0,R0
```
The answer will be r0 = 0xffffff00 (The operation is performed on the entire register and not just on the two bits).
How to get only zeroes in the output? Well use AND R0,R0,#0x000000ff </br>

# Day-4
- Logical shift operations: LSL and LSR which in decimal means "multiply by 2" and "divide by 2" respectively.
- Using the command MOV along with other commands in a single line.
```
MOV R0,#10
MOV R1,R0,LSL #1
```
The above code snippet moves the value 10 to R0 register. Then, the same value is moved to the register R1 and then logical left shift is performed '1' times.
- Conditional Statements: BNE, BEQ, BLE, BGE, BGT, BLT etc.
```
MOV R0,#1
MOV R1,#2
CMP RO,R1
BGT greater
MOV R2,#2
greater:
	MOV R2,#1
```
- After execution, the negative flag will be set and the Branch statement is not executed and the control moves  to `MOV R2,#2`.
- If R0 were '3' , then branch statement will be executed and the control moves to the label `greater`.
- In ARM, execution happens sequentially, so we would end up in 'greater' label anyways.
```
MOV R0,#1
MOV R1,#2
CMP RO,R1
BGT greater
BAL default
greater:
	MOV R2,#1
default:
	MOV R2,#2
```
- BAL: Branch Always can be used with the default case. But the problem here too is, that we reach the default case anyway at the end. </br>

# Day-5
- The below program illustrates the concept of loops, it adds all the values in the list and stores in the R2 register.
```
.global _start
.equ endlist, 0xaaaaaaaa //defining a constant
_start:
	LDR R0,=list
	LDR R3,=endlist
	ldr R1,[R0]
	ADD R2,R2,R1
loop:
	LDR R1,[R0,#4]! //next value in the list
	CMP R1,R3
	BEQ exit
	ADD R2,R2,R1
	BAL loop

exit: //exit condition
	
.data	//defining a list/array
list:
	.word 1,2,3,4,5,6,7,8,9,10
```
- Final value of R2 will be 37 which is 55 in decimal.

# Day-6
- Introduction to conditional insturctions: ADDLT, MOVGE, MOVLE etc. Advantage is that it avoids branches and condenses the code.
- Introduction to functions/sub-routines. Idea of going back to the location that called after branching. We use `BL` which stands for `branch links` and holds the address of the nest instruction after branch in the Link Register.
- To back to the address stored in the LR, we use `BX LR`.
```
.global _start
_start:
	
	MOV R0,#1
	MOV R1,#3
	BL add2
	MOV R3,#4
	
add2:
	ADD R2,R0,R1
	BX lr
```
- The flow does move back to the address stored in the LR. It acts just like a function written in higher level language. But, there is more to it.
- Preserving data from stack memory:
```
.global _start
_start:
	
	  MOV R0,#1
	  MOV R1,#3
	  BL get_value
	  
get_value:
	MOV R0,#5
	MOV R1,#7
	ADD R2,R0,R1
	BX lr 
```
- R0 and R1 have the value 1 and 3 respectively before branching. After branching, the values are overridden to 5 and 7 respectively. R2 gets the value 12 (c) and when we go back, R0 and R1 no longer have the values 1 and 3. So what do we do? We use the PUSH and POPO operations.
- We use them to preserve the values of R0 and R1.
```
.global _start
_start:
	
	  MOV R0,#1
	  MOV R1,#3
	  PUSH {R0,R1}
	  BL get_value
	  POP {R0,R1}
	  B end
	  
get_value: 
	MOV R0,#5
	MOV R1,#7
	ADD R2,R0,R1
	BX lr 
	
end:
```
- The values of R0 and R1 are pushed onto the stack before the branching. After the return, we pop those values out of the stack and put them back into R0 and R1.
- The stack pointer gets activated now and holds the address `ffff fff8` where the value `1` is  stored and at `ffff fffc`, the value `3` is stored.
- After the values are popped, the SP is set back to 0.
- What if we wanted to store R2 as well. Well we could push R2 onto the stack and we could pop the value and store it in a different register.
```
.global _start
_start:
	
	  MOV R0,#1
	  MOV R1,#3
	  PUSH {R0,R1}
	  BL get_value
	  POP {R5}
	  POP {R0,R1}
	  B end
	  
get_value: 
	MOV R0,#5
	MOV R1,#7
	ADD R2,R0,R1
	PUSH {R2}
	BX lr 
	
end:
```
- R5 will hold the value that R2 stored in the function. This is a traditional way of return a value, though not used so much.

# Day-7
- Implemented Linear Search. The program works fine.

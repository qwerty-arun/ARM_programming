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

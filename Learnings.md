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
- Observations: '1e' will be stored in R0 and PC also changes from 04 to 08 and so on. CPSR and SPSR's final values are '01d3'.

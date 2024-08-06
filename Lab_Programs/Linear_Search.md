## ARM Program to implement Linear Search to find an element
```
.global _start

.section .data
array:      .word 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5  @ The array to be searched
array_len:  .word 11                             @ Length of the array
search_for: .word 9                              @ Element to search for

.section .bss

.section .text
_start:
    LDR R1, =array                               @ Load address of the array into R1
    LDR R2, =array_len                           @ Load address of array length into R2
    LDR R2, [R2]                                 @ Load array length into R2
    LDR R3, =search_for                          @ Load address of search element into R3
    LDR R3, [R3]                                 @ Load search element into R3

    MOV R4, #0                                   @ Initialize index to 0

loop:
    CMP R4, R2                                   @ Compare index with array length
    BEQ not_found                                @ If index equals array length, element is not found

    LDR R5, [R1, R4, LSL #2]                     @ Load array element at index R4 into R5
    CMP R5, R3                                   @ Compare array element with search element
    BEQ found                                    @ If equal, element is found

    ADD R4, R4, #1                               @ Increment index
    B loop                                       @ Repeat loop

found:
    @ Element found, R4 contains the index
    MOV R0, #1                                   @ Exit code 1 for found
    B end

not_found:
    @ Element not found
    MOV R0, #0                                   @ Exit code 0 for not found

end:
    B end                                        @ Infinite loop to end the program
```
- Works perfectly fine.
## Explanation: 
Data Section:

- array holds the elements of the array. </br>
- array_len holds the length of the array. </br>
- search_for holds the element to search for.</br>

Text Section:

- Load the base address of the array into register R1.
- Load the length of the array into register R2.
- Load the search element into register R3.
- Initialize the index R4 to 0.
- The loop starts and compares the current index with the array length.
- If the index equals the array length, the element is not found, and the program jumps to not_found.
- Otherwise, it loads the array element at the current index into R5 and compares it with the search element.
- If they are equal, the program jumps to found.
- If not, the index is incremented, and the loop repeats.
- If the element is found, the program sets the exit code to 1.
- If the element is not found after the loop, the program sets the exit code to 0.

## Final Results:
- R3 = 9 , R2 = 11 , R5 = 9, R4 =5  and R0 will be '1' indicating that the element is found.
- Z and C flags are set at the end. 

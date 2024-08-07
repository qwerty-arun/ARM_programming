# ARM program to implement Binary Search to find an element in an array
```
    .section .data
array:
    .word 1, 3, 5, 7, 9, 11, 13, 15, 17, 19   @ Array of sorted integers
length:
    .word 10                                  @ Length of the array

    .section .text
    .global _start

_start:
    LDR r2, =array       @ Load address of array into r2
    LDR r4, =length      @ Load address of length into r4
    LDR r4, [r4]         @ Load length value into r4
    MOV r3, #13          @ Target value to search for (modify this value as needed)

    MOV r5, #0           @ Initialize low index to 0
    SUB r6, r4, #1       @ Initialize high index to length - 1

binary_search:
    CMP r5, r6           @ Compare low index with high index
    BGT not_found        @ If low index > high index, target is not found

    ADD r7, r5, r6       @ Calculate mid index: (low + high)
    LSR r7, r7, #1       @ Divide by 2 using logical shift right

    LDR r8, [r2, r7, LSL #2]  @ Load array[mid] into r8
    CMP r8, r3           @ Compare array[mid] with target
    BEQ found            @ If array[mid] == target, branch to found
    BLT search_right     @ If array[mid] < target, search in right half

    @ Search in left half
    SUB r6, r7, #1       @ high = mid - 1
    B binary_search

search_right:
    ADD r5, r7, #1       @ low = mid + 1
    B binary_search

found:
    @ Target found, store index in r0 and exit
    MOV r0, r7
    B end

not_found:
    @ Target not found, store -1 in r0 and exit
    MOV r0, #-1

end:
    @ Exit the program
    MOV r7, #1           @ System call number for exit
    SWI 0
```
## Explanation: 
Data Section:

- array: Defines the sorted array of integers. </br> 
- length: Defines the length of the array. </br>

Text Section:

- _start: Entry point of the program.
- Load the array's address and length into registers.
- Set the target value in register r3.
- Initialize the low index (r5) to 0 and the high index (r6) to length - 1.

Binary Search Loop:

- Compare the low index (r5) with the high index (r6).
- If r5 > r6, the target is not found.
- Calculate the mid index and load the array value at that index into r8.
- Compare the mid value (r8) with the target (r3).
- If equal, branch to found.
- If the mid value is less than the target, adjust the low index to mid + 1.
- Otherwise, adjust the high index to mid - 1.

Found:
- Store the index of the found target in r0 and exit.
Not Found:
- Store -1 in r0 to indicate the target was not found and exit.
End:
- Exit the program using a system call.

# Final Results
- R7=1 which indicates that the element has been found, R0=6 which is the index of the element in the array, R3=R8=13. 

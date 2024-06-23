Computer Organization - Spring 2024
==============================================================
## Iran Univeristy of Science and Technology
## Assignment 1: Assembly code execution on phoeniX RISC-V core

- Name:ashkan ghaseminezhad
- Team Members:pouria masoomi 400414084
- Student ID: 400414093

## Report

square-root

1-Load the Number
The number whose square root we want to calculate (144) is loaded into register x4.
this part of code:
_start:
    # Load the number n into x4
    addi x4,zero,144


2-Initialize Low and High:
Register x5 is set to 0 (lo = 0).
Register x6 is set to the value of x4, which is 144 (hi = n).

    # Initialize low (lo) and high (hi)
    li      x5, 0      # lo = 0
    add     x6, x4, x0 # hi = n

3-Calculate Midpoint:
Registers x5 and x6 (low and high) are added together and stored in x7.
x7 is then right-shifted by 1 (using srai), effectively dividing the sum by 2 to get the midpoint (mid = (lo + hi) / 2).

    # mid^2
    mul     x8, x7, x7


4-Square Midpoint:
The value in x7 (mid) is squared and stored in x8 (mid^2).
    # if mid^2 == n, we've found the exact square root
    beq     x8, x4, done


5-Check for Exact Square Root:
If mid^2 (in x8) is equal to n (in x4), the exact square root is found, and the program jumps to the done label.

    # if mid^2 < n, move lo to mid + 1
    blt     x8, x4, adjust_lo



6-Adjust Low:
If mid^2 is less than n, the program jumps to adjust_lo to set lo to mid + 1.

    # if mid^2 > n, move hi to mid - 1
    add     x6, x7, x0
    addi    x6, x6, -1
    j       check_lo


7-Adjust High:
If mid^2 is greater than n, hi is set to mid - 1, and the program jumps to check_lo

8- Set Low to Mid + 1:
lo is set to mid + 1.

9-Check If Low Is Less Than or Equal to High:

If lo is less than or equal to hi, the program loops back to binary_search to continue.
check_lo:
    # if lo <= hi, continue the binary search
    ble     x5, x6, binary_search


10-Found Floor of Square Root:
If lo is greater than hi, mid (stored in x7) is the largest integer such that mid^2 <= n.
x9 is set to mid.
If mid^2 is still less than n, x9 is already correct.
Otherwise, x9 is decremented by 1 to get the correct floor value.

    # if lo > hi, we've found the largest integer such that mid^2 <= n
    add     x9, x7, x0
    blt     x8, x4, done
    addi    x9, x7, -1


11-Exit the Program:
The program ends by making an exit syscall with x10 set to 10.

done:
    # Exit the program
    li      x10, 10     # Exit syscall
    ebreak


This code performs a binary search to find the floor value of the square root of a given number (n = 144). The binary search efficiently narrows down the range by comparing the square of the midpoint (mid^2) with n, adjusting the low (lo) and high (hi) bounds until it finds the correct floor value of the square root.

the final results of square-root:

assembly:
0x00000000		0x09000213		addi x4 x0 144
0x00000004		0x00000293		addi x5 x0 0
0x00000008		0x00020333		add x6 x4 x0
0x0000000C		0x006283B3		add x7 x5 x6
0x00000010		0x4013D393		srai x7 x7 1
0x00000014		0x02738433		mul x8 x7 x7
0x00000018		0x02440663		beq x8 x4 44
0x0000001C		0x00444863		blt x8 x4 16
0x00000020		0x00038333		add x6 x7 x0
0x00000024		0xFFF30313		addi x6 x6 -1
0x00000028		0x00C0006F		jal x0 12
0x0000002C		0x000382B3		add x5 x7 x0
0x00000030		0x00128293		addi x5 x5 1
0x00000034		0xFC535CE3		bge x6 x5 -40
0x00000038		0x000384B3		add x9 x7 x0
0x0000003C		0x00444463		blt x8 x4 8
0x00000040		0xFFF38493		addi x9 x7 -1
0x00000044		0x00A00513		addi x10 x0 10
0x00000048		0x00100073		ebreak

and the waveform by using the python Assemblex :

![alt text](<gtkwave 1.png>)

![alt text](<gtkwave 2.png>)





now its time to check the second code :quicksort


quick sort:
the final results of this code assembly:
0x00000000		0x3E810113		addi x2 x2 1000
0x00000004		0x00000513		addi x10 x0 0
0x00000008		0x00A00293		addi x5 x0 10
0x0000000C		0x00552023		sw x5 0(x10)
0x00000010		0x05000293		addi x5 x0 80
0x00000014		0x00552223		sw x5 4(x10)
0x00000018		0x01E00293		addi x5 x0 30
0x0000001C		0x00552423		sw x5 8(x10)
0x00000020		0x05A00293		addi x5 x0 90
0x00000024		0x00552623		sw x5 12(x10)
0x00000028		0x02800293		addi x5 x0 40
0x0000002C		0x00552823		sw x5 16(x10)
0x00000030		0x03200293		addi x5 x0 50
0x00000034		0x00552A23		sw x5 20(x10)
0x00000038		0x04600293		addi x5 x0 70
0x0000003C		0x00552C23		sw x5 24(x10)
0x00000040		0x00000593		addi x11 x0 0
0x00000044		0x00600613		addi x12 x0 6
0x00000048		0x008000EF		jal x1 8
0x0000004C		0x0FC000EF		jal x1 252
0x00000050		0xFEC10113		addi x2 x2 -20
0x00000054		0x00112823		sw x1 16(x2)
0x00000058		0x01312623		sw x19 12(x2)
0x0000005C		0x01212423		sw x18 8(x2)
0x00000060		0x00912223		sw x9 4(x2)
0x00000064		0x00812023		sw x8 0(x2)
0x00000068		0x00050413		addi x8 x10 0
0x0000006C		0x00058493		addi x9 x11 0
0x00000070		0x00060913		addi x18 x12 0
0x00000074		0x02B64663		blt x12 x11 44
0x00000078		0x044000EF		jal x1 68
0x0000007C		0x00050993		addi x19 x10 0
0x00000080		0x00040513		addi x10 x8 0
0x00000084		0x00048593		addi x11 x9 0
0x00000088		0xFFF98613		addi x12 x19 -1
0x0000008C		0xFC5FF0EF		jal x1 -60
0x00000090		0x00040513		addi x10 x8 0
0x00000094		0x00198593		addi x11 x19 1
0x00000098		0x00090613		addi x12 x18 0
0x0000009C		0xFB5FF0EF		jal x1 -76
0x000000A0		0x00012403		lw x8 0(x2)
0x000000A4		0x00412483		lw x9 4(x2)
0x000000A8		0x00812903		lw x18 8(x2)
0x000000AC		0x00C12983		lw x19 12(x2)
0x000000B0		0x01012083		lw x1 16(x2)
0x000000B4		0x01410113		addi x2 x2 20
0x000000B8		0x00008067		jalr x0 x1 0
0x000000BC		0xFFC10113		addi x2 x2 -4
0x000000C0		0x00112023		sw x1 0(x2)
0x000000C4		0x00261293		slli x5 x12 2
0x000000C8		0x00A282B3		add x5 x5 x10
0x000000CC		0x0002A283		lw x5 0(x5)
0x000000D0		0xFFF58313		addi x6 x11 -1
0x000000D4		0x00058393		addi x7 x11 0
0x000000D8		0x02C38C63		beq x7 x12 56
0x000000DC		0x00239E13		slli x28 x7 2
0x000000E0		0x00AE0833		add x16 x28 x10
0x000000E4		0x00082E03		lw x28 0(x16)
0x000000E8		0x00128293		addi x5 x5 1
0x000000EC		0x01C2CE63		blt x5 x28 28
0x000000F0		0x00130313		addi x6 x6 1
0x000000F4		0x00231F13		slli x30 x6 2
0x000000F8		0x00AF08B3		add x17 x30 x10
0x000000FC		0x0008AF03		lw x30 0(x17)
0x00000100		0x01E82023		sw x30 0(x16)
0x00000104		0x01C8A023		sw x28 0(x17)
0x00000108		0x00138393		addi x7 x7 1
0x0000010C		0xFC0006E3		beq x0 x0 -52
0x00000110		0x00130F13		addi x30 x6 1
0x00000114		0x000F0793		addi x15 x30 0
0x00000118		0x002F1F13		slli x30 x30 2
0x0000011C		0x00AF08B3		add x17 x30 x10
0x00000120		0x0008AF03		lw x30 0(x17)
0x00000124		0x00261E13		slli x28 x12 2
0x00000128		0x00AE0833		add x16 x28 x10
0x0000012C		0x00082E03		lw x28 0(x16)
0x00000130		0x01E82023		sw x30 0(x16)
0x00000134		0x01C8A023		sw x28 0(x17)
0x00000138		0x00078513		addi x10 x15 0
0x0000013C		0x00012083		lw x1 0(x2)
0x00000140		0x00410113		addi x2 x2 4
0x00000144		0x00008067		jalr x0 x1 0
0x00000148		0x00100073		ebreak


and the waveform by using the python Assemblex:
![alt text](<gtkwave 4-1.png>)

![alt text](<gtkwave 5-1.png>)

This code implements the QuickSort algorithm in RISC-V assembly. It initializes an array of integers, sorts it using QuickSort, and terminates the program. The QUICKSORT function recursively sorts the array, while the PARTITION function partitions the array around a pivot element. The sorted array will be stored in the same memory locations.

hope you enjoy! 
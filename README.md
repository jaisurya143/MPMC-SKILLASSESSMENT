# MPMC-SKILLASSESSMENT1
# 8086 Assembly Language Programs for display whether the number is found or not found.


## AIM

Write an assembly language program in 8086 to search for a given number in an array of N elements and display whether the number is found or not found.

---

## APPARATUS REQUIRED

* Personal Computer with MASM Software

---

## 1. ADDITION

#### Algorithm
1. Start the program.
2. Initialize the data segment.
3. Display message: "Enter number to search:".
4. Accept a number from the user and store it in variable KEY.
5. Set SI to point to the start of the array.
6. Set CL as the total number of elements (N).
7. Compare KEY with each element of the array:
     a. If equal, display "NUMBER FOUND" and go to Step 9.
     b. Otherwise, move to the next element.
8. If all elements are checked and not equal, display "NUMBER NOT FOUND".
9. Exit the program.

#### Program

```asm
DATA SEGMENT
    array DB 10, 20, 30, 40, 50    ; Array elements
    n     DB 5                     ; Number of elements
    key   DB ?                     ; Number to search (input by user)
    msg1  DB 0DH,0AH,'NUMBER FOUND$',0
    msg2  DB 0DH,0AH,'NUMBER NOT FOUND$',0
    msg3  DB 'Enter number to search (0-99): $',0
DATA ENDS

CODE SEGMENT
ASSUME CS:CODE, DS:DATA

START:
    MOV AX, DATA
    MOV DS, AX

    ; Ask user to enter number
    LEA DX, msg3
    MOV AH, 09H
    INT 21H

    ; ---- Read first digit ----
    MOV AH, 1
    INT 21H
    SUB AL, 30H        ; Convert ASCII to number
    MOV BL, AL         ; Store as tens digit

    ; ---- Read second digit ----
    MOV AH, 1
    INT 21H
    CMP AL, 0DH        ; If user pressed ENTER, skip
    JE SINGLE_DIGIT

    SUB AL, 30H
    MOV BH, AL
    MOV AL, BL
    MOV BL, 10
    MUL BL             ; AL = tens * 10
    ADD AL, BH         ; AL = tens + ones
    JMP STORE_KEY

SINGLE_DIGIT:
    MOV AL, BL         ; Only one digit entered

STORE_KEY:
    MOV key, AL        ; Store input in key

    ; ---- Search in array ----
    MOV CL, n
    MOV CH, 0
    LEA SI, array
    MOV AL, key

SEARCH_LOOP:
    CMP AL, [SI]
    JE FOUND
    INC SI
    LOOP SEARCH_LOOP

NOT_FOUND:
    LEA DX, msg2
    MOV AH, 09H
    INT 21H
    JMP EXIT

FOUND:
    LEA DX, msg1
    MOV AH, 09H
    INT 21H

EXIT:
    MOV AH, 4CH
    INT 21H

CODE ENDS
END START

```
## OUTPUT IMAGE FROM MASM SOFTWARE
<img width="644" height="385" alt="image" src="https://github.com/user-attachments/assets/a646a7d4-5ba1-416d-a54c-1c3af6f55741" />
<img width="638" height="416" alt="image" src="https://github.com/user-attachments/assets/7ba41de5-22ae-4b28-9441-07a386f47376" />

## RESULT
The program successfully searches for a number in an array and displays
whether the number is FOUND or NOT FOUND.

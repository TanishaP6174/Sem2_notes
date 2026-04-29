[[CSO endlab_info]]

General framework:

# 🧱 RISC-V Assembly Program Template (Endlab Notes)

---

## 🔹 1. General Structure

```asm
# =======================
# DATA SECTION
# =======================
.section .data //in assignment i just wrote .data

arr:    .word 1, 2, 3, 4, 5
n:      .word 5
msg:    .asciz "Hello\n"

# =======================
# TEXT SECTION
# =======================
.section .text  //in assignment i just wrote .text
.globl main

main:
    # your code here

    li a0, 0
    ret
```

---

## 🔹 2. Address vs Value (VERY IMPORTANT)

```asm
la t0, arr      # load address of arr
lw t1, 0(t0)    # load value from arr[0]
```

❌ Wrong:

```asm
lw t0, arr
```

---

## 🔹 3. Array Access Template

```asm
# t0 = base address
# t1 = index (i)

slli t2, t1, 2      # offset = i * 4
add t3, t0, t2      # address of arr[i]
lw t4, 0(t3)        # value = arr[i]
```

---

## 🔹 4. Loop Template

```asm
li t0, 0            # i = 0

loop:
    bge t0, t1, end   # if i >= n → exit

    # loop body

    addi t0, t0, 1
    j loop

end:
```

---

## 🔹 5. Function Template (Stack Handling)

```asm
func:
    addi sp, sp, -16
    sd ra, 8(sp)
    sd s0, 0(sp)

    # function body

    ld s0, 0(sp)
    ld ra, 8(sp)
    addi sp, sp, 16
    ret
```

---

## 🔹 6. Calling a Function

```asm
li a0, 5
li a1, 10

call func

# result will be in a0
```

---

## 🔹 7. Minimal Working Program

```asm
.section .text
.globl main

main:
    li a0, 5
    li a1, 7

    add a0, a0, a1   # result in a0

    ret
```

---

## 🔹 8. Common Directives

|Directive|Meaning|
|---|---|
|`.word`|4 bytes|
|`.dword`|8 bytes|
|`.asciz`|null-terminated string|
|`.space`|reserve memory|

---

## 🔹 9. Endlab Checklist ✅

-  Used `.section .text`
    
-  Declared `.globl main`
    
-  Returned value in `a0`
    
-  Used `ret`
    
-  Correct array indexing (`i * 4`)
    
-  No infinite loops
    
-  Saved `ra` (if using functions)
    
-  Restored `sp` properly
    

---

## 🔹 10. Compile & Run (Practice)

```bash
riscv64-unknown-elf-gcc -g file.s -o file
qemu-riscv64 ./file
```

---

## 🔹 11. Debug (GDB)

```bash
qemu-riscv64 -g 1234 ./file
riscv64-unknown-elf-gdb file
```

Inside GDB:

```gdb
target remote :1234
break main
continue
stepi
info registers
```

---

## 🧠 Mental Model

```
.data → inputs
.text → logic
main → execution starts
a0 → return value
```

---

<h2>Note: info to remember</h2>
# 🔢 ASCII Values 

| Character | Decimal |
| --------- | ------- |
| `+`       | 43      |
| `-`       | 45      |
| `*`       | 42      |
| `/`       | 47      |
| `(`       | 40      |
| `)`       | 41      |
| `0`       | 48      |
| `a`       | 97      |
| `A`       | 65      |

---

<h2>Common mistakes I'm making:</h2>


```
Initially didn't write la for the labels, n1 and n2, directly wrote ld, which is wrong
```


```
la t0, n1
ld s0, 0(t0)
la t1, n2
ld s2, 0(t1)

This is correct

initially i just did
la s0, n1
la s1, n2

so i was basically doing the operations with the addresses of n1, n2

```

q5 - main logic

![[Pasted image 20260425081338.png]]

Note:
can do this

# One string to rule them all
fmt_four: .string "%lld %lld %lld %lld\n"

.text
# ... inside your function ...

    la a0, fmt_four      # The format string goes in a0
    mv a1, s1            # First number in a1
    mv a2, s2            # Second number in a2
    mv a3, s3            # Third number in a3
    mv a4, s4            # Fourth number in a4
    call printf          # Printf will look at a1-a4 to fill the %lld

use <b>srai</b> for diving by 2
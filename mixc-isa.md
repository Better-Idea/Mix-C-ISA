```C++
/*
                  _oo0oo_
                 o8888888o
                 88" . "88
                 (| -_- |)
                 0\  =  /0
               ___/`---'\___
             .' \\|     |// '.
            / \\|||  :  |||// \
           / _||||| -:- |||||- \
          |   | \\\  -  /// |   |
          | \_|  ''\---/''  |_/ |
          \  .-\__  '-'  ___/-. /
        ___'. .'  /--.--\  `. .'___
     ."" '<  `.___\_<|>_/___.' >' "".
    | | :  `- \`.;`\ _ /`;.`/ - ` : | |
    \  \ `_.   \_ __\ /__ _/   .-` /  /
=====`-.____`.___ \_____/___.-`___.-'=====
                  `=---='
               作者原本生活富裕
               有一天他离开凡尘
         在一棵菩提树下顿悟了程序的真谛
           那就是不写代码就不会有BUG
                  因执念故
               时空亦为之轮回..

        func binary_search(
            rt.o    match,
            r1.o    less_then_target,
            r2.o    grater_then_target,
            r3.i    sequence,
            r4.i    start,
            r5.i    length,
            r6.i    value
        )
            start   left
            r8      backup
            r9      cmp
            ra      center
            rb      right

            sub     rt, start, length
            ifnae   endif.not.found
            mvd     right, length
            mvd     center, length
            shr     center, 1
            sub     right, 1
            jmp     loop.find.body
        loop.find.next:
            add     rt, left, right
            shr     center, rt, 1
        loop.find.body:
            sub     rt, right, left
            ifa     endlp.find
            mvd     rt, center
            mvd     backup, center
            ldd.u   rt, sequence[rt]
            sub     cmp, rt, value
            ife     endif.match
            mvd     match, center
            ret
        endif.match:
            ifa     endif.above
            sub     center, 1
            mvd     right, center
            jmp     loop.find.next
        endif.above:
            add     center, 1
            mvd     left, center
            jmp     loop.find.next
        endlp.find:
            mov     rt, -1
            sub     cmp, 0
            ifa     endif.grater_then_target
            mvd     grater_then_target, backup
            sub     backup, 1
            ifae    token.over
            mvd     less_then_target = backup
            jmp     token.over
        endif.grater_then_target:
            ifb     token.over
            mvd     less_then_target, backup
            add     backup, 1
            sub     length, backup
            ifa     token.over
            mvd     grater_then_target, backup
            jmp     token.over
        endif.not.found:
            mov     rt, -1
            mov     r1, -1
            mov     r2, -1
        token.over:
            ret
*/
```
</br>

## REGISTER FILE
```C++
/*
+--------------------+ +------------------------------------------+
| gp-register  32bit | | sp-register                        32bit |
+------+-------------+ +---------+--------------------------------+
| 00   | r0/rt       | | ip      | instruction pointer            |
| 01   | r1          | | sp      | stack top pointer              |
| 02   | r2          | | bp      | stack base pointer             |
| 03   | r3          | | pcs     | program call stack             |
| 04   | r4          | | sta     | program state                  |
| 05   | r5          | | imm     | immediate number               |
| 06   | r6          | | hig/rem | high 32bit product / remainder |
| 07   | r7          | |         |                                |
| 08   | r8          | |         |                                |
| 09   | r9          | |         |                                |
| 10   | ra          | | ...     | to be continue                 |
| 11   | rb          | |         |                                |
| 12   | rc          | |         |                                |
| 13   | rd          | |         |                                |
| 14   | re          | |         |                                |
| 15   | rf          | |         |                                |
+------+-------------+ +---------+--------------------------------+

              -         --------7
            -         --------6 | 
          -         --------5 | | 
        -         --------4 | | | 
      -         --------3 | | | | 
    -         --------2 | | | | |
  -         --------1 | | | | | |
+-------0 +-------0 | | | | | | |
| r0/rt | | r8    | | | | | | | |
| r1    | | r9    | | | | | | | |
| r2    | | ra    | | | | | | | |
| r3    | | rb    | | | | | | | |
| r4    | | rc    | | | | | | | |
| r5    | | rd    | | | | | | | 7
| r6    | | re    | | | | | | 6 |
| r7    | | rf    | | | | | 5 | |
+-------+ +-------+ | | | 4 | | |
| 32bit | | 32bit | | | 3 | | | |
+-------+ +-------+ | 2 | | | | |
| share | | group | 1 | | | | | -----------------------------------+
+-------+ +-------0 | | | | | ----------------------------+        |
  |               | | | | | +--------------------+        |        |
  | +-------------+ | | | +-------------+        |        |        |
  | |        +------+ | +------+        |        |        |        |
  | |        |        |        |        |        |        |        |
  | |        |        |        |        |        |        |        |
  +-|------+-|------+-|------+-|------+-|------+-|------+-|------+ |
  | |      | |      | |      | |      | |      | |      | |      | |
  V V      | |      | |      | |      | |      | |      | |      | |
| .... |   | |      | |      | |      | |      | |      | |      | |
| call +-+ v v      | |      | |      | |      | |      | |      | |
| .... | | .... |   | |      | |      | |      | |      | |      | |
         | call +-+ V V      | |      | |      | |      | |      | |
         | .... | | .... |   | |      | |      | |      | |      | |
                  | call +-+ V V      | |      | |      | |      | |
                  | .... | | .... |   | |      | |      | |      | |
                           | call +-+ V V      | |      | |      | |
                           | .... | | .... |   | |      | |      | |
                                    | call +-+ v v      | |      | |
                                    | .... | | .... |   | |      | |
                                             | call +-+ V V      | |
                                             | .... | | .... |   | |
                                                      | call +-+ V V
                                                      | .... | | .... | 
                                                               | call +- OV
                                                               | .... | 

*/
```
</br>

## EIGHT PHASES OPERATION

| OPC  | MODE                   | FUNC          | DESCRIPTION                   | REG/IMM | REG/IMM |
|------|------------------------|---------------|-------------------------------|---------|---------|
| 2bit | 3bit                   | 3bit          |                               | 4bit    | 4bit    |
|      | 000 : opc ra, rt, rb   | 000 : shl     | shift left                    |         |         |
|      | 001 : opc ra, rt, im   | 001 : shr     | shift right                   |         |         |
|      | 010 : opc ra, rb, rt   | 010 : shrc    | shift right with carry        |         |         |
|      | 011 : opc ra, im, rt   | 011 : div     | division                      |         |         |
|      | 100 : opc ra, ra, rb   | 100 : sub     | subtraction                   |         |         |
|      | 101 : opc ra, ra, im   | 101 : sbb     | subtraction with borrow       |         |         |
|      | 110 : opc rt, ra, rb   | 110 : sub.f   | floating point subtraction    |         |         |
|      | 111 : opc rt, ra, im   | 111 : div.f   | floating point division       |         |         |
</br>

## FOUR PHASES OPERATION

| OPC  | MODE                   | FUNC          | DESCRIPTION                   | REG     | REG/IMM |
|------|------------------------|---------------|-------------------------------|---------|---------|
| 3bit | 2bit                   | 3bit          |                               | 4bit    | 4bit    |
|      | 00 : opc ra, ra, rb    | 000 : add     | addition                      |         |         |
|      | 01 : opc ra, ra, im    | 001 : adc     | addition with carry           |         |         |
|      | 10 : opc rt, ra, rb    | 010 : mul     | multiplication                |         |         |
|      | 11 : opc rt, ra, im    | 011 : -       | -                             |         |         |
|      |                        | 100 : add.f   | floating point addition       |         |         |
|      |                        | 101 : mul.f   | floating point multiplication |         |         |
|      |                        | 110 : -       | -                             |         |         |
|      |                        | 111 : -       | -                             |         |         |
</br>

## BITWISE OPERATION 

| OPC  | MODE                   | FUNC          | DESCRIPTION                   | REG     | REG/IMM |
|------|------------------------|---------------|-------------------------------|---------|---------|
| 3bit | 2bit                   | 3bit          |                               | 4bit    | 4bit    |
|      | 00 : opc ra, ra, im    | 000 : bt      | bit test                      |         |         |
|      | 01 : opc ra, ra, im+16 | 001 : bts     | bit test and set              |         |         |
|      | 10 : opc ra, ra, rb    | 010 : btr     | bit test and reset            |         |         |
|      | 11 : opc rt, ra, rb    | 011 : btn     | bit test and inversion        |         |         |
|      |                        | 100 : and     | bitwise and                   |         |         |
|      |                        | 101 : or      | bitwise or                    |         |         |
|      |                        | 110 : xor     | bitwise xor                   |         |         |
|      |                        | 111 : nand    | bitwise nand                  |         |         |

#### BITWISE MUX
C0  = B1 ? ~A : A  
C1  = B0 ? 32{1} : B  
C2  = B0 ? 32{1} : A  
C3  = B1 ? ~B : B  
C4 = C0 & C1 | C2 & C3  

| FUNC  | EXPR                 | B1 | B0 |
|-------|:--------------------:|:--:|:--:|
| and   | A & B                | 0  | 0  |
| or    | A \| B               | 0  | 1  |
| xor   | A & B                | 1  | 0  |
| nand  | ~(A & B) -> ~A \| ~B | 1  | 1  |
</br>

## BP-BASED LOAD

| OPC  | FUNC                        | SIZE       | REG     | IMM     |
|------|-----------------------------|------------|---------|---------|
| 4bit | 1bit                        | 2bit       | 4bit    | 5bit    |
|      | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         |
|      | 1 : ldb.i ldw.i ldd.i ldq.i | 01 : word  |         |         |
|      |                             | 10 : dword |         |         |
|      |                             | 11 : qword |         |         |
note:  
bp(stack base pointer) as the byte/word/dword/qword pointer, offset step is 1/2/4/8


```C++
void ldb(){
    char byte_array[20] = "emmm...";
    char a = byte_array[1];      // ldb.u r0, bp[1]
}

void ldw(){
    uint16_t word_array[10] = { 0 };
    uint16_t a = word_array[3];  // ldw.u r0, bp[3]
}

/*
                 +--------------------------------------+
                 |          offset based on bp          |
                 +------------+------------+------------+
                 | ldb.u      | content    | ldw.u      |  
                 +------------+------------+------------+
                 | 0          | 'a'        | 0 low      |
                 | 1          | 'b'        | 0 high     |
                 | 2          | 'c'        | 1 low      |
byte_array[3] -> | 3          | 'd'        | 1 high     |
                 | 4          | 0x1        | 2 low      | <- word_array[0]
                 | 5          | 0x0        | 2 high     |
                 | 6          | 0x2        | 3 low      | <- word_array[1]
                 | 7          | 0x0        | 3 high     |
                 +------------+------------+------------+
*/
void ldx(){
    uint8_t  byte_array[4] = { 'a', 'b', 'c', 'd' };
    uint16_t word_array[2] = { 0x1, 0x2 };
    uint8_t  a = byte_array[3];  // ldb.u r0, bp[3]
    uint16_t b = word_array[1];  // ldw.u r1, bp[4 / 2 + 1] ; skip two word
}
...
```

</br>

## GENERAL-PURPOSE REGISTER-BASED LOAD

| OPC  | FUNC                        | SIZE       | REG     | REG     | PLUS REG-T                |
|------|-----------------------------|------------|---------|---------|---------------------------|
| 4bit | 1bit                        | 2bit       | 4bit    | 4bit    | 1bit                      |
|      | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         | 0 : ldx reg, reg[im]      |
|      | 1 : ldb.i ldw.i ldd.i ldq.i | 01 : word  |         |         | 1 : ldx reg, reg[im + rt] |
|      |                             | 10 : dword |         |         |                           |
|      |                             | 11 : qword |         |         |                           |
note:  
the source register as the byte/word/dword/qword base pointer, offset step is 1/2/4/8

</br>

## BP-BASED STORE

| OPC  | FUNC                | SIZE       | REG     | IMM     |
|------|---------------------|------------|---------|---------|
| 4bit | 1bit                | 2bit       | 4bit    | 5bit    |
|      | 0 : stb stw std stq | 00 : byte  |         |         |
|      |                     | 01 : word  |         |         |
|      |                     | 10 : dword |         |         |
|      |                     | 11 : qword |         |         |
</br>

## GENERAL-PURPOSE REGISTER-BASED STORE

| OPC  | FUNC                | SIZE       | REG     | REG     | PLUS REG-T                |
|------|---------------------|------------|---------|---------|---------------------------|
| 4bit | 1bit                | 2bit       | 4bit    | 4bit    | 1bit                      |
|      | 1 : stb stw std stq | 00 : byte  |         |         | 0 : stx reg, reg[im]      |
|      |                     | 01 : word  |         |         | 1 : stx reg, reg[im + rt] |
|      |                     | 10 : dword |         |         |                           |
|      |                     | 11 : qword |         |         |                           |
</br>

## IMM-BASED OPERATION
#### PUSH IMM
| OPC  | FUNC | DESCRIPTION                    | IMM                 |
|------|------|--------------------------------|---------------------|
| 4bit | imm  | build a immediate number       | 12bit               |

note:  
a long immediate number will split into several consecutive imm instruction
```C++
/*
imm  0x567
imm  0x234
call 0x01        -> call 0x01234567

imm  0x000
add  ra, ra, 0x1 -> add ra, ra, 0x1000
*/
```

#### CONDITION BRANCH
| OPC  | FUNC                | CONDITION                      | IMM     |
|------|---------------------|--------------------------------|---------|
| 4bit | 3bit                |                                | 9bit    |
|      | 000 : ifeq/ifzr     | equal or zero                  | signed  |
|      | 001 : ifne/ifnz     | not equal or not zero          | signed  |
|      | 010 : ifa           | above                          | signed  |
|      | 011 : ifae          | above equal                    | signed  |
|      | 100 : ifcf/ifov     | carry or overflow              | signed  |
|      | 101 : ifnc/ifno     | not carry or not overflow      | signed  |
|      | 110 : ifuo          | under overflow                 | signed  |
|      | 111 : ifdo          | up overflow                    | signed  |
note:  
ip += imm(signed) * 2(two bytes/instruction) when not match condition

</br>
</br>

#### ADVANCED
| OPC  | FUNC                | DESCRIPTION                    | IMM     |
|------|---------------------|--------------------------------|---------|
| 4bit | 3bit                |                                | 9bit    |
|      | 000 : jmp           | ip relatived jump              | signed  |
|      | 001 : call          | call sub-procedure             | unsigned|
|      | 010 : spa           | allocation memory from stack   | unsigned|
|      | 011 : -             |                                |         |
|      | 100 : -             |                                |         |
|      | 101 : -             |                                |         |
|      | 110 : -             |                                |         |
|      | 111 : -             |                                |         |

</br>

1/16
OPC     FUNC IMM
4       2    9
        |
        +-00:jmp            jump to
        +-01:call           call sub procedue
        +-10:imm            load imm
        +-11:spa            alloction memory from stack, let sp plus the imm * 4(dword per cell) number

note:


1/64
OPC     FUNC REG-W REG-R
6       2    4   4
        |
        +-00:fis            first index of set bit
        +-01:lis            last index of set bit
        +-10:lzc            leading zero count
        +-11:sbc            set bit count

1/64
OPC     BANK IMM-WX, REG-R
6       2    4       4
        |
        +-00:bdc reg, { r0, r1, r2, r3 }
        +-01:bdc reg, { r4, r5, r6, r7 }
        +-10:bdc reg, { r8, r9, ra, rb }
        +-11:bdc reg, { rc, rd, re, rf }
note:
broadcast(bdc) instruction
bdc r9, { rt, r1, r3 } <=> rt = r1 = r3 = r9

1/64
OPC     FUNC IMM-A IMM-B
6       2    4     4
        |
        +-00:psh            push register by range (IMM-A, IMM-B)
        +-01:pop            pop register by range (IMM-A, IMM-B)
        +-10:
        +-11:

OPC     FUNC REG-R IMM
6       2    4     4
        |
        +-00:lock

note:
lock instruction can hang-up the maskable interrupt while processor run in the locked range.
but the processor still need handle the interrupt while executed over 16 instructions or exit the locked area.
- imm indicated the locked area range
- reg indicated the locked token, it will be blocked when the other processor which hold the same token want enter the critical area.
- do not lock the time-consuming operation by this instruction.

1/256
OPC     FUNC
8       8
        |
        +-0000:ret
        +-0001:bpsp         bp = sp

1/4
1/8
1/8
1/16
1/16
1/16 = 1/32 + 1/32
1/16


rest:1/4

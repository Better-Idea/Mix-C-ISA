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
            r9      cmp
            ra      center
            rb      right

            sub     rt, start, length
            ifae    endif.not.found
            bdc     rt, grater_then_target, less_then_target, -1
        endif.not.found:
            bdc     right, center, length
            shr     center, 1
            sub     right, 1
            jmp     loop.find.body
        loop.find.next:
            add     rt, left, right
            shr     center, rt, 1
        loop.find.body:
            sub     rt, right, left
            ifa     endlp.find
            bdc     rt, less_then_target, grater_then_target, center
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
            sub     cmp, 0
            ifa     endif.grater_then_target
            sub     less_then_target, 1; maybe -1 when (grater_then_target) is 0
            jmp     token.over
        endif.grater_then_target:
            add     grater_then_target, 1
            sub     rt, grater_then_target, length
            ifae    token.over
            mov     grater_then_target, -1
        token.over:
            mov     rt, -1
            ret

            MIXC MICRO
划清界限=======================================================================================
            ARM  THUMB
2001dc: b5f0      	push	{r4, r5, r6, r7, lr}
2001de: 429a      	cmp	r2, r3
2001e0: f04f 34ff 	mov.w	r4, #4294967295	; 0xffffffff
2001e4: 6004      	str	r4, [r0, #0]
2001e6: 6044      	str	r4, [r0, #4]
2001e8: 6084      	str	r4, [r0, #8]
2001ea: f8dd e014 	ldr.w	lr, [sp, #20]
2001ee: da27      	bge.n	200240 <_Z13binary_searchPiiii+0x64>
2001f0: 191f      	adds	r7, r3, r4
2001f2: 42ba      	cmp	r2, r7
2001f4: ea4f 0663 	mov.w	r6, r3, asr #1
2001f8: dd07      	ble.n	20020a <_Z13binary_searchPiiii+0x2e>
2001fa: e024      	b.n	200246 <_Z13binary_searchPiiii+0x6a>
2001fc: 1e77      	subs	r7, r6, #1
2001fe: 19d5      	adds	r5, r2, r7
200200: 42ba      	cmp	r2, r7
200202: ea4f 0565 	mov.w	r5, r5, asr #1
200206: dc0d      	bgt.n	200224 <_Z13binary_searchPiiii+0x48>
200208: 462e      	mov	r6, r5
20020a: f851 4026 	ldr.w	r4, [r1, r6, lsl #2]
20020e: ebce 0404 	rsb	r4, lr, r4
200212: 2c00      	cmp	r4, #0
200214: dcf2      	bgt.n	2001fc <_Z13binary_searchPiiii+0x20>
200216: d014      	beq.n	200242 <_Z13binary_searchPiiii+0x66>
200218: 1c72      	adds	r2, r6, #1
20021a: 19d5      	adds	r5, r2, r7
20021c: 42ba      	cmp	r2, r7
20021e: ea4f 0565 	mov.w	r5, r5, asr #1
200222: ddf1      	ble.n	200208 <_Z13binary_searchPiiii+0x2c>
200224: 2c00      	cmp	r4, #0
200226: dd05      	ble.n	200234 <_Z13binary_searchPiiii+0x58>
200228: 2e00      	cmp	r6, #0
20022a: 6086      	str	r6, [r0, #8]
20022c: dd08      	ble.n	200240 <_Z13binary_searchPiiii+0x64>
20022e: 3e01      	subs	r6, #1
200230: 6046      	str	r6, [r0, #4]
200232: bdf0      	pop	{r4, r5, r6, r7, pc}
200234: d004      	beq.n	200240 <_Z13binary_searchPiiii+0x64>
200236: 1c72      	adds	r2, r6, #1
200238: 4293      	cmp	r3, r2
20023a: 6046      	str	r6, [r0, #4]
20023c: bfc8      	it	gt
20023e: 6082      	strgt	r2, [r0, #8]
200240: bdf0      	pop	{r4, r5, r6, r7, pc}
200242: 6006      	str	r6, [r0, #0]
200244: bdf0      	pop	{r4, r5, r6, r7, pc}
200246: bdf0      	pop	{r4, r5, r6, r7, pc}
*/
```
</br>

## REGISTER FILE
```C++
/*
+-----------------------------------------------+
| gp-register                             32bit |
+----+---------+--------------------------------+
| id | name    | description                    |
+----+---------+--------------------------------+
| 00 | r0/rt   |                                |
| 01 | r1      |                                |
| 02 | r2      |                                |
| 03 | r3      |                                |
| 04 | r4      |                                |
| 05 | r5      |                                |
| 06 | r6      |                                |
| 07 | r7      |                                |
|===============================================|
| 08 | r8      |                                |
| 09 | r9      |                                |
| 10 | ra      |                                |
| 11 | rb      |                                |
| 12 | rc      |                                |
| 13 | rd      |                                |
| 14 | re      |                                |
| 15 | rf      |                                |
+----+---------+--------------------------------+

+-----------------------------------------------+
| sp-register                             32bit |
+----+---------+--------------------------------+
| id | name    | description                    |
+----+---------+--------------------------------+
| 00 | pcs     | program call stack             |
| 01 | ip      | instruction pointer            |
| 02 | bp      | stack base pointer             |
| 03 | sp      | stack top pointer              |
| 04 |         |                                |
| 05 |         |                                |
| 06 |         |                                |
| 07 |         |                                |
| 08 | sta     | program state                  |
| 09 | imm     | immediate number               |
| 10 | hig/rem | high 32bit product / remainder |
| 11 |         |                                |
| 12 |         |                                |
| 13 |         |                                |
| 14 |         |                                |
| 15 | ...     | to be continue                 |
+----+---------+--------------------------------+

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
| 00   | 000 : opc ra, rt, rb   | 000 : shl     | shift left                    |         |         |
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
| 010  | 00 : opc ra, ra, rb    | 000 : and     | bitwise and                   |         |         |
|      | 01 : opc ra, ra, im    | 001 : or      | bitwise or                    |         |         |
|      | 10 : opc rt, ra, rb    | 010 : xor     | bitwise xor                   |         |         |
|      | 11 : opc rt, ra, im    | 011 : mul     | multiplication                |         |         |
|      |                        | 100 : add     | addition                      |         |         |
|      |                        | 101 : adc     | addition with carry           |         |         |
|      |                        | 110 : add.f   | floating point addition       |         |         |
|      |                        | 111 : mul.f   | floating point multiplication |         |         |
</br>

#### BITWISE MUX
C0 = B1 ? ~A : A  
C1 = B0 ? 32{1} : B  
C2 = B0 ? 32{1} : A  
C3 = B1 ? ~B : B  
C4 = C0 & C1 | C2 & C3  
RA = ~RB <=> RA = 1 - RB

| FUNC  | EXPR                 | B1 | B0 |
|-------|:--------------------:|:--:|:--:|
| and   | A & B                | 0  | 0  |
| or    | A \| B               | 0  | 1  |
| xor   | A & B                | 1  | 0  |
| nand  | ~(A & B) -> ~A \| ~B | 1  | 1  |

</br>

## BIT OPERATION 

| OPC  | MODE                   | FUNC          | DESCRIPTION                   | REG     | REG/IMM |
|------|------------------------|---------------|-------------------------------|---------|---------|
| 3bit | 2bit                   | 3bit          |                               | 4bit    | 4bit    |
| 011  | 00 : opc ra, ra, im    | 000 : bt      | bit test                      |         |         |
|      | 01 : opc ra, ra, im+16 | 001 : bts     | bit test and set              |         |         |
|      | 10 : opc ra, ra, rb    | 010 : btr     | bit test and reset            |         |         |
|      | 11 : opc rt, ra, rb    | 011 : btn     | bit test and inversion        |         |         |
|      |                        | 100 : -       |                               |         |         |
|      |                        | 101 : -       |                               |         |         |
|      |                        | 110 : -       |                               |         |         |
|      |                        | 111 : -       |                               |         |         |

## BP-BASED LOAD

| OPC  | FUNC                        | SIZE       | REG     | IMM     |
|------|-----------------------------|------------|---------|---------|
| 4bit | 1bit                        | 2bit       | 4bit    | 5bit    |
| 1000 | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         |
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

| OPC    | FUNC                        | SIZE       | REG     | REG     | PLUS REG-T                |
|--------|-----------------------------|------------|---------|---------|---------------------------|
| 4bit   | 1bit                        | 2bit       | 4bit    | 4bit    | 1bit                      |
| 1001   | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         | 0 : ldx reg, reg[im]      |
|        | 1 : ldb.i ldw.i ldd.i ldq.i | 01 : word  |         |         | 1 : ldx reg, reg[im + rt] |
|        |                             | 10 : dword |         |         |                           |
|        |                             | 11 : qword |         |         |                           |

note:  
the source register as the byte/word/dword/qword base pointer, offset step is 1/2/4/8

</br>

## BP-BASED STORE

| OPC    | FUNC                | SIZE       | REG     | IMM     |
|--------|---------------------|------------|---------|---------|
| 5bit   |                     | 2bit       | 4bit    | 5bit    |
| 10100  | stb stw std stq     | 00 : byte  |         |         |
|        |                     | 01 : word  |         |         |
|        |                     | 10 : dword |         |         |
|        |                     | 11 : qword |         |         |
</br>

## GENERAL-PURPOSE REGISTER-BASED STORE

| OPC    | FUNC                | SIZE       | REG     | REG     | PLUS REG-T                |
|--------|---------------------|------------|---------|---------|---------------------------|
| 5bit   |                     | 2bit       | 4bit    | 4bit    | 1bit                      |
| 10101  | stb stw std stq     | 00 : byte  |         |         | 0 : stx reg, reg[im]      |
|        |                     | 01 : word  |         |         | 1 : stx reg, reg[im + rt] |
|        |                     | 10 : dword |         |         |                           |
|        |                     | 11 : qword |         |         |                           |
</br>

## CONDITION BRANCH

| OPC    | FUNC                | CONDITION                      | IMM     |
|--------|---------------------|--------------------------------|---------|
| 4bit   | 3bit                |                                | 9bit    |
| 1011   | 000 : ifeq/ifzr     | equal or zero                  | signed  |
|        | 001 : ifne/ifnz     | not equal or not zero          | signed  |
|        | 010 : ifa           | above                          | signed  |
|        | 011 : ifae          | above equal                    | signed  |
|        | 100 : ifcf/ifov     | carry or overflow              | signed  |
|        | 101 : ifnc/ifno     | not carry or not overflow      | signed  |
|        | 110 : ifuo          | under overflow                 | signed  |
|        | 111 : ifdo          | up overflow                    | signed  |

note:  
ip += imm(signed) * 2(two bytes/instruction) when not match condition

</br>

## EDITION

| OPC    | FUNC                | DESCRIPTION                    | IMM     |
|--------|---------------------|--------------------------------|---------|
| 4bit   | 3bit                |                                | 9bit    |
| 1100   | 000 : jmp           | ip relatived jump              | signed  |
|        | 001 : call          | call sub-procedure             | unsigned|
|        | 010 : spa           | allocation memory from stack   | unsigned|
|        | 011 : -             |                                |         |
|        | 100 : -             |                                |         |
|        | 101 : -             |                                |         |
|        | 110 : -             |                                |         |
|        | 111 : -             |                                |         |

</br>

## MULTI-ASSIGN

| OPC    | FUNC | BANK                            | R/I               | MASK | REG/IMM |
|--------|------|---------------------------------|-------------------|------|---------|
| 5bit   |      | 2bit                            | 1bit              | 4bit | 4bit    |
| 11010  | bdc  | 00 : MASK indicated the r0 ~ r3 | 0 : source is reg |      |         |
|        |      | 01 : MASK indicated the r4 ~ r7 | 1 : source is imm |      |         |
|        |      | 10 : MASK indicated the r8 ~ rb |                   |      |         |
|        |      | 11 : MASK indicated the rc ~ rf |                   |      |         |
```C++
/*
bdc { r0, r2, r3 }, r1      -> r0 = r2 = r3 = r1
bdc { r4, r6, r7 }, r2      -> r4 = r6 = r7 = r2
bdc { r8, r9, ra, rb }, -1  -> r8 = r9 = ra = rb = -1
*/
```
</br>

## USEFUL BITWISE OPERATION

| OPC    | FUNC        | description             | REG  | REG  |
|--------|-------------|-------------------------|------|------|
| 6bit   | 2bit        |                         | 4bit | 4bit |
| 111000 | 00 : fis    | first index of set bit  |      |      |
|        | 01 : lis    | last index of set bit   |      |      |
|        | 10 : lzc    | leading zero count      |      |      |
|        | 11 : sbc    | set bit count           |      |      |
</br>

## BUILD IMM

| OPC  | FUNC | DESCRIPTION              | IMM   |
|------|------|--------------------------|-------|
| 4bit |      |                          | 12bit |
| 1111 | imm  | build a immediate number |       |

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

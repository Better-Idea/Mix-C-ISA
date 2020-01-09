```
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
```

## # EIGHT PHASES OPERATION

| OPC  | FUNC          | MODE                 | REG/IMM | REG/IMM |
|------|---------------|----------------------|---------|---------|
| 2bit | 3bit          | 3bit                 | 4bit    | 4bit    |
|      | 000 : shl     | 000 : opc ra, rt, rb |         |         |
|      | 001 : shr     | 001 : opc ra, rt, im |         |         |
|      | 010 : shrc    | 010 : opc ra, rb, rt |         |         |
|      | 011 : div     | 011 : opc ra, im, rt |         |         |
|      | 100 : sub     | 100 : opc ra, ra, rb |         |         |
|      | 101 : sbb     | 101 : opc ra, ra, im |         |         |
|      | 110 : sub.f   | 110 : opc rt, ra, rb |         |         |
|      | 111 : div.f   | 111 : opc rt, ra, im |         |         |
</br>

## # BITWISE OPERATION 

| OPC  | FUNC          | MODE                   | REG     | REG/IMM |
|------|---------------|------------------------|---------|---------|
| 3bit | 3bit          | 2bit                   | 4bit    | 4bit    |
|      | 000 : bt      | 00 : opc ra, ra, rb    |         |         |
|      | 001 : bts     | 01 : opc ra, ra, im    |         |         |
|      | 010 : btr     | 10 : opc ra, ra, im+16 |         |         |
|      | 011 : btn     | 11 : opc rt, ra, rb    |         |         |
|      | 100 : and     |                        |         |         |
|      | 101 : or      |                        |         |         |
|      | 110 : xor     |                        |         |         |
|      | 111 : nand    |                        |         |         |
</br>

#### BITWISE MUX
A0  = B1 ? ~A : A  
A1  = B0 ? 32{1} : B  
A2  = B0 ? 32{1} : A  
A3  = B1 ? ~B : B  
A4 = A0 & A1 | A2 & A3  

| FUNC  | EXPR                 | B1 | B0 |
|-------|:--------------------:|:--:|:--:|
| and   | A & B                | 0  | 0  |
| or    | A \| B               | 0  | 1  |
| xor   | A & B                | 1  | 0  |
| nand  | ~(A & B) -> ~A \| ~B | 1  | 1  |

</br>

## # LOAD BASED ON BP

>note:  
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

| OPC  | FUNC                        | SIZE       | REG     | IMM     |
|------|-----------------------------|------------|---------|---------|
| 4bit | 1bit                        | 2bit       | 4bit    | 5bit    |
|      | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         |
|      | 1 : ldb.i ldw.i ldd.i ldq.i | 01 : word  |         |         |
|      |                             | 10 : dword |         |         |
|      |                             | 11 : qword |         |         |
</br>

## # LOAD BASED ON GENERAL-PURPOSE REGISTER
>note:  
the source register as the byte/word/dword/qword base pointer, offset step is 1/2/4/8

| OPC  | FUNC                        | SIZE       | REG     | REG     | PLUS REG-T                |
|------|-----------------------------|------------|---------|---------|---------------------------|
| 4bit | 1bit                        | 2bit       | 4bit    | 4bit    | 1bit                      |
|      | 0 : ldb.u ldw.u ldd.u ldq.u | 00 : byte  |         |         | 0 : ldx reg, reg[im]      |
|      | 1 : ldb.i ldw.i ldd.i ldq.i | 01 : word  |         |         | 1 : ldx reg, reg[im + rt] |
|      |                             | 10 : dword |         |         |                           |
|      |                             | 11 : qword |         |         |                           |
</br>

## # STORE BASED ON BP
| OPC  | FUNC                | SIZE       | REG     | IMM     |
|------|---------------------|------------|---------|---------|
| 4bit | 1bit                | 2bit       | 4bit    | 5bit    |
|      | 0 : stb ldw ldd ldq | 00 : byte  |         |         |
|      |                     | 01 : word  |         |         |
|      |                     | 10 : dword |         |         |
|      |                     | 11 : qword |         |         |
</br>

## # STORE BASED ON GENERAL-PURPOSE REGISTER
| OPC  | FUNC                        | SIZE       | REG     | REG     | PLUS REG-T                |
|------|-----------------------------|------------|---------|---------|---------------------------|
| 4bit | 1bit                        | 2bit       | 4bit    | 4bit    | 1bit                      |
|      | 1 : stb.u stw.u std.u stq.u | 00 : byte  |         |         | 0 : stx reg, reg[im]      |
|      |                             | 01 : word  |         |         | 1 : stx reg, reg[im + rt] |
|      |                             | 10 : dword |         |         |                           |
|      |                             | 11 : qword |         |         |                           |
</br>

## # FOUR PHASES OPERATION
| OPC  | FUNC          | MODE                 | REG     | REG/IMM |
|------|---------------|----------------------|---------|---------|
| 2bit | 3bit          | 2bit                 | 4bit    | 4bit    |
|      | 000 : add     | 00 : opc ra, ra, rb  |         |         |
|      | 001 : adc     | 01 : opc ra, ra, im  |         |         |
|      | 010 : mul     | 10 : opc rt, ra, rb  |         |         |
|      | 011 : -       | 11 : opc rt, ra, im  |         |         |
|      | 100 : add.f   |                      |         |         |
|      | 101 : mul.f   |                      |         |         |
|      | 110 : -       |                      |         |         |
|      | 111 : -       |                      |         |         |
</br>

## # CONDITION BRANCH
| OPC  | FUNC        | TYPE           | IMM     |
|------|-------------|----------------|---------|
| 4bit | 3bit        | 1bit           | 8bit    |
|      | 000 : eq/zr | 0 : if   block |         |
|      | 001 : ne/nz | 1 : elif block |         |
|      | 010 : a     |                |         |
|      | 011 : ae    |                |         |
|      | 100 : cf/ov |                |         |
|      | 101 : nc/no |                |         |
|      | 110 : uo    |                |         |
|      | 111 : do    |                |         |
</br>

1/16
OPC     FUNC IF/ELSE IMM
4       3    1       8
        |    |
        |    +-000:if block
        |    +-001:else block
        +------000:e/z      equal/zero
        +------001:ne/nz    not equal/zero
        +------010:a        above
        +------011:ae       above equal
        +------100:c/ov     carry/overflow
        +------101:nc/nov   not carry/overflow
        +------110:uo       up overflow
        +------111:do       down overflow

note:
pc(program pointer) += imm(unsigned) * 2(bytes per instruction) when match condition


if (condition-a){
    if (condition-b){

    }
}
else if (condition-c) {

}
else{

}


rest:2/8

1/16
OPC     FUNC IMM
4       2    10
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
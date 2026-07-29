<a name="contents"></a><a name="gbatek"></a>
Gameboy Advance / Nintendo DS / DSi / 3DS - Technical Info - Extracted from no$gba version 3.06

[About this Document](#aboutthisdocument)


# CPU Reference


## General ARM7TDMI Information

- [ARM CPU Overview](#armcpuoverview)
- [ARM CPU Register Set](#armcpuregisterset)
- [ARM CPU Flags & Condition Field (cond)](#armcpuflagsconditionfieldcond)
- [ARM CPU 26bit Memory Interface](#armcpu26bitmemoryinterface)
- [ARM CPU Exceptions](#armcpuexceptions)
- [ARM CPU Memory Alignments](#armcpumemoryalignments)

## Further Information

- [ARM Pseudo Instructions and Directives](#armpseudoinstructionsanddirectives)
- [ARM CP15 System Control Coprocessor](#armcp15systemcontrolcoprocessor)
- [ARM CPU Instruction Cycle Times](#armcpuinstructioncycletimes)
- [ARM CPU Versions](#armcpuversions)
- [ARM CPU Data Sheet](#armcpudatasheet)

# CPU 32bit ARM Mode


## ARM 32bit Opcodes (ARM Code)

- [ARM Instruction Summary](#arminstructionsummary)
- [ARM Branch and Branch with Link (B, BL, BX, BLX, SWI, BKPT)](#armopcodesbranchandbranchwithlinkbblbxblxswibkpt)
- [ARM Data Processing (ALU)](#armopcodesdataprocessingalu)
- [ARM Multiply and Multiply-Accumulate (MUL, MLA)](#armopcodesmultiplyandmultiplyaccumulatemulmla)
- [ARM Special ARM9 Instructions (CLZ, QADD/QSUB)](#armopcodesspecialarm9instructionsclzqaddqsub)
- [ARM PSR Transfer (MRS, MSR)](#armopcodespsrtransfermrsmsr)
- [ARM Memory: Single Data Transfer (LDR, STR, PLD)](#armopcodesmemorysingledatatransferldrstrpld)
- [ARM Memory: Halfword, Doubleword, and Signed Data Transfer](#armopcodesmemoryhalfworddoublewordandsigneddatatransfer)
- [ARM Memory: Block Data Transfer (LDM, STM)](#armopcodesmemoryblockdatatransferldmstm)
- [ARM Memory: Single Data Swap (SWP)](#armopcodesmemorysingledataswapswp)
- [ARM Coprocessor (MRC/MCR, LDC/STC, CDP, MCRR/MRRC)](#armopcodescoprocessorinstructionsmrcmcrldcstccdpmcrrmrrc)

# CPU 16bit THUMB Mode


## ARM 16bit Opcodes (THUMB Code)

- When operating in THUMB state, cut-down 16bit opcodes are used.
- THUMB is supported on T-variants of ARMv4 and up, ie. ARMv4T, ARMv5T, etc.
- [THUMB Instruction Summary](#thumbinstructionsummary)
- [THUMB Register Operations (ALU, BX)](#thumbopcodesregisteroperationsalubx)
- [THUMB Memory Load/Store (LDR/STR)](#thumbopcodesmemoryloadstoreldrstr)
- [THUMB Memory Addressing (ADD PC/SP)](#thumbopcodesmemoryaddressingaddpcsp)
- [THUMB Memory Multiple Load/Store (PUSH/POP and LDM/STM)](#thumbopcodesmemorymultipleloadstorepushpopandldmstm)
- [THUMB Jumps and Calls](#thumbopcodesjumpsandcalls)


# <a name="78k0opcodelist"></a>78K/0 Opcode List




## Move Byte


```
  85/87/AB/AA/AE              MOV  A, [DE]/[HL]/[HL+B]/[HL+C]/[HL+nn]
  F0/F4/8E                    MOV  A, [FExx]/[FFxx]/[nnnn]
  60/--/62/63/64/65/66/67     MOV  A, X/-/C/B/E/D/L/H
  70/--/72/73/74/75/76/77     MOV  X/-/C/B/E/D/L/H, A
  A0/A1/A2/A3/A4/A5/A6/A7     MOV  X/A/C/B/E/D/L/H, nn
  95/97/BB/BA/BE              MOV  [DE]/[HL]/[HL+B]/[HL+C]/[HL+nn], A
  F2/F6/9E                    MOV  [FExx]/[FFxx]/[nnnn], A
  11/13                       MOV  [FExx]/[FFxx], nn
  30/--/32/33/34/35/36/37     XCH  A, X/-/C/B/E/D/L/H
  05/07/318B/318A/DE          XCH  A, [DE]/[HL]/[HL+B]/[HL+C]/[HL+nn]
  83/93/CE                    XCH  A, [FExx]/[FFxx]/[nnnn]
```



## Move Word


```
  C2/C4/C6/10                 MOVW AX, BC/DE/HL/nnnn
  89/A9/02                    MOVW AX, [FExx]/[FFxx]/[nnnn]
  D2/D4/D6                    MOVW BC/DE/HL, AX
  12/14/16                    MOVW BC/DE/HL, nnnn
  99/B9/03                    MOVW [FExx]/[FFxx]/[nnnn], AX
  EE/FE                       MOVW [FExx]/[FFxx], nnnn
  E2/E4/E6                    XCHW AX,BC/DE/HL
  B1/B3/B5/B7/22              PUSH AX/BC/DE/HL/PSW  ;PSW=[FF1Eh] (8bit)
  B0/B2/B4/B6/23              POP  AX/BC/DE/HL/PSW  ;PSW=[FF1Eh] (8bit)
```



## ALU Byte


```
  6100/6101/6102/6103/6104/6105/6106/6107  ADD  X/A/C/B/E/D/L/H, A
  6120/6121/6122/6123/6124/6125/6126/6127  ADDC X/A/C/B/E/D/L/H, A
  6110/6111/6112/6113/6114/6115/6116/6117  SUB  X/A/C/B/E/D/L/H, A
  6130/6131/6132/6133/6134/6135/6136/6137  SUBC X/A/C/B/E/D/L/H, A
  6140/6141/6142/6143/6144/6145/6146/6147  CMP  X/A/C/B/E/D/L/H, A
  6150/6151/6152/6153/6154/6155/6156/6157  AND  X/A/C/B/E/D/L/H, A
  6160/6161/6162/6163/6164/6165/6166/6167  OR   X/A/C/B/E/D/L/H, A
  6170/6171/6172/6173/6174/6175/6176/6177  XOR  X/A/C/B/E/D/L/H, A
  6108/----/610A/610B/610C/610D/610E/610F  ADD  A, X/-/C/B/E/D/L/H
  6128/----/612A/612B/612C/612D/612E/612F  ADDC A, X/-/C/B/E/D/L/H
  6118/----/611A/611B/611C/611D/611E/611F  SUB  A, X/-/C/B/E/D/L/H
  6138/----/613A/613B/613C/613D/613E/613F  SUBC A, X/-/C/B/E/D/L/H
  6148/----/614A/614B/614C/614D/614E/614F  CMP  A, X/-/C/B/E/D/L/H
  6158/----/615A/615B/615C/615D/615E/615F  AND  A, X/-/C/B/E/D/L/H
  6168/----/616A/616B/616C/616D/616E/616F  OR   A, X/-/C/B/E/D/L/H
  6178/----/617A/617B/617C/617D/617E/617F  XOR  A, X/-/C/B/E/D/L/H
  0D/0F/310B/310A/09/0E/08   ADD  A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  2D/2F/312B/312A/29/2E/28   ADDC A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  1D/1F/311B/311A/19/1E/18   SUB  A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  3D/3F/313B/313A/39/3E/38   SUBC A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  4D/4F/314B/314A/49/4E/48   CMP  A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  5D/5F/315B/315A/59/5E/58   AND  A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  6D/6F/316B/316A/69/6E/68   OR   A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  7D/7F/317B/317A/79/7E/78   XOR  A,nn/[HL]/[HL+B]/[HL+C]/[HL+nn]/[FExx]/[nnnn]
  88                         ADD  [FExx], nn
  A8                         ADDC [FExx], nn
  98                         SUB  [FExx], nn
  B8                         SUBC [FExx], nn
  C8                         CMP  [FExx], nn
  D8                         AND  [FExx], nn
  E8                         OR   [FExx], nn
  F8                         XOR  [FExx], nn
  40/41/42/43/44/45/46/47/81 INC  X/A/C/B/E/D/L/H/[FExx]
  50/51/52/53/54/55/56/57/91 DEC  X/A/C/B/E/D/L/H/[FExx]
```



## ALU Word


```
  CA/DA/EA                   ADDW/SUBW/CMPW AX,nnnn
  80/82/84/86                INCW AX/BC/DE/HL
  90/92/94/96                DECW AX/BC/DE/HL
```



## Rotate/Shift


```
  26                         ROL  A,1
  24                         ROR  A,1
  27                         ROLC A,1
  25                         RORC A,1
  3180                       ROL4 A,[HL] ;12bit A.lsb:[HL],4 ;native: ROL4 [HL]
  3190                       ROR4 A,[HL] ;12bit A.lsb:[HL],4 ;native: ROR4 [HL]
```



## Jump/Call


```
  3198                       JMP  AX                    ;native: BR AX
  9B                         JMP  absolute addr         ;native: BR
  FA                         JMP  relative short addr   ;native: BR
  9A                         CALL absolute addr         ;native: CALL
  C1+(nn AND 3Eh)            CALL [40h..7Eh]            ;native: CALLT
  0C/1C/2C/3C/4C/5C/6C/7C    CALLF 08nn/09nn/0Ann/0Bnn/0Cnn/0Dnn/0Enn/0Fnn
```



## Conditional Relative Jumps


```
  8D                         JC  aka JB  rel  ;carry/below        ;native: BC
  AD                         JZ  aka JE  rel  ;zero/equal         ;native: BZ
  9D                         JNC aka JAE rel  ;not carry/below    ;native: BNC
  BD                         JNZ aka JNE rel  ;not zero/equal     ;native: BNZ
  04/8B/8A                   DJNZ [FExx]/B/C, rel  ;decrement+jnz ;native: DBNZ
```



## Test (and Clear) and Conditional Relative Jump


```
  3101/3105/3185/310D+n*10   BTCLR [FExx]/[FFxx]/[HL]/A.n, rel
  8C/3106/3186/310E+n*10     BT    [FExx]/[FFxx]/[HL]/A.n, rel
  3103/3107/3187/310F+n*10   BF    [FExx]/[FFxx]/[HL]/A.n, rel
```



## Bit Manipulation


```
  7101/7109/7181/6189+n*10   MOV1 [FExx]/[FFxx]/[HL]/A.n, CY
  7104/710C/7184/618C+n*10   MOV1 CY, [FExx]/[FFxx]/[HL]/A.n
  7105/710D/7185/618D+n*10   AND1 CY, [FExx]/[FFxx]/[HL]/A.n
  7106/710E/7186/618E+n*10   OR1  CY, [FExx]/[FFxx]/[HL]/A.n
  7107/710F/7187/618F+n*10   XOR1 CY, [FExx]/[FFxx]/[HL]/A.n
  20/0A/710A/7182/618A+n*10  SET1 CY/[FExx]/[FFxx]/[HL]/A.n
  21/0B/710B/7183/618B+n*10  CLR1 CY/[FExx]/[FFxx]/[HL]/A.n
  01                         NOT1 CY
```



## Misc


```
  00                         NOP
  BF/7110/7100               BRK/HALT/STOP
  AF/9F/8F                   RET/RETB/RETI
  61D0/61D8/61F0/61F8        SEL RB0/1/2/3
  3188                       MULU AX,A,X   ;unsigned AX=A*X    ;native: MULU X
  3182                       DIVUW AX,C    ;unsigned AX=AX/C, C=remainder   ;X
  6180/6190                  ADJBA/ADJBS   ;decimal BCD adjust A after Add/Sub
  7A1E/7B1E                  EI/DI         ;pseudo SET1/CLR1 PSW.7
  FA                         JR addr       ;pseudo jmp short relative addr
```



## Note


Operand [FExx] can he in range FE20h..FF1Fh, of which FE20h..FEFFh is encoded
as 20..FF, and FF00h..FF1Fh is encoded as 00..1F (the latter overlaps with
operand [FFxx]) (for the overlapping addresses, [FExx] is preferred because
BT/SET1/CLR1 have smaller encoding for [FExx] than [FFxx], and DJNZ/ALU support
only [FExx]) (all opcodes that support [FFxx] do also support [FExx], so [FFxx]
should be NEVER used for [FF00..FF1F]).




# <a name="78k0opcodemap"></a>78K/0 Opcode Map




## 78K/0 Primary Opcode Map


```
      00                10                20                30
  00  NOP               MOVW AX,nnnn      SET1 CY           XCH  A,X
  01  NOT1 CY           MOV  [FExx],nn    CLR1 CY           <prefix>
  02  MOVW AX,[nnnn]    MOVW BC,nnnn      PUSH PSW ;[FF1Eh] XCH  A,C
  03  MOVW [nnnn],AX    MOV  [FFxx],nn    POP  PSW ;[FF1Eh] XCH  A,B
  04  DBNZ [FExx],rel   MOVW DE,nnnn      ROR  A,1          XCH  A,E
  05  XCH  A,[DE]       <undef>           RORC A,1          XCH  A,D
  06  <undef>           MOVW HL,nnnn      ROL  A,1          XCH  A,L
  07  XCH  A,[HL]       <undef>           ROLC A,1          XCH  A,H
  08  ADD  A,[nnnn]     SUB  A,[nnnn]     ADDC A,[nnnn]     SUBC A,[nnnn]
  09  ADD  A,[HL+nn]    SUB  A,[HL+nn]    ADDC A,[HL+nn]    SUBC A,[HL+nn]
  0A  SET1 [FExx].0     SET1 [FExx].1     SET1 [FExx].2     SET1 [FExx].3
  0B  CLR1 [FExx].0     CLR1 [FExx].1     CLR1 [FExx].2     CLR1 [FExx].3
  0C  CALLF 08nn        CALLF 09nn        CALLF 0Ann        CALLF 0Bnn
  0D  ADD  A,nn         SUB  A,nn         ADDC A,nn         SUBC A,nn
  0E  ADD  A,[FExx]     SUB  A,[FExx]     ADDC A,[FExx]     SUBC A,[FExx]
  0F  ADD  A,[HL]       SUB  A,[HL]       ADDC A,[HL]       SUBC A,[HL]
```


```
      40                50                60                70
  40  INC  X            DEC  X            MOV  A,X          MOV  X,A
  41  INC  A            DEC  A            <prefix>          <prefix>
  42  INC  C            DEC  C            MOV  A,C          MOV  C,A
  43  INC  B            DEC  B            MOV  A,B          MOV  B,A
  44  INC  E            DEC  E            MOV  A,E          MOV  E,A
  45  INC  D            DEC  D            MOV  A,D          MOV  D,A
  46  INC  L            DEC  L            MOV  A,L          MOV  L,A
  47  INC  H            DEC  H            MOV  A,H          MOV  H,A
  48  CMP  A,[nnnn]     AND  A,[nnnn]     OR   A,[nnnn]     XOR  A,[nnnn]
  49  CMP  A,[HL+nn]    AND  A,[HL+nn]    OR   A,[HL+nn]    XOR  A,[HL+nn]
  4A  SET1 [FExx].4     SET1 [FExx].5     SET1 [FExx].6     SET1 [FExx].7
  4B  CLR1 [FExx].4     CLR1 [FExx].5     CLR1 [FExx].6     CLR1 [FExx].7
  4C  CALLF 0Cnn        CALLF 0Dnn        CALLF 0Enn        CALLF 0Fnn
  4D  CMP  A,nn         AND  A,nn         OR   A,nn         XOR  A,nn
  4E  CMP  A,[FExx]     AND  A,[FExx]     OR   A,[FExx]     XOR  A,[FExx]
  4F  CMP  A,[HL]       AND  A,[HL]       OR   A,[HL]       XOR  A,[HL]
```


```
      80                90                A0                B0
  80  INCW AX           DECW AX           MOV  X,nn         POP  AX
  81  INC  [FExx]       DEC  [FExx]       MOV  A,nn         PUSH AX
  82  INCW BC           DECW BC           MOV  C,nn         POP  BC
  83  XCH  A,[FExx]     XCH  A,[FFxx]     MOV  B,nn         PUSH BC
  84  INCW DE           DECW DE           MOV  E,nn         POP  DE
  85  MOV  A,[DE]       MOV  [DE],A       MOV  D,nn         PUSH DE
  86  INCW HL           DECW HL           MOV  L,nn         POP  HL
  87  MOV  A,[HL]       MOV  [HL],A       MOV  H,nn         PUSH HL
  88  ADD  [FExx],nn    SUB  [FExx],nn    ADDC [FExx],nn    SUBC [FExx],nn
  89  MOVW AX,[FExx]    MOVW [FExx],AX    MOVW AX,[FFxx]    MOVW [FFxx],AX
  8A  DBNZ C,rel        CALL nnnn         MOV  A,[HL+C]     MOV  [HL+C],A
  8B  DBNZ B,rel        BR   nnnn         MOV  A,[HL+B]     MOV  [HL+B],A
  8C  BT   [FExx].0,rel BT   [FExx].1,rel BT   [FExx].2,rel BT   [FExx].3,rel
  8D  BC   rel          BNC  rel          BZ   rel          BNZ  rel
  8E  MOV  A,[nnnn]     MOV  [nnnn],A     MOV  A,[HL+nn]    MOV  [HL+nn],A
  8F  RETI              RETB              RET               BRK
```


```
      C0                D0                E0                F0
  C0  <undef>           <undef>           <undef>           MOV  A,[FExx]
  C1  CALLT [40]        CALLT [50]        CALLT [60]        CALLT [70]
  C2  MOVW AX,BC        MOVW BC,AX        XCHW AX,BC        MOV  [FExx],A
  C3  CALLT [42]        CALLT [52]        CALLT [62]        CALLT [72]
  C4  MOVW AX,DE        MOVW DE,AX        XCHW AX,DE        MOV  A,[FFxx]
  C5  CALLT [44]        CALLT [54]        CALLT [64]        CALLT [74]
  C6  MOVW AX,HL        MOVW HL,AX        XCHW AX,HL        MOV  [FFxx],A
  C7  CALLT [46]        CALLT [56]        CALLT [66]        CALLT [76]
  C8  CMP  [FExx],nn    AND  [FExx],nn    OR   [FExx],nn    XOR  [FExx],nn
  C9  CALLT [48]        CALLT [58]        CALLT [68]        CALLT [78]
  CA  ADDW AX,nnnn      SUBW AX,nnnn      CMPW AX,nnnn      BR   rel
  CB  CALLT [4A]        CALLT [5A]        CALLT [6A]        CALLT [7A]
  CC  BT   [FExx].4,rel BT   [FExx].5,rel BT   [FExx].6,rel BT   [FExx].7,rel
  CD  CALLT [4C]        CALLT [5C]        CALLT [6C]        CALLT [7C]
  CE  XCH  A,[nnnn]     XCH  A,[HL+nn]    MOVW [FExx],nnnn  MOVW [FFxx],nnnn
  CF  CALLT [4E]        CALLT [5E]        CALLT [6E]        CALLT [7E]
```



## 78K/0 Opcodes with Prefix 31


```
  3100+xxx*10  -
  3180+xxx*10  - (unless below ROL/ROR)
  3101+bbb*10  BTCLR   [FExx].n,rel    ;n=0..7
  3181+xxx*10  -
  3102+xxx*10  -
  3182+xxx*10  - (unless below DIV)
  3103+bbb*10  BF      [FExx].n,rel    ;n=0..7
  3183+xxx*10  -
  3104+xxx*10  -
  3184+xxx*10  -
  3105+bbb*10  BTCLR   [FFxx].n,rel    ;n=0..7
  3185+bbb*10  BTCLR   [HL].n,rel      ;n=0..7
  3106+bbb*10  BT      [FFxx].n,rel    ;n=0..7
  3186+bbb*10  BT      [HL].n,rel      ;n=0..7
  3107+bbb*10  BF      [FFxx].n,rel    ;n=0..7
  3187+bbb*10  BF      [HL].n,rel      ;n=0..7
  3108+xxx*10  -
  3188+xxx*10  - (unless below MUL/BR)
  3109+xxx*10  -
  3189+xxx*10  -
  310A+xxx*10  <see below ADD/SUB/ADDC/SUBC/CMP/AND/OR/XOR)
  318A+xxx*10  - (unless below XCH)
  310B+xxx*10  <see below ADD/SUB/ADDC/SUBC/CMP/AND/OR/XOR)
  318B+xxx*10  - (unless below XCH)
  310C+xxx*10  -
  318C+xxx*10  -
  310D+bbb*10  BTCLR   A.n,rel         ;n=0..7
  318D+xxx*10  -
  310E+bbb*10  BT      A.n,rel         ;n=0..7
  318E+xxx*10  -
  310F+bbb*10  BF      A.n,rel         ;n=0..7
  318F+xxx*10  -
  310A         ADD     A,[HL+C]
  310B         ADD     A,[HL+B]
  311A         SUB     A,[HL+C]
  311B         SUB     A,[HL+B]
  312A         ADDC    A,[HL+C]
  312B         ADDC    A,[HL+B]
  313A         SUBC    A,[HL+C]
  313B         SUBC    A,[HL+B]
  314A         CMP     A,[HL+C]
  314B         CMP     A,[HL+B]
  315A         AND     A,[HL+C]
  315B         AND     A,[HL+B]
  316A         OR      A,[HL+C]
  316B         OR      A,[HL+B]
  317A         XOR     A,[HL+C]
  317B         XOR     A,[HL+B]
  318A         XCH     A,[HL+C]
  318B         XCH     A,[HL+B]
  3180         ROL4    A,[HL]   ;rol 12bit A.lsb:[HL],4      ;native: ROL4 [HL]
  3190         ROR4    A,[HL]   ;ror 12bit A.lsb:[HL],4      ;native: ROR4 [HL]
  3182         DIVUW   AX,C     ;unsigned AX=AX/C, C=remainder ;native: DIVWU C
  3188         MULU    AX,A,X   ;unsigned AX=A*X               ;native: MULU X
  3198         BR      AX
```



## 78K/0 Opcodes with Prefix 61


```
  6100+rrr     ADD     r,A            ;r=X,A,C,B,E,D,L,H
  6108+rrr     ADD     A,r            ;r=X,-,C,B,E,D,L,H
  6110+rrr     SUB     r,A            ;r=X,A,C,B,E,D,L,H
  6118+rrr     SUB     A,r            ;r=X,-,C,B,E,D,L,H
  6120+rrr     ADDC    r,A            ;r=X,A,C,B,E,D,L,H
  6128+rrr     ADDC    A,r            ;r=X,-,C,B,E,D,L,H
  6130+rrr     SUBC    r,A            ;r=X,A,C,B,E,D,L,H
  6138+rrr     SUBC    A,r            ;r=X,-,C,B,E,D,L,H
  6140+rrr     CMP     r,A            ;r=X,A,C,B,E,D,L,H
  6148+rrr     CMP     A,r            ;r=X,-,C,B,E,D,L,H
  6150+rrr     AND     r,A            ;r=X,A,C,B,E,D,L,H
  6158+rrr     AND     A,r            ;r=X,-,C,B,E,D,L,H
  6160+rrr     OR      r,A            ;r=X,A,C,B,E,D,L,H
  6168+rrr     OR      A,r            ;r=X,-,C,B,E,D,L,H
  6170+rrr     XOR     r,A            ;r=X,A,C,B,E,D,L,H
  6178+rrr     XOR     A,r            ;r=X,-,C,B,E,D,L,H
  6180+xxx*10  - (unless below ADJ/SEL)
  6181+xxx*10  -
  6182+xxx*10  -
  6183+xxx*10  -
  6184+xxx*10  -
  6185+xxx*10  -
  6186+xxx*10  -
  6187+xxx*10  -
  6188+xxx*10  - (unless below ADJ/SEL)
  6189+bbb*10  MOV1    A.n,CY         ;n=0..7
  618A+bbb*10  SET1    A.n            ;n=0..7
  618B+bbb*10  CLR1    A.n            ;n=0..7
  618C+bbb*10  MOV1    CY,A.n         ;n=0..7
  618D+bbb*10  AND1    CY,A.n         ;n=0..7
  618E+bbb*10  OR1     CY,A.n         ;n=0..7
  618F+bbb*10  XOR1    CY,A.n         ;n=0..7
  6180         ADJBA                ;Decimal BCD adjust A after Addition
  6190         ADJBS                ;Decimal BCD adjust A after Subtraction
  61D0         SEL     RB0
  61D8         SEL     RB1
  61F0         SEL     RB2
  61F8         SEL     RB3
```



## 78K/0 Opcodes with Prefix 71


```
  7100+xxx*10  - (unless below STOP/HALT)
  7180+xxx*10  -
  7101+bbb*10  MOV1    [FExx].n,CY    ;n=0..7
  7181+bbb*10  MOV1    [HL].n,CY      ;n=0..7
  7102+xxx*10  -
  7182+bbb*10  SET1    [HL].n         ;n=0..7
  7103+xxx*10  -
  7183+bbb*10  CLR1    [HL].n         ;n=0..7
  7104+bbb*10  MOV1    CY,[FExx].n    ;n=0..7
  7184+bbb*10  MOV1    CY,[HL].n      ;n=0..7
  7105+bbb*10  AND1    CY,[FExx].n    ;n=0..7
  7185+bbb*10  AND1    CY,[HL].n      ;n=0..7
  7106+bbb*10  OR1     CY,[FExx].n    ;n=0..7
  7186+bbb*10  OR1     CY,[HL].n      ;n=0..7
  7107+bbb*10  XOR1    CY,[FExx].n    ;n=0..7
  7187+bbb*10  XOR1    CY,[HL].n      ;n=0..7
  7108+xxx*10  -
  7188+xxx*10  -
  7109+bbb*10  MOV1    [FFxx].n,CY    ;n=0..7
  7189+xxx*10  -
  710A+bbb*10  SET1    [FFxx].n       ;n=0..7
  718A+xxx*10  -
  710B+bbb*10  CLR1    [FFxx].n       ;n=0..7
  718B+xxx*10  -
  710C+bbb*10  MOV1    CY,[FFxx].n    ;n=0..7
  718C+xxx*10  -
  710D+bbb*10  AND1    CY,[FFxx].n    ;n=0..7
  718D+xxx*10  -
  710E+bbb*10  OR1     CY,[FFxx].n    ;n=0..7
  718E+xxx*10  -
  710F+bbb*10  XOR1    CY,[FFxx].n    ;n=0..7
  718F+xxx*10  -
  7100         STOP
  7110         HALT
```




# <a name="78k0memorymapandsfrs"></a>78K/0 Memory Map and SFRs




## 78K0 References


```
  78K0/Kx2 User's Manual: Hardware       ;info on I/O Ports
  78K0/Kx2 Flash Memory Self Programming ;info on 8100h function
  78K0/Kx2 Flash Memory Programming      ;info on UART/SPI cable
  78K/0 Series Instructions              ;info on opcodes
  78K0/KB2 One-sheet Manual              ;quick reference poster
```



## 78K/0 Memory Map


```
  0000h..003Fh  FLASH ROM Exception Vectors (16bit each)
  0040h..007Fh  FLASH ROM CALLT Vectors (16bit each)
  0080h..0084h  FLASH ROM Option Byte Area (5 bytes)
  0085h..008Eh  FLASH ROM Debug Security Area (10 bytes, if any)
  008Fh..07FFh  FLASH ROM
  0800h..0FFFh  FLASH ROM (accessible via CALLF opcodes)
  1000h..1FFFh  FLASH ROM (can be swapped with 0000h..0FFFh)
  2000h..xFFFh  FLASH ROM (if any, in larger chips)
  8000h..BFFFh  FLASH ROM (if any, in larger chips) (bank-switched via BANK)
  8000h..9FFFh  System ROM area (when mapped, with function handler at 8100h)
  A000h..BFFFh  System ROM mirror (same as 8000h..9FFFh)
  C000h..       System ROM empty           (mostly FFh-filled, some FEh-bytes)
  E000h..F7FFh  Extra RAM (max 6K, if any) (mostly FFh-filled, some F0h-bytes)
  F800h..F8FFh  Undoc SFR's
  F900h..FAFFh  Zerofilled
  FB00h..FBFFh  Unknown (filled with random values) (more RAM?) (unused in DSi)
                (FB00h is ONLY mapped when ROM enabled, otherwise FFh-filled)
  FC00h..FF1Fh  RAM (officially/used area)
  FE20h..FEDFh  RAM (accessible via short "saddr" opcodes)
  FEE0h..FEE7h  Register Bank 3 (X,A,C,B,E,D,L,H aka AX,BC,DE,HL)
  FEE8h..FEEFh  Register Bank 2 (X,A,C,B,E,D,L,H aka AX,BC,DE,HL)
  FEF0h..FEF7h  Register Bank 1 (X,A,C,B,E,D,L,H aka AX,BC,DE,HL)
  FEF8h..FEFFh  Register Bank 0 (X,A,C,B,E,D,L,H aka AX,BC,DE,HL)
  FF00h..FFFFh  Special Function Registers (SFR's)
```

FLASH banks are 1Kbyte tall (aka erase block size?).



## 78K/0 Vectors


```
  0000h RESET input, POC, LVI, WDT
  0002h -
  0004h INTLVI
  0006h INTP0
  0008h INTP1
  000Ah INTP2
  000Ch INTP3
  000Eh INTP4
  0010h INTP5
  0012h INTSRE6
  0014h INTSR6
  0016h INTST6
  0018h INTCSI10/INTST0
  001Ah INTTMH1
  001Ch INTTMH0
  001Eh INTTM50
  0020h INTTM000
  0022h INTTM010
  0024h INTAD
  0026h INTSR0
  0028h INTWTI
  002Ah INTTM51
  002Ch INTKR
  002Eh INTWT
  0030h INTP6
  0032h INTP7
  0034h INTIIC0/NTDMU
  0036h INTCSI11
  0038h INTTM001
  003Ah INTTM011
  003Ch INTACSI
  003Eh BRK
```



## 78K/0 Option Bytes / Debug Security


```
  0080h db   07Eh   ;Option Byte: Watchdog Mode
  0081h db   000h   ;Option Byte: POC Mode (0=1.59V, 1=2.7V)
  0082h db   000h   ;Option Byte: Reserved (00h)
  0083h db   000h   ;Option Byte: Reserved (00h)
  0084h db   000h   ;Option Byte: On-chip Debug Control (0=Off)
       ;---
  0085h db   0CCh   ;On-chip debug security ID (0Ah bytes) ...
  0086h db   023h
  0087h db   0BAh
  0088h db   0C3h
  0089h db   07Dh
  008Ah db   094h
  008Bh db   0FEh
  008Ch db   073h
  008Dh db   0A5h
  008Eh db   0E6h
```



## 78K/0 Program Status Word (PSW) aka [FF1Eh]


```
  7  IE     ;Interrupt Enable
  6  Z      ;Zero Flag
  5  RBS1   ;Register Bank Select bit1
  4  AC     ;Aux Carry Flag
  3  RBS0   ;Register Bank Select bit0
  2  0
  1  ISP    ;In-Service Priority Flag
  0  CY     ;Carry Flag
```



## 78K/0 Special Function Registers (SFR's)


```
  Addr  Symbol R/W 1BW Reset BCDEF Special Function Register (SFR)
  FF00h P0     R/W 1B- 00h   BCDEF Port 0
  FF01h P1     R/W 1B- 00h   BCDEF Port 1
  FF02h P2     R/W 1B- 00h   BCDEF Port 2
  FF03h P3     R/W 1B- 00h   BCDEF Port 3
  FF04h P4     R/W 1B- 00h   -CDEF Port 4
  FF05h P5     R/W 1B- 00h   ---EF Port 5
  FF06h P6     R/W 1B- 00h   BCDEF Port 6
  FF07h P7     R/W 1B- 00h   -CDEF Port 7
  FF08h ADCR   R   --W 0000h BCDEF 10-bit A/D conversion result
  FF09h ADCRH  R   -B- 00h   BCDEF 8-bit A/D conversion result
  FF0Ah RXB6   R   -B- FFh   BCDEF Receive buffer 6
  FF0Bh TXB6   R/W -B- FFh   BCDEF Transmit buffer 6
  FF0Ch P12    R/W 1B- 00h   BCDEF Port 12
  FF0Dh P13    R/W 1B- 00h   -cDEF Port 13 (KC2: 48pin only)
  FF0Eh P14    R/W 1B- 00h   -cDEF Port 14 (KC2: 48pin only)
  FF0Fh SIO10  R   -B- 00h   BCDEF Serial I/O shift 10
  FF10h TM00   R   --W 0000h BCDEF 16-bit timer counter 00
  FF12h CR000  R/W --W 0000h BCDEF 16-bit timer capture/compare 000
  FF14h CR010  R/W --W 0000h BCDEF 16-bit timer capture/compare 010
  FF16h TM50   R   -B- 00h   BCDEF 8-bit timer counter 50
  FF17h CR50   R/W -B- 00h   BCDEF 8-bit timer compare 50
  FF18h CMP00  R/W -B- 00h   BCDEF 8-bit timer H compare 00
  FF19h CMP10  R/W -B- 00h   BCDEF 8-bit timer H compare 10
  FF1Ah CMP01  R/W -B- 00h   BCDEF 8-bit timer H compare 01
  FF1Bh CMP11  R/W -B- 00h   BCDEF 8-bit timer H compare 11
  FF1Ch SP     R/W --W xxxxh BCDEF CPU Stack Pointer      ;\undocumented, used
  FF1Eh PSW    R/W 1B- xxh   BCDEF CPU Program Status Word;/in pseudo opcodes
  FF1Fh TM51   R   -B- 00h   BCDEF 8-bit timer counter 51
  FF20h PM0    R/W 1B- FFh   BCDEF Port mode 0
  FF21h PM1    R/W 1B- FFh   BCDEF Port mode 1
  FF22h PM2    R/W 1B- FFh   BCDEF Port mode 2
  FF23h PM3    R/W 1B- FFh   BCDEF Port mode 3
  FF24h PM4    R/W 1B- FFh   -CDEF Port mode 4
  FF25h PM5    R/W 1B- FFh   ---EF Port mode 5
  FF26h PM6    R/W 1B- FFh   BCDEF Port mode 6
  FF27h PM7    R/W 1B- FFh   -CDEF Port mode 7
  FF28h ADM    R/W 1B- 00h   BCDEF A/D converter mode
  FF29h ADS    R/W 1B- 00h   BCDEF Analog input channel specification
  FF2Ch PM12   R/W 1B- FFh   BCDEF Port mode 12
  FF2Eh PM14   R/W 1B- FFh   -cDEF Port mode 14 (KC2: 48pin only)
  FF2Fh ADPC   R/W 1B- 00h   BCDEF A/D port configuration
  FF30h PU0    R/W 1B- 00h   BCDEF Pull-up resistor option 0
  FF31h PU1    R/W 1B- 00h   BCDEF Pull-up resistor option 1
  FF33h PU3    R/W 1B- 00h   BCDEF Pull-up resistor option 3
  FF34h PU4    R/W 1B- 00h   -CDEF Pull-up resistor option 4
  FF35h PU5    R/W 1B- 00h   ---EF Pull-up resistor option 5
  FF36h PU6    R/W 1B- 00h   ----F Pull-up resistor option 6
  FF37h PU7    R/W 1B- 00h   -CDEF Pull-up resistor option 7
  FF3Ch PU12   R/W 1B- 00h   BCDEF Pull-up resistor option 12
  FF3Eh PU14   R/W 1B- 00h   -cDEF Pull-up resistor option 14 (KC2: 48pin only)
  FF40h CKS    R/W 1B- 00h   -cDEF Clock output selection     (KC2: 48pin only)
  FF41h CR51   R/W -B- 00h   BCDEF 8-bit timer compare 51
  FF43h TMC51  R/W 1B- 00h   BCDEF 8-bit timer mode control 51
  FF48h EGP    R/W 1B- 00h   BCDEF External interrupt rising edge enable
  FF49h EGN    R/W 1B- 00h   BCDEF External interrupt falling edge enable
  FF4Ah SIO11  R   -B- 00h   ---eF Serial I/O shift 11 (KE2: 48KB only)
  FF4Ch SOTB11 R/W -B- 00h   ---eF Transmit buffer 11  (KE2: 48KB only)
  FF4Fh ISC    R/W 1B- 00h   BCDEF Input switch control
  FF50h ASIM6  R/W 1B- 01h   BCDEF Async serial interface operation mode 6
  FF53h ASIS6  R   -B- 00h   BCDEF Async serial interface reception err/stat 6
  FF55h ASIF6  R   -B- 00h   BCDEF Async serial interface transmission status 6
  FF56h CKSR6  R/W -B- 00h   BCDEF Clock selection 6
  FF57h BRGC6  R/W -B- FFh   BCDEF Baud rate generator control 6
  FF58h ASICL6 R/W 1B- 16h   BCDEF Async serial interface control 6
  FF60h SDR0   R   -BW 0000h -cdeF Remainder data 0 bit0..         ;\
  FF61h SDR0H  R   -B- 00h   -cdeF Remainder data 0 bit8..         ;
  FF62h MDA0L  R/W -BW 0000h -cdeF Multiply/divide data A0 bit0..  ; KC2-KE2:
  FF63h MDA0LH R/W -B- 00h   -cdeF Multiply/divide data A0 bit8..  ; 48KB only
  FF64h MDA0H  R/W -BW 0000h -cdeF Multiply/divide data A0 bit16.. ;
  FF65h MDA0HH R/W -B- 00h   -cdeF Multiply/divide data A0 bit24.. ;
  FF66h MDB0   R/W -BW 0000h -cdeF Multiply/divide data B0 bit0..  ;
  FF67h MDB0H  R/W -B- 00h   -cdeF Multiply/divide data B0 bit8..  ;
  FF68h DMUC0  R/W 1B- 00h   -cdeF Multiply/divide control 0       ;/
  FF69h TMHMD0 R/W 1B- 00h   BCDEF 8-bit timer H mode 0
  FF6Ah TCL50  R/W 1B- 00h   BCDEF Timer clock selection 50
  FF6Bh TMC50  R/W 1B- 00h   BCDEF 8-bit timer mode control 50
  FF6Ch TMHMD1 R/W 1B- 00h   BCDEF 8-bit timer H mode 1
  FF6Dh TMCYC1 R/W 1B- 00h   BCDEF 8-bit timer H carrier control 1
  FF6Eh KRM    R/W 1B- 00h   -CDEF Key return mode
  FF6Fh WTM    R/W 1B- 00h   -CDEF Watch timer operation mode
  FF70h ASIM0  R/W 1B- 01h   BCDEF Async serial interface operation mode 0
  FF71h BRGC0  R/W -B- 1Fh   BCDEF Baud rate generator control 0
  FF72h RXB0   R   -B- FFh   BCDEF Receive buffer 0
  FF73h ASIS0  R   -B- 00h   BCDEF Async serial interface reception err/stat 0
  FF74h TXS0   W   -B- FFh   BCDEF Transmit shift 0
  FF80h CSIM10 R/W 1B- 00h   BCDEF Serial operation mode 10
  FF81h CSIC10 R/W 1B- 00h   BCDEF Serial clock selection 10
  FF84h SOTB10 R/W -B- 00h   BCDEF Transmit buffer 10
  FF88h CSIM11 R/W 1B- 00h   ---eF Serial operation mode 11  (KE2: 48KB only)
  FF89h CSIC11 R/W 1B- 00h   ---eF Serial clock selection 11 (KE2: 48KB only)
  FF8Ch TCL51  R/W 1B- 00h   BCDEF Timer clock selection 51
  FF90h CSIMA0 R/W 1B- 00h   ----F Serial operation mode specification 0
  FF91h CSIS0  R/W 1B- 00h   ----F Serial status 0
  FF92h CSIT0  R/W 1B- 00h   ----F Serial trigger 0
  FF93h BRGCA0 R/W -B- 03h   ----F Division value selection 0
  FF94h ADTP0  R/W -B- 00h   ----F Automatic data transfer address point spec 0
  FF95h ADTI0  R/W -B- 00h   ----F Automatic data transfer interval spec 0
  FF96h SIOA0  R/W -B- 00h   ----F Serial I/O shift 0
  FF97h ADTC0  R   -B- 00h   ----F Automatic data transfer address count 0
  FF99h WDTE   R/W -B- xAh   BCDEF Watchdog timer enable (initial=1Ah/9Ah)
  FF9Fh OSCCTL R/W 1B- 00h   BCDEF Clock operation mode select
  FFA0h RCM    R/W 1B- x0h   BCDEF Internal oscillation mode
  FFA1h MCM    R/W 1B- 00h   BCDEF Main clock mode
  FFA2h MOC    R/W 1B- 80h   BCDEF Main OSC control
  FFA3h OSTC   R   1B- 00h   BCDEF Oscillation stabilization time counter stat
  FFA4h OSTS   R/W -B- 05h   BCDEF Oscillation stabilization time select
  FFA5h IIC0   R/W -B- 00h   BCDEF IIC shift 0
  FFA6h IICC0  R/W 1B- 00h   BCDEF IIC control 0
  FFA7h SVA0   R/W -B- 00h   BCDEF Slave address 0
  FFA8h IICCL0 R/W 1B- 00h   BCDEF IIC clock selection 0
  FFA9h IICX0  R/W 1B- 00h   BCDEF IIC function expansion 0
  FFAAh IICS0  R   1B- 00h   BCDEF IIC status 0
  FFABh IICF0  R/W 1B- 00h   BCDEF IIC flag 0
  FFACh RESF   R   -B- xxh   BCDEF Reset control flag
  FFB0h TM01   R   --W 0000h ---eF 16-bit timer counter 01           ;\
  FFB2h CR001  R/W --W 0000h ---eF 16-bit timer capture/compare 001  ;
  FFB4h CR011  R/W --W 0000h ---eF 16-bit timer capture/compare 011  ; KE2:
  FFB6h TMC01  R/W 1B- 00h   ---eF 16-bit timer mode control 01      ; 48KB
  FFB7h PRM01  R/W 1B- 00h   ---eF Prescaler mode 01                 ; only)
  FFB8h CRC01  R/W 1B- 00h   ---eF Capture/compare control 01        ;
  FFB9h TOC01  R/W 1B- 00h   ---eF 16-bit timer output control 01    ;/
  FFBAh TMC00  R/W 1B- 00h   BCDEF 16-bit timer mode control 00
  FFBBh PRM00  R/W 1B- 00h   BCDEF Prescaler mode 00
  FFBCh CRC00  R/W 1B- 00h   BCDEF Capture/compare control 00
  FFBDh TOC00  R/W 1B- 00h   BCDEF 16-bit timer output control 00
  FFBEh LVIM   R/W 1B- xxh   BCDEF Low-voltage detection
  FFBFh LVIS   R/W 1B- xxh   BCDEF Low-voltage detection level selection
  FFC0h PFCMD  W?  ?B? ?     ?  Undoc: Flash Protect Command (write A5h)
  FFC1h -      R/W 1B- ?     ?  Undoc: Flash ... FFC1h ... bits/pulses   FSSQ ?
  FFC2h PFS    R       ?     ?  Undoc: Flash Protect Status
  FFC4h FLPMC  R/W 1B? ?     ?  Undoc: Flash Programming Mode Ctl ;triple write
  FFC5h -      W   1?- ?     ?  Undoc: Flash ... 0FFC5h ... bits/mode   ?
  FFC6h FLAPH  W   1B- ?     ?  Undoc: Flash addr ptr upper 8bit (bit1=flag)
  FFC7h -      R/W 1?- ?     ?  Undoc: Flash ... 0FFC7h ... flag bits
  FFC8h FLAPL  W   1BW ?     ?  Undoc: Flash addr ptr lower 16bit (bit0,1=flag)
  FFCAh -      R/W ?B? ?     ?  Undoc: Flash ... 0FFCAh ...       ;triple write
  FFCBh FLWE   W   ?B- ?     ?  Undoc: Flash write data ECC error correction?
  FFCCh FLWH   W   -BW ?     ?  Undoc: Flash write data upper 16bit  ;\4-byte
  FFCEh FLWL   W   -BW ?     ?  Undoc: Flash write data lower 16bit  ;/
  FFE0h IF0L   R/W 1BW 00h   BCDEF Interrupt request flag 0L      ;\IF0
  FFE1h IF0H   R/W 1BW 00h   BCDEF Interrupt request flag 0H      ;/
  FFE2h IF1L   R/W 1BW 00h   BCDEF Interrupt request flag 1L      ;\IF1
  FFE3h IF1H   R/W 1BW 00h   BCDEF Interrupt request flag 1H      ;/
  FFE4h MK0L   R/W 1BW FFh   BCDEF Interrupt mask flag 0L         ;\MK0
  FFE5h MK0H   R/W 1BW FFh   BCDEF Interrupt mask flag 0H         ;/
  FFE6h MK1L   R/W 1BW FFh   BCDEF Interrupt mask flag 1L         ;\MK1
  FFE7h MK1H   R/W 1BW FFh   BCDEF Interrupt mask flag 1H         ;/
  FFE8h PR0L   R/W 1BW FFh   BCDEF Priority specification flag 0L ;\PR0
  FFE9h PR0H   R/W 1BW FFh   BCDEF Priority specification flag 0H ;/
  FFEAh PR1L   R/W 1BW FFh   BCDEF Priority specification flag 1L ;\PR1
  FFEBh PR1H   R/W 1BW FFh   BCDEF Priority specification flag 1H ;/
  FFF0h IMS    R/W -B- CFh   BCDEF Internal memory size switching
  FFF3h BANK   R/W -B- 00h   --def Memory bank select  (KD2-KF2: min 96KB only)
  FFF4h IXS    R/W -B- 0Ch   bcdeF Internal expansion RAM size switching
  FFF5h -      R   ?B- ?     ?  Undoc: chip_id (package type and memory size)
  FFFBh PCC    R/W 1B- 01h   BCDEF Processor clock control
```

And, more undoc/secret registers:

```
  F800h..F0h   -   --- ?     ?  Undoc: unknown values, unused by software
  F8F1h FLrE   R   -B- ?     ?  Undoc: Flash read data ECC error correction?
  F8F2h FLrH   R   -BW ?     ?  Undoc: Flash read data upper 16bit  ;\4-byte
  F8F4h FLrL   R   -BW ?     ?  Undoc: Flash read data lower 16bit  ;/total
  F8F6h ???    R/W --W ?     ?  Undoc: Flash ...0F8F6h_word     ;-16bit
  F8F8h -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8F9h ???    R/W -B- ?     ?  Undoc: Flash ...0F8F9h          ;-
  F8FAh -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8FBh -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8FCh -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8FDh -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8FEh -      -   --- ?     ?  Undoc:   ;(unused by software)
  F8FFh -      -   --- ?     ?  Undoc:   ;(unused by software)
```


Whereas,

```
  1BT   = supports Bit/Byte/Word access (1bit/8bit/16bit)
  BCDEF = supported on KB2/KC2/KD2/KE2/KF2 chip versions
```

For IMS/IXS: Regardless of the internal memory capacity, the initial values of
the internal memory size switching register (IMS) and internal expansion RAM
size switching register (IXS) of all products in the 78K0/Kx2 microcontrollers
are fixed (IMS = CFH, IXS = 0CH). Therefore, set the value corresponding to
each product as indicated in Tables 3-1 and 3-2.

For IMS/IXS: The ROM and RAM capacities of the products with the on-chip debug
function can be debugged by setting IMS and IXS, according to the debug target
products. Set IMS and IXS according to the debug target products.

For IXS on KB2-KE2: Set IXS only in products with internal expansion RAM.

The reset values of RES, LVIM and LVIS vary depending on the reset source.

The reset value of WDTE is determined by setting of option byte.

The reset value of RCM is 00h immediately after a reset release but
automatically changes to 80h after oscillation accuracy stabilization of
high-speed internal oscillator has been waited.




# <a name="pngbitmaps"></a>PNG Bitmaps



Used by DSi Browser (rom:\\*.png)



## File Header


```
  000h 8    ID (89h,'PNG',0Dh,0Ah,1Ah,0Ah)
  008h ..   Chunks
```



## Chunk Format


```
  000h 4    Chunk Size (LEN)                  ;big-endian
  004h 4    Chunk ID (4-letter ASCII) ("A..Z" and "a..z")
  008h LEN  Chunk Data
  ...  4    Chunk CRC32 on [004h+(0..LEN+3)]  ;big-endian
```

The Chunk ID is 4-letter ASCII, with upper/lowercase having special meanings:

```
  1st char  Uppercase=Critical (Basic)   Lowercase=Ancillary (Optional extras)
  2nd char  Uppercase=Public (Official)  Lowercase=Private (Custom extensions)
  3rd char  Uppercase=Normal             Lowercase=Reserved
  4th char  Uppercase=Unsafe to copy     Lowercase=Safe to copy
```

PNG Chunks

```
  IHDR v1.0  Image header (first chunk)                ;\
  PLTE v1.0  Palette (if any)                          ; Critical chunks
  IDAT v1.0  Image data (can be multiple IDAT chunks!) ; (aka the bitmap data)
  IEND v1.0  Image trailer (last chunk)                ;/
  tRNS v1.0  Transparency                              ;-Transparency
  cHRM v1.0  Primary chromaticities and white point    ;\
  gAMA v1.0  Image gamma                               ;
  iCCP v1.1  Embedded ICC profile                      ; Colour space info
  sBIT v1.0  Significant bits                          ;
  sRGB v1.1  Standard RGB colour space                 ;
  cICP v3rd  Coding-independent code points            ;/
  bKGD v1.0  Background colour                         ;\
  hIST v1.0  Image histogram                           ;
  pHYs v1.0  Physical pixel dimensions                 ; Miscellaneous info
  sPLT v1.1  Suggested palette                         ;
  eXIf v3rd  EXIF chunk (starts with "MM"=byteorder)   ;/
  tEXt v1.0  Textual data                              ;\Textual information
  zTXt v1.0  Compressed textual data                   ; (aka comments)
  iTXt v1.2  International textual data                ;/
  tIME v1.0  Image last-modification time              ;-Time stamp
  acTL v3rd  Animation Control Chunk                   ;\
  fcTL v3rd  Frame Control Chunk                       ; Animation (APNG)
  fdAT v3rd  Frame Data Chunk                          ;/
```

Note: There are some rules for the chunk ordering, see PNG specs for details.



## IHDR Chunk (first chunk)


```
  000h 4    Chunk Size (always 0Dh)       ;big-endian
  004h 4    Chunk ID   ("IHDR")
  008h 4    Width in pixels  (0=invalid)  ;big-endian
  00Ch 4    Height in pixels (0=invalid)  ;big-endian
  010h 1    Bits per sample  (1,2,4,8,16) (allowed values depend on Color Type)
  011h 1    Color Type       (0,2,3,4,6)
  012h 1    Compression method  (always 0=Zlib-Deflate)
  013h 1    Filter method       (always 0=Adaptive; with subtype per line)
  014h 1    Interlace method    (0=None, 1=Adam7)
  015h 4    Chunk CRC32 on [004h..]       ;big-endian
```

The valid Color types and bits per sample are:

```
  Color type             (Samples)  Bits/sample     Bits/pixel
  0=Grayscale            (I)        1,2,4,8,16  --> 1,2,4,8,16
  2=Truecolor            (RGB)      8,16        --> 24,48
  3=Palette indices      (I)        1,2,4,8     --> 1,2,4,8
  4=Grayscale with alpha (IA)       8,16        --> 16,32
  6=Truecolor with alpha (RGBA)     8,16        --> 32,64
```



## PLTE Chunk (required for Color type 3)


```
  000h 4    Chunk Size (LEN=NumColors*3) (3..300h)            ;big-endian
  004h 4    Chunk ID ("PLTE")
  008h LEN  Palette Data (three bytes per color, ordered R,G,B)
  ...  4    Chunk CRC32 on [004h+(0..LEN+3)]  ;big-endian
```

Additional Alpha values for each palette entry can be defined in tRNS chunk.



## IDAT Chunk(s)


```
  000h 4    Chunk Size (LEN)                      ;big-endian
  004h 4    Chunk ID ("IDAT")
  008h LEN  Zlib-Deflate compressed image data
  ...  4    Chunk CRC32 on [004h+(0..LEN+3)]  ;big-endian
```

There may be multiple IDAT chunks; if so, they shall appear consecutively with
no other "intervening" chunks. The compressed datastream is then the
"concatenation" of the contents of the data fields of all the IDAT chunks.



## IEND Chunk (last chunk)


```
  000h 4    Chunk Size (0)                    ;big-endian
  004h 4    Chunk ID ("IEND")
  008h 4    Chunk CRC32 on [004h..007h] (always AE426082h) ;big-endian
```


```
 _________________ Decompression, Unfiltering, Deinterlacing __________________
```



## Overall image decoding flowchart


- merge SDAT chunk(s) into a continous bitstream (remove Chunk Headers/CRCs)

- decompress via Zlib-Inflate

- unfilter

- convert to desired color depth and RGBA bit-order

- deinterlace



## Decompression


The merged IDAT chunks contain:

```
  000h 1   zlib compression method/flags code (usually 78h)
  001h 1   Additional flags/check bits        (usually 9Ch) (or DAh)
  002h ..  Compressed data blocks (deflate)
  ...  4   Adler32 checksum on decompressed data   ;big-endian
```

- [ZIP Decompression](#zipdecompression)
"tinf\_zlib\_uncompress"

All scanlines are padded to 1-byte boundary, and all scanlines include a
leading filter bytes. That is, the decompressed size is:

```
  decompressed_size = ((width*bpp+7)/8+1)*height
```

For interlaced images, compute the width & height for each pass, then
compute the sum of the decompressed\_size's.



## Unfiltering


Pixels can have 1..64 bpp, but unfiltering is always done in BYTE units.

```
  for pass=1 to num_passes   ;for 7-pass interlacing
    calc width/height for current pass
    for x=1 to (bpp+7)/8 + (width*bpp+7)/8
      byte[dst]=00h, dst=dst+1            ;-pad topmost for unfiltering
    for y=1 to height
      filter = byte[src], src=src+1
      for x=1 to (bpp+7)/8
        byte[dst]=00h, dst=dst+1          ;-pad leftmost for unfiltering
      for x=1 to (width*bpp+7)/8
        left      = byte[dst-(bpp+7)/8]
        upper     = byte[dst-bytes_per_scanline]
        upperleft = byte[dst-bytes_per_scanline-(bpp+7)/8]
        if filter=0 then byte[dst]=byte[src]
        if filter=1 then byte[dst]=byte[src]+left
        if filter=2 then byte[dst]=byte[src]+upper
        if filter=3 then byte[dst]=byte[src]+(left+upper)/2
        if filter=4 then
          pa=abs(upper-upperleft)
          pb=abs(left-upperleft)
          pc=abs(left+upper-(upperleft*2))
          if pa<=pb AND pa<=pc then byte[dst]=byte[src]+left
          elseif pb<=pc then        byte[dst]=byte[src]+upper
          else                      byte[dst]=byte[src]+upperleft
        if filter>4 then error
        src=src+1, dst=dst+1
      next x
    next y
  next pass
```

Note: The dst buffer may be as small as two scanlines (when directly forwarding
the unfiltered data to the color conversion & deinterlacing steps).



## Color Depth conversion and RGBA bit-order


The MSB of first pixel is in bit7 of first byte (ie. 16bit samples are
big-endian, and 2bpp would have 1st pixel in bit7-6). Pixels with multiple
samples are ordered R,G,B or R,G,B,A or Luminance,A.

The data should be converted to a format supported by the OS/hardware. Also,
deinterlacing will be easier when converting to at least 8bpp (or simply
convert everything to 32bpp).



## Deinterlacing (if enabled in IHDR chunk)


Interlace allows to display a low-resolution preview during slow network
downloads. The image is divided into 8x8 pixel blocks.

```
  Pass Numbers per 8x8:      Width/Height per pass:
  1 6 4 6 2 6 4 6            Pass 1   (width+7)/8, (height+7)/8  ;\
  7 7 7 7 7 7 7 7            Pass 2   (width+3)/8, (height+7)/8  ;
  5 6 5 6 5 6 5 6            Pass 3   (width+3)/4, (height+3)/8  ; seven-pass   7 7 7 7 7 7 7 7            Pass 4   (width+1)/4, (height+3)/4  ; (Adam7
  3 6 4 6 3 6 4 6            Pass 5   (width+1)/2, (height+1)/4  ; interlace)
  7 7 7 7 7 7 7 7            Pass 6   (width+0)/2, (height+1)/2  ;
  5 6 5 6 5 6 5 6            Pass 7   (width+0)/1, (height+0)/2  ;/
  7 7 7 7 7 7 7 7            Single   (width+0)/1, (height+0)/1  ;-single-pass
```

For example, a 12x2 pixel image would be interlaced as so:

```
  1 6 4 6 2 6 4 6 1 6 4 6      ;\Pass1=2x1, Pass2=1x1, Pass3=Empty, Pass4=4x1,
  7 7 7 7 7 7 7 7 7 7 7 7      ;/Pass5=Empty, Pass6=6x1, Pass7=12x1.
```

All seven passes are stored in a single Zlib-bitstream (for low-res preview,
one must pause the Zlib decompression every once and then to display the yet
downloaded pixels).


```
 ____________________________________ Misc ____________________________________
```



## tRNS Chuunk


Colour type 0    Grayscale

```
  Grey sample value              2 bytes   (big-endian) (0..SampleSize-1)
```

Colour type 2    RGB

```
  Red sample value               2 bytes   (big-endian) (0..SampleSize-1)
  Blue sample value              2 bytes   (big-endian)
  Green sample value             2 bytes   (big-endian)
```

Colour type 3    Color-indexed

```
  Alpha for palette index 0      1 byte (alpha value, 00h..FFh; FFh=Solid)
  Alpha for palette index 1      1 byte
  ...etc...                      1 byte
```



## References


Official PNG specs are filled with overhelming details (without actually
getting to the point).

- [http://www.libpng.org/pub/png/spec/1.0/](http://www.libpng.org/pub/png/spec/1.0/)
- (gzip)      PNG v1.0
- [http://www.libpng.org/pub/png/spec/1.1/](http://www.libpng.org/pub/png/spec/1.1/)
- (html/txt)  PNG v1.1
- [http://www.libpng.org/pub/png/spec/1.2/](http://www.libpng.org/pub/png/spec/1.2/)
- (html/txt)  PNG v1.2
- [http://www.w3.org/TR/2003/REC-PNG-20031110/#minitoc](http://www.w3.org/TR/2003/REC-PNG-20031110/#minitoc)
- PNG "second edition"
- [http://w3c.github.io/PNG-spec/](http://w3c.github.io/PNG-spec/)
- (corrupted html)     PNG "third edition"

To figure out "how to decode the damn bitmap", there is this valuable resource:

- [http://pyokagan.name/blog/2019-10-14-png/](http://pyokagan.name/blog/2019-10-14-png/)
- Writing a (simple) PNG decoder

For testing different color depths and interlacing:

- [http://www.schaik.com/pngsuite/](http://www.schaik.com/pngsuite/)
- PNG test images (color types, interlace, etc)




# <a name="parentalcontrolsreset"></a>Parental Controls Reset




## Parental Controls Reset


Click "I Forgot" when getting prompted to enter the PIN and Secret Question,
the console will then display an 8-digit Enquiry Number (or 10-digit with
region/version RV/RVV in upper digits), the customer service hotline uses that
number (and the current month/day, MMDD) to compute a 5-digit Master Key for
resetting Parental Controls.


## DSi/Wii version


```
  This uses a standard CRC32 with polynomial EDB88320h.
  Enquiry = "XXXXNNNN" (8-digit, based on MAC but only last 4-digits are used)
  MasterKey = ((StandardCRC32("MMDDNNNN") XOR 0000AAAAh)+14C1h mod 100000
  The XOR 0000AAAAh is replacing the standard crc32's final XOR FFFFFFFFh?
```


## 3DS firmware version 1.0.0-X through 6.3.0-X


```
  Same as DSi/Wii, but using a nonstandard CRC32 (with polynomial EDBA6320h
  instead of EDB88320h, and adding 1657h instead of 14C1h to the final result.
  Enquiry = "XXXXNNNN" (8-digit, based on MAC but only last 4-digits are used)
  MasterKey = ((NonStandardCRC32("MMDDNNNN") XOR 0000AAAAh)+1657h mod 100000
  "This had a minor refactor in 6.0.0-X but is functionally identical."
```


## 3DS firmware version7.0.0-X through 7.1.0-X (with version V=0)


```
  Similar as above, using a 10-digit Enquiry, and SHA256-HMAC instead CRC32.
  Enquiry = "RVNNNNNNNN" (10-digit, based on MAC?, upper digits=region/ver?)
  HmacKey = from mset .rodata, differs between regions.
  MasterKey = (FirstWordOf(SHA256HMAC("MMDDR0NNNNNNNN",HmacKey)) mod 100000
```


## 3DS firmware versio n7.2.0-X and up (with version VV=10 and up)


```
  As above, but with HmacKey stored in a separate encrypted file.
  AesIv,RVV = from masterkey.bin (found in System Data Archives 2, CVer)
  AesKey = from mset .rodata, using RVV as decrypt key number
  Enquiry = "RVVNNNNNNN" (10-digit, based on MAC?, upper digits=region/ver)
  HmacKey = from masterkey.bin (decrypt via AES-CTR with AesIv and AesKey)
  MasterKey = (FirstWordOf(SHA256HMAC("MMDDR0NNNNNNNN",HmacKey)) mod 100000
```

Newer(?) firmwares are using R=9 for japan (instead of R=0, maybe because
people did tend to omit leading zeroes when specifying the Enquiry Number).

Alternately, an easier way to reset parental controls is to dump the PIN from
"config" (3DS) or TWLCFG (DSi).





# <a name="armvectorfloatingpointunitvfp"></a>ARM Vector Floating-point Unit (VFP)



The VFP unit exists on 3DS ARM11.


- [ARM VFP Floating Point Registers](#armvfpfloatingpointregisters)
- [ARM VFP Floating Point Control/Status Registers](#armvfpfloatingpointcontrolstatusregisters)
- [ARM VFP Floating Point Opcode Encoding](#armvfpfloatingpointopcodeencoding)
- [ARM VFP Floating Point Maths Opcodes](#armvfpfloatingpointmathsopcodes)
- [ARM VFP Floating Point Load/Store Opcodes](#armvfpfloatingpointloadstoreopcodes)


## Floating Point


The floating point hardware is called VFPv2 (Vector Floating-point).

- ARM DDI 0100I ARM Architecture Reference Manual (for ARMv6 wirh VFPv2)
- ARM DDI 0360F ARM11 MPCore r2p0, contains more (mostly useless) VFPv2 info

The Fxxxx floating point opcodes are aliases for CP10/CP11 copressor numbers;
CP10 used for single, and CP11 for double precision instructions.




# <a name="armvfpfloatingpointregisters"></a>ARM VFP Floating Point Registers




## Floating point Registers


Registers S0-S31 can contain Single-precision float values, or 32bit Integers
(for conversion to/from float format), or a pair of two Single-precision
registers can contain one Double-precision float value.

```
  Scalar Bank       Vector bank 1       Vector bank 2       Vector bank 3
  S1:S0    D0       S9:S8    D4         S17:S16  D8         S25:S24  D12
  S2:S3    D1       S11:S10  D5         S19:S18  D9         S27:S26  D13
  S5:S4    D2       S13:S12  D6         S21:S20  D10        S29:S28  D14
  S7:S6    D3       S15:S14  D7         S23:S22  D11        S31:S30  D15
```

The VFP supports "Scalar" and "Vector" modes (and a mixed "Vector/Scalar"
mode).

The "Vector" mode can perform simultaneous operations on up to 8 singles, or up
to 4 doubles (via Vector LEN and STRIDE selected in FPSCR register).

The registers are organized in "banks", and vectors cannot cross banks (eg.
using operand S23 with LEN=3, STRIDE=2 would use registers S23,S17,S19).



## Scalar Mode, Fd=Fm \<op> Fn


The "Scalar" mode performs operations on 1 single or double. This done in any
of the following situations:

- - When FPSCR register is set to Vector LEN=1 (and STRIDE=1), or
- - When Destination is S0..S7 or D0..D3 (scalar bank), or
- - When using FCMP comparision opcodes, or
- - When using FCVT or FxxTOxx conversion opcodes, or
- - When using FMxxRxx register transfer opcodes, or
- - When using FLDxx/FSTxx load/store (whereof, FLDM/FSTM can transfer multiple
    - registers in vector-like fashion; regardless of LEN/STRIDE settings)



## Vector Mode, Fd[LEN]=Fm[LEN] \<op> Fn[LEN]


The vector mode does merely perform the selected operation on all array
elements, this is correct for cases like Vector+Vector addition, but incorrect
for Vector\*Vector multiplication (to get the final result one must manually
compute the sum of the results).

- - When FPSCR register is set to Vector LEN=2..8 (and STRIDE=1..2), and
- - When Source and Destination are S8..S31 or D4..D15 (vector banks), and
- - When using FADD, FSUB, FDIV, FCPY, FABS, FNEG, FSQRT, or FxMxx multiply



## Mixed Mode, Fd[LEN]=Fm \<op> Fn[LEN]


This allows to add/multiply/etc. all elements of a vector by a scalar value.
This is done when combining vectors operands with the following:

- - When Source operand Fm is S0..S7 or D0..D3 (scalar bank), and
- - othersise same conditions as for Vector mode



## Integer Format (S0..S31 aka I0..I31)


```
  31-0   Integer (signed or unsigned, depending on FxxTOxx opcode)
```

The VFP can't do integer maths, however, one can load/store integer values in
S0..S31, and then use the FxxTOxx opcodes to convert integers to/from float
format. The integers are always 32bit (no matter if converting Single/Double
precision float values).



## Single Precision Registers (float1.8.23) (S0..S31)


```
  31     1bit  Sign (0=Positive, 1=Negative)
  30-23  8bit  Exponent (01h..FEh=for 2^(N-7Fh), or 00h/FFh=Special)
  22-0   23bit Fraction (0..7FFFFFh)
```



## Double Precision Registers (float1.11.52) (D0..D15)


```
  63     1bit  Sign (0=Positive, 1=Negative)
  62-52  11bit Exponent (001h..7FEh=for 2^(N-3FFh), or 000h/7FFh=Special)
  51-0   52bit Fraction (0..FFFFFFFFFFFFFh)
```



## Exponent 01h..FEh (Single) or 001h..7FEh (Double):


```
  Sign * 2^(exponent-7Fh) * (1.fraction)    ;Single
  Sign * 2^(exponent-3FFh) * (1.fraction)   ;Double
```


## Exponent 00h (Single) or 000h (Double), aka Small Numbers and Zero:


```
  Sign * 2^(-7Eh) * (0.fraction)            ;Single
  Sign * 2^(-3FEh) * (0.fraction)           ;Double
  The above includes 0 being encoded as fraction=0, the sign bit is ignored
  for cases like "compare +/-0", but the sign is used for "divide by +/-0".
  Small numbers in 0.fraction format may require extra clock cycles for
  counting leading zeroes; unknown if that problem does actually exist on
  ARM hardware, however, the "flush to zero" feature (see FPSCR.bit24) can
  be used to avoid that issue; 0.fraction will be then replaced by 0.000.
```


## Exponent FFh (Single) or 7FFh (Double), aka NaN's and Infinite:


```
  fraction=000000h          or 0000000000000h                 +/-Infinite
  fraction=000001h..3FFFFFh or 0000000000001h..7FFFFFFFFFFFFh +/-Signaling NaNs
  fraction=400000h          or 8000000000000h                 +/-Default NaN
  fraction=400000h..7FFFFFh or 8000000000000h..FFFFFFFFFFFFFh +/-Quite NaNs
  NaNs (Not a Number) can be used for abstract non-numeric expressions; this
  isn't useful for normal maths, but may be useful if a database contains
  entries like "Weight=UNKNOWN". If so, one may handle the NaN before passing
  it to the floating point unit, or otherwise the hardware will either trigger
  an exception (Signaling NaNs) or leave the NaN unchanged (Quite NaNs),
  eg. "UNKNOWN*2+3 = UNKNOWN", or replace it by Default NaN (if FPSCR.bit25=1).
  Different NaNs can be compared using integer comparisons, float comparisions
  of NaNs have "unordered" results (even when comparing a NaN with itself).
```



## Multiply Note


When multiplying vector\*vector, the hardware does merely multiply the
components (without computing the sum of the multiply results). To get sum, one
could use FMUL and several FADD's with different strides. Or better, for
multiple vector\*vector multiplications, use FMUL and several FMAC's with source
data rearranged as so:

```
  FMUL (X,X,X,X,X,X,X,X)*(X,X,X,X,X,X,X,X)
  FMAC (Y,Y,Y,Y,Y,Y,Y,Y)*(Y,Y,Y,Y,Y,Y,Y,Y)
  FMAC (Z,Z,Z,Z,Z,Z,Z,Z)*(Z,Z,Z,Z,Z,Z,Z,Z)
  FMAC (W,W,W,W,W,W,W,W)*(W,W,W,W,W,W,W,W)
```

Ie. in that case, the 1st "vector" contains the X components from up to eight
(X,Y,Z,W) vectors.




# <a name="armvfpfloatingpointcontrolstatusregisters"></a>ARM VFP Floating Point Control/Status Registers




## FPSID Register (Floating Point System ID) (R)


```
  31-24  Implementor code (41h=ARM)
  23     Hardware/software implementation (0=Hardware, 1=Software)
  22-21  FSTMX/FLDMX format  (0=Format 1, Other=Reserved)
  20     Supported Precision (0=Single and Double, 1=Single only)
  19-16  Architecture version number (0=VFPv1, 1=VFPv2, 2-15=Reserved)
  15-8   Primary part number of VFP implementation (20h=VFP11) ;\Implementation
  7-4    Variant number                            (0Bh=MPCore); defined
  3-0    Revision number of the part               (04h=Fourth);/
```

New3DS: 410120b4h = VFPv2 D variant (with single AND double precision).



## FPSCR Register (Floating Point Status/Control Register for user-level) (R/W)


```
  31     N Flag (1=Comparision result is Less Than)
  30     Z Flag (1=Comparision result is Equal)
  29     C Flag (1=Comparision result is Equal, Greater Than, or Unordered)
  28     V Flag (1=Comparision result is Unordered)
         Note: Use FMSTAT opcode to transfer above flags to ARM CPSR flags
  27-26  Unused (0)
  25     Default Nan mode   (XXX see page C2-16) (0=Disable, 1=Enable)
  24     Flush-to-zero mode (XXX see page C2-14) (0=Disable, 1=Enable)
  23-22  Rounding mode (0=To Nearest, 1=Up, 2=Down, 3=Towards Zero)
  21-20  Vector Stride (0/3 = 1/2 Singles; or 0/3 = 1/2 Doubles) (1/2=Reserved)
  19     Unused (0)
  18-16  Vector Len    (0..7 = 1..8 Singles; or 0..3 = 1..4 Doubles)
  15     Trap Enable Input Denormal (aka Subnormal)     ;\
  14-13  Unused (0)                                     ;
  12     Trap Enable Inexact                            ; Trap Enable aka
  11     Trap Enable Underflow                          ; Exception Enable
  10     Trap Enable Overflow                           ;
  9      Trap Enable Division by Zero                   ;
  8      Trap Enable Invalid Operation                  ;/
  7      Cumulative Exception Input Denormal            ;\
  6-5    RES                                            ;
  4      Cumulative Exception Inexact                   ; Cumulative what...?
  3      Cumulative Exception Underflow                 ;
  2      Cumulative Exception Overflow                  ;
  1      Cumulative Exception Division by Zero          ;
  0      Cumulative Exception Invalid Operation         ;/
```



## FPEXC Register (Floating Point Exception Register for system-level) (R/W)


```
  31     Exception Flag ... long blurb replated to process swap code
  30     Enable Floating Point Instructions (0=Disable, 1=Enable)
  29-0   Sub-architecture defined (see below for mpcore)
 Extra mpcore bits:
  29     Unused (0)
  28     FPINST2 instruction valid flag
  27-11  Unused (0)
  10-8   VECITR Number of remaining iterations after exception (0..6=1..7, 7=0)
  7      INV Input exception flag
  6-4    Unused (0)
  3      UFC Potential Underflow Flag
  2      OFC Potential Overflow Flag
  1      Unused (0)
  0      IOC Potential invalid operation flag
```

The exception handler must clear bit31 and bit28.



## FPINST - Floating-Point Instruction Register, Privileged 0xEE000A00 (R/W)


Contains the opcode that has triggered the exception. The Cond field in
bit28-31 is changed to 0Eh (Always), and the Fd:D, Fn:N, Fm:M are changed to
indicated the fault-location within a vector (with FPEXC.bit8-10 indicating the
remaining unprocessed elements of the vector).



## FPINST2 - Floating-Point Instruction Register 2, Privileged UNP (R/W)


If FPEXC.bit28=1, then this register contains another float opcode (that was
prefetched, but not yet executed). The Cond field in bit28-31 is changed to 0Eh
(Always). The exception handler should handle the failed FPINST opcode, then
try to execute prefetched FPINST2 opcode, and then return from exception.



## MVFR0, Media and VFP Feature Register 0, Any 0x11111111 (R)


```
  31-28  VFP hardware support level when user traps are disabled
         (01h=In MPCore processors when Flush-to-Zero and Default_NaN and
         Round-to-Nearest are all selected in FPSCR, the coprocessor does not
         require support code. Otherwise floating-point support code is
         required)
  27-24  Support for short vectors           (01h=Yes)
  23-20  Support for hardware square root    (01h=Yes)
  19-16  Support for hardware divide         (01h=Yes)
  15-12  Support for software/user traps     (01h=Yes/support code is required)
  11-8   Support for double precision VFP    (01h=Yes, v2)
  7-4    Support for single precision VFP    (01h=Yes, v2)
  3-0    Support for the media register bank (01h=Yes/support 16, 64bit regs)
```



## MVFR1 - Media and VFP Feature Register 1, Any 0x00000000 (R)


```
  31-28  Reserved
  11-8   Support for media extension, single precision floating-point (00h=No)
  7-4    Support for media extension, integer instructions            (00h=No)
  3-0    Support for media extension, load/store instructions         (00h=No)
```




# <a name="armvfpfloatingpointopcodeencoding"></a>ARM VFP Floating Point Opcode Encoding




## Comparision of normal ARM copro opcodes and VFP opcodes


```
  |..3 ..................2 ..................1 ..................0|
  |1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0|
  |_Cond__|1_1_0_0_0_1_0|L|__Rn___|__Rd___|__CP#__|_CPopc_|__CRm__| 2reg normal
  |_Cond__|1_1_0_0_0_1_0|L|__Rn___|__Rd___|__CP#__|0|0|M|1|__Fm___| 2reg on VFP
  |_Cond__|1_1_0|P|U|N|W|L|__Rn___|__CRd__|__CP#__|____Offset_____| Mem normal
  |_Cond__|1_1_0|P|U|D|W|L|__Rn___|__Fd___|__CP#__|____Offset_____| Mem on VFP
  |_Cond__|1_1_1_0|_CPopc_|__CRn__|__CRd__|__CP#__|_CP__|0|__CRm__| CDP normal
  |_Cond__|1_1_1_0|p|D|q|r|__Fn___|__Fd___|__CP#__|N|s|M|0|__Fm___| CDP on VFP
  |_Cond__|1_1_1_0|CPopc|L|__CRn__|__Rd___|__CP#__|_CP__|1|__CRm__| 1reg normal
  |_Cond__|1_1_1_0|CPopc|L|__Fn___|__Rd___|__CP#__|N|0_0|1|0_0_0_0| 1reg on VFP
```


```
  Cond             = Condition
  L                = Load/Store direction for memory/register transfers
  Fm:M, Fn:N, Fd:D = Float Registers S0..S31 (or D0..D15, with LSB=0)
  Rd, Rn           = ARM Registers
  PUW, pqrs, CPopc = Opcode bits
  CP#              = Coprocessor number (0Ah=Single-, 0Bh=Double-Precision)
  Offset           = Address step, implies number of registers for FLDM/FSTM
```




# <a name="armvfpfloatingpointmathsopcodes"></a>ARM VFP Floating Point Maths Opcodes




## VFP data-processing primary opcodes


```
  pqrs cp10/cp11                  Instruction functionality
  0000 FMAC{S|D}{cond} Fd,Fn,Fm   Fd = +(Fn*Fm)+Fd   ;Multiply, Add
  0001 FNMAC{S|D}{cond} Fd,Fn,Fm  Fd = -(Fn*Fm)+Fd   ;Multiply, Negate, Add
  0010 FMSC{S|D}{cond} Fd,Fn,Fm   Fd = +(Fn*Fm)-Fd   ;Multiply, Subtract
  0011 FNMSC{S|D}{cond} Fd,Fn,Fm  Fd = -(Fn*Fm)-Fd   ;Multiply, Negate, Sub
  0100 FMUL{S|D}{cond} Fd,Fn,Fm   Fd = +(Fn*Fm)      ;Multiply
  0101 FNMUL{S|D}{cond} Fd,Fn,Fm  Fd = -(Fn*Fm)      ;Multiply, Negate
  0110 FADD{S|D}{cond} Fd,Fn,Fm   Fd = Fn+Fm         ;Add
  0111 FSUB{S|D}{cond} Fd,Fn,Fm   Fd = Fn-Fm         ;Sub
  1000 FDIV{S|D}{cond} Fd,Fn,Fm   Fd = Fn/Fm         ;Divide
  1001 -Undefined-
  1010 -Undefined-
  1011 -Undefined-
  1100 -Undefined-
  1101 -Undefined-
  1110 -Undefined-
  1111 -Extension instructions-
```



## VFP data-processing extension opcodes


```
  Fn   N cp10/cp11                  Instruction functionality
  0000 0 FCPY{S|D}{cond} Fd,Fm      Fd = Fm        ;Copy
  0000 1 FABS{S|D}{cond} Fd,Fm      Fd = abs(Fm)   ;Absolute
  0001 0 FNEG{S|D}{cond} Fd,Fm      Fd = -Fm       ;Negate
  0001 1 FSQRT{S|D}{cond} Fd,Fm     Fd = sqrt(Fm)  ;Square root
  001x x -Undefined-
  0100 0 FCMP{S|D}{cond} Fd,Fm      Fd-Fm     ;Compare
  0100 1 FCMPE{S|D}{cond} Fd,Fm     Fd-Fm     ;Compare, exception on quiet NaNs
  0101 0 FCMPZ{S|D}{cond} Fd        Fd-0      ;Compare
  0101 1 FCMPEZ{S|D}{cond} Fd       Fd-0      ;Compare, exception on quiet NaNs
  0110 x -Undefined-
  0111 0 -Undefined-
  0111 1 FCVT{DS|SD}{cond} Fd,Fm    Single <--> Double-precision conversion
  1000 0 FUITO{S|D}{cond} Fd,Im     Unsigned integer --> float
  1000 1 FSITO{S|D}{cond} Fd,Im     Signed integer --> float
  1001 x -Undefined-
  101x x -Undefined-
  1100 0 FTOUI{S|D}{cond} Id,Fm     Float --> unsigned integer
  1100 1 FTOUIZ{S|D}{cond} Id,Fm    Float --> unsigned integer, round to zero
  1101 0 FTOSI{S|D}{cond} Id,Fm     Float --> signed integer
  1101 1 FTOSIZ{S|D}{cond} Id,Fm    Float --> signed integer, round to zero
  111x x -Undefined-
```

cp10: FCVTDS Dd,Sm  ;Double \<-- Single

cp11: FCVTSD Sd,Dm  ;Single \<-- Double



## Nocash syntax


The useless {S|D} and {DS|SD} suffixes are ommitted. FCVT is renamed to FMOV.
F{UI|SI}TO{UI|SI}{Z} is renamed to FMOV{UI|SI}{Z}, with operand I0..I31 for the
integer register.




# <a name="armvfpfloatingpointloadstoreopcodes"></a>ARM VFP Floating Point Load/Store Opcodes




## VFP single register transfer instructions


```
  cp   opcode L Instruction name   Instruction functionality
  cp10 000    0 FMSR{cond} Sn,Rd   Sn = Rd        ;\Single-Precision or Integer
  cp10 000    1 FMRS{cond} Rd,Sn   Rd = Sn        ;/
  cp10 111    0 FMXR{cond} sys,Rd  Reg(Fn,N) = Rd ;\SystemReg (FPSID, etc.)
  cp10 111    1 FMRX{cond} Rd,sys  Rd = Reg(Fn,N) ;/  ;<-- or FMSTAT{cond}
  cp11 000    0 FMDLR{cond} Dn,Rd  Dn.31-0 = Rd   ;\LSW of Double-Precision
  cp11 000    1 FMRDL{cond} Rd,Dn  Rd = Dn.31-0   ;/
  cp11 001    0 FMDHR{cond} Dn,Rd  Dn.63-32 = Rd  ;\MSW of Double-Precision
  cp11 001    1 FMRDH{cond} Rd,Dn  Rd = Dn.63-32  ;/
  other's       -Undefined-
```

System Register encodings:

```
  Fn   N  System register
  0000 0  FPSID (New3DS: 410120b4h = VFPv2 with single AND double precision)
  0001 0  FPSCR      ;(FMSTAT opcode encodes as FMRX R15,FPSCR)
  0110 0? MVFR1                       ;\mpcore only
  0111 0? MVFR0                       ;/
  1000 0  FPEXC
  1001 0? FPINST                      ;\mpcore only
  1010 0? FPINST2                     ;/
```



## VFP two register transfer instructions (VFPv2 and above)


```
  cp   L Instruction name               Instruction functionality
  cp10 0 FMSRR{cond} {Sm,Sm+1},Rd,Rn    Fm = Rn, (Fm+1) = Rd    ;XXX swapped?
  cp10 1 FMRRS{cond} Rd,Rn,{Sm,Sm+1}    Rn = Fm, Rd = (Fm+1)    ;XXX swapped?
  cp11 0 FMDRR{cond} Dm,Rd,Rn           Fm[31:0] = Rd, Fm[63:32] = Rn
  cp11 1 FMRRD{cond} Rd,Rn,Dm           Rd = Fm[31:0], Rn = Fm[63:32]
```



## VFP load and store instructions


```
  PUW  L=0/1,cp10/cp11                             Registers transferred
  000  -Two-register transfer instructions-        -
  001  -Undefined-                                 -
  010  FSTM|FLDMIA{S|D|X}{<cond>} Rn,{Fd,Fd+1,..}  Multiple Registers
  011  FSTM|FLDMIA{S|D|X}{<cond>} Rn!,{Fd,Fd+1,..} Multiple Registers Increment
  100  FST|FLD{S|D}{<cond>} Fd, [Rn{,-offs*4}]     One register, -offs
  101  FSTM|FLDMDB{S|D|X}{<cond>} Rn!,{Fd,Fd+1,..} Multiple Registers Decrement
  110  FST|FLD{S|D}{<cond>} Fd, [Rn{,+offs*4}]     One register, +offs
  111  -Undefined-                                 -
```

FSTM/FLDM do transfer multiple words (with offs containing the number of words
to be transferred, 1..32 for {S}, or an even number 2..32 for {D}.

VFP load/store multiple addressing modes

```
  Non-stacking mnemonic   Stacking mnemonic
  FLDMIA{S|D|X}           FLDMFD{S|D|X}     FPOP{S|D|X}
  FLDMDB{S|D|X}           FLDMEA{S|D|X}
  FSTMIA{S|D|X}           FSTMEA{S|D|X}
  FSTMDB{S|D|X}           FSTMFD{S|D|X}     FPUSH{S|D|X}
```



## Nocash syntax


The useless {S|D} suffixes are ommitted, the weird {X} suffix is kept used to
preserve weirdness.

Fancy {} brackets are omitted, LDM/STM must use square [Rn] brackets, the
register list in LDM/STM is specified as Fx-Fz (rather than Fx,Fy,Fz).

All FMxxxx opcodes are renamed to FMOV{LSW|MSW}.



## Weird STM/LDM{X} - for registers with unknown precision


The weird {X} mode is same as {D}, but with offset.bit0=1 (ie. with offs=3..33
instead of 2..32; and thereby actually transferring an unused dummy word; that
extra word might be intended to contain datatype flags).

The weird {X} mode is/was intended for registers with unknown content (eg. when
pushing/popping registers without knowing if they contain integer/single/double
precision values; which might be a problem with internal accumulators in the
VFP unit).

The weird {X} mode was declared as "deprecated in ARMv6" in DDI 0100I, but
later re-declared as required for "compatibility with future VFP
implementations" in DDI 360F. However, unknown if there are/were/will be any
such implementations that do require it.

For now, it should be best to use {D} mode instead of weird {X}. Probably even
{S} should also work the same (if the endianess-based word-order doesn't
matter).




# <a name="nondsfilesrarcarchives"></a>Non-DS Files - RARC Archives



RARC archives are used in Wii and GameCube games like Super Mario Galaxy 1
& 2 and Supermario Sunshine. The RARC format is unique in terms of storing
duplicated folder data in both Folder List and Directory Entries.



## RARC Header (...probably bug-endian?)


```
  000h 4   ID "RARC"
  004h 4   Total Filesize
  008h 4   Size of RARC Header (always 20h)
  00Ch 4   Size of Info Block (plus Folder List, Directory List, String Table)
  010h 4   Size of File Data area
  014h 4   Size of File Data again? (always same as the previous value)
  018h 8   Unknown (zerofilled)
  020h ..  Info Block (plus Folder List, Directory List, String Table)
  ...  ..  File Data
```



## Info Block (20h-bytes, plus Folder List, Directory List, String Table)


```
  000h 4   Folder List Size/10h    (number of folders)
  004h 4   Folder List Offset      (relative to the start of Info block)
  008h 4   Directory List Size/14h (number of files+folders)
  00Ch 4   Directory List Offset   (relative to the start of Info block)
  010h 4   String Table Size/1     (in bytes)
  014h 4   String Table Offset     (relative to the start of Info block)
  018h 2   Number of directory entries that are Files (number of files)
  01Ah 6   Unknown (zerofilled)
  020h ..  Folder List (10h bytes per entry)
  ...  ..  Directory List (14h bytes per entry)
  ...  ..  String Table
```



## Folder List Entries (10h-bytes each) (first entry is root folder)


```
  000h 4   Short name (first 4 chars of name, uppercase, spc-padded, eg. ROOT)
  004h 4   Name Offset (relative to start of string table)
  008h 2   Name Checksum
  00Ah 2   Number of Directory entries in this folder
  00Ch 4   Index of first Directory entry for this folder
```



## Directory Entries (14h-bytes each) (contains both Files and Folders)


```
  000h 2   Index of this directory entry (aka File ID or so) (or FFFFh=Folder)
  002h 2   Name Checksum
  004h 2   Type/Attributes? (1100h=File, 0200h=Folder)
  006h 2   Name Offset (relative to start of string table)
  008h 4   File Data Offset   (or, for folders: Index in Folder List)
  00Ch 4   File Size in bytes (or, for folders: unused)
  010h 4   Unknown (zerofilled)
```



## String Table & Name Checksums


The string table consists of null-terminated ASCII(?) strings. The Name
Checksums (in Folder List and Directory Entries) are calculated as so:

```
  sum=0, i=0, while name[i]<>00h do sum=((sum*3)+name[i]) AND FFFFh, i=i+1
```




# <a name="nondsfilesu8archives"></a>Non-DS Files - U8 Archives



U8 archives are used in Mario Kart Wii and maybe other games (something called
Wii discs is reportedly using a similar format, too).



## U8 Format (big endian)


```
  000h 4     ID (55AA382Dh) (appears as U.8- in hex editors) (nicknamed U8)
  004h 4     Offset to Directory (aka 20h?)
  008h 4     Size of Directory including String Table
  00Ch 4     Offset to File Data
  010h 10h   Reserved
  020h N*0Ch Directory Entries (0Ch-bytes each)
  ...  ..    String Table (ASCII strings, terminated by 00h)
  ...  ..    File Data
```



## Directory Entries (starting with root entry)


```
  000h 1    Type (00h=File, 01h=Directory) (other values never seen)
  001h 3    24bit Filename Offset (relative to begin of String Table)
  004h 4    File: Offset to data
            Directory: Index of the parent directory
  008h 4    File: Size of data
            Directory: Index of the first node that is not part of this
              directory (skip to node)
```


Note: The U8 format is about same as 3DS "darc" format (with different header,
different byte-order, and ASCII instead of UTF-16 names).

- [3DS Files - Archive DARC](#3dsfilesarchivedarc)



# <a name="armcpureference"></a>ARM CPU Reference




## General ARM7TDMI Information


- [ARM CPU Overview](#armcpuoverview)
- [ARM CPU Register Set](#armcpuregisterset)
- [ARM CPU Flags & Condition Field (cond)](#armcpuflagsconditionfieldcond)
- [ARM CPU 26bit Memory Interface](#armcpu26bitmemoryinterface)
- [ARM CPU Exceptions](#armcpuexceptions)
- [ARM CPU Memory Alignments](#armcpumemoryalignments)


## ARM 32bit Instruction Set (ARM Code)


- [ARM Instruction Summary](#arminstructionsummary)
- [ARM Opcodes: Branch and Branch with Link (B, BL, BX, BLX, SWI, BKPT)](#armopcodesbranchandbranchwithlinkbblbxblxswibkpt)
- [ARM Opcodes: Data Processing (ALU)](#armopcodesdataprocessingalu)
- [ARM Opcodes: Multiply and Multiply-Accumulate (MUL, MLA)](#armopcodesmultiplyandmultiplyaccumulatemulmla)
- [ARM Opcodes: Special ARM9 Instructions (CLZ, QADD/QSUB)](#armopcodesspecialarm9instructionsclzqaddqsub)
- [ARM Opcodes: Special ARM11 Instructions (Misc)](#armopcodesspecialarm11instructionsmisc)
- [ARM Opcodes: Special ARM11 Instructions (SIMD)](#armopcodesspecialarm11instructionssimd)
- [ARM Opcodes: PSR Transfer (MRS, MSR)](#armopcodespsrtransfermrsmsr)
- [ARM Opcodes: Memory: Single Data Transfer (LDR, STR, PLD)](#armopcodesmemorysingledatatransferldrstrpld)
- [ARM Opcodes: Memory: Halfword, Doubleword, and Signed Data Transfer](#armopcodesmemoryhalfworddoublewordandsigneddatatransfer)
- [ARM Opcodes: Memory: Block Data Transfer (LDM, STM)](#armopcodesmemoryblockdatatransferldmstm)
- [ARM Opcodes: Memory: Single Data Swap (SWP)](#armopcodesmemorysingledataswapswp)
- [ARM Opcodes: Coprocessor Instructions (MRC/MCR, LDC/STC, CDP, MCRR/MRRC)](#armopcodescoprocessorinstructionsmrcmcrldcstccdpmcrrmrrc)


## ARM 16bit Instruction Set (THUMB Code)


When operating in THUMB state, cut-down 16bit opcodes are used.

THUMB is supported on T-variants of ARMv4 and up, ie. ARMv4T, ARMv5T, etc.

- [THUMB Instruction Summary](#thumbinstructionsummary)
- [THUMB Opcodes: Register Operations (ALU, BX)](#thumbopcodesregisteroperationsalubx)
- [THUMB Opcodes: Memory Load/Store (LDR/STR)](#thumbopcodesmemoryloadstoreldrstr)
- [THUMB Opcodes: Memory Addressing (ADD PC/SP)](#thumbopcodesmemoryaddressingaddpcsp)
- [THUMB Opcodes: Memory Multiple Load/Store (PUSH/POP and LDM/STM)](#thumbopcodesmemorymultipleloadstorepushpopandldmstm)
- [THUMB Opcodes: Jumps and Calls](#thumbopcodesjumpsandcalls)
- [THUMB Opcodes: New THUMB Opcodes in ARM11](#thumbopcodesnewthumbopcodesinarm11)


## Note


Switching between ARM and THUMB state can be done by using the Branch and
Exchange (BX) instruction.



## Further Information


- [ARM Classic vs UAL Syntax (Unified Assembler Language)](#armclassicvsualsyntaxunifiedassemblerlanguage)
- [ARM Pseudo Instructions and Directives](#armpseudoinstructionsanddirectives)
- [ARM CP15 System Control Coprocessor](#armcp15systemcontrolcoprocessor)
- [ARM Vector Floating-point Unit (VFP)](#armvectorfloatingpointunitvfp)
- [ARM CPU Instruction Cycle Times](#armcpuinstructioncycletimes)
- [ARM CPU Versions](#armcpuversions)
- [ARM CPU Data Sheet](#armcpudatasheet)



# <a name="armcpuoverview"></a>ARM CPU Overview



The ARM7TDMI is a 32bit RISC (Reduced Instruction Set Computer) CPU, designed
by ARM (Advanced RISC Machines), and designed for both high performance and low
power consumption.



## Fast Execution


Depending on the CPU state, all opcodes are sized 32bit or 16bit (that's
counting both the opcode bits and its parameters bits) providing fast decoding
and execution. Additionally, pipelining allows - (a) one instruction to be
executed while (b) the next instruction is decoded and (c) the next instruction
is fetched from memory - all at the same time.



## Data Formats


The CPU manages to deal with 8bit, 16bit, and 32bit data, that are called:

```
   8bit - Byte
  16bit - Halfword
  32bit - Word
```



## The two CPU states


As mentioned above, two CPU states exist:

- ARM state: Uses the full 32bit instruction set (32bit opcodes)

- THUMB state: Uses a cutdown 16bit instruction set (16bit opcodes)

Regardless of the opcode-width, both states are using 32bit registers, allowing
32bit memory addressing as well as 32bit arithmetic/logical operations.



## When to use ARM state


Basically, there are two advantages in ARM state:

- - Each single opcode provides more functionality, resulting
    - in faster execution when using a 32bit bus memory system
    - (such like opcodes stored in GBA Work RAM).
- - All registers R0-R15 can be accessed directly.

The downsides are:

- - Not so fast when using 16bit memory system
    - (but it still works though).
- - Program code occupies more memory space.



## When to use THUMB state


There are two major advantages in THUMB state:

- - Faster execution up to approx 160% when using a 16bit bus
    - memory system (such like opcodes stored in GBA GamePak ROM).
- - Reduces code size, decreases memory overload down to approx 65%.

The disadvantages are:

- - Not as multi-functional opcodes as in ARM state, so it will
    - be sometimes required use more than one opcode to gain a
    - similar result as for a single opcode in ARM state.
- - Most opcodes allow only registers R0-R7 to be used directly.



## Combining ARM and THUMB state


Switching between ARM and THUMB state is done by a normal branch (BX)
instruction which takes only a handful of cycles to execute (allowing to change
states as often as desired - with almost no overload).


Also, as both ARM and THUMB are using the same register set, it is possible to
pass data between ARM and THUMB mode very easily.


The best memory & execution performance can be gained by combining both
states: THUMB for normal program code, and ARM code for timing critical
subroutines (such like interrupt handlers, or complicated algorithms).


Note: ARM and THUMB code cannot be executed simultaneously.



## Automatic state changes


Beside for the above manual state switching by using BX instructions, the
following situations involve automatic state changes:

- CPU switches to ARM state when executing an exception

- User switches back to old state when leaving an exception




# <a name="armcpuregisterset"></a>ARM CPU Register Set




## Overview


The following table shows the ARM7TDMI register set which is available in each
mode. There's a total of 37 registers (32bit each), 31 general registers (Rxx)
and 6 status registers (xPSR).

Note that only some registers are 'banked', for example, each mode has it's own
R14 register: called R14, R14\_fiq, R14\_svc, etc. for each mode respectively.

However, other registers are not banked, for example, each mode is using the
same R0 register, so writing to R0 will always affect the content of R0 in
other modes also.


```
  System/User FIQ       Supervisor Abort     IRQ       Undefined
  --------------------------------------------------------------
  R0          R0        R0         R0        R0        R0
  R1          R1        R1         R1        R1        R1
  R2          R2        R2         R2        R2        R2
  R3          R3        R3         R3        R3        R3
  R4          R4        R4         R4        R4        R4
  R5          R5        R5         R5        R5        R5
  R6          R6        R6         R6        R6        R6
  R7          R7        R7         R7        R7        R7
  --------------------------------------------------------------
  R8          R8_fiq    R8         R8        R8        R8
  R9          R9_fiq    R9         R9        R9        R9
  R10         R10_fiq   R10        R10       R10       R10
  R11         R11_fiq   R11        R11       R11       R11
  R12         R12_fiq   R12        R12       R12       R12
  R13 (SP)    R13_fiq   R13_svc    R13_abt   R13_irq   R13_und
  R14 (LR)    R14_fiq   R14_svc    R14_abt   R14_irq   R14_und
  R15 (PC)    R15       R15        R15       R15       R15
  --------------------------------------------------------------
  CPSR        CPSR      CPSR       CPSR      CPSR      CPSR
  --          SPSR_fiq  SPSR_svc   SPSR_abt  SPSR_irq  SPSR_und
  --------------------------------------------------------------
```



## R0-R12 Registers (General Purpose Registers)


These thirteen registers may be used for whatever general purposes. Basically,
each is having same functionality and performance, ie. there is no 'fast
accumulator' for arithmetic operations, and no 'special pointer register' for
memory addressing.

However, in THUMB mode only R0-R7 (Lo registers) may be accessed freely, while
R8-R12 and up (Hi registers) can be accessed only by some instructions.



## R13 Register (SP)


This register is used as Stack Pointer (SP) in THUMB state. While in ARM state
the user may decided to use R13 and/or other register(s) as stack pointer(s),
or as general purpose register.

As shown in the table above, there's a separate R13 register in each mode, and
(when used as SP) each exception handler may (and MUST!) use its own stack.



## R14 Register (LR)


This register is used as Link Register (LR). That is, when calling to a
sub-routine by a Branch with Link (BL) instruction, then the return address
(ie. old value of PC) is saved in this register.

Storing the return address in the LR register is obviously faster than pushing
it into memory, however, as there's only one LR register for each mode, the
user must manually push its content before issuing 'nested' subroutines.

Same happens when an exception is called, PC is saved in LR of new mode.

Note: In ARM mode, R14 may be used as general purpose register also, provided
that above usage as LR register isn't required.



## R15 Register (PC)


R15 is always used as program counter (PC). Note that when reading R15, this
will usually return a value of PC+nn because of read-ahead (pipelining),
whereas 'nn' depends on the instruction and on the CPU state (ARM or THUMB).



## CPSR and SPSR (Program Status Registers) (ARMv3 and up)


The current condition codes (flags) and CPU control bits are stored in the CPSR
register. When an exception arises, the old CPSR is saved in the SPSR of the
respective exception-mode (much like PC is saved in LR).

For details refer to chapter about CPU Flags.




# <a name="armcpuflagsconditionfieldcond"></a>ARM CPU Flags & Condition Field (cond)




## ARM Condition Field {cond}


The opcode {cond} suffixes can be used for conditionally executed code based on
the C,N,Z,V flags in CPSR register. For example: BEQ = Branch if Equal, MOVMI =
Move if Signed.

In ARM mode, {cond} can be used with all opcodes (except for a few newer ARMv5
instructions: BKPT, PLD, CDP2, LDC2, MCR2, MRC2, STC2, and BLX\_imm are
nonconditional; however BLX\_reg can be conditional).

In THUMB mode, {cond} can be used only for branch opcodes.

```
  Code Suffix Flags         Meaning
  0:   EQ     Z=1           equal (zero) (same)
  1:   NE     Z=0           not equal (nonzero) (not same)
  2:   CS/HS  C=1           unsigned higher or same (carry set)
  3:   CC/LO  C=0           unsigned lower (carry cleared)
  4:   MI     N=1           signed negative (minus)
  5:   PL     N=0           signed positive or zero (plus)
  6:   VS     V=1           signed overflow (V set)
  7:   VC     V=0           signed no overflow (V cleared)
  8:   HI     C=1 and Z=0   unsigned higher
  9:   LS     C=0 or Z=1    unsigned lower or same
  A:   GE     N=V           signed greater or equal
  B:   LT     N<>V          signed less than
  C:   GT     Z=0 and N=V   signed greater than
  D:   LE     Z=1 or N<>V   signed less or equal
  E:   AL     -             always (the "AL" suffix can be omitted)
  F:   NV     -             never (ARMv1,v2 only) (Reserved ARMv3 and up)
```

Execution Time: If condition=false: 1S cycle. Otherwise: as specified for the
respective opcode.



## Current Program Status Register (CPSR)


```
  Bit   Expl.
  31    N - Sign Flag       (0=Not Signed, 1=Signed)               ;\
  30    Z - Zero Flag       (0=Not Zero, 1=Zero)                   ; Condition
  29    C - Carry Flag      (0=Borrow/No Carry, 1=Carry/No Borrow) ; Code Flags
  28    V - Overflow Flag   (0=No Overflow, 1=Overflow)            ;/
  27    Q - Sticky Overflow (1=Sticky Overflow, ARMv5TE and up only)
  26-25 Reserved            (For future use) - Do not change manually!
  24    J - Jazelle Mode    (1=Jazelle Bytecode instructions) (if supported)
  23-10 Reserved            (For future use) - Do not change manually!
  9     E - Endian          (... Big endian)                  (ARM11 ?)
  8     A - Abort disable   (1=Disable Imprecise Data Aborts) (ARM11 only)
  7     I - IRQ disable     (0=Enable, 1=Disable)                     ;\
  6     F - FIQ disable     (0=Enable, 1=Disable)                     ; Control
  5     T - State Bit       (0=ARM, 1=THUMB) - Do not change manually!; Bits
  4-0   M4-M0 - Mode Bits   (See below)                               ;/
```



## Bit 31-28: Condition Code Flags (N,Z,C,V)


These bits reflect results of logical or arithmetic instructions. In ARM mode,
it is often optionally whether an instruction should modify flags or not, for
example, it is possible to execute a SUB instruction that does NOT modify the
condition flags.

In ARM state, all instructions can be executed conditionally depending on the
settings of the flags, such like MOVEQ (Move if Z=1). While In THUMB state,
only Branch instructions (jumps) can be made conditionally.



## Bit 27: Sticky Overflow Flag (Q) - ARMv5TE and ARMv5TExP and up only


Used by QADD, QSUB, QDADD, QDSUB, SMLAxy, and SMLAWy only. These opcodes set
the Q-flag in case of overflows, but leave it unchanged otherwise. The Q-flag
can be tested/reset by MSR/MRS opcodes only.



## Bit 27-8: Reserved Bits (except Bit 27 on ARMv5TE and up, see above)


These bits are reserved for possible future implementations. For best forwards
compatibility, the user should never change the state of these bits, and should
not expect these bits to be set to a specific value.



## Bit 7-0: Control Bits (I,F,T,M4-M0)


These bits may change when an exception occurs. In privileged modes (non-user
modes) they may be also changed manually.

The interrupt bits I and F are used to disable IRQ and FIQ interrupts
respectively (a setting of "1" means disabled).

The T Bit signalizes the current state of the CPU (0=ARM, 1=THUMB), this bit
should never be changed manually - instead, changing between ARM and THUMB
state must be done by BX instructions.

The Mode Bits M4-M0 contain the current operating mode.

```
  Binary Hex Dec  Expl.
  0xx00b 00h 0  - Old User       ;\26bit Backward Compatibility modes
  0xx01b 01h 1  - Old FIQ        ; (supported only on ARMv3, except ARMv3G,
  0xx10b 02h 2  - Old IRQ        ; and on some non-T variants of ARMv4)
  0xx11b 03h 3  - Old Supervisor ;/
  10000b 10h 16 - User (non-privileged)
  10001b 11h 17 - FIQ
  10010b 12h 18 - IRQ
  10011b 13h 19 - Supervisor (SWI)
  10111b 17h 23 - Abort
  11011b 1Bh 27 - Undefined
  11111b 1Fh 31 - System (privileged 'User' mode) (ARMv4 and up)
```

Writing any other values into the Mode bits is not allowed.



## Saved Program Status Registers (SPSR\_\<mode>)


Additionally to above CPSR, five Saved Program Status Registers exist:

SPSR\_fiq, SPSR\_svc, SPSR\_abt, SPSR\_irq, SPSR\_und

Whenever the CPU enters an exception, the current status register (CPSR) is
copied to the respective SPSR\_\<mode> register. Note that there is only
one SPSR for each mode, so nested exceptions inside of the same mode are
allowed only if the exception handler saves the content of SPSR in memory.

For example, for an IRQ exception: IRQ-mode is entered, and CPSR is copied to
SPSR\_irq. If the interrupt handler wants to enable nested IRQs, then it must
first push SPSR\_irq before doing so.




# <a name="armcpu26bitmemoryinterface"></a>ARM CPU 26bit Memory Interface



The 26bit Memory Interface was used by ARMv1 and ARMv2. The 32bit interface is
used by ARMv3 and newer, however, 26bit backward compatibility was included in
all ARMv3 (except ARMv3G), and optionally in some non-T variants of ARMv4.



## Format of R15 in 26bit Mode (Program Counter Register)


```
  Bit   Name     Expl.
  31-28 N,Z,C,V  Flags (Sign, Zero, Carry, Overflow)
  27-26 I,F      Interrupt Disable bits (IRQ, FIQ) (1=Disable)
  25-2  PC       Program Counter, 24bit, Step 4 (64M range)
  1-0   M1,M0    Mode (0=User, 1=FIQ, 2=IRQ, 3=Supervisor)
```

Branches with +/-32M range wrap the PC register, and can reach all 64M memory.



## Reading from R15


If R15 is specified in bit16-19 of an opcode, then NZCVIF and M0,1 are masked
(zero), otherwise the full 32bits are used.



## Writing to R15


ALU opcodes with S=1, and LDM opcodes with PSR=1 can write to all 32bits in R15
(in 26bit mode, that is allowed even in user mode, though it does then affect
only NZCF, not the write protected IFMM bits ???), other opcodes which write to
R15 will modify only the program counter bits. Also, special CMP/CMN/TST/TEQ{P}
opcodes can be used to write to the PSR bits in R15 without modifying the PC
bits.



## Exceptions


SWIs, Reset, Data/Prefetch Aborts and Undefined instructions enter Supervisor
mode. Interrupts enter IRQ and FIQ mode. Additionally, a special 26bit Address
Exception exists, which enters Supervisor mode on accesses to memory
addresses>=64M as follows:

```
  R14_svc = PC ($+8, including old PSR bits)
  M1,M0 = 11b = supervisor mode, F=same, I=1, PC=14h,
  to continue at the fault location, return by SUBS PC,LR,8.
```

32bit CPUs with 26bit compatibility mode can be configured to switch into 32bit
mode when encountering exceptions.




# <a name="armcpuexceptions"></a>ARM CPU Exceptions




## Exception Vectors


The following are the exception vectors in memory. That is, when an exception
arises, CPU is switched into ARM state, and the program counter (PC) is loaded
by the respective address.

```
  Address  Prio  Exception                  Mode on Entry      Interrupt Flags
  BASE+00h 1     Reset                      Supervisor (_svc)  I=1, F=1
  BASE+04h 7     Undefined Instruction      Undefined  (_und)  I=1, F=unchanged
  BASE+08h 6     Software Interrupt (SWI)   Supervisor (_svc)  I=1, F=unchanged
  BASE+0Ch 5     Prefetch Abort             Abort      (_abt)  I=1, F=unchanged
  BASE+10h 2     Data Abort                 Abort      (_abt)  I=1, F=unchanged
  BASE+14h ??    Address Exceeds 26bit      Supervisor (_svc)  I=1, F=unchanged
  BASE+18h 4     Normal Interrupt (IRQ)     IRQ        (_irq)  I=1, F=unchanged
  BASE+1Ch 3     Fast Interrupt (FIQ)       FIQ        (_fiq)  I=1, F=1
```

BASE is normally 00000000h, but may be optionally FFFF0000h in some ARM CPUs.
Priority for simultaneously occuring exceptions ranges from Prio=1=Highest to
Prio=7=Lowest.

As there's only space for one ARM opcode at each of the above addresses, it'd
be usually recommended to deposit a Branch opcode into each vector, which'd
then redirect to the actual exception handlers address.



## Actions performed by CPU when entering an exception


```
  - R14_<new mode>=PC+nn   ;save old PC, ie. return address
  - SPSR_<new mode>=CPSR   ;save old flags
  - CPSR new T,M bits      ;set to T=0 (ARM state), and M4-0=new mode
  - CPSR new I bit         ;IRQs disabled (I=1), done by ALL exceptions
  - CPSR new F bit         ;FIQs disabled (F=1), done by Reset and FIQ only
  - PC=exception_vector    ;see table above
```

Above "PC+nn" depends on the type of exception. Basically, in ARM state that
nn-offset is caused by pipelining, and in THUMB state an identical ARM-style
'offset' is generated (even though the 'base address' may be only
halfword-aligned).



## Required user-handler actions when returning from an exception


Restore any general registers (R0-R14) which might have been modified by the
exception handler. Use return-instruction as listed in the respective
descriptions below, this will both restore PC and CPSR - that automatically
involves that the old CPU state (THUMB or ARM) as well as old state of FIQ and
IRQ disable flags are restored.

As mentioned above (see action on entering...), the return address is always
saved in ARM-style format, so that exception handler may use the same
return-instruction, regardless of whether the exception has been generated from
inside of ARM or THUMB state.



## FIQ (Fast Interrupt Request)


This interrupt is generated by a LOW level on the nFIQ input. It is supposed to
process timing critical interrupts at a high priority, as fast as possible.

Additionally to the common banked registers (R13\_fiq,R14\_fiq), five extra
banked registers (R8\_fiq-R12\_fiq) are available in FIQ mode. The exception
handler may freely access these registers without modifying the main programs
R8-R12 registers (and without having to save that registers on stack).

In privileged (non-user) modes, FIQs may be also manually disabled by setting
the F Bit in CPSR.



## IRQ (Normal Interrupt Request)


This interrupt is generated by a LOW level on the nIRQ input. Unlike FIQ, the
IRQ mode is not having its own banked R8-R12 registers.

IRQ is having lower priority than FIQ, and IRQs are automatically disabled when
a FIQ exception becomes executed. In privileged (non-user) modes, IRQs may be
also manually disabled by setting the I Bit in CPSR.

To return from IRQ Mode (continuing at following opcode):

```
  SUBS PC,R14,4   ;both PC=R14_irq-4, and CPSR=SPSR_irq
```



## Software Interrupt


Generated by a software interrupt instruction (SWI). Recommended to request a
supervisor (operating system) function. The SWI instruction may also contain a
parameter in the 'comment field' of the opcode:

In case that your main program issues SWIs from both inside of THUMB and ARM
states, then your exception handler must separate between 24bit comment fields
in ARM opcodes, and 8bit comment fields in THUMB opcodes (if necessary
determine old state by examining T Bit in SPSR\_svc); However, in Little Endian
mode, you could use only the most significant 8bits of the 24bit ARM comment
field (as done in the GBA, for example) - the exception handler could then
process the BYTE at [R14-2], regardless of whether it's been called from ARM or
THUMB state.

To return from Supervisor Mode (continuing at following opcode):

```
  MOVS PC,R14   ;both PC=R14_svc, and CPSR=SPSR_svc
```

Note: Like all other exceptions, SWIs are always executed in ARM state, no
matter whether it's been caused by an ARM or THUMB state SWI instruction.



## Undefined Instruction Exception (supported by ARMv3 and up)


This exception is generated when the CPU comes across an instruction which it
cannot handle. Most likely signalizing that the program has locked up, and that
an errormessage should be displayed.

However, it might be also used to emulate custom functions, ie. as an
additional 'SWI' instruction (which'd use R14\_und and SPSR\_und though, and it'd
thus allow to execute the Undefined Instruction handler from inside of
Supervisor mode without having to save R14\_svc and SPSR\_svc).

To return from Undefined Mode (continuing at following opcode):

```
  MOVS PC,R14   ;both PC=R14_und, and CPSR=SPSR_und
```

Note that not all unused opcodes are necessarily producing an exception, for
example, an ARM state Multiply instruction with Bit6=1 would be blindly
accepted as 'legal' opcode.



## Abort (supported by ARMv3 and up)


Aborts (page faults) are mostly supposed for virtual memory systems (ie. not
used in GBA, as far as I know), otherwise they might be used just to display an
error message. Two types of aborts exists:

- Prefetch Abort (occurs during an instruction prefetch)

- Prefetch Abort (also occurs on BKPT opcodes, ARMv5 and up)

- Data Abort (occurs during a data access)

A virtual memory systems abort handler would then most likely determine the
fault address: For prefetch abort that's just "R14\_abt-4". For Data abort, the
THUMB or ARM instruction at "R14\_abt-8" needs to be 'disassembled' in order to
determine the addressed data in memory.

The handler would then fix the error by loading the respective memory page into
physical memory, and then retry to execute the SAME instruction again, by
returning as follows:

```
  prefetch abort: SUBS PC,R14,#4   ;PC=R14_abt-4, and CPSR=SPSR_abt
  data abort:     SUBS PC,R14,#8   ;PC=R14_abt-8, and CPSR=SPSR_abt
```

Separate exception vectors for prefetch/data abort exists, each should use the
respective return instruction as shown above.



## Address Exceeds 26bit


This exception can occur only on old ARM CPUs with 26bit address scheme (or in
26bit backwards compatibility mode).



## Reset


Forces PC=VVVV0000h, and forces control bits of CPSR to T=0 (ARM state), F=1
and I=1 (disable FIQ and IRQ), and M4-0=10011b (Supervisor mode).




# <a name="armcpumemoryalignments"></a>ARM CPU Memory Alignments



The CPU does NOT support accessing mis-aligned addresses (which would be rather
slow because it'd have to merge/split that data into two accesses).

When reading/writing code/data to/from memory, Words and Halfwords must be
located at well-aligned memory address, ie. 32bit words aligned by 4, and 16bit
halfwords aligned by 2.



## Mis-aligned STR,STRH,STM,LDM,LDRD,STRD,PUSH,POP (forced align)


The mis-aligned low bit(s) are ignored, the memory access goes to a forcibly
aligned (rounded-down) memory address.

For LDRD/STRD, it isn't clearly defined if the address must be aligned by 8 (on
the NDS, align-4 seems to be okay) (align-8 may be required on other CPUs with
64bit databus).



## Mis-aligned LDR,SWP (rotated read)


Reads from forcibly aligned address "addr AND (NOT 3)", and does then rotate
the data as "ROR (addr AND 3)\*8". That effect is internally used by LDRB and
LDRH opcodes (which do then mask-out the unused bits).

The SWP opcode works like a combination of LDR and STR, that means, it does
read-rotated, but does write-unrotated.



## Mis-aligned LDRH,LDRSH (does or does not do strange things)


On ARM9 aka ARMv5 aka NDS9:

```
  LDRH Rd,[odd]   -->  LDRH Rd,[odd-1]        ;forced align
  LDRSH Rd,[odd]  -->  LDRSH Rd,[odd-1]       ;forced align
```

On ARM7 aka ARMv4 aka NDS7/GBA:

```
  LDRH Rd,[odd]   -->  LDRH Rd,[odd-1] ROR 8  ;read to bit0-7 and bit24-31
  LDRSH Rd,[odd]  -->  LDRSB Rd,[odd]         ;sign-expand BYTE value
```



## Mis-aligned PC/R15 (branch opcodes, or MOV/ALU/LDR with Rd=R15)


For ARM code, the low bits of the target address should be usually zero,
otherwise, R15 is forcibly aligned by clearing the lower two bits.

For THUMB code, the low bit of the target address may/should/must be set, the
bit is (or is not) interpreted as thumb-bit (depending on the opcode), and R15
is then forcibly aligned by clearing the lower bit.

In short, R15 will be always forcibly aligned, so mis-aligned branches won't
have effect on subsequent opcodes that use R15, or [R15+disp] as operand.




# <a name="arminstructionsummary"></a>ARM Instruction Summary



Modification of CPSR flags is optional for all {S} instructions.



## Logical ALU Operations


```
  Instruction                      Cycles   Flags  Expl.
  MOV{cond}{S} Rd,Op2              1S+x+y    NZc-  Rd = Op2
  MVN{cond}{S} Rd,Op2              1S+x+y    NZc-  Rd = NOT Op2
  ORR{cond}{S} Rd,Rn,Op2           1S+x+y    NZc-  Rd = Rn OR Op2
  EOR{cond}{S} Rd,Rn,Op2           1S+x+y    NZc-  Rd = Rn XOR Op2
  AND{cond}{S} Rd,Rn,Op2           1S+x+y    NZc-  Rd = Rn AND Op2
  BIC{cond}{S} Rd,Rn,Op2           1S+x+y    NZc-  Rd = Rn AND NOT Op2
  TST{cond}{P}    Rn,Op2           1S+x      NZc-  Void = Rn AND Op2
  TEQ{cond}{P}    Rn,Op2           1S+x      NZc-  Void = Rn XOR Op2
```

Add x=1I cycles if Op2 shifted-by-register. Add y=1S+1N cycles if Rd=R15.

Carry flag affected only if Op2 contains a non-zero shift amount.



## Arithmetic ALU Operations


```
  Instruction                      Cycles   Flags  Expl.
  ADD{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Rn+Op2
  ADC{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Rn+Op2+Cy
  SUB{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Rn-Op2
  SBC{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Rn-Op2+Cy-1
  RSB{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Op2-Rn
  RSC{cond}{S} Rd,Rn,Op2           1S+x+y    NZCV  Rd = Op2-Rn+Cy-1
  CMP{cond}{P}    Rn,Op2           1S+x      NZCV  Void = Rn-Op2
  CMN{cond}{P}    Rn,Op2           1S+x      NZCV  Void = Rn+Op2
```

Add x=1I cycles if Op2 shifted-by-register. Add y=1S+1N cycles if Rd=R15.



## Multiply


```
  Instruction                      Cycles   Flags  Expl.
  MUL{cond}{S} Rd,Rm,Rs            1S+mI     NZx-  Rd = Rm*Rs
  MLA{cond}{S} Rd,Rm,Rs,Rn         1S+mI+1I  NZx-  Rd = Rm*Rs+Rn
  UMULL{cond}{S} RdLo,RdHi,Rm,Rs   1S+mI+1I  NZx-  RdHiLo = Rm*Rs
  UMLAL{cond}{S} RdLo,RdHi,Rm,Rs   1S+mI+2I  NZx-  RdHiLo = Rm*Rs+RdHiLo
  SMULL{cond}{S} RdLo,RdHi,Rm,Rs   1S+mI+1I  NZx-  RdHiLo = Rm*Rs
  SMLAL{cond}{S} RdLo,RdHi,Rm,Rs   1S+mI+2I  NZx-  RdHiLo = Rm*Rs+RdHiLo
  SMLAxy{cond}  Rd,Rm,Rs,Rn     ARMv5TE(xP)  ----q Rd=HalfRm*HalfRs+Rn
  SMLAWy{cond}  Rd,Rm,Rs,Rn     ARMv5TE(xP)  ----q Rd=(Rm*HalfRs)/10000h+Rn
  SMULWy{cond}  Rd,Rm,Rs        ARMv5TE(xP)  ----  Rd=(Rm*HalfRs)/10000h
  SMLALxy{cond} RdLo,RdHi,Rm,Rs ARMv5TE(xP)  ----  RdHiLo=RdHiLo+HalfRm*HalfRs
  SMULxy{cond}  Rd,Rm,Rs        ARMv5TE(xP)  ----  Rd=HalfRm*HalfRs
```



## Memory Load/Store


```
  Instruction                      Cycles    Flags  Expl.
  LDR{cond}{B}{T} Rd,<Address>     1S+1N+1I+y ----  Rd=[Rn+/-<offset>]
  LDR{cond}H      Rd,<Address>     1S+1N+1I+y ----  Load Unsigned halfword
  LDR{cond}D      Rd,<Address>                ----  Load Dword ARMv5TE
  LDR{cond}SB     Rd,<Address>     1S+1N+1I+y ----  Load Signed byte
  LDR{cond}SH     Rd,<Address>     1S+1N+1I+y ----  Load Signed halfword
  LDM{cond}{amod} Rn{!},<Rlist>{^} nS+1N+1I+y ----  Load Multiple
  STR{cond}{B}{T} Rd,<Address>     2N         ----  [Rn+/-<offset>]=Rd
  STR{cond}H      Rd,<Address>     2N         ----  Store halfword
  STR{cond}D      Rd,<Address>                ----  Store Dword ARMv5TE
  STM{cond}{amod} Rn{!},<Rlist>{^} (n-1)S+2N  ----  Store Multiple
  SWP{cond}{B}    Rd,Rm,[Rn]       1S+2N+1I   ----  Rd=[Rn], [Rn]=Rm
  PLD             <Address>        1S         ----  Prepare Cache ARMv5TE
```

For LDR/LDM, add y=1S+1N if Rd=R15, or if R15 in Rlist.



## Jumps, Calls, CPSR Mode, and others


```
  Instruction                  Cycles  Flags  Expl.
  B{cond}   label              2S+1N    ----  PC=$+8+/-32M
  BL{cond}  label              2S+1N    ----  PC=$+8+/-32M, LR=$+4
  BX{cond}  Rn                 2S+1N    ----  PC=Rn, T=Rn.0 (THUMB/ARM)
  BLX{cond} Rn                 2S+1N    ----  PC=Rn, T=Rn.0, LR=PC+4, ARM9
  BLX       label              2S+1N    ----  PC=PC+$+/-32M, LR=$+4, T=1, ARM9
  MRS{cond} Rd,Psr             1S       ----  Rd=Psr
  MSR{cond} Psr{_field},Op     1S      (psr)  Psr[field]=Op
  SWI{cond} Imm24bit           2S+1N    ----  PC=8, ARM Svc mode, LR=$+4
  BKPT      Imm16bit           ???      ----  PC=C, ARM Abt mode, LR=$+4 ARM9
  The Undefined Instruction    2S+1I+1N ----  PC=4, ARM Und mode, LR=$+4
  cond=false                   1S       ----  Any opcode with condition=false
  NOP                          1S       ----  R0=R0
```


```
  CLZ{cond} Rd,Rm              ???      ----    Count Leading Zeros ARMv5
  QADD{cond} Rd,Rm,Rn                   ----q   Rd=Rm+Rn       ARMv5TE(xP)
  QSUB{cond} Rd,Rm,Rn                   ----q   Rd=Rm-Rn       ARMv5TE(xP)
  QDADD{cond} Rd,Rm,Rn                  ----q   Rd=Rm+Rn*2     ARMv5TE(xP)
  QDSUB{cond} Rd,Rm,Rn                  ----q   Rd=Rm-Rn*2     ARMv5TE(xP)
```



## Coprocessor Functions (if any)


```
  Instruction                         Cycles  Flags  Expl.
  CDP{cond} Pn,<cpopc>,Cd,Cn,Cm{,<cp>} 1S+bI   ----  Coprocessor specific
  STC{cond}{L} Pn,Cd,<Address>         (n-1)S+2N+bI  [address] = CRd
  LDC{cond}{L} Pn,Cd,<Address>         (n-1)S+2N+bI  CRd = [address]
  MCR{cond} Pn,<cpopc>,Rd,Cn,Cm{,<cp>} 1S+bI+1C      CRn = Rn {<op> CRm}
  MRC{cond} Pn,<cpopc>,Rd,Cn,Cm{,<cp>} 1S+(b+1)I+1C  Rn = CRn {<op> CRm}
  CDP2,STC2,LDC2,MCR2,MRC2 - ARMv5 Extensions similar above, without {cond}
  MCRR{cond} Pn,<cpopc>,Rd,Rn,Cm  ;write Rd,Rn to coproc ARMv5TE
  MRRC{cond} Pn,<cpopc>,Rd,Rn,Cm  ;read Rd,Rn from coproc ARMv5TE
```



## ARM Binary Opcode Format


```
  |..3 ..................2 ..................1 ..................0|
  |1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0|
  |_Cond__|0_0_0|___Op__|S|__Rn___|__Rd___|__Shift__|Typ|0|__Rm___| DataProc
  |_Cond__|0_0_0|___Op__|S|__Rn___|__Rd___|__Rs___|0|Typ|1|__Rm___| DataProc
  |_Cond__|0_0_1|___Op__|S|__Rn___|__Rd___|_Shift_|___Immediate___| DataProc
  |_Cond__|0_0_1_1_0_0_1_0_0_0_0_0_1_1_1_1_0_0_0_0|_____Hint______| ARM11:Hint
  |_Cond__|0_0_1_1_0|P|1|0|_Field_|__Rd___|_Shift_|___Immediate___| PSR Imm
  |_Cond__|0_0_0_1_0|P|L|0|_Field_|__Rd___|0_0_0_0|0_0_0_0|__Rm___| PSR Reg
  |_Cond__|0_0_0_1_0_0_1_0_1_1_1_1_1_1_1_1_1_1_1_1|0_0|L|1|__Rn___| BX,BLX
  |1_1_1_0|0_0_0_1_0_0_1_0|_____immediate_________|0_1_1_1|_immed_| ARM9:BKPT
  |_Cond__|0_0_0_1_0_1_1_0_1_1_1_1|__Rd___|1_1_1_1|0_0_0_1|__Rm___| ARM9:CLZ
  |_Cond__|0_0_0_1_0|Op_|0|__Rn___|__Rd___|0_0_0_0|0_1_0_1|__Rm___| ARM9:QALU
  |_Cond__|0_0_0_0_0_0|A|S|__Rd___|__Rn___|__Rs___|1_0_0_1|__Rm___| Multiply
  |_Cond__|0_0_0_0_0_1_0_0|_RdHi__|_RdLo__|__Rs___|1_0_0_1|__Rm___| ARM11:UMAAL
  |_Cond__|0_0_0_0_1|U|A|S|_RdHi__|_RdLo__|__Rs___|1_0_0_1|__Rm___| MulLong
  |_Cond__|0_0_0_1_0|Op_|0|Rd/RdHi|Rn/RdLo|__Rs___|1|y|x|0|__Rm___| MulHalfARM9
  |_Cond__|0_0_0_1_0|B|0_0|__Rn___|__Rd___|0_0_0_0|1_0_0_1|__Rm___| TransSwp12
  |_Cond__|0_0_0_1_1|_Op__|__Rn___|__Rd___|1_1_1_1|1_0_0_1|__Rm___| ARM11:LDREX
  |_Cond__|0_0_0|P|U|0|W|L|__Rn___|__Rd___|0_0_0_0|1|S|H|1|__Rm___| TransReg10
  |_Cond__|0_0_0|P|U|1|W|L|__Rn___|__Rd___|OffsetH|1|S|H|1|OffsetL| TransImm10
  |_Cond__|0_1_0|P|U|B|W|L|__Rn___|__Rd___|_________Offset________| TransImm9
  |_Cond__|0_1_1|P|U|B|W|L|__Rn___|__Rd___|__Shift__|Typ|0|__Rm___| TransReg9
  |_Cond__|0_1_1|________________xxx____________________|1|__xxx__| Undefined
  |_Cond__|0_1_1|Op_|x_x_x_x_x_x_x_x_x_x_x_x_x_x_x_x_x_x|1|x_x_x_x| ARM11:Media
  |1_1_1_1_0_1_0_1_0_1_1_1_1_1_1_1_1_1_1_1_0_0_0_0_0_0_0_1_1_1_1_1| ARM11:CLREX
  |_Cond__|1_0_0|P|U|S|W|L|__Rn___|__________Register_List________| BlockTrans
  |_Cond__|1_0_1|L|___________________Offset______________________| B,BL,BLX
  |_Cond__|1_1_0|P|U|N|W|L|__Rn___|__CRd__|__CP#__|____Offset_____| CoDataTrans
  |_Cond__|1_1_0_0_0_1_0|L|__Rn___|__Rd___|__CP#__|_CPopc_|__CRm__| CoRR ARM9
  |_Cond__|1_1_1_0|_CPopc_|__CRn__|__CRd__|__CP#__|_CP__|0|__CRm__| CoDataOp
  |_Cond__|1_1_1_0|CPopc|L|__CRn__|__Rd___|__CP#__|_CP__|1|__CRm__| CoRegTrans
  |_Cond__|1_1_1_1|_____________Ignored_by_Processor______________| SWI
```



## ARM11 SIMD Media instructions (ARM11/ARMv6) (formerly Undef opcode space)


```
  |..3 ..................2 ..................1 ..................0|
  |1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0|
  |_Cond__|0_1_1|0_0|_Op__|__Rn___|__Rd___|1_1_1_1|_Op__|1|__Rm___| ParaAddSub
  |_Cond__|0_1_1|0_1|0_0_0|__Rn___|__Rd___|_shift___|x|0_1|__Rm___| HalfPack
  |_Cond__|0_1_1|0_1|U|1|____sat__|__Rd___|_shift___|x|0_1|__Rm___| WordSat
  |_Cond__|0_1_1|0_1|U|1_0|__sat__|__Rd___|1_1_1_1|0_0_1_1|__Rm___| ParaHalfSat
  |_Cond__|0_1_1|0_1|0_1_1|1_1_1_1|__Rd___|1_1_1_1|0_0_1_1|__Rm___| RevWord
  |_Cond__|0_1_1|0_1|0_1_1|1_1_1_1|__Rd___|1_1_1_1|1_0_1_1|__Rm___| RevPackHalf
  |_Cond__|0_1_1|0_1|1_1_1|1_1_1_1|__Rd___|1_1_1_1|1_0_1_1|__Rm___| RevSignHalf
  |_Cond__|0_1_1|0_1|0_0_0|__Rn___|__Rd___|1_1_1_1|1_0_1_1|__Rm___| SelectBytes
  |_Cond__|0_1_1|0_1|_Op__|__Rn___|__Rd___|rot|0_0|0_1_1_1|__Rm___| Extend U/S
  |_Cond__|0_1_1|1_0|_Op__|Rd/RdHi|Rn/RdLo|__Rs___|_Op__|1|__Rm___| Multiplies
  |_Cond__|0_1_1|1_1|0_0_0|__Rd___|__Rn___|__Rs___|0_0_0_1|__Rm___| DiffSum
  |_Cond__|0_1_1|1_1|0_0_0|__Rd___|1_1_1_1|__Rs___|0_0_0_1|__Rm___| DiffSumAcc
  |_Cond__|0_1_1_1_1_1_1_1|x_x_x_x_x_x_x_x_x_x_x_x|1_1_1_1|x_x_x_x| Undef
```




# <a name="armopcodesbranchandbranchwithlinkbblbxblxswibkpt"></a>ARM Opcodes: Branch and Branch with Link (B, BL, BX, BLX, SWI, BKPT)




## Branch and Branch with Link (B, BL, BLX\_imm)


Branch (B) is supposed to jump to a subroutine. Branch with Link is meant to be
used to call to a subroutine, return address is then saved in R14.

```
  Bit    Expl.
  31-28  Condition (must be 1111b for BLX)
  27-25  Must be "101" for this instruction
  24     Opcode (0-1) (or Halfword Offset for BLX)
          0: B{cond} label    ;branch            PC=PC+8+nn*4
          1: BL{cond} label   ;branch/link       PC=PC+8+nn*4, LR=PC+4
          H: BLX label ;ARM9  ;branch/link/thumb PC=PC+8+nn*4+H*2, LR=PC+4, T=1
  23-0   nn - Signed Offset, step 4      (-32M..+32M in steps of 4)
```

Branch with Link can be used to 'call' to a sub-routine, which may then
'return' by MOV PC,R14 for example.

Execution Time: 2S + 1N

Return: No flags affected.



## Branch and Exchange (BX, BLX\_reg)


```
  Bit    Expl.
  31-28  Condition
  27-8   Must be "0001.0010.1111.1111.1111" for this instruction
  7-4    Opcode
          0001b: BX{cond}  Rn    ;PC=Rn, T=Rn.0   (ARMv4T and ARMv5 and up)
          0010b: BXJ{cond} Rn    ;Change to Jazelle bytecode (ARMv5TEJ and up)
          0011b: BLX{cond} Rn    ;PC=Rn, T=Rn.0, LR=PC+4     (ARMv5 and up)
  3-0    Rn - Operand Register  (R0-R14)
```

Switching to THUMB Mode: Set Bit 0 of the value in Rn to 1, program continues
then at Rn-1 in THUMB mode.

Using BLX R14 is possible (sets PC=Old\_LR, and New\_LR=retadr).

Using BX R15 acts as BX $+8 (tested and working on ARM7/ARM9, although it isn't
officially defined as predictable behaviour).

Execution Time: 2S + 1N

Return: No flags affected.



## Notes on BXJ (Branch or change to Jazelle bytecode state):


If Jazelle is suppored/enabled: Executes Java bytecode at address R14

If Jazelle is NOT suppored/enabled: Behaves exactly same as BX Rn

That is, R14 is the byte code address, and Rn is an ARM/THUMB error handler.

Supported on ARMv5TEJ and ARMv6 and up.



## Branch via ALU, LDR, LDM


Most ALU, LDR, LDM opcodes can also change PC/R15.



## Software Interrupt (SWI/BKPT) (svc/abt exceptions)


SWI supposed for calls to the operating system - Enter Supervisor mode (SVC) in
ARM state. BKPT intended for debugging - enters Abort mode in ARM state via
Prefetch Abort vector.

```
  Bit    Expl.
  31-28  Condition (must be 1110b for BKPT, ie. Condition=always)
  27-24  Opcode
          1111b: SWI{cond} nn   ;software interrupt
          0001b: BKPT      nn   ;breakpoint (ARMv5 and up)
  For SWI:
   23-0   nn - Comment Field, ignored by processor (24bit value)
  For BKPT:
   23-20  Must be 0010b for BKPT
   19-8   nn - upper 12bits of comment field, ignored by processor
   7-4    Must be 0111b for BKPT
   3-0    nn - lower 4bits of comment field, ignored by processor
```

Execution Time: 2S+1N

The exception handler may interprete the SWI Comment Field by examining the
lower 24bit of the 32bit opcode opcode at [R14\_svc-4]. If your are also using
SWI's from inside of THUMB, then the SWI handler must examine the T Bit
SPSR\_svc in order to determine whether it's been a THUMB SWI - and if so,
examine the lower 8bit of the 16bit opcode opcode at [R14\_svc-2].

For Returning from SWI use "MOVS PC,R14", that instruction does restore both PC
and CPSR, ie. PC=R14\_svc, and CPSR=SPSR\_svc.

Nesting SWIs: SPSR\_svc and R14\_svc should be saved on stack before either
invoking nested SWIs, or (if the IRQ handler uses SWIs) before enabling IRQs.

Execution SWI/BKPT:

```
  R14_svc=PC+4     R14_abt=PC+4   ;save return address
  SPSR_svc=CPSR    SPSR_abt=CPSR  ;save CPSR flags
  CPSR=<changed>   CPSR=<changed> ;Enter svc/abt, ARM state, IRQs disabled
  PC=VVVV0008h     PC=VVVV000Ch   ;jump to SWI/PrefetchAbort vector address
```



## Undefined Instruction (und exception)


```
  Bit    Expl.
  31-28  Condition
  27-25  Must be 011b for this instruction
  24-5   Reserved for future use
  4      Must be 1b for this instruction
  3-0    Reserved for future use
```

No assembler mnemonic exists, following bitstreams are (not) reserved.

```
  cond011xxxxxxxxxxxxxxxxxxxx1xxxx - reserved for future use (except below).
  cond01111111xxxxxxxxxxxx1111xxxx - free for user.
```

Note: ARMv6 does use the "reserved for future" space for the new SIMD opcodes
(but still has the "free for user" space available as "undefined").

Execution time: 2S+1I+1N.




# <a name="armopcodesdataprocessingalu"></a>ARM Opcodes: Data Processing (ALU)




## Opcode Format


```
  Bit    Expl.
  31-28  Condition
  27-26  Must be 00b for this instruction
  25     I - Immediate 2nd Operand Flag (0=Register, 1=Immediate)
  24-21  Opcode (0-Fh)               ;*=Arithmetic, otherwise Logical
           0: AND{cond}{S} Rd,Rn,Op2    ;AND logical       Rd = Rn AND Op2
           1: EOR{cond}{S} Rd,Rn,Op2    ;XOR logical       Rd = Rn XOR Op2
           2: SUB{cond}{S} Rd,Rn,Op2 ;* ;subtract          Rd = Rn-Op2
           3: RSB{cond}{S} Rd,Rn,Op2 ;* ;subtract reversed Rd = Op2-Rn
           4: ADD{cond}{S} Rd,Rn,Op2 ;* ;add               Rd = Rn+Op2
           5: ADC{cond}{S} Rd,Rn,Op2 ;* ;add with carry    Rd = Rn+Op2+Cy
           6: SBC{cond}{S} Rd,Rn,Op2 ;* ;sub with carry    Rd = Rn-Op2+Cy-1
           7: RSC{cond}{S} Rd,Rn,Op2 ;* ;sub cy. reversed  Rd = Op2-Rn+Cy-1
           8: TST{cond}{P}    Rn,Op2    ;test            Void = Rn AND Op2
           9: TEQ{cond}{P}    Rn,Op2    ;test exclusive  Void = Rn XOR Op2
           A: CMP{cond}{P}    Rn,Op2 ;* ;compare         Void = Rn-Op2
           B: CMN{cond}{P}    Rn,Op2 ;* ;compare neg.    Void = Rn+Op2
           C: ORR{cond}{S} Rd,Rn,Op2    ;OR logical        Rd = Rn OR Op2
           D: MOV{cond}{S} Rd,Op2       ;move              Rd = Op2
           E: BIC{cond}{S} Rd,Rn,Op2    ;bit clear         Rd = Rn AND NOT Op2
           F: MVN{cond}{S} Rd,Op2       ;not               Rd = NOT Op2
  20     S - Set Condition Codes (0=No, 1=Yes) (Must be 1 for opcode 8-B)
  19-16  Rn - 1st Operand Register (R0..R15) (including PC=R15)
              Must be 0000b for MOV/MVN.
  15-12  Rd - Destination Register (R0..R15) (including PC=R15)
              Must be 0000b (or 1111b) for CMP/CMN/TST/TEQ{P}.
  When above Bit 25 I=0 (Register as 2nd Operand)
    When below Bit 4 R=0 - Shift by Immediate
      11-7   Is - Shift amount   (1-31, 0=Special/See below)
    When below Bit 4 R=1 - Shift by Register
      11-8   Rs - Shift register (R0-R14) - only lower 8bit 0-255 used
      7      Reserved, must be zero  (otherwise multiply or LDREX or undefined)
    6-5    Shift Type (0=LSL, 1=LSR, 2=ASR, 3=ROR)
    4      R - Shift by Register Flag (0=Immediate, 1=Register)
    3-0    Rm - 2nd Operand Register (R0..R15) (including PC=R15)
  When above Bit 25 I=1 (Immediate as 2nd Operand)
    11-8   Is - ROR-Shift applied to nn (0-30, in steps of 2)
    7-0    nn - 2nd Operand Unsigned 8bit Immediate
```



## Second Operand (Op2)


This may be a shifted register, or a shifted immediate. See Bit 25 and 11-0.

Unshifted Register: Specify Op2 as "Rm", assembler converts to "Rm,LSL#0".

Shifted Register: Specify as "Rm,SSS#Is" or "Rm,SSS Rs" (SSS=LSL/LSR/ASR/ROR).

Immediate: Specify as 32bit value, for example: "#000NN000h", assembler should
automatically convert into "#0NNh,ROR#0ssh" as far as possible (ie. as far as a
section of not more than 8bits of the immediate is non-zero).



## Zero Shift Amount (Shift Register by Immediate, with Immediate=0)


```
  LSL#0: No shift performed, ie. directly Op2=Rm, the C flag is NOT affected.
  LSR#0: Interpreted as LSR#32, ie. Op2 becomes zero, C becomes Bit 31 of Rm.
  ASR#0: Interpreted as ASR#32, ie. Op2 and C are filled by Bit 31 of Rm.
  ROR#0: Interpreted as RRX#1 (RCR), like ROR#1, but Op2 Bit 31 set to old C.
```

In source code, LSR#32, ASR#32, and RRX#1 should be specified as such -
attempts to specify LSR#0, ASR#0, or ROR#0 will be internally converted to
LSL#0 by the assembler.



## Using R15 (PC)


When using R15 as Destination (Rd), note below CPSR description and Execution
time description.

When using R15 as operand (Rm or Rn), the returned value depends on the
instruction: PC+12 if I=0,R=1 (shift by register), otherwise PC+8 (shift by
immediate).



## Returned CPSR Flags


If S=1, Rd\<>R15, logical operations (AND,EOR,TST,TEQ,ORR,MOV,BIC,MVN):

```
  V=not affected
  C=carryflag of shift operation (not affected if LSL#0 or Rs=00h)
  Z=zeroflag of result
  N=signflag of result (result bit 31)
```

If S=1, Rd\<>R15, arithmetic operations (SUB,RSB,ADD,ADC,SBC,RSC,CMP,CMN):

```
  V=overflowflag of result
  C=carryflag of result
  Z=zeroflag of result
  N=signflag of result (result bit 31)
```

IF S=1, with unused Rd bits=1111b, {P} opcodes (CMPP/CMNP/TSTP/TEQP):

```
  R15=result  ;modify PSR bits in R15, ARMv2 and below only.
  In user mode only N,Z,C,V bits of R15 can be changed.
  In other modes additionally I,F,M1,M0 can be changed.
  The PC bits in R15 are left unchanged in all modes.
```

If S=1, Rd=R15; should not be used in user mode:

```
  CPSR = SPSR_<current mode>
  PC = result
  For example: MOVS PC,R14  ;return from SWI (PC=R14_svc, CPSR=SPSR_svc).
```

If S=0: Flags are not affected (not allowed for CMP,CMN,TEQ,TST).


The instruction "MOV R0,R0" is used as "NOP" opcode in 32bit ARM state.

Execution Time: (1+p)S+rI+pN. Whereas r=1 if I=0 and R=1 (ie. shift by
register); otherwise r=0. And p=1 if Rd=R15; otherwise p=0.




# <a name="armopcodesmultiplyandmultiplyaccumulatemulmla"></a>ARM Opcodes: Multiply and Multiply-Accumulate (MUL, MLA)




## Opcode Format


```
  Bit    Expl.
  31-28  Condition
  27-25  Must be 000b for this instruction
  24-21  Opcode
          0000b: MUL{cond}{S}   Rd,Rm,Rs        ;Rd=Rm*Rs     ;\works as both
          0001b: MLA{cond}{S}   Rd,Rm,Rs,Rn     ;Rd=Rm*Rs+Rn  ;/signed+unsigned
          0010b: UMAAL{cond}    RdLo,RdHi,Rm Rs ;RdHiLo=Rm*Rs+RdHi+RdLo;\un-
          0100b: UMULL{cond}{S} RdLo,RdHi,Rm,Rs ;RdHiLo=Rm*Rs          ; signed
          0101b: UMLAL{cond}{S} RdLo,RdHi,Rm,Rs ;RdHiLo=Rm*Rs+RdHiLo   ;/
          0110b: SMULL{cond}{S} RdLo,RdHi,Rm,Rs ;RdHiLo=Rm*Rs
          0111b: SMLAL{cond}{S} RdLo,RdHi,Rm,Rs ;RdHiLo=Rm*Rs+RdHiLo
          1000b: SMLAxy{cond}   Rd,Rm,Rs,Rn     ;Rd=HalfRm*HalfRs+Rn
          1001b: SMLAWy{cond}   Rd,Rm,Rs,Rn     ;Rd=(Rm*HalfRs)/10000h+Rn
          1001b: SMULWy{cond}   Rd,Rm,Rs        ;Rd=(Rm*HalfRs)/10000h
          1010b: SMLALxy{cond}  RdLo,RdHi,Rm,Rs ;RdHiLo=RdHiLo+HalfRm*HalfRs
          1011b: SMULxy{cond}   Rd,Rm,Rs        ;Rd=HalfRm*HalfRs
  20     S - Set Condition Codes (0=No, 1=Yes) (Must be 0 for Halfword & UMAAL)
  19-16  Rd (or RdHi) - Destination Register (R0-R14)
  15-12  Rn (or RdLo) - Accumulate Register  (R0-R14) (Set to 0000b if unused)
  11-8   Rs - Operand Register               (R0-R14)
  For Non-Halfword Multiplies
    7-4  Must be 1001b for these instructions
  For Halfword Multiplies
    7    Must be 1 for these instructions
    6    y - Rs Top/Bottom flag (0=B=Lower 16bit, 1=T=Upper 16bit)
    5    x - Rm Top/Bottom flag (as above), or 0 for SMLAW, or 1 for SMULW
    4    Must be 0 for these instructions
  3-0    Rm - Operand Register               (R0-R14)
```



## Multiply and Multiply-Accumulate (MUL, MLA)


Restrictions: Rd may not be same as Rm. Rd,Rn,Rs,Rm may not be R15.

Note: Only the lower 32bit of the internal 64bit result are stored in Rd, thus
no sign/zero extension is required and MUL and MLA can be used for both signed
and unsigned calculations!

Execution Time: 1S+mI for MUL, and 1S+(m+1)I for MLA. Whereas 'm' depends on
whether/how many most significant bits of Rs are all zero or all one. That is
m=1 for Bit 31-8, m=2 for Bit 31-16, m=3 for Bit 31-24, and m=4 otherwise.

Flags (if S=1): Z=zeroflag, N=signflag, C=destroyed (ARMv4 and below) or C=not
affected (ARMv5 and up), V=not affected. MUL/MLA supported by ARMv2 and up.



## Multiply Long and Multiply-Accumulate Long (MULL, MLAL)


Optionally supported, INCLUDED in ARMv3M, EXCLUDED in ARMv4xM/ARMv5xM.

Restrictions: RdHi,RdLo,Rm must be different registers. R15 may not be used.

Execution Time: 1S+(m+1)I for MULL, and 1S+(m+2)I for MLAL. Whereas 'm' depends
on whether/how many most significant bits of Rs are "all zero" (UMULL/UMLAL) or
"all zero or all one" (SMULL,SMLAL). That is m=1 for Bit31-8, m=2 for Bit31-16,
m=3 for Bit31-24, and m=4 otherwise.

Flags (if S=1): Z=zeroflag, N=signflag, C=destroyed (ARMv4 and below) or C=not
affected (ARMv5 and up), V=destroyed??? (ARMv4 and below???) or V=not affected
(ARMv5 and up).



## Signed Halfword Multiply (SMLAxy,SMLAWy,SMLALxy,SMULxy,SMULWy)


Supported by E variants of ARMv5 and up, ie. ARMv5TE(xP).

Q-flag gets set on 32bit SMLAxy/SMLAWy addition overflows, however, the result
is NOT truncated (as it'd be done with QADD opcodes).

Q-flag is NOT affected on (rare) 64bit SMLALxy addition overflows.

SMULxy/SMULWy cannot overflow, and thus leave Q-flag unchanged as well.

NZCV-flags are not affected by Halfword multiplies.

Execution Time: 1S+Interlock (SMULxy,SMLAxy,SMULWx,SMLAWx)

Execution Time: 1S+1I+Interlock (SMLALxy)



## Unsigned Multiply Accumulate Accumulate Long (UMAAL)


Multiply with two 32bit additions, supposed to be used for cryptography.

Flags are not affected.

ARMv6 and above.




# <a name="armopcodesspecialarm9instructionsclzqaddqsub"></a>ARM Opcodes: Special ARM9 Instructions (CLZ, QADD/QSUB)




## Count Leading Zeros (CLZ)


```
  Bit    Expl.
  31-28  Condition
  27-16  Must be 0001.0110.1111b for this instruction
         Opcode (fixed)
           CLZ{cond} Rd,Rm  ;Rd=Number of leading zeros in Rm
  15-12  Rd - Destination Register              (R0-R14)
  11-4   Must be 1111.0001b for this instruction
  3-0    Rm - Source Register                   (R0-R14)
```

CLZ supported by ARMv5 and up. Execution time: 1S.

Return: No Flags affected. Rd=0..32.



## Opcode Format (QADD/QSUB)


```
  Bit    Expl.
  31-28  Condition
  27-24  Must be 0001b for this instruction
  23-20  Opcode
          0000b: QADD{cond}  Rd,Rm,Rn    ;Rd=Rm+Rn
          0010b: QSUB{cond}  Rd,Rm,Rn    ;Rd=Rm-Rn
          0100b: QDADD{cond} Rd,Rm,Rn    ;Rd=Rm+Rn*2 (doubled)
          0110b: QDSUB{cond} Rd,Rm,Rn    ;Rd=Rm-Rn*2 (doubled)
  19-16  Rn - Second Source Register            (R0-R14)
  15-12  Rd - Destination Register              (R0-R14)
  11-4   Must be 00000101b for this instruction
  3-0    Rm - First Source Register             (R0-R14)
```

Supported by E variants of ARMv5 and up, ie. ARMv5TE(xP).

Execution time: 1S+Interlock.

Results truncated to signed 32bit range in case of overflows, with the Q-flag
being set (and being left unchanged otherwise). NZCV flags are not affected.

Note: Rn\*2 is internally processed first, and may get truncated - even if the
final result would fit into range.




# <a name="armopcodesspecialarm11instructionsmisc"></a>ARM Opcodes: Special ARM11 Instructions (Misc)




## Load/Store Byte/Halfword/Doubleword Exclusive (LDREX/STREX)


```
  Bit    Expl.
  31-28  Condition
  27-23  Must be 00011b for this instruction
  22-20  Opcode
           0: STREX{cond}  Rd, Rm, [Rn]  ;[Rn]=Rm, Rd=flag       ;32bit
           1: LDREX{cond}  Rd, [Rn]      ;Rd=[Rm]                ;32bit
           2: STREXD{cond} Rd, Rm, [Rn]  ;[Rn]=Rm,Rm+1, Rd=flag  ;64bit
           3: LDREXD{cond} Rd, [Rn]      ;Rd,Rd+1=[Rm]           ;64bit
           4: STREXB{cond} Rd, Rm, [Rn]  ;[Rn]=Rm, Rd=flag       ;8bit
           5: LDREXB{cond} Rd, [Rn]      ;Rd=[Rm]                ;8bit
           6: STREXH{cond} Rd, Rm, [Rn]  ;[Rn]=Rm, Rd=flag       ;16bit
           7: LDREXH{cond} Rd, [Rn]      ;Rd=[Rm]                ;16bit
  19-16  Rn - Base Register                      (R0-R14)
  15-12  Rd - Destination Register               (R0-R14)
  11-4   Must be 1111.1001b for this instruction
  3-0    Rm - Source Register (1111b for LDREX)  (R0-R14)
```

For exclusive access to shared memory regions (ie. in dual-core CPUs?)

Unknown what "exclusive" means?

Return: No Flags affected.

ARMv6 and up supports LDREX/STREX (with 32bit operand).

ARMv6K and up supports LDREX/STREX{B|H|D} (with 8/16/64bit operand).



## Clear Exclusive (CLREX)


```
  Bit    Expl.
  31-0   Opcode 1111.0101.0111.1111.1111.0000.0001.1111b for CLREX
```

Supported on ARMv6K and up.



## Hint (TrueNOP, YIELD, WFE, WFI, SEV)


```
  Bit    Expl.
  31-28  Condition (should be always 0Eh=Always for NOP)
  27-8   Must be 0011.0010.0000.1111.0000b
  7-0    Opcode/Hint
           00h: TrueNOP      ;no operation
           01h: YIELD{cond}
           02h: WFE{cond}    ;wait for event
           03h: WFI{cond}    ;wait for interrupt
           04h: SEV{cond}    ;send event
```

Supported on ARMv6K and up.



## Change Processor State aka CPSR Mode/Flags (CPS, CPSID, CPSIE)


```
  31-20  Must be 1111.0001.0000b
  19-17  Opcode
          001b: CPS   #<mode>           ;change mode
          100b: CPSIE {A}{I}{F}         ;interrupt bit(s) enable
          101b: CPSIE {A}{I}{F},#<mode> ;interrupt bit(s) enable & change mode
          110b: CPSID {A}{I}{F}         ;interrupt bit(s) disable
          111b: CPSID {A}{I}{F},#<mode> ;interrupt bit(s) disable & change mode
  16-9   Must be 0000.0000b
  8      Change A flag (what?) (0=No, 1=Yes)  ;\must be all zero for CPS,
  7      Change I flag         (0=No, 1=Yes)  ; must be at least one flag set
  6      Change F flag         (0=No, 1=Yes)  ;/for CPSID/CPSIE
  5      Must be 0
  4-0    Mode (00h..1Fh) (must be 00h if bit17=0)
```



## Set Endianness for Data Access (SETEND)


```
  31-10  Fixed, 1111000100000001000000b
  9      Opcode/operand
          0: SETEND LE  ;clears the CPSR E bit
          1: SETEND BE  ;sets the CPSR E bit
  8-0    Fixed, 000000000b
```

Changes the byte order for data access to Big/Little endian. Does probably not
affect opcode fetches, but probably affect all other memory including push/pop?
Or affects "LDR" only?

Supported on ARMv6 and above.



## Store Return State (SRS) (stores the R14 and SPSR of the current mode)


to the word at the specified address and the following word respectively. The
address is determined from the banked version of R13 belonging to a specified
mode.

```
  Syntax
  SRS<addressing_mode> #<mode>{!}   ;<-- original (pre-UAL)
  SRS<addressing_mode> sp!,#<mode>  ;<-- more common (UAL)
```

\<addressing\_mode>: Is similar to the \<addressing\_mode> in LDM and
STM instructions, see Addressing Mode 4 - Load and Store Multiple on page
A5-41, but with the following differences:

- The base register, Rn, is the banked version of R13 for the mode specified by
\<mode>, rather than the current mode.

- The number of registers to store is 2.

- The register list is {R14, SPSR}, with both R14 and the SPSR being the
versions belonging to the current mode.

\<mode> Specifies the number of the mode whose banked register is used as
the base register for \<addressing\_mode>. The mode number is the 5-bit
encoding of the chosen mode in a PSR, as described in The mode bits on page
A2-14.

! If present, sets the W bit. This causes the instruction to write a modified
value back to its base register, in a manner similar to that specified for
Addressing Mode 4 - Load and Store Multiple on page A5-41. If ! is omitted, the
W bit is 0 and the instruction does not change the base register.

```
  31-25  Fixed, 1111100b
  24     P
  23     U
  22     Fixed, 1
  21     W
  20     Fixed, 0 (store)
  19-5   Fixed, 110100000101000b
  4-0    mode
```

Supported on ARMv6 and above.



## Return From Exception (RFE) (RFEDA, RFEDB, RFEIA, RFEIB)


loads the PC and the CPSR from the word at the specified address and the

following word respectively.

```
  Syntax
  RFE<addressing_mode> Rn{!}
```

\<addressing\_mode>: Is similar to the \<addressing\_mode> in LDM and
STM instructions, see Addressing Mode 4 - Load and Store Multiple on page
A5-41, but with the following differences:

- The number of registers to load is 2.

- The register list is {PC, CPSR}.

\<Rn> Specifies the base register to be used by \<addressing\_mode>.
If R15 is specified as the base register, the result is UNPREDICTABLE.

! If present, sets the W bit. This causes the instruction to write a modified
value back to its base register, in a manner similar to that specified for
Addressing Mode 4 - Load and Store Multiple on page A5-41. If ! is omitted, the
W bit is 0 and the instruction does not change the base register.

While RFE supports different base registers, a general usage case is where Rn
== sp (the stack pointer), held in R13. The instruction can then be used as the
return method associated with instructions SRS and CPS. See New instructions to
improve exception handling on page A2-28 for more details.

```
  31-25  Fixed, 1111100b
  24     P
  23     U
  22     Fixed, 0
  21     W
  20     Fixed, 1 (load)
  19-16  Rn
  15-0   Fixed, 0000101000000000b
```

Supported on ARMv6 and above.



## SVC (formerly SWI)


SWI was renamed to SVC in new UAL syntax.



## CPY


CPY Rd,Rm is an alias for MOV Rd,Rm (without flags affected).

```
  31-28  Condition
  27-16  Fixed, 000110100000
  15-12  Rd - Destination Register
  11-4   Fixed, 00000000
  3-0    Rm
```

The alias was invented in ARMv6 for THUMB and ARM code (apparently because
pre-UAL THUMB syntax couldn't distinguish between MOV and MOVS) (ie. in ARM
it's useless, but in THUMB it does allow using a formerly undocumented opcode).

ARMv6 and above.




# <a name="armopcodesspecialarm11instructionssimd"></a>ARM Opcodes: Special ARM11 Instructions (SIMD)




## Sign Extend / Zero Extend, with optional Addition (SXT, UXT)


```
  31-28  Condition
  27-23  Fixed, 01101b
  22-20  Opcode:
          0: SXTAB16{cond} Rd, Rn, Rm{,ROR #imm} ;\
          2: SXTAB{cond}   Rd, Rn, Rm{,ROR #imm} ;
          3: SXTAH{cond}   Rd, Rn, Rm{,ROR #imm} ; with add
          4: UXTAB16{cond} Rd, Rn, Rm{,ROR #imm} ;
          6: UXTAB{cond}   Rd, Rn, Rm{,ROR #imm} ;
          7: UXTAH{cond}   Rd, Rn, Rm{,ROR #imm} ;/
          0: SXTB16{cond}  Rd,     Rm{,ROR #imm} ;\
          2: SXTB{cond}    Rd,     Rm{,ROR #imm} ;
          3: SXTH{cond}    Rd,     Rm{,ROR #imm} ; without add
          4: UXTB16{cond}  Rd,     Rm{,ROR #imm} ;
          6: UXTB{cond}    Rd,     Rm{,ROR #imm} ;
          7: UXTH{cond}    Rd,     Rm{,ROR #imm} ;/
  19-16  Rn - Addition Register (or 1111b when no addition)            (R0-R14)
  15-12  Rd - Destination Register                                     (R0-R14)
  11-10  Rotation (0=None/ROR#0, 1=ROR#8, 2=ROR#16, 3=ROR#24)
  9-4    Fixed, 000111b
  3-0    Rm - Source register (to be rotated/extracted from)           (R0-R14)
```

Supported on ARMv6 and above.



## Select Bytes from 1st/2nd operand, depending on GE flags (SEL)


Syntax

```
  SEL{cond} Rd, Rn, Rm
```

Operation

if ConditionPassed(cond) then

Rd[7:0] = if GE[0] == 1 then Rn[7:0] else Rm[7:0]

Rd[15:8] = if GE[1] == 1 then Rn[15:8] else Rm[15:8]

Rd[23:16] = if GE[2] == 1 then Rn[23:16] else Rm[23:16]

Rd[31:24] = if GE[3] == 1 then Rn[31:24] else Rm[31:24]

Usage

Use SEL after instructions such as SADD8, SADD16, SSUB8, SSUB16, UADD8, UADD16,
USUB8, USUB16, SADDSUBX, SSUBADDX, UADDSUBX and USUBADDX, that set the GE
flags. For example, the following sequence of instructions sets each byte of Rd
equal to the unsigned minimum of the corresponding bytes of Ra and Rb:

```
  USUB8 Rd, Ra, Rb
  SEL Rd, Rb, Ra
```

Notes

Specifying R15 for register Rd, Rm, or Rn has UNPREDICTABLE results.

```
  31-28  Condition
  27-20  Fixed, 01101000b
  19-16  Rn - First Source Register (8bit snippets used when GE bits = 1)
  15-12  Rd - Destination Register
  11-4   Fixed, 11111011b
  3-0    Rm - Second Source Register (8bit snippets used when GE bits = 0)
```

ARMv6 and above.



## SADD16


SADD16 (Signed Add) performs two 16-bit signed integer additions. It sets the
GE bits in the CPSR according to the results of the additions.

ARMv6 and above.

Operation

sum = Rn[15:0] + Rm[15:0] /\* Signed addition \*/

Rd[15:0] = sum[15:0]

GE[1:0] = if sum >= 0 then 0b11 else 0

sum = Rn[31:16] + Rm[31:16] /\* Signed addition \*/

Rd[31:16] = sum[15:0]

GE[3:2] = if sum >= 0 then 0b11 else 0

```
  31-28  Condition
  27-23  Fixed, 01100b
  22-20  Opcode, part 1:
          001b: for Sxx           (signed, with GE flags)
          010b: for Qxx           (signed, with saturation)
          011b: for SHxx          (signed, with halving)
          101b: for Uxx           (unsigned, with GE flags)
          110b: for UQxx          (unsigned, with saturation)
          111b: for UHxx          (unsigned, with halving)
  19-16  Rn - First Source Register                                    (R0-R14)
  15-12  Rd - Destination Register                                     (R0-R14)
  11-8   Fixed, 1111b
  7-5    Opcode, part 2:
          000b: xxADD16{cond}   Rd, Rn, Rm
          001b: xxADDSUBX{cond} Rd, Rn, Rm
          010b: xxSUBADDX{cond} Rd, Rn, Rm
          011b: xxSUB16{cond}   Rd, Rn, Rm
          100b: xxADD8{cond}    Rd, Rn, Rm
          111b: xxSUB8{cond}    Rd, Rn, Rm
  4      Fixed, 1
  3-0    Rm - Second Source register                                   (R0-R14)
```



## SMLAD


SMLAD (Signed Multiply Accumulate Dual) performs two signed 16 x 16-bit
multiplications. It adds the products to a 32-bit accumulate operand.

Optionally, you can exchange the halfwords of the second operand before
performing the arithmetic. This produces top x bottom and bottom x top
multiplication.

This instruction sets the Q flag if the accumulate operation overflows.
Overflow cannot occur during the multiplications.

X Sets the X bit of the instruction to 1, and the multiplications are bottom x
top and top x bottom.

If the X is omitted, sets the X bit to 0, and the multiplications are bottom x
bottom and top x top.

\<Rm> Specifies the register that contains the first operand.

\<Rs> Specifies the register that contains the second operand.

\<Rn> Specifies the register that contains the accumulate operand.

```
  31-28  Condition
  27-23  Fixed, 01110
  22-20  Opcode part 1 (part2 in bit7-6)
         Merged part1.part2:
           000.00: SMUAD{X}{cond}  Rd,Rm,Rs
           000.00: SMLAD{X}{cond}  Rd,Rm,Rs,Rn
           000.01: SMUSD{X}{cond}  Rd,Rm,Rs
           000.01: SMLSD{X}{cond}  Rd,Rm,Rs,Rn
           100.00: SMLALD{X}{cond} RdLo,RdHi,Rm,Rs
           100.01: SMLSLD{X}{cond} RdLo,RdHi,Rm,Rs
           101.00: SMMUL{R}{cond}  Rd,Rm,Rs        ;Rd=(Rm*Rs)/100000000h
           101.00: SMMLA{R}{cond}  Rd,Rm,Rs,Rn     ;Rd=(Rm*Rs)/100000000h+Rz
           101.11: SMMLS{R}{cond}  Rd,Rm,Rs,Rn     ;Rd=(Rm*Rs)/100000000h-Rz
  19-16  Rd or RdHi - Destination Register
  15-12  Rn or RdLo or unused (must be 0Fh for SMUAD/SMUSD/SMMUL)
  11-8   Rs
  7-6    Opcode part 2 (part1 in bit22-20, see there)
  5      X or R flag   (X=Exchange, R=Rounded)
  4      Fixed, 1
  3-0    Rm
```

ARMv6 and above.


## Most significant word multiply


There are three signed 32-bit x 32-bit Multiply instructions that produce top
32-bit results:

```
  SMMUL Multiplies the 32-bit values of two registers together, and stores
        the top 32 bits of the signed 64-bit result in a third register.
  SMMLA Multiplies the 32-bit values of two registers together, extracts the
        top 32 bits, adds the 32-bit value from a third register, and stores
        the signed 32-bit result in a fourth register.
  SMMLS Multiplies the 32-bit value of two registers together, extracts the
        top 32 bits, subtracts this from a 32-bit value from a third
        register, and stores the signed 32-bit result in a fourth register.
```

The above three instructions do not affect any flags.


## Dual halfword multiply


There are six dual, signed 16-bit x 16-bit Multiply instructions:

```
  SMUAD Multiplies the values of the top halfwords of two registers together,
        multiplies the values of the bottom halfwords of the same two
        registers together, adds the products, and stores the 32-bit result
        in a third register.
  SMUSD Multiplies the values of the top halfwords of two registers together,
        multiplies the values of the bottom halfwords of the same two
        registers together, subtracts one product from the other, and stores
        the 32-bit result in a third register.
  ---(descriptions below seem to be bugged, seem to describe 32bit SMMxx)---
  SMLAD Multiplies the 32-bit value of two registers together, extracts the
        top 32 bits, subtracts this from a 32-bit value from a third
        register, and stores the signed 32-bit result in a fourth register.
  SMLSD Multiplies the 32-bit values of two registers together, extracts the
        top 32 bits, adds the 32-bit value from a third register, and stores
        the signed 32-bit result in a fourth register.
  SMLALD Multiplies the 32-bit value of two registers together, extracts the
         top 32 bits, subtracts this from a 32-bit value from a third
         register, and stores the signed 32-bit result in a fourth register.
  SMLSLD Multiplies the 32-bit value of two registers together, extracts the
         top 32 bits, subtracts this from a 32-bit value from a third
         register, and stores the signed 32-bit result in a fourth register.
```

SMUAD, SMLAD, and SMLSLD can set the Q flag if overflow occurs in the
operation. All other instructions do not affect any flags.



## Signed/Unsigned Saturate (SSAT, USAT)


Saturates a 32bit value (with optional shift) to the specified range (of 1..32
bits).

```
  31-28  Condition
  27-21  Fixed, 0110101b: SSAT{cond} Rd, #<range>, Rm {,LSL/ASR #imm}
                0110111b: USAT{cond} Rd, #<range>, Rm {,LSL/ASR #imm}
  20-16  Immediate: range-1 (0..31 = Saturate to 1..32 bits range)
  15-12  Rd - Destination Register
  11-7   shift_imm (1..31 for LSL/ASR#1..31, or 0 for LSL#0, or 0 for ASR#32)
  6      Shift type (0=LSL, 1=ASR) (note: really ASR, even "unsigned" USAT)
  5-4    Fixed, 01b
  3-0    Rm - Source Register
```

The Q flag is set if the operation saturates.

ARMv6 and above.



## Signed/Unsigned Saturate on Halfwords (SSAT16, USAT16)


Saturates two 16bit values to the specified range (of 1..16 bits).

```
  31-28  Condition
  27-21  Fixed, 0110101b: SSAT16{cond} Rd, #<range>, Rm
                0110111b: USAT16{cond} Rd, #<range>, Rm
  20     Fixed, 0
  19-16  Immediate: range-1 (0..15 = Saturate to 1..16 bits range)
  15-12  Rd - Destination Register
  11-4   Fixed, 11110011b
  3-0    Rm - Source Register
```

The Q flag is set if either halfword operation saturates.

ARMv6 and above.



## Unsigned Sum of Absolute Differences, optional Accumulate (USAD8, USADA8)


Performs four unsigned 8bit subtractions, and adds the absolute values of the
differences together.

```
  31-28  Condition
  27-20  Fixed, 01111000b
  19-16  Rd - Destination Register
  15-12  Rn (if any) (otherwise fixed, 1111)
           USAD8{cond}  Rd, Rm, Rs        ;without Rn   ;Rd=sum
           USADA8{cond} Rd, Rm, Rs, Rn    ;with Rn      ;Rd=sum + Rn
  11-8   Rs
  7-4    Fixed, 0001b
  3-0    Rm
```

ARMv6 and above.



## Pack Halfword, Bottom-Top or Top-Bottom (PKHBT, PKHTB)


PKHBT (Pack Halfword Bottom Top) combines the bottom (least significant)
halfword of its first operand with the top (most significant) halfword of its
shifted second operand. The shift is a left shift, by any amount from 0 to 31.

\<Rn> Specifies the register that contains the first operand. Bits[15:0]
of this operand become bits[15:0] of the result of the operation.

\<Rm> Specifies the register that contains the second operand. This is
shifted left by the specified amount, then bits[31:16] of this operand become
bits[31:16] of the result of the operation.

\<shift\_imm> Specifies the amount by which Rm is to be shifted left. This
is a value from 0 to 31.

If the shift specifier is omitted, a left shift by 0 is used.

Operation

if ConditionPassed(cond) then

Rd[15:0] = Rn[15:0]

Rd[31:16] = (Rm Logical\_Shift\_Left shift\_imm)[31:16]

```
  31-28  Condition
  27-20  Fixed, 01101000b
  19-16  Rn
  15-12  Rd - Destination Register
  11-7   shift_imm (0..31)
  6-4    Fixed, 001  PKHBT {cond} Rd, Rn, Rm {, LSL #<shift_imm>}
                101  PKHTB {cond} Rd, Rn, Rm {, ASR #<shift_imm>}
  3-0    Rm
```

ARMv6 and above.



## Reverse Byte Order for Big/Little-Endian Conversion (REV, REV16, REVSH)


Specifying R15 for register Rd or Rm has UNPREDICTABLE results.

```
  31-28  Condition
  27-23  Fixed, 01101b
  22     Opcode (with bit7)
          Bit22=0, Bit7=0: REV{cond}   Rd, Rm   ;reverse 32bit
          Bit22=0, Bit7=1: REV16{cond} Rd, Rm
          Bit22=1, Bit7=1: REVSH{cond} Rd, Rm
  21-16  Fixed, 111111b
  15-12  Rd - Destination Register
  11-8   Fixed, 1111b
  7      Opcode (with bit22, see there)
  6-4    Fixed, 011b
  3-0    Rm - Source Register
```

ARMv6 and above.




# <a name="armopcodespsrtransfermrsmsr"></a>ARM Opcodes: PSR Transfer (MRS, MSR)




## Opcode Format


These instructions occupy an unused area (TEQ,TST,CMP,CMN with S=0) of ALU
opcodes.

```
  Bit    Expl.
  31-28  Condition
  27-26  Must be 00b for this instruction
  25     I - Immediate Operand Flag  (0=Register, 1=Immediate) (Zero for MRS)
  24-23  Must be 10b for this instruction
  22     Psr - Source/Destination PSR  (0=CPSR, 1=SPSR_<current mode>)
  21     Opcode
           0: MRS{cond} Rd,Psr          ;Rd = Psr
           1: MSR{cond} Psr{_field},Op  ;Psr[field] = Op
  20     Must be 0b for this instruction (otherwise TST,TEQ,CMP,CMN)
  For MRS:
    19-16   Must be 1111b for this instruction (otherwise SWP)
    15-12   Rd - Destination Register  (R0-R14)
    11-0    Not used, must be zero.
  For MSR:
    19      f  write to flags field     Bit 31-24 (aka _flg)
    18      s  write to status field    Bit 23-16 (reserved, don't change)
    17      x  write to extension field Bit 15-8  (reserved, don't change)
    16      c  write to control field   Bit 7-0   (aka _ctl)
    15-12   Not used, must be 1111b.
  For MSR Psr,Rm (I=0)
    11-4    Not used, must be zero. (otherwise BX)
    3-0     Rm - Source Register <op>  (R0-R14)
  For MSR Psr,Imm (I=1)
    11-8    Shift applied to Imm   (ROR in steps of two 0-30)
    7-0     Imm - Unsigned 8bit Immediate
    In source code, a 32bit immediate should be specified as operand.
    The assembler should then convert that into a shifted 8bit value.
```

MSR/MRS and CPSR/SPSR supported by ARMv3 and up.

ARMv2 and below contained PSR flags in R15, accessed by CMP/CMN/TST/TEQ{P}.

The field mask bits specify which bits of the destination Psr are write-able
(or write-protected), one or more of these bits should be set, for example,
CPSR\_fsxc (aka CPSR aka CPSR\_all) unlocks all bits (see below user mode
restriction though).

Restrictions:

In non-privileged mode (user mode): only condition code bits of CPSR can be
changed, control bits can't.

Only the SPSR of the current mode can be accessed; In User and System modes no
SPSR exists.

The T-bit may not be changed; for THUMB/ARM switching use BX instruction.

Unused Bits in CPSR are reserved for future use and should never be changed
(except for unused bits in the flags field).

Execution Time: 1S.


Note: The A22i assembler recognizes MOV as alias for both MSR and MRS because
it is practically not possible to remember whether MSR or MRS was the load or
store opcode, and/or whether it does load to or from the Psr register.




# <a name="armopcodesmemorysingledatatransferldrstrpld"></a>ARM Opcodes: Memory: Single Data Transfer (LDR, STR, PLD)




## Opcode Format


```
  Bit    Expl.
  31-28  Condition (Must be 1111b for PLD)
  27-26  Must be 01b for this instruction
  25     I - Immediate Offset Flag (0=Immediate, 1=Shifted Register)
  24     P - Pre/Post (0=post; add offset after transfer, 1=pre; before trans.)
  23     U - Up/Down Bit (0=down; subtract offset from base, 1=up; add to base)
  22     B - Byte/Word bit (0=transfer 32bit/word, 1=transfer 8bit/byte)
  When above Bit 24 P=0 (Post-indexing, write-back is ALWAYS enabled):
    21     T - Memory Management (0=Normal, 1=Force non-privileged access)
  When above Bit 24 P=1 (Pre-indexing, write-back is optional):
    21     W - Write-back bit (0=no write-back, 1=write address into base)
  20     L - Load/Store bit (0=Store to memory, 1=Load from memory)
          0: STR{cond}{B}{T} Rd,<Address>   ;[Rn+/-<offset>]=Rd
          1: LDR{cond}{B}{T} Rd,<Address>   ;Rd=[Rn+/-<offset>]
         (1: PLD <Address> ;Prepare Cache for Load, see notes below)
          Whereas, B=Byte, T=Force User Mode (only for POST-Indexing)
  19-16  Rn - Base register               (R0..R15) (including R15=PC+8)
  15-12  Rd - Source/Destination Register (R0..R15) (including R15=PC+12)
  When above I=0 (Immediate as Offset)
    11-0   Unsigned 12bit Immediate Offset (0-4095, steps of 1)
  When above I=1 (Register shifted by Immediate as Offset)
    11-7   Is - Shift amount      (1-31, 0=Special/See below)
    6-5    Shift Type             (0=LSL, 1=LSR, 2=ASR, 3=ROR)
    4      Must be 0 (Reserved, see The Undefined Instruction)
    3-0    Rm - Offset Register   (R0..R14) (not including PC=R15)
```



## Instruction Formats for \<Address>


An expression which generates an address:

```
  <expression>                  ;an immediate used as address
  ;*** restriction: must be located in range PC+/-4095+8, if so,
  ;*** assembler will calculate offset and use PC (R15) as base.
```

Pre-indexed addressing specification:

```
  [Rn]                          ;offset = zero
  [Rn, <#{+/-}expression>]{!}   ;offset = immediate
  [Rn, {+/-}Rm{,<shift>} ]{!}   ;offset = register shifted by immediate
```

Post-indexed addressing specification:

```
  [Rn], <#{+/-}expression>      ;offset = immediate
  [Rn], {+/-}Rm{,<shift>}       ;offset = register shifted by immediate
```

Whereas...

```
  <shift>  immediate shift such like LSL#4, ROR#2, etc. (see ALU opcodes).
  {!}      exclamation mark ("!") indicates write-back (Rn will be updated).
```



## Notes


Shift amount 0 has special meaning, as described for ALU opcodes.

When writing a word (32bit) to memory, the address should be word-aligned.

When reading a byte from memory, upper 24 bits of Rd are zero-extended.

LDR PC,\<op> on ARMv4 leaves CPSR.T unchanged.

LDR PC,\<op> on ARMv5 sets CPSR.T to \<op> Bit0, (1=Switch to Thumb).


When reading a word from a halfword-aligned address (which is located in the
middle between two word-aligned addresses), the lower 16bit of Rd will contain
[address] ie. the addressed halfword, and the upper 16bit of Rd will contain
[Rd-2] ie. more or less unwanted garbage. However, by isolating lower bits this
may be used to read a halfword from memory. (Above applies to little endian
mode, as used in GBA.)


In a virtual memory based environment (ie. not in the GBA), aborts (ie. page
faults) may take place during execution, if so, Rm and Rn should not specify
the same register when post-indexing is used, as the abort-handler might have
problems to reconstruct the original value of the register.


Return: CPSR flags are not affected.

Execution Time: For normal LDR: 1S+1N+1I. For LDR PC: 2S+2N+1I. For STR: 2N.



## PLD \<Address> ;Prepare Cache for Load


PLD must use following settings cond=1111b, P=1, B=1, W=0, L=1, Rd=1111b, the
address may not use post-indexing, and may not use writeback, the opcode is
encoded identical as LDRNVB R15,\<Address>.

PLD signalizes to the memory system that a specific memory address will be soon
accessed, the memory system may use this hint to prepare caching/pipelining,
aside from that, PLD does not have any affect to the program logic, and behaves
identical as NOP.

PLD supported by ARMv5TE and up only, not ARMv5, not ARMv5TExP.




# <a name="armopcodesmemoryhalfworddoublewordandsigneddatatransfer"></a>ARM Opcodes: Memory: Halfword, Doubleword, and Signed Data Transfer




## Opcode Format


```
  Bit    Expl.
  31-28  Condition
  27-25  Must be 000b for this instruction
  24     P - Pre/Post (0=post; add offset after transfer, 1=pre; before trans.)
  23     U - Up/Down Bit (0=down; subtract offset from base, 1=up; add to base)
  22     I - Immediate Offset Flag (0=Register Offset, 1=Immediate Offset)
  When above Bit 24 P=0 (Post-indexing, write-back is ALWAYS enabled):
    21     Not used, must be zero (0)
  When above Bit 24 P=1 (Pre-indexing, write-back is optional):
    21     W - Write-back bit (0=no write-back, 1=write address into base)
  20     L - Load/Store bit (0=Store to memory, 1=Load from memory)
  19-16  Rn - Base register                (R0-R15) (Including R15=PC+8)
  15-12  Rd - Source/Destination Register  (R0-R15) (Including R15=PC+12)
  11-8   When above Bit 22 I=0 (Register as Offset):
           Not used. Must be 0000b
         When above Bit 22 I=1 (immediate as Offset):
           Immediate Offset (upper 4bits)
  7      Reserved, must be set (1)
  6-5    Opcode (0-3)
         When Bit 20 L=0 (Store) (and Doubleword Load/Store):
          0: Reserved for SWP instruction
          1: STR{cond}H  Rd,<Address>  ;Store halfword   [a]=Rd
          2: LDR{cond}D  Rd,<Address>  ;Load Doubleword  R(d)=[a], R(d+1)=[a+4]
          3: STR{cond}D  Rd,<Address>  ;Store Doubleword [a]=R(d), [a+4]=R(d+1)
         When Bit 20 L=1 (Load):
          0: Reserved.
          1: LDR{cond}H  Rd,<Address>  ;Load Unsigned halfword (zero-extended)
          2: LDR{cond}SB Rd,<Address>  ;Load Signed byte (sign extended)
          3: LDR{cond}SH Rd,<Address>  ;Load Signed halfword (sign extended)
  4      Reserved, must be set (1)
  3-0    When above Bit 22 I=0:
           Rm - Offset Register            (R0-R14) (not including R15)
         When above Bit 22 I=1:
           Immediate Offset (lower 4bits)  (0-255, together with upper bits)
```

STRH,LDRH,LDRSB,LDRSH supported on ARMv4 and up.

STRD/LDRD supported on ARMv5TE and up only, not ARMv5, not ARMv5TExP.

STRD/LDRD: base writeback: Rn should not be same as R(d) or R(d+1).

STRD: index register: Rm should not be same as R(d) or R(d+1).

STRD/LDRD: Rd must be an even numbered register (R0,R2,R4,R6,R8,R10,R12).

STRD/LDRD: Address must be double-word aligned (multiple of eight).



## Instruction Formats for \<Address>


An expression which generates an address:

```
  <expression>                  ;an immediate used as address
  ;*** restriction: must be located in range PC+/-255+8, if so,
  ;*** assembler will calculate offset and use PC (R15) as base.
```

Pre-indexed addressing specification:

```
  [Rn]                          ;offset = zero
  [Rn, <#{+/-}expression>]{!}   ;offset = immediate
  [Rn, {+/-}Rm]{!}              ;offset = register
```

Post-indexed addressing specification:

```
  [Rn], <#{+/-}expression>      ;offset = immediate
  [Rn], {+/-}Rm                 ;offset = register
```

Whereas...

```
  {!}      exclamation mark ("!") indicates write-back (Rn will be updated).
```


Return: No Flags affected.

Execution Time: For Normal LDR, 1S+1N+1I. For LDR PC, 2S+2N+1I. For STRH 2N.




# <a name="armopcodesmemoryblockdatatransferldmstm"></a>ARM Opcodes: Memory: Block Data Transfer (LDM, STM)




## Opcode Format


```
  Bit    Expl.
  31-28  Condition
  27-25  Must be 100b for this instruction
  24     P - Pre/Post (0=post; add offset after transfer, 1=pre; before trans.)
  23     U - Up/Down Bit (0=down; subtract offset from base, 1=up; add to base)
  22     S - PSR & force user bit (0=No, 1=load PSR or force user mode)
  21     W - Write-back bit (0=no write-back, 1=write address into base)
  20     L - Load/Store bit (0=Store to memory, 1=Load from memory)
          0: STM{cond}{amod} Rn{!},<Rlist>{^}  ;Store (Push)
          1: LDM{cond}{amod} Rn{!},<Rlist>{^}  ;Load  (Pop)
          Whereas, {!}=Write-Back (W), and {^}=PSR/User Mode (S)
  19-16  Rn - Base register                (R0-R14) (not including R15)
  15-0   Rlist - Register List
  (Above 'offset' is meant to be the number of words specified in Rlist.)
```

Return: No Flags affected.

Execution Time: For normal LDM, nS+1N+1I. For LDM PC, (n+1)S+2N+1I. For STM
(n-1)S+2N. Where n is the number of words transferred.



## Addressing Modes {amod}


The IB,IA,DB,DA suffixes directly specify the desired U and P bits:

```
  IB  increment before          ;P=1, U=1
  IA  increment after           ;P=0, U=1
  DB  decrement before          ;P=1, U=0
  DA  decrement after           ;P=0, U=0
```

Alternately, FD,ED,FA,EA could be used, mostly to simplify mnemonics for stack
transfers.

```
  ED  empty stack, descending   ;LDM: P=1, U=1  ;STM: P=0, U=0
  FD  full stack,  descending   ;     P=0, U=1  ;     P=1, U=0
  EA  empty stack, ascending    ;     P=1, U=0  ;     P=0, U=1
  FA  full stack,  ascending    ;     P=0, U=0  ;     P=1, U=1
```

Ie. the following expressions are aliases for each other:

```
  STMFD=STMDB=PUSH   STMED=STMDA   STMFA=STMIB   STMEA=STMIA
  LDMFD=LDMIA=POP    LDMED=LDMIB   LDMFA=LDMDA   LDMEA=LDMDB
```

Note: The equivalent THUMB functions use fixed organization:

```
  PUSH/POP: full descending     ;base register SP (R13)
  LDM/STM:  increment after     ;base register R0..R7
```

Descending is common stack organization as used in 80x86 and Z80 CPUs, SP is
decremented when pushing/storing data, and incremented when popping/loading
data.



## When S Bit is set (S=1)


If instruction is LDM and R15 is in the list: (Mode Changes)

```
  While R15 loaded, additionally: CPSR=SPSR_<current mode>
```

Otherwise: (User bank transfer)

```
  Rlist is referring to User Bank Registers, R0-R15 (rather than
  register related to the current mode, such like R14_svc etc.)
  Base write-back should not be used for User bank transfer.
  Caution - When instruction is LDM:
  If the following instruction reads from a banked register (eg. R14_svc),
  then CPU might still read R14 instead; if necessary insert a dummy NOP.
```



## Notes


The base address should be usually word-aligned.

LDM Rn,...,PC on ARMv4 leaves CPSR.T unchanged.

LDR Rn,...,PC on ARMv5 sets CPSR.T to \<op> Bit0, (1=Switch to Thumb).



## Transfer Order


The lowest Register in Rlist (R0 if its in the list) will be loaded/stored
to/from the lowest memory address.

Internally, the rlist register are always processed with INCREASING addresses
(ie. for DECREASING addressing modes, the CPU does first calculate the lowest
address, and does then process rlist with increasing addresses; this detail can
be important when accessing memory mapped I/O ports).



## Strange Effects on Invalid Rlist's


Empty Rlist: R15 loaded/stored (ARMv4 only), and Rb=Rb+/-40h (ARMv4-v5).

Writeback with Rb included in Rlist: Store OLD base if Rb is FIRST entry in
Rlist, otherwise store NEW base (STM/ARMv4), always store OLD base (STM/ARMv5),
no writeback (LDM/ARMv4), writeback if Rb is "the ONLY register, or NOT the
LAST register" in Rlist (LDM/ARMv5).




# <a name="armopcodesmemorysingledataswapswp"></a>ARM Opcodes: Memory: Single Data Swap (SWP)




## Opcode Format


```
  Bit    Expl.
  31-28  Condition
  27-23  Must be 00010b for this instruction
         Opcode (fixed)
           SWP{cond}{B} Rd,Rm,[Rn]      ;Rd=[Rn], [Rn]=Rm
  22     B - Byte/Word bit (0=swap 32bit/word, 1=swap 8bit/byte)
  21-20  Must be 00b for this instruction
  19-16  Rn - Base register                     (R0-R14)
  15-12  Rd - Destination Register              (R0-R14)
  11-4   Must be 00001001b for this instruction
  3-0    Rm - Source Register                   (R0-R14)
```

SWP/SWPB supported by ARMv2a and up. SWP/SWPB has been deprecated in ARMv6,
made optional in ARMv7 (with the possibility of disabling it if still
available), and removed in ARMv8.

Swap works properly including if Rm and Rn specify the same register.

R15 may not be used for either Rn,Rd,Rm. (Rn=R15 would be MRS opcode).

Upper bits of Rd are zero-expanded when using Byte quantity. For info about
byte and word data memory addressing, read LDR and STR opcode description.

Execution Time: 1S+2N+1I. That is, 2N data cycles, 1S code cycle, plus 1I.




# <a name="armopcodescoprocessorinstructionsmrcmcrldcstccdpmcrrmrrc"></a>ARM Opcodes: Coprocessor Instructions (MRC/MCR, LDC/STC, CDP, MCRR/MRRC)




## Coprocessor Register Transfers (MRC, MCR) (with ARM Register read/write)


```
  Bit    Expl.
  31-28  Condition (or 1111b for MRC2/MCR2 opcodes on ARMv5 and up)
  27-24  Must be 1110b for this instruction
  23-21  CP Opc - Coprocessor operation code         (0-7)
  20     ARM-Opcode (0-1)
          0: MCR{cond} Pn,<cpopc>,Rd,Cn,Cm{,<cp>}   ;move from ARM to CoPro
          0: MCR2      Pn,<cpopc>,Rd,Cn,Cm{,<cp>}   ;move from ARM to CoPro
          1: MRC{cond} Pn,<cpopc>,Rd,Cn,Cm{,<cp>}   ;move from CoPro to ARM
          1: MRC2      Pn,<cpopc>,Rd,Cn,Cm{,<cp>}   ;move from CoPro to ARM
  19-16  Cn     - Coprocessor source/dest. Register  (C0-C15)
  15-12  Rd     - ARM source/destination Register    (R0-R15)
  11-8   Pn     - Coprocessor number                 (P0-P15)
  7-5    CP     - Coprocessor information            (0-7)
  4      Reserved, must be one (1) (otherwise CDP opcode)
  3-0    Cm     - Coprocessor operand Register       (C0-C15)
```

MCR/MRC supported by ARMv2 and up, MCR2/MRC2 by ARMv5 and up.

A22i syntax allows to use MOV with Rd specified as first (dest), or last
(source) operand. Native MCR/MRC syntax uses Rd as middle operand, \<cp>
can be ommited if \<cp> is zero.

When using MCR with R15: Coprocessor will receive a data value of PC+12.

When using MRC with R15: Bit 31-28 of data are copied to Bit 31-28 of CPSR (ie.
N,Z,C,V flags), other data bits are ignored, CPSR Bit 27-0 are not affected,
R15 (PC) is not affected.

Execution time: 1S+bI+1C for MCR, 1S+(b+1)I+1C for MRC.

Return: For MRC only: Either R0-R14 modified, or flags affected (see above).

For details refer to original ARM docs. The opcodes are irrelevant for GBA/NDS7
because no coprocessor exists (except for a dummy CP14 unit). However, NDS9
includes a working CP15 unit. And, 3DS ARM11 uses CP10/CP11 as VFP floating
point unit.

- [ARM CP14 ICEbreaker Debug Communications Channel](#armcp14icebreakerdebugcommunicationschannel)
- [ARM CP15 System Control Coprocessor](#armcp15systemcontrolcoprocessor)
- [ARM Vector Floating-point Unit (VFP)](#armvectorfloatingpointunitvfp)


## Coprocessor Data Transfers (LDC, STC) (with Memory read/write)


```
  Bit    Expl.
  31-28  Condition (or 1111b for LDC2/STC2 opcodes on ARMv5 and up)
  27-25  Must be 110b for this instruction
  24     P - Pre/Post (0=post; add offset after transfer, 1=pre; before trans.)
  23     U - Up/Down Bit (0=down; subtract offset from base, 1=up; add to base)
  22     N - Transfer length (0-1, interpretation depends on co-processor)
  21     W - Write-back bit (0=no write-back, 1=write address into base)
  20     Opcode (0-1)
          0: STC{cond}{L} Pn,Cd,<Address>  ;Store to memory (from coprocessor)
          0: STC2{L}      Pn,Cd,<Address>  ;Store to memory (from coprocessor)
          1: LDC{cond}{L} Pn,Cd,<Address>  ;Read from memory (to coprocessor)
          1: LDC2{L}      Pn,Cd,<Address>  ;Read from memory (to coprocessor)
          whereas {L} indicates long transfer (Bit 22: N=1)
  19-16  Rn     - ARM Base Register              (R0-R15)     (R15=PC+8)
  15-12  Cd     - Coprocessor src/dest Register  (C0-C15)
  11-8   Pn     - Coprocessor number             (P0-P15)
  7-0    Offset - Unsigned Immediate, step 4     (0-1020, in steps of 4)
```

LDC/STC supported by ARMv2 and up, LDC2/STC2 by ARMv5 and up.

Execution time: (n-1)S+2N+bI, n=number of words transferred.

For details refer to original ARM docs, irrelevant in GBA because no
coprocessor exists.



## Coprocessor Data Operations (CDP) (without Memory or ARM Register operand)


```
  Bit    Expl.
  31-28  Condition (or 1111b for CDP2 opcode on ARMv5 and up)
  27-24  Must be 1110b for this instruction
         ARM-Opcode (fixed)
           CDP{cond} Pn,<cpopc>,Cd,Cn,Cm{,<cp>}
           CDP2      Pn,<cpopc>,Cd,Cn,Cm{,<cp>}
  23-20  CP Opc - Coprocessor operation code       (0-15)
  19-16  Cn     - Coprocessor operand Register     (C0-C15)
  15-12  Cd     - Coprocessor destination Register (C0-C15)
  11-8   Pn     - Coprocessor number               (P0-P15)
  7-5    CP     - Coprocessor information          (0-7)
  4      Reserved, must be zero (otherwise MCR/MRC opcode)
  3-0    Cm     - Coprocessor operand Register     (C0-C15)
```

CDP supported by ARMv2 and up, CDP2 by ARMv5 and up.

Execution time: 1S+bI, b=number of cycles in coprocessor busy-wait loop.

Return: No flags affected, no ARM-registers used/modified.

For details refer to original ARM docs, irrelevant in GBA because no
coprocessor exists.



## Coprocessor Double-Register Transfer (MCRR, MRRC) - ARMv5TE and up only


```
  Bit    Expl.
  31-28  Condition
  27-21  Must be 1100010b for this instruction
  20     L - Opcode (Load/Store)
          0: MCRR{cond} Pn,opcode,Rd,Rn,Cm  ;write Rd,Rn to coproc
          0: MCRR2      Pn,opcode,Rd,Rn,Cm  ;write Rd,Rn to coproc
          1: MRRC{cond} Pn,opcode,Rd,Rn,Cm  ;read Rd,Rn from coproc
          1: MRRC2      Pn,opcode,Rd,Rn,Cm  ;read Rd,Rn from coproc
  19-16  Rn - Second source/dest register      (R0-R14)
  15-12  Rd - First source/dest register       (R0-R14)
  11-8   Pn     - Coprocessor number           (P0-P15)
  7-4    CP Opc - Coprocessor operation code   (0-15)
  3-0    Cm     - Coprocessor operand Register (C0-C15)
```

MCRR/MRRC are supported by ARMv5TE and up (not ARMv5, not ARMv5TExP).

MCRR2/MRRC2 are supported by ARMv6 and up.




# <a name="thumbinstructionsummary"></a>THUMB Instruction Summary



The table below lists all THUMB mode instructions with clock cycles, affected
CPSR flags, Format/chapter number, and description.

Only register R0..R7 can be used in thumb mode (unless R8-15,SP,PC are
explicitly mentioned).



## Logical Operations


```
  Instruction           Cycles Flags Format Expl.
  MOV{S} Rd,Imm8bit      1S     NZ--  3   Rd=nn
  MOV{S} Rd,Rs           1S     NZ00  2   Rd=Rs+0       (Pre-UAL)
  CPY    Rd,Rs           1S     ----  5   Rd=Rs         (Pre-UAL, ARMv6)
  MOV    Rd,Rs           1S     ----  5   Rd=Rs         (UAL)
  MOV{S} Rd,Rs           1S     NZ--  2   Rd=Rs SHL 0   (UAL)
  MOV    R0..14,R8..15   1S     ----  5   Rd=Rs
  MOV    R8..14,R0..15   1S     ----  5   Rd=Rs
  MOV    R15,R0..15      2S+1N  ----  5   PC=Rs
  MVN{S} Rd,Rs           1S     NZ--  4   Rd=NOT Rs
  AND{S} Rd,Rs           1S     NZ--  4   Rd=Rd AND Rs
  TST    Rd,Rs           1S     NZ--  4 Void=Rd AND Rs
  BIC{S} Rd,Rs           1S     NZ--  4   Rd=Rd AND NOT Rs
  ORR{S} Rd,Rs           1S     NZ--  4   Rd=Rd OR Rs
  EOR{S} Rd,Rs           1S     NZ--  4   Rd=Rd XOR Rs
  LSL{S} Rd,Rs,Imm5bit   1S     NZc-  1   Rd=Rs SHL nn
  LSL{S} Rd,Rs           1S+1I  NZc-  4   Rd=Rd SHL (Rs AND 0FFh)
  LSR{S} Rd,Rs,Imm5bit   1S     NZc-  1   Rd=Rs SHR nn
  LSR{S} Rd,Rs           1S+1I  NZc-  4   Rd=Rd SHR (Rs AND 0FFh)
  ASR{S} Rd,Rs,Imm5bit   1S     NZc-  1   Rd=Rs SAR nn
  ASR{S} Rd,Rs           1S+1I  NZc-  4   Rd=Rd SAR (Rs AND 0FFh)
  ROR{S} Rd,Rs           1S+1I  NZc-  4   Rd=Rd ROR (Rs AND 0FFh)
  NOP                    1S     ----  5   R8=R8
```

Carry flag affected only if shift amount is non-zero.



## Arithmetic Operations and Multiply


```
  Instruction           Cycles Flags Format Expl.
  ADD{S} Rd,Rs,Imm3bit   1S     NZCV  2   Rd=Rs+nn
  ADD{S} Rd,Imm8bit      1S     NZCV  3   Rd=Rd+nn
  ADD{S} Rd,Rs,Rn        1S     NZCV  2   Rd=Rs+Rn
  ADD    Rd,Rs           1S     ----  5   Rd=Rd+Rs (UAL only)
  ADD    R0..14,R8..15   1S     ----  5   Rd=Rd+Rs
  ADD    R8..14,R0..15   1S     ----  5   Rd=Rd+Rs
  ADD    R15,R0..15      2S+1N  ----  5   PC=Rd+Rs
  ADD    Rd,PC,Imm8bit*4 1S     ---- 12   Rd=(($+4) AND NOT 2)+nn
  ADD    Rd,SP,Imm8bit*4 1S     ---- 12   Rd=SP+nn
  ADD    SP,Imm7bit*4    1S     ---- 13   SP=SP+nn
  ADD    SP,-Imm7bit*4   1S     ---- 13   SP=SP-nn
  ADC{S} Rd,Rs           1S     NZCV  4   Rd=Rd+Rs+Cy
  SUB{S} Rd,Rs,Imm3Bit   1S     NZCV  2   Rd=Rs-nn
  SUB{S} Rd,Imm8bit      1S     NZCV  3   Rd=Rd-nn
  SUB{S} Rd,Rs,Rn        1S     NZCV  2   Rd=Rs-Rn
  SUB    SP,Imm7bit*4    1S     ---- 13   SP=SP-nn  ;\alias for ADD
  SUB    SP,-Imm7bit*4   1S     ---- 13   SP=SP+nn  ;/with flipped sign
  SBC{S} Rd,Rs           1S     NZCV  4   Rd=Rd-Rs-NOT Cy
  NEG{S} Rd,Rs           1S     NZCV  4   Rd=0-Rs
  CMP    Rd,Imm8bit      1S     NZCV  3 Void=Rd-nn
  CMP    Rd,Rs           1S     NZCV  4 Void=Rd-Rs
  CMP    R0-15,R8-15     1S     NZCV  5 Void=Rd-Rs
  CMP    R8-15,R0-15     1S     NZCV  5 Void=Rd-Rs
  CMN    Rd,Rs           1S     NZCV  4 Void=Rd+Rs
  MUL{S} Rd,Rs           1S+mI  NZx-  4   Rd=Rd*Rs
```



## Jumps and Calls


```
  Instruction        Cycles    Flags Format Expl.
  B disp              2S+1N     ---- 18  PC=$+/-2048
  BL disp             3S+1N     ---- 19  PC=$+/-4M, LR=$+5
  B{cond=true} disp   2S+1N     ---- 16  PC=$+/-0..256
  B{cond=false} disp  1S        ---- 16  N/A
  BX R0..15           2S+1N     ----  5  PC=Rs, ARM/THUMB (Rs bit0)
  SWI Imm8bit         2S+1N     ---- 17  PC=8, ARM SVC mode, LR=$+2
  BKPT Imm8bit        ???       ---- 17  ??? ARM9 Prefetch Abort
  BLX disp            ???       ---- ??? ??? ARM9
  BLX R0..R14         ???       ---- ??? ??? ARM9
  POP {Rlist,}PC   (n+1)S+2N+1I ---- 14
  MOV R15,R0..15      2S+1N     ----  5  PC=Rs
  ADD R15,R0..15      2S+1N     ----  5  PC=Rd+Rs
```

The thumb BL instruction occupies two 16bit opcodes, 32bit in total.



## Memory Load/Store


```
  Instruction        Cycles    Flags Format Expl.
  LDR  Rd,[Rb,5bit*4] 1S+1N+1I  ----  9  Rd = WORD[Rb+nn]
  LDR  Rd,[PC,8bit*4] 1S+1N+1I  ----  6  Rd = WORD[PC+nn]
  LDR  Rd,[SP,8bit*4] 1S+1N+1I  ---- 11  Rd = WORD[SP+nn]
  LDR  Rd,[Rb,Ro]     1S+1N+1I  ----  7  Rd = WORD[Rb+Ro]
  LDRB Rd,[Rb,5bit*1] 1S+1N+1I  ----  9  Rd = BYTE[Rb+nn]
  LDRB Rd,[Rb,Ro]     1S+1N+1I  ----  7  Rd = BYTE[Rb+Ro]
  LDRH Rd,[Rb,5bit*2] 1S+1N+1I  ---- 10  Rd = HALFWORD[Rb+nn]
  LDRH Rd,[Rb,Ro]     1S+1N+1I  ----  8  Rd = HALFWORD[Rb+Ro]
  LDSB Rd,[Rb,Ro]     1S+1N+1I  ----  8  Rd = SIGNED_BYTE[Rb+Ro]
  LDSH Rd,[Rb,Ro]     1S+1N+1I  ----  8  Rd = SIGNED_HALFWORD[Rb+Ro]
  STR  Rd,[Rb,5bit*4] 2N        ----  9  WORD[Rb+nn] = Rd
  STR  Rd,[SP,8bit*4] 2N        ---- 11  WORD[SP+nn] = Rd
  STR  Rd,[Rb,Ro]     2N        ----  7  WORD[Rb+Ro] = Rd
  STRB Rd,[Rb,5bit*1] 2N        ----  9  BYTE[Rb+nn] = Rd
  STRB Rd,[Rb,Ro]     2N        ----  7  BYTE[Rb+Ro] = Rd
  STRH Rd,[Rb,5bit*2] 2N        ---- 10  HALFWORD[Rb+nn] = Rd
  STRH Rd,[Rb,Ro]     2N        ----  8  HALFWORD[Rb+Ro]=Rd
  PUSH {Rlist}{LR}    (n-1)S+2N ---- 14
  POP  {Rlist}{PC}              ---- 14  (ARM9: with mode switch)
  STMIA Rb!,{Rlist}   (n-1)S+2N ---- 15
  LDMIA Rb!,{Rlist}   nS+1N+1I  ---- 15
```



## THUMB Binary Opcode Format


This table summarizes the position of opcode/parameter bits for THUMB mode
instructions, Format 1-19.


```
 Form|_15|_14|_13|_12|_11|_10|_9_|_8_|_7_|_6_|_5_|_4_|_3_|_2_|_1_|_0_|
 __1_|_0___0___0_|__Op___|_______Offset______|____Rs_____|____Rd_____|Shifted
 __2_|_0___0___0___1___1_|_I,_Op_|___Rn/nn___|____Rs_____|____Rd_____|ADD/SUB
 __3_|_0___0___1_|__Op___|____Rd_____|_____________Offset____________|Immedi.
 __4_|_0___1___0___0___0___0_|______Op_______|____Rs_____|____Rd_____|AluOp
 __5_|_0___1___0___0___0___1_|__Op___|Hd_|Hs_|____Rs_____|____Rd_____|HiReg/BX
 __6_|_0___1___0___0___1_|____Rd_____|_____________Word______________|LDR PC
 __7_|_0___1___0___1_|__Op___|_0_|___Ro______|____Rb_____|____Rd_____|LDR/STR
 __8_|_0___1___0___1_|__Op___|_1_|___Ro______|____Rb_____|____Rd_____|""H/SB/SH
 __9_|_0___1___1_|__Op___|_______Offset______|____Rb_____|____Rd_____|""{B}
 _10_|_1___0___0___0_|Op_|_______Offset______|____Rb_____|____Rd_____|""H
 _11_|_1___0___0___1_|Op_|____Rd_____|_____________Word______________|"" SP
 _12_|_1___0___1___0_|Op_|____Rd_____|_____________Word______________|ADD PC/SP
 _13_|_1___0___1___1___0___0___0___0_|_S_|___________Word____________|ADD SP,nn
 _14_|_1___0___1___1_|Op_|_1___0_|_R_|____________Rlist______________|PUSH/POP
 _17_|_1___0___1___1___1___1___1___0_|___________User_Data___________|ARM9:BKPT
 _X__|_1___0___1___1___   ..............................             |ARM11...
  0110 011 Change Processor State        CPS on page B4-2
  0001 xxx Compare and Branch on Zero    CBNZ, CBZ on page A6-52
  1011 xxx Compare and Branch on Nonzero CBNZ, CBZ on page A6-52
  0011 xxx Compare and Branch on Zero    CBNZ, CBZ on page A6-52
  1001 xxx Compare and Branch on Nonzero CBNZ, CBZ on page A6-52
  0010 00x Signed Extend Halfword        SXTH on page A6-256
  0010 01x Signed Extend Byte            SXTB on page A6-254
  0010 10x Unsigned Extend Halfword      UXTH on page A6-274
  0010 11x Unsigned Extend Byte          UXTB on page A6-272
  1010 00x Byte-Reverse Word             REV on page A6-191
  1010 01x Byte-Reverse Packed Halfword  REV16 on page A6-192
  1010 11x Byte-Reverse Signed Halfword  REVSH on page A6-193
  1111 xxx If-Then, and hints If-Then, and hints on page A5-11
 _15_|_1___1___0___0_|Op_|____Rb_____|____________Rlist______________|STM/LDM
 _16_|_1___1___0___1_|_____Cond______|_________Signed_Offset_________|B{cond}
 _U__|_1___1___0___1___1___1___1___0_|_____________var_______________|UndefARM9
 _17_|_1___1___0___1___1___1___1___1_|___________User_Data___________|SWI
 _18_|_1___1___1___0___0_|________________Offset_____________________|B
 _19_|_1___1___1___0___1_|_________________________var___________|_0_|BLX.ARM9
 _U__|_1___1___1___0___1_|_________________________var___________|_1_|UndefARM9
 _19_|_1___1___1___1_|_H_|______________Offset_Low/High______________|BL,BLX
```


Further UNDEFS ??? ARM9?

- 1011 0001 xxxxxxxx (reserved)
- 1011 0x1x xxxxxxxx (reserved)
- 1011 10xx xxxxxxxx (reserved)
- 1011 1111 xxxxxxxx (reserved)
- 1101 1110 xxxxxxxx (free for user)




# <a name="thumbopcodesregisteroperationsalubx"></a>THUMB Opcodes: Register Operations (ALU, BX)




## THUMB.1: move shifted register


```
  15-13  Must be 000b for 'move shifted register' instructions
  12-11  Opcode
           00b: LSL{S} Rd,Rs,#Offset   (logical/arithmetic shift left)
           01b: LSR{S} Rd,Rs,#Offset   (logical    shift right)
           10b: ASR{S} Rd,Rs,#Offset   (arithmetic shift right)
           11b: Reserved (used for add/subtract instructions)
  10-6   Offset                     (0-31)
  5-3    Rs - Source register       (R0..R7)
  2-0    Rd - Destination register  (R0..R7)
```

Example: LSL Rd,Rs,#nn ; Rd = Rs \<\< nn ; ARM equivalent: MOVS Rd,Rs,LSL
#nn

Zero shift amount is having special meaning (same as for ARM shifts), LSL#0
performs no shift (the carry flag remains unchanged), LSR/ASR#0 are interpreted
as LSR/ASR#32. Attempts to specify LSR/ASR#0 in source code are automatically
redirected as LSL#0, and source LSR/ASR#32 is redirected as opcode LSR/ASR#0.

Execution Time: 1S

Flags: Z=zeroflag, N=sign, C=carry (except LSL#0: C=unchanged), V=unchanged.



## THUMB.2: add/subtract


```
  15-11  Must be 00011b for 'add/subtract' instructions
  10-9   Opcode (0-3)
           0: ADD{S} Rd,Rs,Rn   ;add register        Rd=Rs+Rn
           1: SUB{S} Rd,Rs,Rn   ;subtract register   Rd=Rs-Rn
           2: ADD{S} Rd,Rs,#nn  ;add immediate       Rd=Rs+nn
           3: SUB{S} Rd,Rs,#nn  ;subtract immediate  Rd=Rs-nn
         Pseudo/alias opcode with Imm=0:
           2: MOV{ADDS} Rd,Rs   ;move (affects cpsr) Rd=Rs+0
  8-6    For Register Operand:
           Rn - Register Operand (R0..R7)
         For Immediate Operand:
           nn - Immediate Value  (0-7)
  5-3    Rs - Source register       (R0..R7)
  2-0    Rd - Destination register  (R0..R7)
```

Return: Rd contains result, N,Z,C,V affected (including MOV).

Execution Time: 1S



## THUMB.3: move/compare/add/subtract immediate


```
  15-13  Must be 001b for this type of instructions
  12-11  Opcode
           00b: MOV{S} Rd,#nn      ;move     Rd   = #nn
           01b: CMP    Rd,#nn      ;compare  Void = Rd - #nn
           10b: ADD{S} Rd,#nn      ;add      Rd   = Rd + #nn
           11b: SUB{S} Rd,#nn      ;subtract Rd   = Rd - #nn
  10-8   Rd - Destination Register  (R0..R7)
  7-0    nn - Unsigned Immediate    (0-255)
```

ARM equivalents for MOV/CMP/ADD/SUB are MOVS/CMP/ADDS/SUBS same format.

Execution Time: 1S

Return: Rd contains result (except CMP), N,Z,C,V affected (for MOV only N,Z).



## THUMB.4: ALU operations


```
  15-10  Must be 010000b for this type of instructions
  9-6    Opcode (0-Fh)
           0: AND{S} Rd,Rs     ;AND logical       Rd = Rd AND Rs
           1: EOR{S} Rd,Rs     ;XOR logical       Rd = Rd XOR Rs
           2: LSL{S} Rd,Rs     ;log. shift left   Rd = Rd << (Rs AND 0FFh)
           3: LSR{S} Rd,Rs     ;log. shift right  Rd = Rd >> (Rs AND 0FFh)
           4: ASR{S} Rd,Rs     ;arit shift right  Rd = Rd SAR (Rs AND 0FFh)
           5: ADC{S} Rd,Rs     ;add with carry    Rd = Rd + Rs + Cy
           6: SBC{S} Rd,Rs     ;sub with carry    Rd = Rd - Rs - NOT Cy
           7: ROR{S} Rd,Rs     ;rotate right      Rd = Rd ROR (Rs AND 0FFh)
           8: TST    Rd,Rs     ;test            Void = Rd AND Rs
           9: NEG{S} Rd,Rs     ;negate            Rd = 0 - Rs
           A: CMP    Rd,Rs     ;compare         Void = Rd - Rs
           B: CMN    Rd,Rs     ;neg.compare     Void = Rd + Rs
           C: ORR{S} Rd,Rs     ;OR logical        Rd = Rd OR Rs
           D: MUL{S} Rd,Rs     ;multiply          Rd = Rd * Rs
           E: BIC{S} Rd,Rs     ;bit clear         Rd = Rd AND NOT Rs
           F: MVN{S} Rd,Rs     ;not               Rd = NOT Rs
  5-3    Rs - Source Register       (R0..R7)
  2-0    Rd - Destination Register  (R0..R7)
```

ARM equivalent for NEG would be RSBS.

Return: Rd contains result (except TST,CMP,CMN),

Affected Flags:

```
  N,Z,C,V for  ADC,SBC,NEG,CMP,CMN
  N,Z,C   for  LSL,LSR,ASR,ROR (carry flag unchanged if zero shift amount)
  N,Z,C   for  MUL on ARMv4 and below: carry flag destroyed
  N,Z     for  MUL on ARMv5 and above: carry flag unchanged
  N,Z     for  AND,EOR,TST,ORR,BIC,MVN
```

Execution Time:

```
  1S      for  AND,EOR,ADC,SBC,TST,NEG,CMP,CMN,ORR,BIC,MVN
  1S+1I   for  LSL,LSR,ASR,ROR
  1S+mI   for  MUL on ARMv4 (m=1..4; depending on MSBs of incoming Rd value)
  1S+mI   for  MUL on ARMv5 (m=3; fucking slow, no matter of MSBs of Rd value)
```



## THUMB.5: Hi register operations/branch exchange


```
  15-10  Must be 010001b for this type of instructions
  9-8    Opcode (0-3)
           0: ADD Rd,Rs   ;add        Rd = Rd+Rs
           1: CMP Rd,Rs   ;compare  Void = Rd-Rs  ;CPSR affected
           2: MOV Rd,Rs   ;move       Rd = Rs
           2: NOP         ;nop        R8 = R8
           3: BX  Rs      ;jump       PC = Rs     ;may switch THUMB/ARM
           3: BLX Rs      ;call       PC = Rs     ;may switch THUMB/ARM (ARM9)
  7      MSBd - Destination Register most significant bit (or BL/BLX flag)
  6      MSBs - Source Register most significant bit
  5-3    Rs - Source Register        (together with MSBs: R0..R15)
  2-0    Rd - Destination Register   (together with MSBd: R0..R15)
```

Restrictions: For ADD/CMP/MOV, MSBs and/or MSBd must be set, ie. it is not
allowed that both are cleared.

When using R15 (PC) as operand, the value will be the address of the
instruction plus 4 (ie. $+4). Except for BX R15: CPU switches to ARM state, and
PC is auto-aligned as (($+4) AND NOT 2).

For BX, MSBs may be 0 or 1, MSBd must be zero, Rd is not used/zero.

For BLX, MSBs may be 0 or 1, MSBd must be set, Rd is not used/zero.

For BX/BLX, when Bit 0 of the value in Rs is zero:

```
  Processor will be switched into ARM mode!
  If so, Bit 1 of Rs must be cleared (32bit word aligned).
  Thus, BX PC (switch to ARM) may be issued from word-aligned address
  only, the destination is PC+4 (ie. the following halfword is skipped).
```

BLX may not use R15. BLX saves the return address as LR=PC+3 (with thumb bit).

Using BLX R14 is possible (sets PC=Old\_LR, and New\_LR=retadr).

Assemblers/Disassemblers should use MOV R8,R8 as NOP (in THUMB mode).

Return: Only CMP affects CPSR condition flags!

Execution Time:

```
 1S     for ADD/MOV/CMP
 2S+1N  for ADD/MOV with Rd=R15, and for BX
```




# <a name="thumbopcodesmemoryloadstoreldrstr"></a>THUMB Opcodes: Memory Load/Store (LDR/STR)




## THUMB.6: load PC-relative (for loading immediates from literal pool)


```
  15-11  Must be 01001b for this type of instructions
  N/A    Opcode (fixed)
           LDR Rd,[PC,#nn]      ;load 32bit    Rd = WORD[PC+nn]
  10-8   Rd - Destination Register   (R0..R7)
  7-0    nn - Unsigned offset        (0-1020 in steps of 4)
```

The value of PC will be interpreted as (($+4) AND NOT 2).

Return: No flags affected, data loaded into Rd.

Execution Time: 1S+1N+1I



## THUMB.7: load/store with register offset


```
  15-12  Must be 0101b for this type of instructions
  11-10  Opcode (0-3)
          0: STR  Rd,[Rb,Ro]   ;store 32bit data  WORD[Rb+Ro] = Rd
          1: STRB Rd,[Rb,Ro]   ;store  8bit data  BYTE[Rb+Ro] = Rd
          2: LDR  Rd,[Rb,Ro]   ;load  32bit data  Rd = WORD[Rb+Ro]
          3: LDRB Rd,[Rb,Ro]   ;load   8bit data  Rd = BYTE[Rb+Ro]
  9      Must be zero (0) for this type of instructions
  8-6    Ro - Offset Register              (R0..R7)
  5-3    Rb - Base Register                (R0..R7)
  2-0    Rd - Source/Destination Register  (R0..R7)
```

Return: No flags affected, data loaded either into Rd or into memory.

Execution Time: 1S+1N+1I for LDR, or 2N for STR



## THUMB.8: load/store sign-extended byte/halfword


```
  15-12  Must be 0101b for this type of instructions
  11-10  Opcode (0-3)
          0: STRH Rd,[Rb,Ro]  ;store 16bit data          HALFWORD[Rb+Ro] = Rd
          1: LDSB Rd,[Rb,Ro]  ;load sign-extended 8bit   Rd = BYTE[Rb+Ro]
          2: LDRH Rd,[Rb,Ro]  ;load zero-extended 16bit  Rd = HALFWORD[Rb+Ro]
          3: LDSH Rd,[Rb,Ro]  ;load sign-extended 16bit  Rd = HALFWORD[Rb+Ro]
  9      Must be set (1) for this type of instructions
  8-6    Ro - Offset Register              (R0..R7)
  5-3    Rb - Base Register                (R0..R7)
  2-0    Rd - Source/Destination Register  (R0..R7)
```

Return: No flags affected, data loaded either into Rd or into memory.

Execution Time: 1S+1N+1I for LDR, or 2N for STR



## THUMB.9: load/store with immediate offset


```
  15-13  Must be 011b for this type of instructions
  12-11  Opcode (0-3)
          0: STR  Rd,[Rb,#nn]  ;store 32bit data   WORD[Rb+nn] = Rd
          1: LDR  Rd,[Rb,#nn]  ;load  32bit data   Rd = WORD[Rb+nn]
          2: STRB Rd,[Rb,#nn]  ;store  8bit data   BYTE[Rb+nn] = Rd
          3: LDRB Rd,[Rb,#nn]  ;load   8bit data   Rd = BYTE[Rb+nn]
  10-6   nn - Unsigned Offset              (0-31 for BYTE, 0-124 for WORD)
  5-3    Rb - Base Register                (R0..R7)
  2-0    Rd - Source/Destination Register  (R0..R7)
```

Return: No flags affected, data loaded either into Rd or into memory.

Execution Time: 1S+1N+1I for LDR, or 2N for STR



## THUMB.10: load/store halfword


```
  15-12  Must be 1000b for this type of instructions
  11     Opcode (0-1)
          0: STRH Rd,[Rb,#nn]  ;store 16bit data   HALFWORD[Rb+nn] = Rd
          1: LDRH Rd,[Rb,#nn]  ;load  16bit data   Rd = HALFWORD[Rb+nn]
  10-6   nn - Unsigned Offset              (0-62, step 2)
  5-3    Rb - Base Register                (R0..R7)
  2-0    Rd - Source/Destination Register  (R0..R7)
```

Return: No flags affected, data loaded either into Rd or into memory.

Execution Time: 1S+1N+1I for LDR, or 2N for STR



## THUMB.11: load/store SP-relative


```
  15-12  Must be 1001b for this type of instructions
  11     Opcode (0-1)
          0: STR  Rd,[SP,#nn]  ;store 32bit data   WORD[SP+nn] = Rd
          1: LDR  Rd,[SP,#nn]  ;load  32bit data   Rd = WORD[SP+nn]
  10-8   Rd - Source/Destination Register  (R0..R7)
  7-0    nn - Unsigned Offset              (0-1020, step 4)
```

Return: No flags affected, data loaded either into Rd or into memory.

Execution Time: 1S+1N+1I for LDR, or 2N for STR




# <a name="thumbopcodesmemoryaddressingaddpcsp"></a>THUMB Opcodes: Memory Addressing (ADD PC/SP)




## THUMB.12: get relative address


```
  15-12  Must be 1010b for this type of instructions
  11     Opcode/Source Register (0-1)
          0: ADD  Rd,PC,#nn    ;Rd = (($+4) AND NOT 2) + nn
          1: ADD  Rd,SP,#nn    ;Rd = SP + nn
  10-8   Rd - Destination Register         (R0..R7)
  7-0    nn - Unsigned Offset              (0-1020, step 4)
```

Return: No flags affected, result in Rd.

Execution Time: 1S



## THUMB.13: add offset to stack pointer


```
  15-8   Must be 10110000b for this type of instructions
  7      Opcode/Sign
          0: ADD  SP,#nn       ;SP = SP + nn
          1: ADD  SP,#-nn      ;SP = SP - nn
          1: SUB  SP,#nn       ;SP = SP - nn  ;\alias for ADD
          0: SUB  SP,#-nn      ;SP = SP + nn  ;/with flipped sign
  6-0    nn - Unsigned Offset    (0-508, step 4)
```

Return: No flags affected, SP adjusted.

Execution Time: 1S




# <a name="thumbopcodesmemorymultipleloadstorepushpopandldmstm"></a>THUMB Opcodes: Memory Multiple Load/Store (PUSH/POP and LDM/STM)




## THUMB.14: push/pop registers


```
  15-12  Must be 1011b for this type of instructions
  11     Opcode (0-1)
          0: PUSH {Rlist}{LR}   ;store in memory, decrements SP (R13)
          1: POP  {Rlist}{PC}   ;load from memory, increments SP (R13)
  10-9   Must be 10b for this type of instructions
  8      PC/LR Bit (0-1)
          0: No
          1: PUSH LR (R14), or POP PC (R15)
  7-0    Rlist - List of Registers (R7..R0)
```

In THUMB mode stack is always meant to be 'full descending', ie. PUSH is
equivalent to 'STMFD/STMDB' and POP to 'LDMFD/LDMIA' in ARM mode.

Examples:

```
 PUSH {R0-R3}     ;push R0,R1,R2,R3
 PUSH {R0,R2,LR}  ;push R0,R2,LR
 POP  {R4,R7}     ;pop R4,R7
 POP  {R2-R4,PC}  ;pop R2,R3,R4,PC
```

Note: When calling to a sub-routine, the return address is stored in LR
register, when calling further sub-routines, PUSH {LR} must be used to save
higher return address on stack. If so, POP {PC} can be later used to return
from the sub-routine.

POP {PC} ignores the least significant bit of the return address (processor
remains in thumb state even if bit0 was cleared), when intending to return with
optional mode switch, use a POP/BX combination (eg. POP {R3} / BX R3).

ARM9: POP {PC} copies the LSB to thumb bit (switches to ARM if bit0=0).

Return: No flags affected, SP adjusted, registers loaded/stored.

Execution Time: nS+1N+1I (POP), (n+1)S+2N+1I (POP PC), or (n-1)S+2N (PUSH).



## THUMB.15: multiple load/store


```
  15-12  Must be 1100b for this type of instructions
  11     Opcode (0-1)
          0: STMIA Rb!,{Rlist}   ;store in memory, increments Rb
          1: LDMIA Rb!,{Rlist}   ;load from memory, increments Rb
  10-8   Rb - Base register (modified) (R0-R7)
  7-0    Rlist - List of Registers     (R7..R0)
```

Both STM and LDM are incrementing the Base Register.

The lowest register in the list (ie. R0, if it's in the list) is stored/loaded
at the lowest memory address.

Examples:

```
 STMIA R7!,{R0-R2}  ;store R0,R1,R2
 LDMIA R0!,{R1,R5}  ;store R1,R5
```

Return: No flags affected, Rb adjusted, registers loaded/stored.

Execution Time: nS+1N+1I for LDM, or (n-1)S+2N for STM.



## Strange Effects on Invalid Rlist's


Empty Rlist: R15 loaded/stored (ARMv4 only), and Rb=Rb+40h (ARMv4-v5).

Writeback with Rb included in Rlist: Store OLD base if Rb is FIRST entry in
Rlist, otherwise store NEW base (STM/ARMv4), always store OLD base (STM/ARMv5),
no writeback (LDM/ARMv4/ARMv5; at this point, THUMB opcodes work different than
ARM opcodes).




# <a name="thumbopcodesjumpsandcalls"></a>THUMB Opcodes: Jumps and Calls




## THUMB.16: conditional branch


```
  15-12  Must be 1101b for this type of instructions
  11-8   Opcode/Condition (0-Fh)
          0: BEQ label        ;Z=1         ;equal (zero) (same)
          1: BNE label        ;Z=0         ;not equal (nonzero) (not same)
          2: BCS/BHS label    ;C=1         ;unsigned higher or same (carry set)
          3: BCC/BLO label    ;C=0         ;unsigned lower (carry cleared)
          4: BMI label        ;N=1         ;signed  negative (minus)
          5: BPL label        ;N=0         ;signed  positive or zero (plus)
          6: BVS label        ;V=1         ;signed  overflow (V set)
          7: BVC label        ;V=0         ;signed  no overflow (V cleared)
          8: BHI label        ;C=1 and Z=0 ;unsigned higher
          9: BLS label        ;C=0 or Z=1  ;unsigned lower or same
          A: BGE label        ;N=V         ;signed greater or equal
          B: BLT label        ;N<>V        ;signed less than
          C: BGT label        ;Z=0 and N=V ;signed greater than
          D: BLE label        ;Z=1 or N<>V ;signed less or equal
          E: Undefined, should not be used
          F: Reserved for SWI instruction (see SWI opcode)
  7-0    Signed Offset, step 2 ($+4-256..$+4+254)
```

Destination address must by halfword aligned (ie. bit 0 cleared)

Return: No flags affected, PC adjusted if condition true

Execution Time:

```
  2S+1N   if condition true (jump executed)
  1S      if condition false
```



## BX and ADD/MOV PC


See also THUMB.5: BX Rs, and ADD/MOV PC,Rs.



## THUMB.18: unconditional branch


```
  15-11  Must be 11100b for this type of instructions
  N/A    Opcode (fixed)
          B label   ;branch (jump)
  10-0   Signed Offset, step 2 ($+4-2048..$+4+2046)
```

Return: No flags affected, PC adjusted.

Execution Time: 2S+1N



## THUMB.19: long branch with link


This may be used to call (or jump) to a subroutine, return address is saved in
LR (R14).

Unlike all other THUMB mode instructions, this instruction occupies 32bit of
memory which are split into two 16bit THUMB opcodes.

- First Instruction - LR = PC+4+(nn SHL 12)
  - 15-11  Must be 11110b for BL/BLX type of instructions
  - 10-0   nn - Upper 11 bits of Target Address
- Second Instruction - PC = LR + (nn SHL 1), and LR = PC+2 OR 1 (and BLX: T=0)
  - 15-11  Opcode
                  - 11111b: BL label   ;branch long with link
                  - 11101b: BLX label  ;branch long with link switch to ARM mode (ARM9)
  - 10-0   nn - Lower 11 bits of Target Address (BLX: Bit0 Must be zero)

The destination address range is (PC+4)-400000h..+3FFFFEh, ie. PC+/-4M.

Target must be halfword-aligned. As Bit 0 in LR is set, it may be used to
return by a BX LR instruction (keeping CPU in THUMB mode).

Return: No flags affected, PC adjusted, return address in LR.

Execution Time: 3S+1N (first opcode 1S, second opcode 2S+1N).

Note: Exceptions may or may not occur between first and second opcode, this is
"implementation defined" (unknown how this is implemented in GBA and NDS).

Using only the 2nd half of BL as "BL LR+imm" is possible (for example, Mario
Golf Advance Tour for GBA uses opcode F800h as "BL LR+0").



## THUMB.17: software interrupt and breakpoint


SWI supposed for calls to the operating system - Enter Supervisor mode (SVC) in
ARM state. BKPT intended for debugging - enters Abort mode in ARM state via
Prefetch Abort vector.

```
  15-8   Opcode
          11011111b: SWI nn   ;software interrupt
          10111110b: BKPT nn  ;software breakpoint (ARMv5 and up)
  7-0    nn - Comment Field, ignored by processor (8bit value) (0-255)
```

Execution Time: 2S+1N

The exception handler may interprete the SWI Comment Field by examining the
lower 8bit of the 16bit opcode opcode at [R14\_svc-2].

If your are also using SWI's from inside of ARM mode, then the SWI handler must
examine the T Bit SPSR\_svc in order to determine whether it's been a ARM SWI -
and if so, examine the lower 24bit of the 32bit opcode opcode at [R14\_svc-4].

For Returning from SWI use "MOVS PC,R14", that instruction does restore both PC
and CPSR, ie. PC=R14\_svc, and CPSR=SPSR\_svc, and (as called from THUMB mode),
it'll also restore THUMB mode.

Nesting SWIs: SPSR\_svc and R14\_svc should be saved on stack before either
invoking nested SWIs, or (if the IRQ handler uses SWIs) before enabling IRQs.

Execution SWI/BKPT:

```
  R14_svc=PC+2     R14_abt=PC+4   ;save return address
  SPSR_svc=CPSR    SPSR_abt=CPSR  ;save CPSR flags
  CPSR=<changed>   CPSR=<changed> ;Enter svc/abt, ARM state, IRQs disabled
  PC=VVVV0008h     PC=VVVV000Ch   ;jump to SWI/PrefetchAbort vector address
```




# <a name="thumbopcodesnewthumbopcodesinarm11"></a>THUMB Opcodes: New THUMB Opcodes in ARM11




## THUMB.X: signed/unsigned extend byte/halfword (ARMv6)


These unneccessary opcodes are apparently meant to reduce typical compiler
overload (eg. for ensuring that "for i=0 to 7" won't end up with i>255).

```
  15-8   Must be 1011.0010b for this type of instructions
  7-6    Opcode
          00b  SXTH  Rd,Rm      ;sign-extend halfword
          01b  SXTB  Rd,Rm      ;sign-extend byte
          10b  UXTH  Rd,Rm      ;zero-extend halfword
          11b  UXTB  Rd,Rm      ;zero-extend byte
  5-3    Rm Source Register                    (R0-R7)
  2-0    Rd Destination Register               (R0-R7)
```

Return: No flags affected (though unspecified?).

Execution Time: ?



## THUMB.X: byte-reverse word/halfword(s) (ARMv6)


```
  15-8   Must be 1011.1010b for this type of instructions
  7-6    Opcode
          00b  REV   Rd,Rm      ;swap byte3/byte0 and byte2/byte1  ;word
          01b  REV16 Rd,Rm      ;swap byte3/byte2 and byte1/byte0  ;packed half
          11b  REVSH Rd,Rm      ;swap byte1/byte0 and sign-extend  ;signed half
  5-3    Rm Source Register                    (R0-R7)
  2-0    Rd Destination Register               (R0-R7)
```

Return: No flags affected (though unspecified?).

Execution Time: ?



## SETEND (ARMv6)


```
  15-4   Must be 101101100101b
  3      Opcode:
           0: SETEND LE   ;clear CPSR.E bit (little endian data access)
           1: SETEND BE   ;set CPSR.E bit (big endian data access)
  2-0    Should be 000b
```



## THUMB.X: change processor state (ARMv6)


```
  15-5   Must be 10110110011b
  4      Opcode:
           0: CPSIE {A}{I}{F}   ;Interrupt Enable
           1: CPSID {A}{I}{F}   ;Interrupt Disable
  3      Must be 0b
  2      A  Affect CPSR.A (?)    (0=No, 1=Yes)
  1      I  Affect CPSR.I (IRQ)  (0=No, 1=Yes)
  0      F  Affect CPSR.F (FIQ)  (0=No, 1=Yes)
```



## CPY


```
  15-8   Must be 01000110b for CPY Rd,Rm
  7      MSB of Rd
  6      MSB of Rm
  5-3    Rm Source Register
  2-0    Rd Destination Register
```

Move without changing flags. It's called CPY because THUMB syntax didn't
distinguish between MOV and MOVS (until UAL syntax was invented, which makes
the name CPY irrelevant).

CPY is officially supported in "T variants of ARMv6 and above" (in practice, it
should work on ARMv4T and up, but wasn't officially documented until ARMv6).





# <a name="armclassicvsualsyntaxunifiedassemblerlanguage"></a>ARM Classic vs UAL Syntax (Unified Assembler Language)



ARM has changed the ARM/THUMB syntax (apparently at some point between ARMv6
and ARMv6K). The new UAL syntax contains some cosmetic changes, and fixes some
serious flaws in the old THUMB syntax.

GBA/NDS software did traditionally use old syntax. DSi software probably, too.
3DS software uses ARMv6K, so it's more likely to use the new UAL syntax.



## THUMB code with {S} suffix


Most ALU opcodes do always affect flags in THUMB mode, although they were
written without {S} suffix in Pre-UAL syntax, which is problematic because the
same source code can behave differently for ARM and THUMB.

MOV/ADD/SUB don't affect flags when using SP, PC, or R8..R15 as source/dest
operands.

MOV/ADD have variants that don't affect flags for operands R0..R7 (these
variants were originally undocumented/reserved; the flag-less CPY variant was
officially allowed in ARMv6, the flag-less ADD seems to be allowed (?) in UAL).

MOV is a total mess because it relies on poorly defined pseudo opcodes (in the
Pre-UAL syntax it was common to encode it as ADD{S} opcode, which destroyed the
C,V flags).

```
  Pre-UAL               UAL
  MOV Rd,Imm            MOVS Rd,Imm                ;\
  LSL with R0..R7,0     MOVS with R0..R7           ;
  MOV with R0..R7       ADDS Rd,Rs,0               ; move
  MOV with R8..R15      MOV  with R8..R15          ;
  CPY with R0..R7       MOV  with R0..R7           ;
  SHIFT Rd,..,s         MOVS Rd,..,SHIFT s         ;/
  ADD with R0..R7       ADDS with R0..R7           ;\
  ADD with R8..R15      ADD  with R8..R15          ; add
  N/A (reserved)        ADD  with R0..R7           ;/
  SUB with R0..R7       SUBS with R0..R7           ;\subtract
  SUB SP,Imm            SUB SP,Imm  (aka ADD -Imm) ;/
  ADC/SBC/NEG/RSB/MUL   ADCS/SBCS/NEGS/RSBS/MULS   ;\
  AND/ORR/EOR/BIC/MVN   ANDS/ORRS/EORS/BICS/MVNS   ; always affects flags
  LSL/LSR/ASR/ROR       LSLS/LSRS/ASRS/RORS        ;/
```

These changes are solving compatibility issues between ARM and THUMB code.
However, in case of MOV and ADD they do bring up compatibility issues between
old and new THUMB code.



## Renamed Opcodes


```
  Pre-UAL               UAL
  LDR{cond}{xx}         LDR{xx}{cond}              ;\
  ADD{cond}{S}          ADD{S}{cond}               ; cond suffix moved to end
  xxx{cond}{xx}         xxx{xx}{cond}              ;/
  LDMIA                 LDM                        ;\amod IA can be ommited
  STMIA                 STM                        ;/
  LDRD Rd,[addr]        LDRD Rd,Rd+1,[addr]        ;\double word load/store
  STRD Rd,[addr]        STRD Rd,Rd+1,[addr]        ;/
  SWI (software irq)    SVC (supervisior)          ;-renamed SWI to SVC
```



## Alternate Opcode Names


Some opcodes had different names in classic ARM/THUMB syntax, UAL syntax allows
to use either one in both ARM and THUMB mode.

```
  THUMB-style           ARM-style
  PUSH rlist            STMFD r13!,rlist           ;\PUSH/POP
  POP rlist             LDMFD r13!,rlist           ;/
  NEG{S} Rd,Rs          RSB{S} Rd,Rs,0             ;-NEG/RSB
  LSL{S} Rd,Rn,shift    MOV{S} Rd,Rn,LSL shift     ;\
  LSR{S} Rd,Rn,shift    MOV{S} Rd,Rn,LSR shift     ;
  ASR{S} Rd,Rn,shift    MOV{S} Rd,Rn,ASR shift     ; MOV/SHIFT
  ROR{S} Rd,Rn,shift    MOV{S} Rd,Rn,ROR shift     ;
  RRX{S} Rd,Rn          MOV{S} Rd,Rn,RRX           ;/
  CMD{S} Rd,Op2         CMD{S} Rd,Rd,Op2           ;-long/short forms
```



## Further UAL changes


```
  NOP: uses new dedicated NOP opcodes (instead of MOV Rd,Rd pseudo opcodes)
  LDMIA: should omit "!" if Rn in rlist
  LDR Rd,[pc,#offset]: should be somehow written as LDR Rd,label
  Newer opcodes (for ARMv6K and up) seem to exist only in UAL syntax
```

Moreover, unrelated to UAL syntax, the 4-letter LDSB/LDSH/STSB/STSH THUMB
opcodes seem have been renamed to LDRSB/LDRSH/STRSB/STRSH at some point. And,
SUB SP,imm seems to have been added as an official THUMB alias for ADD SP,-imm.




# <a name="armpseudoinstructionsanddirectives"></a>ARM Pseudo Instructions and Directives




## ARM Pseudo Instructions


```
  nop              mov r0,r0
  ldr Rd,=Imm      ldr Rd,[r15,disp] ;use .pool as parameter field
  add Rd,=addr     add/sub Rd,r15,disp
  adr Rd,addr      add/sub Rd,r15,disp
  adrl Rd,addr     two add/sub opcodes with disp=xx00h+00yyh
  mov Rd,Imm       mvn Rd,NOT Imm    ;or vice-versa
  and Rd,Rn,Imm    bic Rd,Rn,NOT Imm ;or vice-versa
  cmp Rd,Rn,Imm    cmn Rd,Rn,-Imm    ;or vice-versa
  add Rd,Rn,Imm    sub Rd,Rn,-Imm    ;or vice-versa
```

All above opcodes may be made conditional by specifying a {cond} field.



## THUMB Pseudo Instructions


```
  nop              mov r8,r8
  ldr Rd,=Imm      ldr Rd,[r15,disp] ;use .pool as parameter field
  add Rd,=addr     add Rd,r15,disp
  adr Rd,addr      add Rd,r15,disp
  mov Rd,Rs        add Rd,Rs,0       ;with Rd,Rs in range r0-r7 each
```



## A22i Directives


```
  org  adr     assume following code from this address on
  .gba         indicate GBA program
  .nds         indicate NDS program
  .dsi         indicate DSi program
  .firm3ds     indicate 3DS program (.firm format)
  .fix         fix GBA/NDS/DSi header checksum
  .ual         select newer ARM syntax (UAL)
  .pre_ual     select older ARM syntax (classic)
  .ereader_create_bmp      create GBA e-Reader dotcode .BMP file(s) (bitmaps)
  .ereader_create_raw      create GBA e-Reader dotcode .RAW file (useless)
  .ereader_create_bin      create GBA e-Reader dotcode .BIN file (smallest)
  .ereader_japan_plus      japanese/plus     (default is non-japanese)
  .ereader_japan_original  japanese/original (with Z80-stub for GBA-code)
  .title 'Txt' defines a title (used for e-Reader dotcodes)
  .teak        select TeakLiteII instruction set (for DSi DSP)
  .xtensa      select Xtensa instruction set (for DSi Atheros Wifi)
  .78k0        select 78K/0 instruction set  (for DSi MCU)
  .78k0r       select 78K0R/ instruction set (for 3DS MCU)
  .rl78        alias for .78k0r
  .norewrite   do not delete existing output file (keep following data in file)
  .no_auto_run do not autostart binary after assembling
  .data?       following defines RAM data structure (assembled to nowhere)
  .code        following is normal ROM code/data (assembled to ROM image)
  .include     includes specified source code file (no nesting/error handling)
  .import      imports specified binary file (optional parameters: ,begin,len)
  .radix nn    changes default numeric format (nn=2,8,10,16 = bin/oct/dec/hex)
  .errif expr  generates an error message if expression is nonzero
  .if expr     assembles following code only if expression is nonzero
  .else        invert previous .if condition
  .endif       terminate .if/.ifdef/.ifndef
  .ifdef sym   assemble following only if symbol is defined
  .ifndef sym  assemble following only if symbol is not defined
  .align nn    aligns to an address divisible-by-nn, inserts 00's
  .msg         defines a no$gba debugmessage string, such like .msg 'Init Okay'
  .brk         defines a no$gba source code break opcode
  l equ n      l=n   (symbol)
  l:   [cmd]   l=$   (global label)
  @@l: [cmd]   @@l=$ (local label, all locals are reset at next global label)
  end          end of source code
  db ...       define 8bit data (bytes)
  dw ...       define 16bit data (halfwords)
  dd ...       define 32bit data (words)
  defs nn      define nn bytes space (zero-filled)
  ;...         defines a comment (ignored by the assembler)
  //           alias for CRLF, eg. allows <db 'Text',0 // dw addr> in one line
  .rept nn     repeat nn times, terminate by .endm
  .macro name  define a macro, terminate by .endm (see notes)
  .endm        end of .rept or .macro
  .undef sym   forget label or symbol name
```



## A22i macros


The macro feature is a bit quirky and unusual: It doesn't support macros with
parameters, but one can define parameters as "param equ 1" before calling the
macro.

The downside is that the assembler will complain about "param" being already
defined when doing multiple macro calls that way. The workaround is to use
".undef param" (either inside of the macro or after the macro call).

```
  Examples:
  .macro add_something
    add r1,@@x*4+2
    .undef @@x
  .endm
  @@x=1 // add_something
  @@x=2 // add_something
  @@x=3 // add_something
  .rept 10h
    nop
  .endm
```



## A22i Alias Directives (for compatibility with other assemblers)


```
  align        .align 4          code16    .thumb
  align nn     .align nn         .code 16  .thumb
  % nn         defs nn           code32    .arm
  .space nn    defs nn           .code 32  .arm
  ..ds nn      defs nn           ltorg     .pool
  x=n          x equ n           .ltorg    .pool
  .equ x,n     x equ n           ..ltorg   .pool
  .define x n  x equ n           dcb       db (8bit data)
  incbin       .import           defb      db (8bit data)
  @@@...       ;comment          .byte     db (8bit data)
  @ ...        ;comment          .ascii    db (8bit string)
  @*...        ;comment          dcw       dw (16bit data)
  @...         ;comment          defw      dw (16bit data)
  .text        .code             .hword    dw (16bit data)
  .bss         .data?            dcd       dd (32bit data)
  .global      (ignored)         defd      dd (32bit data)
  .extern      (ignored)         .long     dd (32bit data)
  .thumb_func  (ignored)         .word     dw/dd, don't use
  #directive   .directive        .end      end
  .fill nn,1,0 defs nn
```



## Alias Conditions, Opcodes, Operands


```
  hs   cs   ;condition higher or same = carry set
  lo   cc   ;condition lower = carry cleared
  asl  lsl  ;arithmetic shift left = logical shift left
```



## A22i Numeric Formats & Dialects


```
  Type          Normal       Alias
  Decimal       85           #85  &d85
  Hexadecimal   55h          #55h  0x55  #0x55  $55  &h55
  Octal         125o         0o125  &o125
  Ascii         'U'          "U"
  Binary        01010101b    %01010101  0b01010101  &b01010101
  Roman         &rLXXXV      (very useful for arrays of kings and chapters)
```

Note: The default numeric format can be changed by the .radix directive
(usually 10=decimal). For example, with radix 16, values like "85" and "0101b"
are treated as hexadecimal numbers (in that case, decimal and binary numbers
can be still defined with prefixes &d and &b).



## A22i Numeric Operators Priority


```
  Prio  Operator           Aliases
  8     (,) brackets
  7     +,- sign
  6     *,/,MOD,SHL,SHR    MUL,DIV,<<,>>
  5     +,- operation
  4     EQ,GE,GT,LE,LT,NE  =,>=,>,<=,<,<>,==,!=
  3     NOT
  2     AND
  1     OR,XOR             EOR
```

Operators of same priority are processed from left to right.

Boolean operators (priority 4) return 1=TRUE, 0=FALSE.



## A22i Nocash Syntax


Even though A22i does recognize the official ARM syntax, it's also allowing to
use friendly code:

```
  mov   r0,0ffh         ;no C64-style "#", and no C-style "0x" required
  stmia [r7]!,r0,r4-r5  ;square [base] brackets, no fancy {rlist} brackets
  mov   r0,cpsr         ;no confusing MSR and MRS (whatever which is which)
  mov   r0,p0,0,c0,c0,0 ;no confusing MCR and MRC (whatever which is which)
  ldr   r0,[score]      ;allows to use clean brackets for relative addresses
  push  rlist           ;alias for stmfd [r13]!,rlist (and same for pop/ldmfd)
  label:                ;label definitions recommended to use ":" colons
  xor   rd,..           ;alias for eor
  not   rd,..           ;alias for mvn
```


[A22i is the no$gba debug version's built-in source code assembler.]




# <a name="armcp14icebreakerdebugcommunicationschannel"></a>ARM CP14 ICEbreaker Debug Communications Channel



The ICEbreaker aka EmbeddedICE module may be found in ARM7TDMI and possibly
also in other ARM processors. The main functionality of the module relies on
external inputs (BREAKPT signal, etc.) being controlled by external debugging
hardware. At software side, ICEbreaker contains a Debug Communications Channel
(again to access external hardware), which can be accessed as coprocessor 14
via following opcodes:

```
  MRC{cond} P14,0,Rd,C0,C0,0  ;Read Debug Comms Control Register
  MRC{cond} P14,0,Rd,C1,C0,0  ;Read Debug Comms Data Register
  MRC{cond} P14,0,Rd,C2,C0,0  ;Read Debug Comms Status Register
  MCR{cond} P14,0,Rd,C1,C0,0  ;Write Debug Comms Data Register
  MCR{cond} P14,0,Rd,C2,C0,0  ;Write Debug Comms Status Register
```

The Control register consists of Bit31-28=ICEbreaker version (0001b for
ARM7TDMI), Bit27-2=Not specified, Bit0/Bit1=Data Read/Write Status Flags.


The NDS7 and GBA allow to access CP14 (unlike as for CP0..CP13 & CP15,
access to CP14 doesn't generate any exceptions), however, the ICEbreaker module
appears to be disabled (or completely unimplemented), any reads from
P14,0,Rd,C0,C0,0 through P14,7,Rd,C15,C15,7 are simply returning the prefetched
opcode value from [$+8]. ICEbreaker might be eventually used and enabled in
Nintendo's hardware debuggers, although external breakpoints are reportedly
implemented via /FIQ input rather than via ICEbreaker hardware.

The NDS9 doesn't include a CP14 unit (or it is fully disabled), any attempts to
access it are causing invalid instruction exceptions.




# <a name="armcp15systemcontrolcoprocessor"></a>ARM CP15 System Control Coprocessor



- [ARM CP15 Overview](#armcp15overview)
- [ARM CP15 ID Codes](#armcp15idcodes)
- [ARM CP15 Control Register](#armcp15controlregister)
- [ARM CP15 Memory Managment Unit (MMU)](#armcp15memorymanagmentunitmmu)
- [ARM CP15 Protection Unit (PU)](#armcp15protectionunitpu)
- [ARM CP15 Cache Control](#armcp15cachecontrol)
- [ARM CP15 Tightly Coupled Memory (TCM)](#armcp15tightlycoupledmemorytcm)
- [ARM CP15 Misc](#armcp15misc)



# <a name="armcp15overview"></a>ARM CP15 Overview




## CP15


In many ARM CPUs, particulary such with memory control facilities, coprocessor
number 15 (CP15) is used as built-in System Control Coprocessor.

CPUs without memory control functions typically don't include a CP15 at all, in
that case even an attempt to read the Main ID register will cause an Undefined
Instruction exception.



## CP15 Opcodes


CP15 can be accessed via MCR and MRC opcodes, with Pn=P15, and \<cpopc>=0.

```
  MCR{cond} P15,<cpopc>,Rd,Cn,Cm,<cp>   ;move from ARM to CP15
  MRC{cond} P15,<cpopc>,Rd,Cn,Cm,<cp>   ;move from CP15 to ARM
```

Rd can be any ARM register in range R0-R14, R15 should not be used with P15.

\<cpopc>,Cn,Cm,\<cp> are used to select a CP15 register, eg.
0,C0,C0,0 = Main ID Register.

Other coprocessor opcodes (CDP, LDC, STC) cannot be used with P15.



## CP15 Register List


```
  Register       Expl.
  0,C0,C0,0      Main ID Register (R)
  0,C0,C0,1      Cache Type and Size (R)
  0,C0,C0,2      TCM Physical Size (R)
  0,C0,C0,3      ARM11: TLB Type Register
  0,C0,C1,0      ARM11: Processor Feature Register 0
  0,C0,C1,1      ARM11: Processor Feature Register 1
  0,C0,C1,2      ARM11: Debug Feature Register 0
  0,C0,C1,3      ARM11: Auxiliary Feature Register 0
  0,C0,C1,4      ARM11: Memory Model Feature Register 0
  0,C0,C1,5      ARM11: Memory Model Feature Register 1
  0,C0,C1,6      ARM11: Memory Model Feature Register 2
  0,C0,C1,7      ARM11: Memory Model Feature Register 3
  0,C0,C2,0      ARM11: Set Attributes Register 0
  0,C0,C2,1      ARM11: Set Attributes Register 1
  0,C0,C2,2      ARM11: Set Attributes Register 2
  0,C0,C2,3      ARM11: Set Attributes Register 3
  0,C0,C2,4      ARM11: Set Attributes Register 4
  0,C0,C2,5      ARM11: Set Attributes Register 5
```


```
  0,C1,C0,0      Control Register (R/W, or R=Fixed)
  0,C1,C0,1      ARM11: Auxiliary Control Register
  0,C1,C0,2      ARM11: Coprocessor Access Control Register
  0,C2,C0,0      ARM11: Translation Table Base Register 0
  0,C2,C0,1      ARM11: Translation Table Base Register 1
  0,C2,C0,2      ARM11: Translation Table Base Control Register
  0,C3,C0,0      ARM11: Domain Access Control Register
  0,C5,C0,0      ARM11: Data Fault Status Register
  0,C5,C0,1      ARM11: Instruction Fault Status Register
  0,C6,C0,0      ARM11: Fault Address Register (FAR)
  0,C6,C0,1      ARM11: Watchpoint Fault Address Register (WFAR)
  0,C2,C0,0      PU Cachability Bits for Data/Unified Protection Region
  0,C2,C0,1      PU Cachability Bits for Instruction Protection Region
  0,C3,C0,0      PU Cache Write-Bufferability Bits for Data Protection Regions
  0,C5,C0,0      PU Access Permission Data/Unified Protection Region
  0,C5,C0,1      PU Access Permission Instruction Protection Region
  0,C5,C0,2      PU Extended Access Permission Data/Unified Protection Region
  0,C5,C0,3      PU Extended Access Permission Instruction Protection Region
  0,C6,C0..C7,0  PU Protection Unit Data/Unified Region 0..7
  0,C6,C0..C7,1  PU Protection Unit Instruction Region 0..7
  0,C7,Cm,Op2    Cache Commands and Halt Function (W)
  0,C9,C0,0      Cache Data Lockdown
  0,C9,C0,1      Cache Instruction Lockdown
  0,C9,C1,0      TCM Data TCM Base and Virtual Size
  0,C9,C1,1      TCM Instruction TCM Base and Virtual Size
  0,C13,Cm,Op2   Misc Process ID registers
  0,C15,Cm,Op2   Misc Implementation Defined and Test/Debug registers
```



## Data/Unified Registers


Some Cache/PU/TCM registers are declared as "Data/Unified".

That registers are used for Data accesses in case that the CPU contains
separate Data and Instruction registers, otherwise the registers are used for
both (unified) Data and Instruction accesses.




# <a name="armcp15idcodes"></a>ARM CP15 ID Codes




## C0,C0,0 - Main ID Register (R)


```
  12-15 ARM Era (0=Pre-ARM7, 7=ARM7, other=Post-ARM7)
```

Post-ARM7 Processors

```
  0-3   Revision Number
  4-15  Primary Part Number (Bit12-15 must be other than 0 or 7)
        (eg. 946h for ARM946)
  16-19 Architecture        (1=v4, 2=v4T, 3=v5, 4=v5T, 5=v5TE, 6=v6, ?=v7)
  20-23 Variant Number
  24-31 Implementor         (41h=ARM, 44h=Digital Equipment Corp, 69h=Intel)
```

ARM7 Processors

```
  0-3   Revision Number
  4-15  Primary Part Number (Bit12-15 must be 7)
  16-22 Variant Number
  23    Architecture        (0=v3, 1=v4T)
  24-31 Implementor         (41h=ARM, 44h=Digital Equipment Corp, 69h=Intel)
```

Pre-ARM7 Processors

```
  0-3   Revision Number
  4-11  Processor ID LSBs (30h=ARM3/v2, 60h,61h,62=ARM600,610,620/v3)
  12-31 Processor ID MSBs (fixed, 41560h)
```

Note: On the NDS9, this register is 41059461h (ARMv5TE, ARM946, rev1). NDS7 and
GBA don't have CP15s.



## C0,C0,1 - Cache Type Register (R)


```
  0-11  Instruction Cache (bits 0-1=len, 2=m, 3-5=assoc, 6-8=size, 9-11=zero)
  12-23 Data Cache        (bits 0-1=len, 2=m, 3-5=assoc, 6-8=size, 9-11=zero)
  24    Separate Cache Flag (0=Unified, 1=Separate Data/Instruction Caches)
  25-28 Cache Type (0,1,2,6,7=see below, other=reserved)
         Type Method         Cache cleaning         Cache lock-down
         0    Write-through  Not needed             Not supported
         1    Write-back     Read data block        Not supported
         2    Write-back     Register 7 operations  Not supported
         6    Write-back     Register 7 operations  Format A
         7    Write-back     Register 7 operations  Format B      ;<-- NDS9
  29-31 Reserved (zero)
```

The 12bit Instruction/Data values are decoded as shown below,

```
  Cache Absent  = (ASSOC=0 and M=1)       ;in that case overriding below
  Cache Size    = 200h+(100h*M) shl SIZE  ;min 0.5Kbytes, max 96Kbytes
  Associativity = (1+(0.5*M)) shl ASSOC   ;min 1-way,     max 192-way
  Line Length   = 8 shl LEN               ;min 8 bytes,   max 64 bytes
```

For Unified cache (Bit 24=0), Instruction and Data values are identical.

Note: On the NDS9, this register is 0F0D2112h (Code=2000h bytes, Data=1000h
bytes, assoc=whatever, and line size 32 bytes each). NDS7 and GBA don't have
CP15s (nor any code/data cache).



## C0,C0,2 - Tightly Coupled Memory (TCM) Size Register (R)


```
  0-1   Reserved    (0)
  2     ITCM Absent (0=Present, 1=Absent)
  3-5   Reserved    (0)
  6-9   ITCM Size   (Size = 512 SHL N) (or 0=None)
  10-13 Reserved    (0)
  14    DTCM Absent (0=Present, 1=Absent)
  15-17 Reserved    (0)
  18-21 DTCM Size   (Size = 512 SHL N) (or 0=None)
  22-31 Reserved    (0)
```

Note: On the NDS9, this register is 00140180h (ITCM=8000h bytes, DTCM=4000h
bytes)). NDS7 and GBA don't have CP15s (nor any ITCM/DTCM).



## C0,C0,3..7 - Reserved (R)


Unused/Reserved registers, containing the same value as C0,C0,0.




# <a name="armcp15controlregister"></a>ARM CP15 Control Register




## C1,C0,0 - Control Register (R/W, or R=Fixed)


```
  0  MMU/PU Enable         (0=Disable, 1=Enable) (Fixed 0 if none)
  1  Alignment Fault Check (0=Disable, 1=Enable) (Fixed 0/1 if none/always on)
  2  Data/Unified Cache    (0=Disable, 1=Enable) (Fixed 0/1 if none/always on)
  3  Write Buffer          (0=Disable, 1=Enable) (Fixed 0/1 if none/always on)
  4  Exception Handling    (0=26bit, 1=32bit)    (Fixed 1 if always 32bit)
  5  26bit-address faults  (0=Enable, 1=Disable) (Fixed 1 if always 32bit)
  6  Abort Model (pre v4)  (0=Early, 1=Late Abort) (Fixed 1 if ARMv4 and up)
  7  Endian                (0=Little, 1=Big)     (Fixed 0/1 if fixed)
  8  System Protection bit (MMU-only)
  9  ROM Protection bit    (MMU-only)
  10 Implementation defined
  11 Branch Prediction     (0=Disable, 1=Enable)
  12 Instruction Cache     (0=Disable, 1=Enable) (ignored if Unified cache)
  13 Exception Vectors     (0=00000000h, 1=FFFF0000h)
  14 Cache Replacement     (0=Normal/PseudoRandom, 1=Predictable/RoundRobin)
  15 Pre-ARMv5 Mode        (0=Normal, 1=Pre ARMv5; LDM/LDR/POP_PC.Bit0/Thumb)
  16 DTCM Enable           (0=Disable, 1=Enable)
  17 DTCM Load Mode        (0=R/W, 1=DTCM Write-only)
  18 ITCM Enable           (0=Disable, 1=Enable)
  19 ITCM Load Mode        (0=R/W, 1=ITCM Write-only)
  20 Reserved              (0)
  21 Reserved              (0)
  22 Unaligned Access      (?=Enable unaligned access and mixed endian)
  23 Extended Page Table   (0=Subpage AP Bits Enabled, 1=Disabled)
  24 Reserved              (0)
  25 CPSR E on exceptions  (0=Clear E bit, 1=Set E bit)
  26 Reserved              (0)
  27 FIQ Behaviour         (0=Normal FIQ behaviour, 1=FIQs behave as NMFI)
  28 TEX Remap bit         (0=No remapping, 1=Remap registers used)
  29 Force AP              (0=Access Bit not used, 1=AP[0] used as Access bit)
  30 Reserved              (0)
  31 Reserved              (0)
```

Various bits in this register may be read-only (fixed 0 if unsupported, or
fixed 1 if always activated).

On NDS ARM9, bit0,2,7,12..19 are R/W, bit3..6 are always set, all other bits
are always zero.

On 3DS ARM11, bit0..2,8..9,11..13,15,22..23,25,28..29 are R/W, bit3..6,14,16,18
are always set, all other bits are always zero.




# <a name="armcp15memorymanagmentunitmmu"></a>ARM CP15 Memory Managment Unit (MMU)



Nintendo consoles have a MMU on the ARM11 processor (in 3DS).


The MMU allows to assign virtual memory addresses, and to disable the data
cache in sensitive memory areas (eg. I/O area, and memory that is shared for
ARM9/ARM11). Additionally, it can trigger page fault exceptions (eg. for error
handling or memory mapped files).



## Related CP15 registers used in 3DS bootrom MMU init


```
  p15,0,c8,c5,0 ;Invalidate Instruction TLB               (=0)
  p15,0,c8,c6,0 ;Invalidate Data TLB                      (=0)
  p15,0,c2,c0,0 ;Translation Table Base 0 (for process)   (=1FFF4000h+2)
  p15,0,c2,c0,1 ;Translation Table Base 1 (for OS and I/O)(=1FFF4000h+2)
  p15,0,c2,c0,2 ;Translation Table Base Control           (=0)
  p15,0,c3,c0,0 ;Domain Access Control                    (=55555555h)
  p15,0,c1,c0,2 ;Coprocessor Access Control (unrelated?)  (=0F00000h)
```



## MMU Tables


1st Level table (size 4000h) divides 4Gbyte address space into 1Mbyte sections

2nd Level table(s) (size 400h each) divides a 1Mbyte section into 4Kbyte pages


```
 ________________________________ ARMv6 Tables ________________________________
```



## ARMv6 First-Level Page Table entries


```
   3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1
   1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0
  |__Ignored__________________________________________________|0_0| Fault
  |__Second-Level-Table-Address_______________|P|Domain_|_SBZ_|0_1| Coarse
  |__Address______________|0|0|n_S_A|_TEX_|AP_|P|Domain_|X_C_B|1_0| Section 1MB
  |__Address______|___SBZ___|1|_SBZ_|_TEX_|AP_|P|Ignored|X_C_B|1_0| Super 16MB
  |___________________________________________________________|1_1| Reserved
```

BUG: The Supersections do probably also have "n,S,A" flags, probably in
bit17-15, too (the 3DS bootrom seems to set those bits as so).



## ARMv6 Second-Level Page Table entries


```
   3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1
   1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0
  |__Ignored__________________________________________________|0_0| Fault
  |__Address______________________|X|_TEX_|n_S_A|_SBZ_|AP_|C_B|0_1| Large 64K
  |__Address______________________________|n_S_A|_TEX_|AP_|C_B|1_X| Small 4K
```


```
 ______________________________ Pre-ARMv6 Tables ______________________________
```


This is an older format, similar to the newer ARMv6 tables, but without the
"n,S,A,X" bits, and, instead, somehow allowing to divide 4K pages into smaller
1K snippets (with separate access privileges in AP0/AP1/AP2/AP3 bits, but
without TEX bits).



## Backwards-compatible First-Level Page Table entries


```
   3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1
   1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0
  |__Ignored__________________________________________________|0_0| Fault
  |__Second-Level-Table-Address_______________|P|Domain_|_SBZ_|0_1| Coarse
  |__Address______________|0|0|_SBZ_|_TEX_|AP_|P|Domain_|0_C_B|1_0| Section 1MB
  |__Address______|___SBZ___|1|_SBZ_|_TEX_|AP_|P|Ignored|0_C_B|1_0| Super 16MB
  |___________________________________________________________|1_1| Reserved
```



## Backwards-compatible Second-Level Page Table entries


```
   3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1
   1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0_9_8_7_6_5_4_3_2_1_0
  |__Ignored__________________________________________________|0_0| Fault
  |__Address______________________|_?TEX?_|AP3|AP2|AP1|AP0|C_B|0_1| Large 4x16K
  |__Address______________________________|AP3|AP2|AP1|AP0|C_B|1_0| Small 4x1K
  |__Address______________________________|_SBZ_|_TEX_|AP_|C_B|1_1| Extended 4K
```

BUG: The 4bit ?TEX? value is probably meant to be a 3bit TEX value, probably
with an unused/zero bit; in bit15 or so.


```
 ___________________________________ Notes ___________________________________
```



## Supersections and Large Pages


16Mbyte Supersections and 64Kbyte Large pages consist of 16 identical table
entries. Which is basically same as 16 Sections or 16 Pages with continously
increasing entries.

The advantage is internal caching: If one table entry is cached, then the
hardware knows that the other 15 nearby entries are supposed to be same; and
that they won't need to be loaded into internal cache.




# <a name="armcp15protectionunitpu"></a>ARM CP15 Protection Unit (PU)



Nintendo consoles have a PU on the ARM9 processor (in NDS, DSi, 3DS).


Protection Unit can be enabled in Bit0 of C1,C0,0 (Control Register).



## C2,C0,0 - Cachability Bits for Data/Unified Protection Region (R/W)



## C2,C0,1 - Cachability Bits for Instruction Protection Region (if any) (R/W)


```
  0-7  Cachable (C) bits for region 0-7
  8-31 Reserved/zero
```



## C3,C0,0 - Cache Write-Bufferability Bits for Data Protection Regions (R/W)


Allows to select what to do when writing to a cached memory snippet:

Write-Through stores the data in the cache line (so subsequent cache reads
return correct data), and additionally writes the data to underlaying memory.

Write-Back stores the data in the cache line only, and marks the line as dirty,
but doesn't update the underlaying memory (underlaying memory is updated only
when the CPU decides to use the cache line for other purposes, or when the user
is manually "Cleaning" the cache line).

```
  0-7  Bufferable (B) bits for region 0-7  (0=Write-Through, 1=Write-Back)
  8-31 Reserved/zero
```

Instruction fetches are, obviously, always read-operations. So, there are no
write-bufferability bits for Instruction Protection Regions.

Note: Unrelated to the "Cache Write-Bufferability", the ARM does also have a
"Write Buffer" (a small FIFO that can queue only a few writes).



## C5,C0,0 - Access Permission Data/Unified Protection Region (R/W)



## C5,C0,1 - Access Permission Instruction Protection Region (if any) (R/W)



## C5,C0,2 - Extended Access Permission Data/Unified Protection Region (R/W)



## C5,C0,3 - Extended Access Permission Instruction Protection Region (if any) (R/W)


For C5,C0,0 and C5,C0,1:

```
  0-15  Access Permission (AP) bits for region 0-7 (Bits 0-1=AP0, 2-3=AP1, etc)
  16-31 Reserved/zero
```

For C5,C0,2 and C5,C0,3 (Extended):

```
  0-31  Access Permission (AP) bits for region 0-7 (Bits 0-3=AP0, 4-7=AP1, etc)
```

The possible AP settings (0-3 for C5,C0,0..1, or 0-15 for C5,C0,2..3) are:

```
  AP  Privileged User
  0   -          -
  1   R/W        -
  2   R/W        R
  3   R/W        R/W
  5   R          -
  6   R          R
```

Settings 5,6 only for Extended Registers, settings 4,7..15 are Reserved.



## C6,C0..C7,0 - Protection Unit Data/Unified Region 0..7 (R/W)



## C6,C0..C7,1 - Protection Unit Instruction Region 0..7 (R/W) if any


```
  0     Protection Region Enable (0=Disable, 1=Enable)
  1-5   Protection Region Size   (2 SHL X) ;min=(X=11)=4KB, max=(X=31)=4GB
  6-11  Reserved/zero
  12-31 Protection Region Base address (Addr = Y*4K; must be SIZE-aligned)
```

Overlapping Regions are allowed, Region 7 is having highest priority, region 0
lowest priority.



## Background Region


Additionally, any memory areas outside of the eight Protection Regions are
handled as Background Region, this region has neither Read nor Write access.



## Unified Region Note


On the NDS, the Region registers are unified (C6,C0..C7,1 are read/write-able
mirrors of C6,C0..C7,0). Nethertheless, the Cachabilty and Permission registers
are NOT unified (separate registers exists for code and data settings).




# <a name="armcp15cachecontrol"></a>ARM CP15 Cache Control



Cache is enabled/controlled by Bit 2,3,12,14 in Control Register.

Cache regions are controlled via Protection Unit (PU).

Cache type can be detected via Cache Type Register.



## C7,C0..C15,0..7 - Cache Commands (W)


Write-only Cache Command Register. Cm,Op2 operands used to select a specific
command, with parameter value in Rd.

```
  Cn,Cm,Op2 Rd   ARM9 Command
  C7,C0,4   0    Yes  Wait For Interrupt (Halt)
  C7,C5,0   0    Yes  Invalidate Entire Instruction Cache
  C7,C5,1   VA   Yes  Invalidate Instruction Cache Line
  C7,C5,2   S/I  -    Invalidate Instruction Cache Line
  C7,C5,4   0    -    Flush Prefetch Buffer
  C7,C5,6   0    -    Flush Entire Branch Target Cache
  C7,C5,7   IMP? -    Flush Branch Target Cache Entry
  C7,C6,0   0    Yes  Invalidate Entire Data Cache
  C7,C6,1   VA   Yes  Invalidate Data Cache Line
  C7,C6,2   S/I  -    Invalidate Data Cache Line
  C7,C7,0   0    -    Invalidate Entire Unified Cache
  C7,C7,1   VA   -    Invalidate Unified Cache Line
  C7,C7,2   S/I  -    Invalidate Unified Cache Line
  C7,C8,2   0    Yes  Wait For Interrupt (Halt), alternately to C7,C0,4
  C7,C10,1  VA   Yes  Clean Data Cache Line
  C7,C10,2  S/I  Yes  Clean Data Cache Line
  C7,C10,4  0    -    Drain Write Buffer
  C7,C11,1  VA   -    Clean Unified Cache Line
  C7,C11,2  S/I  -    Clean Unified Cache Line
  C7,C13,1  VA   Yes  Prefetch Instruction Cache Line
  C7,C14,1  VA   Yes  Clean and Invalidate Data Cache Line
  C7,C14,2  S/I  Yes  Clean and Invalidate Data Cache Line
  C7,C15,1  VA   -    Clean and Invalidate Unified Cache Line
  C7,C15,2  S/I  -    Clean and Invalidate Unified Cache Line
```

Parameter values (Rd) formats:

```
  0    Not used, should be zero
  VA   Virtual Address
  S/I  Set/index; Bit 31..(32-A) = Index, Bit (L+S-1)..L = Set ?
```

Note:

```
  Invalidate means to forget all data
  Clean means to write-back dirty cache lines to underlaying memory
  (Clean is important when having "Cache Write-Bufferability" enabled in PU)
```



## C9,C0,0 - Data Cache Lockdown



## C9,C0,1 - Instruction Cache Lockdown


(Width (W) of index field depends on cache ASSOCIATIVETY.)

Format A:

```
  0..(31-W)  Reserved/zero
  (32-W)..31 Lockdown Block Index
```

Format B:

```
  0..(W-1)   Lockdown Block Index
  W..30      Reserved/zero
  31         L
```


Cache/Write-buffer should not be enabled for the whole 4GB memory area,
high-speed TCM memory doesn't require caching, and caching would have fatal
results on I/O ports. So, cache can be used only in combination with the
Protection Unit, which allows to enable/disable caching in specified regions.



## Note


ARMv5 instruction set supports a Cache Prepare for Load opcode (PLD), see

- [ARM Opcodes: Memory: Single Data Transfer (LDR, STR, PLD)](#armopcodesmemorysingledatatransferldrstrpld)


## Note


Data cache always requires PU/MMU. Code cache requires PU/MMU on ARM9, but also
works without PU/MMU on ARM11.




# <a name="armcp15tightlycoupledmemorytcm"></a>ARM CP15 Tightly Coupled Memory (TCM)



TCM is high-speed memory, directly contained in the ARM CPU core.



## TCM and DMA


TCM doesn't use the ARM bus. A minor disadvantage is that TCM cannot be
accessed by DMA. However, the main advantage is that, when using TCM, the CPU
can be kept running without any waitstates even while the bus is used for DMA
transfers. Operation during DMA works only if all code/data is located in TCM,
waitstates are generated if any code/data outside TCM is accessed; in worst
case (if there are no gaps in the DMA) then the CPU is halted until the DMA
finishes.



## TCM and DMA and IRQ


No idea if/how IRQs are handled during DMA? Eventually (unlikely) code in TCM
is kept executed until DMA finishes (ie. until the IRQ vector can be accessed.
Eventually the IRQ vector is instantly accessed (causing to halt the CPU until
DMA finishes). In both cases: Assuming that IRQs are enabled, and that the IRQ
vector and/or IRQ handler are located outside TCM.



## Separate Instruction (ITCM) and Data (DTCM) Memory


DTCM can be used only for Data accesses, typically used for stacks and other
frequently accessed data.

ITCM is primarily intended for instruction accesses, but it can be also used
for Data accesses (among others allowing to copy code to ITCM), however,
performance isn't optimal when simultaneously accessing ITCM for code and data
(such like opcodes in ITCM that use literal pool values in ITCM).



## TCM Enable, TCM Load Mode


CP15 Control Register allows to enable ITCM and DTCM, and to switch ITCM/DTCM
into Load Mode. In Load Mode (when TCM is enabled), TCM becomes write-only;
this allows to read data from source addresses in main memory, and to write
data to destination addresses in TCM by using the same addresses; useful for
initializing TCM with overlapping source/dest addresses; Load mode works with
all Load/Store opcodes, it does NOT work with SWP/SWPB opcodes.


TCM Physical Size can be detected in 3rd ID Code Register. (C0,C0,2)



## C9,C1,0 - Data TCM Size/Base (R/W)



## C9,C1,1 - Instruction TCM Size/Base (R/W)


```
  0     Reserved     (0)
  1-5   Virtual Size (Size = 512 SHL N) ;min=(N=3)=4KB, max=(N=23)=4GB
  6-11  Reserved     (0)
  12-31 Region Base  (Base = X SHL 12)  ;Base must be Size-aligned
```

The Virtual size settings should be normally same as the Physical sizes (see
C0,C0,2). However, smaller sizes are allowed (using only the 1st some KB), as
well as bigger sizes (TCM area is then filled with mirrors of physical TCM).

The ITCM region base may be fixed (read-only), for example, on the NDS, ITCM
base is always 00000000h, nethertheless the virtual size may be changed
(allowing to mirror ITCM to higher addresses).

If DTCM and ITCM do overlap, then ITCM appears to have priority.



## TCM and PU


TCM can be used without Protection Unit.

When the protection unit is enabled, TCM is controlled by the PU just like
normal memory, the PU should provide R/W Access Permission for TCM regions;
cache and write-buffer are not required for high-speed TCM (so both should be
disabled for TCM regions).




# <a name="armcp15misc"></a>ARM CP15 Misc




## C13,C0,0 - Process ID for Fast Context Switch Extension (FCSE) (R/W)


```
  0-24  Reserved/zero
  25-31 Process ID (PID) (0-127) (0=Disable)
```

The FCSE allows different processes (each assembled with ORG 0) to be located
at virtual addresses in the 1st 32MB area. The FCSE splits the total 4GB
address space into blocks of 32MB, accesses to Block(0) are redirected to
Block(PID):

```
  IF addr<32M then addr=addr+PID*32M
  Respectively, with PID=0, the address remains unchanged (FCSE disabled).
```

The CPU-to-Memory address handling is shown below:

```
  1. CPU outputs a virtual address (VA)
  2. FCSE adjusts the VA to a modified virtual address (MVA)
  3. Cache hits determined by examining the MVA, continue below if no hit
  4. MMU translates MVA to physical address (PA) (if no MMU present: PA=MVA)
  5. Memory access occurs at PA
```

The FCSE allows limited virtual addressing even if no MMU is present.

If the MMU is present, then either the FCSE and/or the MMU can be used for
virtual addressing; the advantage of using the FCSE (a single write to
C13,C0,0) is less overload; using the MMU for the same purpose would require to
change virtual address translation table in memory, and to flush the cache.

The NDS doesn't have a FCSE (the FCSE register is read-only, always zero).



## C13,C0,1 - Trace Process ID (R/W)



## C13,C1,1 - Trace Process ID (Mirror) (R/W)


This value is output to ETMPROCID pins (if any), allowing to notify external
hardware about the currently executed process within multi-tasking programs.

```
  0-31  Process ID
```

C13,C1,1 is a mirror of C13,C0,1 (for compatibility with other ARM processors).

Both registers are read/write-able on NDS9, but there are no external pin-outs.



## \<cpopc>


Unlike for all other CP15 registers, the \<cpopc> operand of the MRC/MCR
opcodes isn't always zero for below registers, so below registers are using
"cpopc,Cn,Cm,op2" notation (instead of the normal "Cn,Cm,op2" notation).



## Built-In-Self-Test (BIST)


Allows to test internal memory (ie. TCM, Cache Memory, and Cache TAGs). The
tests are filling (and verifying) the selected memory region thrice (once with
the fillvalue, then with the inverted fillvalue, and then again with the
fillvalue). The BIST functions are intended for diagnostics purposes only, not
for use in normal program code (ARM doesn't guarantee future processors to have
backwards compatible BIST functions).



## 0,C15,C0,1 - BIST TAG Control Register (R/W)



## 1,C15,C0,1 - BIST TCM Control Register (R/W)



## 2,C15,C0,1 - BIST Cache Control Register (R/W)


```
  0-15  Data Control (see below)
  16-31 Instruction Control (see below)
```

The above 16bit control values are:

```
  0     Start bit     (Write: 1=Start) (Read: 1=Busy)
  1     Pause bit     (1=Pause)
  2     Enable bit    (1=Enable)
  3     Fail Flag     (1=Error) (Read Only)
  4     Complete Flag (1=Ready) (Read Only)
  5-15  Size (2^(N+2) bytes) (min=N=1=8bytes, max=N=24=64MB)
```

Size and Pause are not supported in all implementations.

Caution: While and as long as the Enable bit is set, the corresponding memory
region(s) will be disabled. Eg. when testing \<either> DTCM \<and/or>
ITCM, \<both> DTCM \<and> ITCM are forcefully disabled in C1,C0,0
(Control Register), after the test the software must first clear the BIST
enable bit, and then restore DTCM/ITCM bits in C1,C0,0. And of course, the
content of the tested memory region must be restored when needed.



## 0,C15,C0,2 - BIST Instruction TAG Address (R/W)



## 1,C15,C0,2 - BIST Instruction TCM Address (R/W)



## 2,C15,C0,2 - BIST Instruction Cache Address (R/W)



## 0,C15,C0,6 - BIST Data TAG Address (R/W)



## 1,C15,C0,6 - BIST Data TCM Address (R/W)



## 2,C15,C0,6 - BIST Data Cache Address (R/W)


```
  0-31  Word-aligned Destination Address within Memory Block (eg. within ITCM)
```

On the NDS9, bit0-1, and bit21-31 are always zero.



## 0,C15,C0,3 - BIST Instruction TAG Fillvalue (R/W)



## 1,C15,C0,3 - BIST Instruction TCM Fillvalue (R/W)



## 2,C15,C0,3 - BIST Instruction Cache Fillvalue (R/W)



## 0,C15,C0,7 - BIST Data TAG Fillvalue (R/W)



## 1,C15,C0,7 - BIST Data TCM Fillvalue (R/W)



## 2,C15,C0,7 - BIST Data Cache Fillvalue (R/W)


```
  0-31  Fillvalue for BIST
```

After BIST, the selected memory region is filled by that value. That is, on the
NDS9 at least, all words will be filled with the SAME value (ie. NOT with
increasing or randomly generated numbers).



## 0,C15,C0,0 - Cache Debug Test State Register (R/W)


```
  0-8    Reserved (zero)
  9      Disable Instruction Cache Linefill
  10     Disable Data Cache Linefill
  11     Disable Instruction Cache Streaming
  12     Disable Data Cache Streaming
  13-31  Reserved (zero/unpredictable)
```



## 3,C15,C0,0 - Cache Debug Index Register (R/W)


```
  0..1    Reserved (zero)
  2..4    Word Address
  5..N    Index
  N+1..29 Reserved (zero)
  30..31  Segment
```



## 3,C15,C1,0 - Cache Debug Instruction TAG (R/W)



## 3,C15,C2,0 - Cache Debug Data TAG (R/W)



## 3,C15,C3,0 - Cache Debug Instruction Cache (R/W)



## 3,C15,C4,0 - Cache Debug Data Cache (R/W)


```
  0..1    Set
  2..3    Dirty Bits
  4       Valid
  5..N    Index
  N+1..31 TAG Address
```




# <a name="armcpuinstructioncycletimes"></a>ARM CPU Instruction Cycle Times



Instruction Cycle Summary


```
  Instruction      Cycles      Additional
  ---------------------------------------------------------------------
  ALU              1S          +1S+1N if R15 loaded, +1I if SHIFT(Rs)
  MSR,MRS          1S
  LDR              1S+1N+1I    +1S+1N if R15 loaded
  STR              2N
  LDM              nS+1N+1I    +1S+1N if R15 loaded
  STM              (n-1)S+2N
  SWP              1S+2N+1I
  BL (THUMB)       3S+1N
  B,BL             2S+1N
  SWI,trap         2S+1N
  MUL              1S+ml
  MLA              1S+(m+1)I
  MULL             1S+(m+1)I
  MLAL             1S+(m+2)I
  CDP              1S+bI
  LDC,STC          (n-1)S+2N+bI
  MCR              1N+bI+1C
  MRC              1S+(b+1)I+1C
  {cond} false     1S
```



ARM9:

```
  Q{D}ADD/SUB      1S+Interlock.
  CLZ              1S.
  LDR              1S+1N+1L
  LDRB,LDRH,LDRmis 1S+1N+2L
  LDR PC ...
  STR              1S+1N        (not 2N, and both in parallel)
```

Execution Time: 1S+Interlock    (SMULxy,SMLAxy,SMULWx,SMLAWx)

Execution Time: 1S+1I+Interlock (SMLALxy)


ARM11:

```
  Observe that Branch Prediction (enabled in CP15 Control register) can affect
  timings for conditional jumps (and presumably non-conditional ones, too).
  Without prediction: A waitloop made of "SUBS+BNE" takes 4 clks per loop cycle
  With prediction: A waitloop made of "SUBS+BNE" takes 2.5 clks per loop cycle
  (apparently the prediction assumes 75% of the jumps to be taken)
```


Whereas,

```
  n = number of words transferred
  b = number of cycles spent in coprocessor busy-wait loop
  m = depends on most significant byte(s) of multiplier operand
```

Above 'trap' is meant to be the execution time for exceptions. And '{cond}
false' is meant to be the execution time for conditional instructions which
haven't been actually executed because the condition has been false.


The separate meaning of the N,S,I,C cycles is:



## N - Non-sequential cycle


Requests a transfer to/from an address which is NOT related to the address used
in the previous cycle. (Called 1st Access in GBA language).

The execution time for 1N is 1 clock cycle (plus non-sequential access
waitstates).



## S - Sequential cycle


Requests a transfer to/from an address which is located directly after the
address used in the previous cycle. Ie. for 16bit or 32bit accesses at
incrementing addresses, the first access is Non-sequential, the following
accesses are sequential. (Called 2nd Access in GBA language).

The execution time for 1S is 1 clock cycle (plus sequential access waitstates).



## I - Internal Cycle


CPU is just too busy, not even requesting a memory transfer for now.

The execution time for 1I is 1 clock cycle (without any waitstates).



## C - Coprocessor Cycle


The CPU uses the data bus to communicate with the coprocessor (if any), but no
memory transfers are requested.



## Memory Waitstates


Ideally, memory may be accessed free of waitstates (1N and 1S are then equal to
1 clock cycle each). However, a memory system may generate waitstates for
several reasons: The memory may be just too slow. Memory is currently accessed
by DMA, eg. sound, video, memory transfers, etc. Or when data is squeezed
through a 16bit data bus (in that special case, 32bit access may have more
waitstates than 8bit and 16bit accesses). Also, the memory system may separate
between S and N cycles (if so, S cycles would be typically faster than N
cycles).



## Memory Waitstates for Different Memory Areas


Different memory areas (eg. ROM and RAM) may have different waitstates. When
executing code in one area which accesses data in another area, then the S+N
cycles must be split into code and data accesses: 1N is used for data access,
plus (n-1)S for LDM/STM, the remaining S+N are code access. If an instruction
jumps to a different memory area, then all code cycles for that opcode are
having waitstate characteristics of the NEW memory area (except Thumb BL which
still executes 1S in OLD area).




# <a name="armcpuversions"></a>ARM CPU Versions




## Version Numbers


ARM CPUs are distributed by name ARM#, and are described as ARMv# in
specifications, whereas "#" is NOT the same than "v#", for example, ARM7TDMI is
ARMv4TM. That is so confusing, that ARM didn't even attempt to clarify the
relationship between the various "#" and "v#" values.



## Version Variants


Suffixes like "M" (long multiply), "T" (Thumb support), "E" (Enhanced DSP)
indicate presence of special features, additionally to the standard instruction
set of a given version, or, when preceded by an "x", indicate the absence of
that features.



## ARMv1 aka ARM1


Some sort of a beta version, according to ARM never been used in any commercial
products.



## ARMv2 and up


MUL,MLA

CDP,LDC,MCR,MRC,STC

SWP/SWPB (ARMv2a and up only)

Two new FIQ registers



## ARMv3 and up


MRS,MSR opcodes (instead CMP/CMN/TST/TEQ{P} opcodes)

CPSR,SPSR registers (instead PSR bits in R15)

Removed never condition, cond=NV no longer valid

32bit addressing (instead 26bit addressing in older versions)

26bit addressing backwards comptibility mode (except v3G)

Abt and Und modes (instead handling aborts/undefined in Svc mode)

SMLAL,SMULL,UMLAL,UMULL (optionally, INCLUDED in v3M, EXCLUDED in v4xM/v5xM)



## ARMv4 aka ARM7 and up


LDRH,LDRSB,LDRSH,STRH

Sys mode (privileged user mode)

BX (only ARMv4T, and any ARMv5 or ARMv5T and up)

THUMB code (only T variants, ie. ARMv4T, ARMv5T)



## ARMv5 aka ARM9 and up


BKPT,BLX,CLZ (BKPT,BLX also in THUMB mode)

LDM/LDR/POP PC with mode switch (POP PC also in THUMB mode)

CDP2,LDC2,MCR2,MRC2,STC2 (new coprocessor opcodes)

C-flag unchanged by MUL (instead undefined flag value)

changed instruction cycle timings / interlock ??? or not ???

QADD,QDADD,QDSUB,QSUB opcodes, CPSR.Q flag (v5TE and V5TExP only)

SMLAxy,SMLALxy,SMLAWy,SMULxy,SMULWy (v5TE and V5TExP only)

LDRD,STRD,PLD,MCRR,MRRC (v5TE only, not v5, not v5TExP)

BXJ (jump to Jazelle bytecode) (v5TEJ only)



## ARMv6 aka ARM11 and up (see ARM DDI 0100I)


Supports all ARMv5TEJ features (=including Jazelle?).

The following ARM instructions are added:

- CPS, SRS and RFE instructions for improved exception handling

- REV, REV16 and REVSH byte reversal instructions

- SETEND for a revised endian (memory) model

- LDREX and STREX exclusive access instructions

- SXTB, SXTH, UXTB, UXTH byte/halfword extend instructions

- A set of Single Instruction Multiple Data (SIMD) media instructions

- Additional forms of multiply with accumulation into a 64-bit result

The following Thumb instructions are added:

- CPS, CPY (a form of MOV), REV, REV16, REVSH, SETEND, SXTB, SXTH, UXTB, UXTH

SWP/SWPB has been deprecated in ARMv6, made optional in ARMv7 (with the
possibility of disabling it if still available), and removed in ARMv8.



## ARMv6K aka "ARM11 MPCore" (see ARM DDI 0360F) (used in 3DS)


Unknown how this is abbreviated (ARMv6-M would be something else: Cortex-M).

The MPCore processor provides support for extensions to ARMv6 that include:

- LDREX/STREX{B|H|D} for 8bit/16bit/64bit and CLREX

- HINT (TrueNOP, YIELD, WFI, WFE, SEV)

- Architectural remap registers.

- Revised use of TEX remap bits. The ARMv6 MMU page table descriptors use a
large number of bits to describe all of the options for inner and outer
cachability. In reality, no application requires all of these options
simultaneously. Therefore it is possible to use the TEX remap mechanism to
configure the MP11 CPUs to support only a small number of options. This implies
a level of indirection in the page table mappings.

The TEX CB encoding table provides two OS managed page table bits. For binary
compatibility with existing ARMv6 ports of OSs, this gives a separate mode of
operation of the MMU. This is called the TEX Remap configuration and is
controlled by bit [28] TR in CP15 Register 1.

- Revised use of AP bits. In the MP11 CPUs the APX and AP[1:0] encoding b111 is
Privileged or User mode read-only access. AP[0] indicates an abort type, Access
Bit fault, when CP15 c1[29] is 1.

For more information see the ARM Architecture Reference Manual.



## ARMv6T2 aka THUMB-2


This doesn't seem to be supported in 3DS. THUMB-2 adds more 2x16bit opcodes in
THUMB mode, ie. THUMB code can contain classic 16bit opcodes, mixed with new
2x16bit opcodes. As far as I understand, the old 2x16bit "BL" and "BLX" do no
longer exists, or are replaced by differently encode THUMB-2 opcodes?



## ARMv6Z


ARMv6K with Security Extensions. Aka TrustZone, see Processor Feature Register
1.



## A Milestone in Computer History


Original ARMv2 has been used in the relative rare and expensive Archimedes
deluxe home computers in the late eighties, the Archimedes has caught a lot of
attention, particularly for being the first home computer that used a BIOS
being programmed in BASIC language - which has been a absolutely revolutionary
decadency at that time.

Inspired, programmers all over the world have successfully developed even
slower and much more inefficient programming languages, which are nowadays
consequently used by nearly all ARM programmers, and by most non-ARM
programmers as well.




# <a name="armcpudatasheet"></a>ARM CPU Data Sheet



This present document is an attempt to supply a brief ARM7TDMI reference,
hopefully including all information which is relevant for programmers.


Some details that I have treated as meaningless for GBA programming aren't
included - such like Big Endian format, and Virtual Memory data aborts, and
most of the chapters listed below.


Have a look at the complete data sheet (URL see below) for more detailed
verbose information about ARM7TDMI instructions. That document also includes:

- - Signal Description
    - Pins of the original CPU, probably other for GBA.
- - Memory Interface
    - Optional virtual memory circuits, etc. not for GBA.
- - Coprocessor Interface
    - As far as I know, none such in GBA.
- - Debug Interface
    - For external hardware-based debugging.
- - ICEBreaker Module
    - For external hardware-based debugging also.
- - Instruction Cycle Operations
    - Detailed: What happens during each cycle of each instruction.
- - DC Parameters (Power supply)
- - AC Parameters (Signal timings)

The official ARM7TDMI data sheet can be downloaded from ARMs webpage,

- [http://www.arm.com/Documentation/UserMans/PDF/ARM7TDMI.html](http://www.arm.com/Documentation/UserMans/PDF/ARM7TDMI.html)

Be prepared for bloated PDF Format, approx 1.3 MB, about 200 pages.




# <a name="biosfunctions"></a>BIOS Functions



The BIOS includes several System Call Functions which can be accessed by SWI
instructions. Incoming parameters are usually passed through registers
R0,R1,R2,R3. Outgoing registers R0,R1,R3 are typically containing either
garbage, or return value(s). All other registers (R2,R4-R14) are kept
unchanged.



## Caution


When invoking SWIs from inside of ARM state specify SWI NN\*10000h, instead of
SWI NN as in THUMB state.



## Overview


- [BIOS Function Summary](#biosfunctionsummary)
- [BIOS Differences between GBA and NDS functions](#biosdifferencesbetweengbaandndsfunctions)


## All Functions Described


- [BIOS Arithmetic Functions](#biosarithmeticfunctions)
- [BIOS Rotation/Scaling Functions](#biosrotationscalingfunctions)
- [BIOS Decompression Functions](#biosdecompressionfunctions)
- [BIOS Memory Copy](#biosmemorycopy)
- [BIOS Halt Functions](#bioshaltfunctions)
- [BIOS Reset Functions](#biosresetfunctions)
- [BIOS Misc Functions](#biosmiscfunctions)
- [BIOS Multi Boot (Single Game Pak)](#biosmultibootsinglegamepak)
- [BIOS Sound Functions](#biossoundfunctions)
- [BIOS SHA1 Functions (DSi only)](#biossha1functionsdsionly)
- [BIOS RSA Functions (DSi only)](#biosrsafunctionsdsionly)


## 3DS bootroms


- [BIOS 3DS Exception Vectors](#bios3dsexceptionvectors)


## RAM Usage, BIOS Dumps


- [BIOS RAM Usage](#biosramusage)
- [BIOS Dumping](#biosdumping)


## How BIOS Processes SWIs


SWIs can be called from both within THUMB and ARM mode. In ARM mode, only the
upper 8bit of the 24bit comment field are interpreted.

Each time when calling a BIOS function 4 words (SPSR, R11, R12, R14) are saved
on Supervisor stack (\_svc). Once it has saved that data, the SWI handler
switches into System mode, so that all further stack operations are using user
stack.

In some cases the BIOS may allow interrupts to be executed from inside of the
SWI procedure. If so, and if the interrupt handler calls further SWIs, then
care should be taken that the Supervisor Stack does not overflow.




# <a name="biosfunctionsummary"></a>BIOS Function Summary



```
  GBA  NDS7 NDS9 DSi7 DSi9 Basic Functions
  00h  00h  00h  -    -    SoftReset
  01h  -    -    -    -    RegisterRamReset
  02h  06h  06h  06h  06h  Halt
  03h  07h  -    07h  -    Stop/Sleep
  04h  04h  04h  04h  04h  IntrWait       ;DSi7/DSi9: both bugged?
  05h  05h  05h  05h  05h  VBlankIntrWait ;DSi7/DSi9: both bugged?
  06h  09h  09h  09h  09h  Div
  07h  -    -    -    -    DivArm
  08h  0Dh  0Dh  0Dh  0Dh  Sqrt
  09h  -    -    -    -    ArcTan
  0Ah  -    -    -    -    ArcTan2
  0Bh  0Bh  0Bh  0Bh  0Bh  CpuSet
  0Ch  0Ch  0Ch  0Ch  0Ch  CpuFastSet
  0Dh  -    -    -    -    GetBiosChecksum
  0Eh  -    -    -    -    BgAffineSet
  0Fh  -    -    -    -    ObjAffineSet
  GBA  NDS7 NDS9 DSi7 DSi9 Decompression Functions
  10h  10h  10h  10h  10h  BitUnPack
  11h  11h  11h  11h  11h  LZ77UnCompReadNormalWrite8bit   ;"Wram"
  12h  -    -    -    -    LZ77UnCompReadNormalWrite16bit  ;"Vram"
  -    -    -    01h  01h  LZ77UnCompReadByCallbackWrite8bit
  -    12h  12h  02h  02h  LZ77UnCompReadByCallbackWrite16bit
  -    -    -    19h  19h  LZ77UnCompReadByCallbackWrite16bit (same as above)
  13h  -    -    -    -    HuffUnCompReadNormal
  -    13h  13h  13h  13h  HuffUnCompReadByCallback
  14h  14h  14h  14h  14h  RLUnCompReadNormalWrite8bit     ;"Wram"
  15h  -    -    -    -    RLUnCompReadNormalWrite16bit    ;"Vram"
  -    15h  15h  15h  15h  RLUnCompReadByCallbackWrite16bit
  16h  -    16h  -    16h  Diff8bitUnFilterWrite8bit       ;"Wram"
  17h  -    -    -    -    Diff8bitUnFilterWrite16bit      ;"Vram"
  18h  -    18h  -    18h  Diff16bitUnFilter
  GBA  NDS7 NDS9 DSi7 DSi9 Sound (and Multiboot/HardReset/CustomHalt)
  19h  08h  -    08h  -    SoundBias
  1Ah  -    -    -    -    SoundDriverInit
  1Bh  -    -    -    -    SoundDriverMode
  1Ch  -    -    -    -    SoundDriverMain
  1Dh  -    -    -    -    SoundDriverVSync
  1Eh  -    -    -    -    SoundChannelClear
  1Fh  -    -    -    -    MidiKey2Freq
  20h  -    -    -    -    SoundWhatever0
  21h  -    -    -    -    SoundWhatever1
  22h  -    -    -    -    SoundWhatever2
  23h  -    -    -    -    SoundWhatever3
  24h  -    -    -    -    SoundWhatever4
  25h  -    -    -    -    MultiBoot
  26h  -    -    -    -    HardReset
  27h  1Fh  -    1Fh  -    CustomHalt
  28h  -    -    -    -    SoundDriverVSyncOff
  29h  -    -    -    -    SoundDriverVSyncOn
  2Ah  -    -    -    -    SoundGetJumpList
  GBA  NDS7 NDS9 DSi7 DSi9 New NDS Functions
  -    03h  03h  03h  03h  WaitByLoop
  -    0Eh  0Eh  0Eh  0Eh  GetCRC16
  -    0Fh  0Fh  -    -    IsDebugger
  -    1Ah  -    1Ah  -    GetSineTable
  -    1Bh  -    1Bh  -    GetPitchTable (DSi7: bugged)
  -    1Ch  -    1Ch  -    GetVolumeTable
  -    1Dh  -    1Dh  -    GetBootProcs (DSi7: only 1 proc)
  -    -    1Fh  -    1Fh  CustomPost
  GBA  NDS7 NDS9 DSi7 DSi9 New DSi Functions (RSA/SHA1)
  -    -    -    20h  20h  RSA_Init_crypto_heap
  -    -    -    21h  21h  RSA_Decrypt
  -    -    -    22h  22h  RSA_Decrypt_Unpad
  -    -    -    23h  23h  RSA_Decrypt_Unpad_OpenPGP_SHA1
  -    -    -    24h  24h  SHA1_Init
  -    -    -    25h  25h  SHA1_Update
  -    -    -    26h  26h  SHA1_Finish
  -    -    -    27h  27h  SHA1_Init_update_fin
  -    -    -    28h  28h  SHA1_Compare_20_bytes
  -    -    -    29h  29h  SHA1_Random_maybe
  GBA  NDS7 NDS9 DSi7 DSi9 Invalid Functions
  2Bh+ 20h+ 20h+ -    -    Crash (SWI xxh..FFh do jump to garbage addresses)
  -    xxh  xxh  -    -    Jump to 0   (on any SWI numbers not listed above)
  -    -    -    12h  12h  No function (ignored)
  -    -    -    2Bh  2Bh  No function (ignored)
  -    -    -    40h+ 40h+ Mirror      (SWI 40h..FFh mirror to 00h..3Fh)
  -    -    -    xxh  xxh  Hang        (on any SWI numbers not listed above)
```

Invalid NDS functions: NDS7 SWI 01h, 02h, 0Ah, 16h-19h, 1Eh, and NDS9 SWI 01h,
02h, 07h, 08h, 0Ah, 17h, 19h-1Eh will jump to zero (ie. to the NDS7 reset
vector, or to NDS9 unused (usually PU-locked ITCM) memory, which will be both
redirected to the debug handler, if any).

Invalid DSi functions: DSi9 SWI 00h, 07h-08h, 0Ah, 0Fh, 17h, 1Ah-1Eh, 2Ah,
2Ch-3Fh do hang in endless loop.




# <a name="biosdifferencesbetweengbaandndsfunctions"></a>BIOS Differences between GBA and NDS functions




## Differences between GBA and NDS BIOS functions


- SoftReset uses different addresses

- SWI numbers for Halt, Stop/Sleep, Div, Sqrt have changed

- Halt destroys r0 on NDS9, IntrWait bugged on NDS9

- CpuFastSet allows 4-byte blocks (nice), but...

- CpuFastSet works very SLOW because of a programming bug (uncool)

- Some of the decompression functions are now using callbacks

- SoundBias uses new delay parameter

And, a number of GBA functions have been removed, and some new NDS functions
have been added, see:

- [BIOS Function Summary](#biosfunctionsummary)



# <a name="biosarithmeticfunctions"></a>BIOS Arithmetic Functions



Div

DivArm

Sqrt

ArcTan

ArcTan2



## SWI 06h (GBA) or SWI 09h (NDS7/NDS9/DSi7/DSi9) - Div


Signed Division, r0/r1.

```
  r0  signed 32bit Number
  r1  signed 32bit Denom
```

Return:

```
  r0  Number DIV Denom ;signed
  r1  Number MOD Denom ;signed
  r3  ABS (Number DIV Denom) ;unsigned
```

For example, incoming -1234, 10 should return -123, -4, +123.

The function usually gets caught in an endless loop upon division by zero.

Note: The NDS9 and DSi9 additionally support hardware division, by math
coprocessor, accessed via I/O Ports, however, the SWI function is a raw
software division.



## SWI 07h (GBA) - DivArm


Same as above (SWI 06h Div), but incoming parameters are exchanged, r1/r0
(r0=Denom, r1=number). For compatibility with ARM's library. Slightly slower (3
clock cycles) than SWI 06h.



## SWI 08h (GBA) or SWI 0Dh (NDS7/NDS9/DSi7/DSi9) - Sqrt


Calculate square root.

```
  r0   unsigned 32bit number
```

Return:

```
  r0   unsigned 16bit number
```

The result is an integer value, so Sqrt(2) would return 1, to avoid this
inaccuracy, shift left incoming number by 2\*N as much as possible (the result
is then shifted left by 1\*N). Ie. Sqrt(2 shl 30) would return 1.41421 shl 15.

Note: The NDS9 and DSi9 additionally support hardware square root calculation,
by math coprocessor, accessed via I/O Ports, however, the SWI function is a raw
software calculation.



## SWI 09h (GBA) - ArcTan


Calculates the arc tangent.

```
  r0   Tan, 16bit (1bit sign, 1bit integral part, 14bit decimal part)
```

Return:

```
  r0   "-PI/2<THETA/<PI/2" in a range of C000h-4000h.
```

Note: there is a problem in accuracy with "THETA\<-PI/4, PI/4\<THETA".



## SWI 0Ah (GBA) - ArcTan2


Calculates the arc tangent after correction processing.

Use this in normal situations.

```
  r0   X, 16bit (1bit sign, 1bit integral part, 14bit decimal part)
  r1   Y, 16bit (1bit sign, 1bit integral part, 14bit decimal part)
```

Return:

```
  r0   0000h-FFFFh for 0<=THETA<2PI.
```




# <a name="biosrotationscalingfunctions"></a>BIOS Rotation/Scaling Functions



BgAffineSet

ObjAffineSet



## SWI 0Eh (GBA) - BgAffineSet


Used to calculate BG Rotation/Scaling parameters.

```
  r0   Pointer to Source Data Field with entries as follows:
        s32  Original data's center X coordinate (8bit fractional portion)
        s32  Original data's center Y coordinate (8bit fractional portion)
        s16  Display's center X coordinate
        s16  Display's center Y coordinate
        s16  Scaling ratio in X direction (8bit fractional portion)
        s16  Scaling ratio in Y direction (8bit fractional portion)
        u16  Angle of rotation (8bit fractional portion) Effective Range 0-FFFF
  r1   Pointer to Destination Data Field with entries as follows:
        s16  Difference in X coordinate along same line
        s16  Difference in X coordinate along next line
        s16  Difference in Y coordinate along same line
        s16  Difference in Y coordinate along next line
        s32  Start X coordinate
        s32  Start Y coordinate
  r2   Number of Calculations
```

Return: No return value, Data written to destination address.



## SWI 0Fh (GBA) - ObjAffineSet


Calculates and sets the OBJ's affine parameters from the scaling ratio and
angle of rotation.

The affine parameters are calculated from the parameters set in Srcp.

The four affine parameters are set every Offset bytes, starting from the Destp
address.

If the Offset value is 2, the parameters are stored contiguously. If the value
is 8, they match the structure of OAM.

When Srcp is arrayed, the calculation can be performed continuously by
specifying Num.

```
  r0   Source Address, pointing to data structure as such:
        s16  Scaling ratio in X direction (8bit fractional portion)
        s16  Scaling ratio in Y direction (8bit fractional portion)
        u16  Angle of rotation (8bit fractional portion) Effective Range 0-FFFF
  r1   Destination Address, pointing to data structure as such:
        s16  Difference in X coordinate along same line
        s16  Difference in X coordinate along next line
        s16  Difference in Y coordinate along same line
        s16  Difference in Y coordinate along next line
  r2   Number of calculations
  r3   Offset in bytes for parameter addresses (2=continuous, 8=OAM)
```

Return: No return value, Data written to destination address.


For both Bg- and ObjAffineSet, Rotation angles are specified as 0-FFFFh
(covering a range of 360 degrees), however, the GBA BIOS recurses only the
upper 8bit; the lower 8bit may contain a fractional portion, but it is ignored
by the BIOS.




# <a name="biosdecompressionfunctions"></a>BIOS Decompression Functions



BitUnPack

Diff8bitUnFilter

HuffUnComp

LZ77UnComp

RLUnComp



## Decompression Read/Write Variants


```
  ReadNormal:      Fast (src must be memory mapped)
  ReadByCallback:  Slow (src can be non-memory, eg. serial Firmware SPI bus)
  Write8bitUnits:  Fast (dest must support 8bit writes, eg. not VRAM)
  Write16bitUnits: Slow (dest must be halfword-aligned) (for VRAM)
```



## BitUnPack - SWI 10h (GBA/NDS7/NDS9/DSi7/DSi9)


Used to increase the color depth of bitmaps or tile data. For example, to
convert a 1bit monochrome font into 4bit or 8bit GBA tiles. The Unpack Info is
specified separately, allowing to convert the same source data into different
formats.

```
  r0  Source Address      (no alignment required)
  r1  Destination Address (must be 32bit-word aligned)
  r2  Pointer to UnPack information:
       16bit  Length of Source Data in bytes     (0-FFFFh)
       8bit   Width of Source Units in bits      (only 1,2,4,8 supported)
       8bit   Width of Destination Units in bits (only 1,2,4,8,16,32 supported)
       32bit  Data Offset (Bit 0-30), and Zero Data Flag (Bit 31)
      The Data Offset is always added to all non-zero source units.
      If the Zero Data Flag was set, it is also added to zero units.
```

Data is written in 32bit units, Destination can be Wram or Vram. The size of
unpacked data must be a multiple of 4 bytes. The width of source units (plus
the offset) should not exceed the destination width.

Return: No return value, Data written to destination address.



## Diff8bitUnFilterWrite8bit (Wram) - SWI 16h (GBA/NDS9/DSi9)



## Diff8bitUnFilterWrite16bit (Vram) - SWI 17h (GBA)



## Diff16bitUnFilter - SWI 18h (GBA/NDS9/DSi9)


These aren't actually real decompression functions, destination data will have
exactly the same size as source data. However, assume a bitmap or wave form to
contain a stream of increasing numbers such like 10..19, the
filtered/unfiltered data would be:

```
  unfiltered:   10  11  12  13  14  15  16  17  18  19
  filtered:     10  +1  +1  +1  +1  +1  +1  +1  +1  +1
```

In this case using filtered data (combined with actual compression algorithms)
will obviously produce better compression results.

Data units may be either 8bit or 16bit used with Diff8bit or Diff16bit
functions respectively.

```
  r0  Source address (must be aligned by 4) pointing to data as follows:
       Data Header (32bit)
         Bit 0-3   Data size (must be 1 for Diff8bit, 2 for Diff16bit)
         Bit 4-7   Type (must be 8 for DiffFiltered)
         Bit 8-31  24bit size after decompression
       Data Units (each 8bit or 16bit depending on used SWI function)
         Data0          ;original data
         Data1-Data0    ;difference data
         Data2-Data1    ;...
         Data3-Data2
         ...
  r1  Destination address
```

Return: No return value, Data written to destination address.



## HuffUnCompReadNormal - SWI 13h (GBA)



## HuffUnCompReadByCallback - SWI 13h (NDS/DSi)


The decoder starts in root node, the separate bits in the bitstream specify if
the next node is node0 or node1, if that node is a data node, then the data is
stored in memory, and the decoder is reset to the root node. The most often
used data should be as close to the root node as possible. For example, the
4-byte string "Huff" could be compressed to 6 bits: 10-11-0-0, with root.0
pointing directly to data "f", and root.1 pointing to a child node, whose nodes
point to data "H" and data "u".

Data is written in units of 32bits, if the size of the compressed data is not a
multiple of 4, please adjust it as much as possible by padding with 0.

Align the source address to a 4Byte boundary.

```
  r0  Source Address, aligned by 4, pointing to:
       Data Header (32bit)
         Bit0-3   Data size in bit units (normally 4 or 8)
         Bit4-7   Compressed type (must be 2 for Huffman)
         Bit8-31  24bit size of decompressed data in bytes
       Tree Size (8bit)
         Bit0-7   Size of Tree Table/2-1 (ie. Offset to Compressed Bitstream)
       Tree Table (list of 8bit nodes, starting with the root node)
        Root Node and Non-Data-Child Nodes are:
         Bit0-5   Offset to next child node,
                  Next child node0 is at (CurrentAddr AND NOT 1)+Offset*2+2
                  Next child node1 is at (CurrentAddr AND NOT 1)+Offset*2+2+1
         Bit6     Node1 End Flag (1=Next child node is data)
         Bit7     Node0 End Flag (1=Next child node is data)
        Data nodes are (when End Flag was set in parent node):
         Bit0-7   Data (upper bits should be zero if Data Size is less than 8)
       Compressed Bitstream (stored in units of 32bits)
         Bit0-31  Node Bits (Bit31=First Bit)  (0=Node0, 1=Node1)
  r1  Destination Address
  r2  Callback temp buffer      ;\for NDS/DSi "ReadByCallback" variants only
  r3  Callback structure        ;/(see Callback notes below)
```

Return: No return value, Data written to destination address.



## LZ77UnCompReadNormalWrite8bit (Wram) - SWI 11h (GBA/NDS7/NDS9/DSi7/DSi9)



## LZ77UnCompReadNormalWrite16bit (Vram) - SWI 12h (GBA)



## LZ77UnCompReadByCallbackWrite8bit - SWI 01h (DSi7/DSi9)



## LZ77UnCompReadByCallbackWrite16bit - SWI 12h (NDS), SWI 02h or 19h (DSi)


Expands LZ77-compressed data. The Wram function is faster, and writes in units
of 8bits. For the Vram function the destination must be halfword aligned, data
is written in units of 16bits.

- [LZ Decompression Functions](#lzdecompressionfunctions)
CAUTION: Writing 16bit units to [dest-1] instead of 8bit units to [dest] means
that reading from [dest-1] won't work, ie. the "Vram" function works only with
disp=001h..FFFh, but not with disp=000h.

If the size of the compressed data is not a multiple of 4, please adjust it as
much as possible by padding with 0. Align the source address to a 4-Byte
boundary.

```
  r0  Source address, pointing to data as such:
       Data header (32bit)
         Bit 0-3   Reserved (0)
         Bit 4-7   Compressed type (must be 1 for LZ77)
         Bit 8-31  Size of decompressed data
       Repeat below. Each Flag Byte followed by eight Blocks.
       Flag data (8bit)
         Bit 0-7   Type Flags for next 8 Blocks, MSB first
       Block Type 0 - Uncompressed - Copy 1 Byte from Source to Dest
         Bit 0-7   One data byte to be copied to dest
       Block Type 1 - Compressed - Copy N+3 Bytes from Dest-Disp-1 to Dest
         Bit 0-3   Disp MSBs
         Bit 4-7   Number of bytes to copy (minus 3)
         Bit 8-15  Disp LSBs
  r1  Destination address
  r2  Callback parameter        ;\for NDS/DSi "ReadByCallback" variants only
  r3  Callback structure        ;/(see Callback notes below)
```

Return: No return value.



## RLUnCompReadNormalWrite8bit (Wram) - SWI 14h (GBA/NDS7/NDS9/DSi7/DSi9)



## RLUnCompReadNormalWrite16bit (Vram) - SWI 15h (GBA)



## RLUnCompReadByCallbackWrite16bit - SWI 15h (NDS7/NDS9/DSi7/DSi9)


Expands run-length compressed data. The Wram function is faster, and writes in
units of 8bits. For the Vram function the destination must be halfword aligned,
data is written in units of 16bits.

If the size of the compressed data is not a multiple of 4, please adjust it as
much as possible by padding with 0. Align the source address to a 4Byte
boundary.

```
  r0  Source Address, pointing to data as such:
       Data header (32bit)
         Bit 0-3   Reserved
         Bit 4-7   Compressed type (must be 3 for run-length)
         Bit 8-31  Size of decompressed data
       Repeat below. Each Flag Byte followed by one or more Data Bytes.
       Flag data (8bit)
         Bit 0-6   Expanded Data Length (uncompressed N-1, compressed N-3)
         Bit 7     Flag (0=uncompressed, 1=compressed)
       Data Byte(s) - N uncompressed bytes, or 1 byte repeated N times
  r1  Destination Address
  r2  Callback parameter        ;\for NDS/DSi "ReadByCallback" variants only
  r3  Callback structure        ;/(see Callback notes below)
```

Return: No return value, Data written to destination address.



## NDS/DSi Decompression Callbacks


On NDS and DSi, the "ReadByCallback" variants are reading source data from
callback functions (rather than directly from memory). The callback functions
may read normal data from memory, or from other devices, such like directly
from the gamepak bus, without storing the source data in memory. The downside
is that the callback mechanism makes the function very slow, furthermore,
NDS7/NDS9 SWI 12h, 13h, 15h are using THUMB code, and variables on stack,
alltogether that makes the whole shit very-very-very slow.

```
  r2 = user defined callback parameter (passed on to Open function)
        (or, for Huffman: pointer to temp buffer, max 200h bytes needed)
  r3 = pointer to callback structure
```

Callback structure (five 32bit pointers to callback functions)

```
  Open_and_get_32bit (eg. LDR r0,[r0], get header)
  Close              (optional, 0=none)
  Get_8bit           (eg. LDRB r0,[r0])
  Get_16bit          (not used)
  Get_32bit          (used by Huffman only)
```

All functions may use ARM or THUMB code (indicated by address bit0). The
current source address (r0) is passed to all callback functions. Additionally,
the initial destination address (r1), and a user defined parameter (r2) are
passed to the Open function. For Huffman r2 must point to a temp buffer (max
200h bytes needed, internally used by the SWI function to make a copy of the
huffman tree; needed for random-access to the tree, which wouldn't work with
the sequentially reading callbacks).

All functions have return values in r0. The Open function normally returns the
first word (containing positive length and type), alternatively it may return a
negative error code to abort/reject decompression. The Close function, if it is
defined, should return zero (or any positive value), or a negative errorcode.
The other functions return raw data, without errorcodes. The SWI returns the
length of decompressed data, or the signed errorcode from the Open/Close
functions.




# <a name="biosmemorycopy"></a>BIOS Memory Copy



CpuFastSet

CpuSet



## SWI 0Ch (GBA/NDS7/NDS9/DSi7/DSi9) - CpuFastSet


Memory copy/fill in units of 32 bytes. Memcopy is implemented as repeated
LDMIA/STMIA [Rb]!,r2-r9 instructions. Memfill as single LDR followed by
repeated STMIA [Rb]!,r2-r9.

After processing all 32-byte-blocks, the NDS/DSi additonally processes the
remaining words as 4-byte blocks. BUG: The NDS/DSi uses the fast 32-byte-block
processing only for the first N bytes (not for the first N words), so only the
first quarter of the memory block is FAST, the remaining three quarters are
SLOWLY copied word-by-word.

The length is specifed as wordcount, ie. the number of bytes divided by 4.

On the GBA, the length should be a multiple of 8 words (32 bytes) (otherwise
the GBA is forcefully rounding-up the length). On NDS/DSi, the length may be
any number of words (4 bytes).

```
  r0    Source address        (must be aligned by 4)
  r1    Destination address   (must be aligned by 4)
  r2    Length/Mode
          Bit 0-20  Wordcount (GBA: rounded-up to multiple of 8 words)
          Bit 24    Fixed Source Address (0=Copy, 1=Fill by WORD[r0])
```

Return: No return value, Data written to destination address.



## SWI 0Bh (GBA/NDS7/NDS9/DSi7/DSi9) - CpuSet


Memory copy/fill in units of 4 bytes or 2 bytes. Memcopy is implemented as
repeated LDMIA/STMIA [Rb]!,r3 or LDRH/STRH r3,[r0,r5] instructions. Memfill as
single LDMIA or LDRH followed by repeated STMIA [Rb]!,r3 or STRH r3,[r0,r5].

The length must be a multiple of 4 bytes (32bit mode) or 2 bytes (16bit mode).
The (half)wordcount in r2 must be length/4 (32bit mode) or length/2 (16bit
mode), ie. length in word/halfword units rather than byte units.

```
  r0    Source address        (must be aligned by 4 for 32bit, by 2 for 16bit)
  r1    Destination address   (must be aligned by 4 for 32bit, by 2 for 16bit)
  r2    Length/Mode
          Bit 0-20  Wordcount (for 32bit), or Halfwordcount (for 16bit)
          Bit 24    Fixed Source Address (0=Copy, 1=Fill by {HALF}WORD[r0])
          Bit 26    Datasize (0=16bit, 1=32bit)
```

Return: No return value, Data written to destination address.


Note: On GBA, NDS7 and DSi7, these two functions will silently reject to do
anything if the source start or end addresses are reaching into the BIOS area.
The NDS9 and DSi9 don't have such read-proctections.




# <a name="bioshaltfunctions"></a>BIOS Halt Functions



Halt

IntrWait

VBlankIntrWait

Stop/Sleep

CustomHalt



## SWI 02h (GBA) or SWI 06h (NDS7/NDS9/DSi7/DSi9) - Halt


Halts the CPU until an interrupt request occurs. The CPU is switched into
low-power mode, all other circuits (video, sound, timers, serial, keypad,
system clock) are kept operating.

Halt mode is terminated when any enabled interrupts are requested, that is when
(IE AND IF) is not zero, the GBA locks up if that condition doesn't get true.
However, the state of CPUs IRQ disable bit in CPSR register, and the IME
register are don't care, Halt passes through even if either one has disabled
interrupts.

On GBA and NDS7/DSi7, Halt is implemented by writing to HALTCNT, Port 4000301h.
On NDS9/DSi9, Halt is implemted by writing to System Control Coprocessor (mov
p15,0,c7,c0,4,r0 opcode), this opcode hangs if IME=0.

No parameters, no return value.

(GBA/NDS7/DSi7: all registers unchanged, NDS9/DSi9: R0 destroyed)



## SWI 04h (GBA/NDS7/NDS9/DSi7/DSi9) - IntrWait ;DSi7/DSi9=bugged?


Continues to wait in Halt state until one (or more) of the specified
interrupt(s) do occur. The function forcefully sets IME=1. When using multiple
interrupts at the same time, this function is having less overhead than
repeatedly calling the Halt function.

```
  r0    0=Return immediately if an old flag was already set (NDS9: bugged!)
        1=Discard old flags, wait until a NEW flag becomes set
  r1    Interrupt flag(s) to wait for (same format as IE/IF registers)
  r2    DSi7 only: Extra flags (same format as DSi7's IE2/IF2 registers)
```

Caution: When using IntrWait or VBlankIntrWait, the user interrupt handler MUST
update the BIOS Interrupt Flags value in RAM; when acknowleding processed
interrupt(s) by writing a value to the IF register, the same value should be
also ORed to the BIOS Interrupt Flags value, at following memory location:

```
  Host     GBA (16bit)  NDS7 (32bit)  NDS9 (32bit)  DSi7-IF2 (32bit)
  Address  [3007FF8h]   [380FFF8h]    [DTCM+3FF8h]  [380FFC0h]
```

NDS9: BUG: No Discard (r0=0) doesn't work. The function always waits for at
least one IRQ to occur (no matter which, including IRQs that are not selected
in r1), even if the desired flag was already set. NB. the same bug is also
found in the GBA/NDS7 functions, but it's compensated by a second bug, ie. the
GBA/NDS7 functions are working okay because their "bug doesn't work".

Return: No return value, the selected flag(s) are automatically reset in BIOS
Interrupt Flags value in RAM upon return.

DSi9: BUG: The function tries to enter Halt state via Port 4000301h (which
would be okay on ARM7, but it's probably ignored on ARM9, which should normally
use CP15 to enter Halt state; if Port 4000301h is really ignored, then the
function will "successfully" wait for interrupts, but without actually entering
any kind of low power mode).

DSi7: BUG: The function tries to wait for IF and IF2 interrupts, but it does
accidently ignore the old IF interrupts, and works only with new IF2 ones.



## SWI 05h (GBA/NDS7/NDS9/DSi7/DSi9) - VBlankIntrWait ;DSi7/DSi9=bugged?


Continues to wait in Halt status until a new V-Blank interrupt occurs.

The function sets r0=1 and r1=1 (plus r2=0 on DSi7) and does then execute
IntrWait (SWI 04h), see IntrWait for details.

No parameters, no return value.



## SWI 03h (GBA) - Stop


Switches the GBA into very low power mode (to be used similar as a
screen-saver). The CPU, System Clock, Sound, Video, SIO-Shift Clock, DMAs, and
Timers are stopped.

Stop state can be terminated by the following interrupts only (as far as
enabled in IE register): Joypad, Game Pak, or General-Purpose-SIO.

"The system clock is stopped so the IF flag is not set."

Preparation for Stop:

Disable Video before implementing Stop (otherwise Video just freezes, but still
keeps consuming battery power). Possibly required to disable Sound also?
Obviously, it'd be also recommended to disable any external hardware (such like
Rumble or Infra-Red) as far as possible.

No parameters, no return value.



## SWI 07h (NDS7/DSi7) - Sleep


No info, probably similar as GBA SWI 03h (Stop). Sleep is implemented for ARM7
only, not for ARM9. But maybe the ARM7 function does stop \<both> ARM7 and
ARM9 (?)



## SWI 27h (GBA) or SWI 1Fh (NDS7/DSi7) - CustomHalt (Undocumented)


Writes the 8bit parameter value to HALTCNT, below values are equivalent to Halt
and Stop/Sleep functions, other values reserved, purpose unknown.

```
  r2  8bit parameter (GBA: 00h=Halt, 80h=Stop) (NDS7/DSi7: 80h=Halt, C0h=Sleep)
```

No return value.




# <a name="biosresetfunctions"></a>BIOS Reset Functions



SoftReset

RegisterRamReset

HardReset



## SWI 00h (GBA/NDS7/NDS9) - SoftReset


Clears 200h bytes of RAM (containing stacks, and BIOS IRQ vector/flags),
initializes system, supervisor, and irq stack pointers, sets R0-R12, LR\_svc,
SPSR\_svc, LR\_irq, and SPSR\_irq to zero, and enters system mode.

Note that the NDS9 stack registers are hardcoded (the DTCM base should be set
to the default setting of 0800000h). The NDS9 function additionally flushes
caches and write buffer, and sets the CP15 control register to 12078h.

```
  Host  sp_svc    sp_irq    sp_sys    zerofilled area       return address
  GBA   3007FE0h  3007FA0h  3007F00h  [3007E00h..3007FFFh]  Flag[3007FFAh]
  NDS7  380FFDCh  380FFB0h  380FF00h  [380FE00h..380FFFFh]  Addr[27FFE34h]
  NDS9  0803FC0h  0803FA0h  0803EC0h  [DTCM+3E00h..3FFFh]   Addr[27FFE24h]
```

The NDS7/NDS9 return addresses at [27FFE34h/27FFE24h] are usually containing
copies of Cartridge Header [034h/024h] entry points, which may select ARM/THUMB
state via bit0. The GBA return address 8bit flag is interpreted as 00h=8000000h
(ROM), or 01h-FFh=2000000h (RAM), entered in ARM state.

Note: The reset is applied only to the CPU that has executed the SWI (ie. on
the NDS, the other CPU will remain unaffected).

Return: Does not return to calling procedure, instead, loads the above return
address into R14, and then jumps to that address by a "BX R14" opcode.



## SWI 01h (GBA) - RegisterRamReset


Resets the I/O registers and RAM specified in ResetFlags. However, it does not
clear the CPU internal RAM area from 3007E00h-3007FFFh.

```
  r0  ResetFlags
       Bit   Expl.
       0     Clear 256K on-board WRAM  ;-don't use when returning to WRAM
       1     Clear 32K on-chip WRAM    ;-excluding last 200h bytes
       2     Clear Palette
       3     Clear VRAM
       4     Clear OAM              ;-zerofilled! does NOT disable OBJs!
       5     Reset SIO registers    ;-switches to general purpose mode!
       6     Reset Sound registers
       7     Reset all other registers (except SIO, Sound)
```

Return: No return value.

Bug: LSBs of SIODATA32 are always destroyed, even if Bit5 of R0 was cleared.

The function always switches the screen into forced blank by setting
DISPCNT=0080h (regardless of incoming R0, screen becomes white).



## SWI 26h (GBA) - HardReset (Undocumented)


This function reboots the GBA (including for getting through the time-consuming
nintendo intro, which is making the function particularly useless and
annoying).

Parameters: None. Return: Never/Reboot.

Execution Time: About 2 seconds (!)




# <a name="biosmiscfunctions"></a>BIOS Misc Functions



GetBiosChecksum

WaitByLoop

GetCRC16

IsDebugger

GetSineTable

GetPitchTable

GetVolumeTable

CustomPost

GetBootProcs



## SWI 0Dh (GBA) - GetBiosChecksum (Undocumented)


Calculates the checksum of the BIOS ROM (by reading in 32bit units, and adding
up these values). IRQ and FIQ are disabled during execution.

The checksum is BAAE187Fh (GBA and GBA SP), or BAAE1880h (NDS/3DS in GBA mode,
whereas the only difference is that the byte at [3F0Ch] is changed from 00h to
01h, otherwise the BIOS is 1:1 same as GBA BIOS, it does even include multiboot
code).

Parameters: None. Return: r0=Checksum.



## SWI 03h (NDS7/NDS9/DSi7/DSi9) - WaitByLoop


Performs a "LOP: SUB R0,1 / BGT LOP" wait loop, the loop is executed in BIOS
memory, which provides reliable timings (regardless of the memory waitstates
& cache state of the calling procedure). Intended only for short delays
(eg. flash memory programming cycles).

```
  r0  Delay value (should be in range 1..7FFFFFFFh)
```

Execution time varies for ARM7 vs ARM9. On ARM9 it does also depend on whether
ROM is cached, and on DSi it does further depended on the ARM9 CPU clock, and
on whether using NDS or DSi BIOS ROM (NDS uses faster THUMB code, whilst DSi
uses ARM code, which is slow on uncached ARM9 ROM reads). For example, to get a
1 millisecond delay, use following values:

```
  CPU  Clock     Cache   BIOS     Value for 1ms
  ARM7 33.51MHz  none    NDS/DSi  r0=20BAh    ;=20BAh  ;-ARM7
  ARM9 67.03MHz  on      NDS/DSi  r0=20BAh*2  ;=4174h  ;\ARM9 with cache
  ARM9 134.06MHz on      DSi      r0=20BAh*4  ;=82E8h  ;/
  ARM9 67.03MHz  off     NDS      r0=20BAh/2  ;=105Dh  ;\
  ARM9 67.03MHz  off     DSi      r0=20BAh/4  ;=082Eh  ; ARM9 without cache
  ARM9 134.06MHz off     DSi      r0=20BAh/3  ;=0AE8h  ;/
```

Return: No return value.



## SWI 0Eh (NDS7/NDS9/DSi7/DSi9) - GetCRC16


```
  r0  Initial CRC value (16bit, usually FFFFh)
  r1  Start Address   (must be aligned by 2)
  r2  Length in bytes (must be aligned by 2)
```

CRC16 checksums can be calculated as such:

```
  val[0..7] = C0C1h,C181h,C301h,C601h,CC01h,D801h,F001h,A001h
  for i=start to end
    crc=crc xor byte[i]
    for j=0 to 7
      crc=crc shr 1:if carry then crc=crc xor (val[j] shl (7-j))
    next j
  next i
```

Return:

```
  r0  Calculated 16bit CRC Value
```

Additionally, if the length is nonzero, r3 contains the last processed halfword
at [addr+len-2]. Unlike most other NDS7/DSi7 SWI functions (which do reject
reading from BIOS memory), this allows to dump the NDS7/DSi7 BIOS (except for
the memory region that is locked via BIOSPROT Port 4000308h).



## SWI 0Fh (NDS7/NDS9) - IsDebugger


Detects if 4MB (normal) or 8MB (debug version) Main RAM installed.

Caution: Fails on ARM9 when cache is enabled (always returns 8MB state).

Return: r0 = result (0=normal console 4MB, 1=debug version 8MB)

Destroys halfword at [27FFFFAh] (NDS7) or [27FFFF8h] (NDS9)!

The SWI 0Fh function doesn't work stable if it gets interrupted by an interrupt
which is calling SWI 0Fh, which would destroy the above halfword scratch value
(unless the IRQ handler has saved/restored the halfword).



## SWI 1Ah (NDS7/DSi7) - GetSineTable


```
  r0  Index (0..3Fh) (must be in that range, otherwise returns garbage)
```

Return: r0 = Desired Entry (0000h..7FF5h) ;SIN(0 .. 88.6 degrees)\*8000h



## SWI 1Bh (NDS7/DSi7) - GetPitchTable (DSi7: bugged)


```
  r0  Index (0..2FFh) (must be in that range, otherwise returns garbage)
```

BUG: DSi7 accidently reads from SineTable instead of PitchTable, as workaround
for obtaining PitchTable values, one can set "r0=(0..2FFh)-46Ah" on DSi.

Return: r0 = Desired Entry (0000h..FF8Ah) (unsigned)



## SWI 1Ch (NDS7/DSi7) - GetVolumeTable


```
  r0  Index (0..2D3h) (must be in that range, otherwise returns garbage)
```

Return: r0 = Desired Entry (00h..7Fh) (unsigned)



## SWI 1Fh (NDS9/DSi9) - CustomPost


Writes to the POSTFLG register, probably for use by Firmware boot procedure.

```
  r0  32bit value, to be written to POSTFLG, Port 4000300h
```

Return: No return value.



## SWI 1Dh (NDS7/DSi7) - GetBootProcs


Returns addresses of Gamecart boot procedure/interrupt handler, probably for
use by Firmware boot procedure. Most of the returned NDS7 functions won't work
if the POSTFLG register is set.

The return values are somewhat XORed by each other (on DSi7 most of the values
are zero; which does rather negate the XORing effect, and, as a special
gimmick, one of the zero values is XORed by incoming r2).




# <a name="biosmultibootsinglegamepak"></a>BIOS Multi Boot (Single Game Pak)



MultiBoot



## SWI 25h (GBA) - MultiBoot


This function uploads & starts program code to slave GBAs, allowing to
launch programs on slave units even if no cartridge is inserted into the slaves
(this works because all GBA BIOSes contain built-in download procedures in
ROM).

However, the SWI 25h BIOS upload function covers only 45% of the required
Transmission Protocol, the other 55% must be coded in the master cartridge (see
Transmission Protocol below).

```
  r0  Pointer to MultiBootParam structure
  r1  Transfer Mode (undocumented)
       0=256KHz, 32bit, Normal mode    (fast and stable)
       1=115KHz, 16bit, MultiPlay mode (default, slow, up to three slaves)
       2=2MHz,   32bit, Normal mode    (fastest but maybe unstable)
  Note: HLL-programmers that are using the MultiBoot(param_ptr) macro cannot
  specify the transfer mode and will be forcefully using MultiPlay mode.
```

Return:

```
  r0  0=okay, 1=failed
```

See below for more details.



## Multiboot Parameter Structure


Size of parameter structure should be 4Ch bytes (the current GBA BIOS uses only
first 44h bytes though). The following entries must be set before calling SWI
25h:

```
  Addr Size Name/Expl.
  14h  1    handshake_data (entry used for normal mode only)
  19h  3    client_data[1,2,3]
  1Ch  1    palette_data
  1Eh  1    client_bit (Bit 1-3 set if child 1-3 detected)
  20h  4    boot_srcp  (typically 8000000h+0C0h)
  24h  4    boot_endp  (typically 8000000h+0C0h+length)
```

The transfer length (excluding header data) should be a multiple of 10h,
minimum length 100h, max 3FF40h (ca. 256KBytes). Set palette\_data as
"81h+color\*10h+direction\*8+speed\*2", or as "0f1h+color\*2" for fixed palette,
whereas color=0..6, speed=0..3, direction=0..1. The other entries
(handshake\_data, client\_data[1-3], and client\_bit) must be same as specified in
Transmission Protocol (see below hh,cc,y).



## Multiboot Transfer Protocol


Below describes the complete transfer protocol, normally only the Initiation
part must be programmed in the master cartridge, the main data transfer can be
then performed by calling SWI 25h, the slave program is started after SWI 25h
completion.

The ending handshake is normally not required, when using it, note that you
will need custom code in BOTH master and slave programs.

```
  Times  Send   Receive  Expl.
  -----------------------Required Transfer Initiation in master program
  ...    6200   FFFF     Slave not in multiplay/normal mode yet
  1      6200   0000     Slave entered correct mode now
  15     6200   720x     Repeat 15 times, if failed: delay 1/16s and restart
  1      610y   720x     Recognition okay, exchange master/slave info
  60h    xxxx   NN0x     Transfer C0h bytes header data in units of 16bits
  1      6200   000x     Transfer of header data completed
  1      620y   720x     Exchange master/slave info again
  ...    63pp   720x     Wait until all slaves reply 73cc instead 720x
  1      63pp   73cc     Send palette_data and receive client_data[1-3]
  1      64hh   73uu     Send handshake_data for final transfer completion
  -----------------------Below is SWI 25h MultiBoot handler in BIOS
  DELAY  -      -        Wait 1/16 seconds at master side
  1      llll   73rr     Send length information and receive random data[1-3]
  LEN    yyyy   nnnn     Transfer main data block in units of 16 or 32 bits
  1      0065   nnnn     Transfer of main data block completed, request CRC
  ...    0065   0074     Wait until all slaves reply 0075 instead 0074
  1      0065   0075     All slaves ready for CRC transfer
  1      0066   0075     Signalize that transfer of CRC follows
  1      zzzz   zzzz     Exchange CRC must be same for master and slaves
  -----------------------Optional Handshake (NOT part of master/slave BIOS)
  ...    ....   ....     Exchange whatever custom data
```

Legend for above Protocol

```
  y     client_bit, bit(s) 1-3 set if slave(s) 1-3 detected
  x     bit 1,2,or 3 set if slave 1,2,or 3
  xxxx  header data, transferred in 16bit (!) units (even in 32bit normal mode)
  nn    response value for header transfer, decreasing 60h..01h
  pp    palette_data
  cc    random client_data[1..3] from slave 1-3, FFh if slave not exists
  hh    handshake_data, 11h+client_data[1]+client_data[2]+client_data[3]
  uu    random data, not used, ignore this value
```

Below automatically calculated by SWI 25h BIOS function (don't care about)

```
  llll  download length/4-34h
  rr    random data from each slave for encryption, FFh if slave not exists
  yyyy  encoded data in 16bit (multiplay) or 32bit (normal mode) units
  nnnn  response value, lower 16bit of destadr in GBA memory (00C0h and up)
  zzzz  16bit download CRC value, must be same for master and slaves
```

Pseudo Code for SWI 25h Transfer with Checksum and Encryption calculations

```
  if normal_mode    then c=C387h:x=C37Bh:k=43202F2Fh
  if multiplay_mode then c=FFF8h:x=A517h:k=6465646Fh
  m=dword(pp,cc,cc,cc):f=dword(hh,rr,rr,rr)
  for ptr=000000C0h to (file_size-4) step 4
    c=c xor data[ptr]:for i=1 to 32:c=c shr 1:if carry then c=c xor x:next
    m=(6F646573h*m)+1
    send_32_or_2x16 (data[ptr] xor (-2000000h-ptr) xor m xor k)
  next
  c=c xor f:for i=1 to 32:c=c shr 1:if carry then c=c xor x:next
  wait_all_units_ready_for_checksum:send_32_or_1x16 (c)
```

Whereas, explained: c=chksum,x=chkxor,f=chkfin,k=keyxor,m=keymul



## Multiboot Communication


In Multiplay mode, master sends 16bit data, and receives 16bit data from each
slave (or FFFFh if none). In Normal mode, master sends 32bit data (upper 16bit
zero, lower 16bit as for multiplay mode), and receives 32bit data (upper 16bit
as for multiplay mode, and lower 16bit same as lower 16bit previously sent by
master). Because SIODATA32 occupies same addresses as SIOMULTI0-1, the same
transfer code can be used for both multiplay and normal mode (in normal mode
SIOMULTI2-3 should be forced to FFFFh though). After each transfer, master
should wait for Start bit cleared in SIOCNT register, followed by a 36us delay.

Note: The multiboot slave would also recognize data being sent in Joybus mode,
however, master GBAs cannot use joybus mode (because GBA hardware cannot act as
master in joybus mode).



## Multiboot Slave Header


The transferred Header block is written to 2000000-20000BFh in slave RAM, the
header must contain valid data (identically as for normal ROM-cartridge
headers, including a copy of the Nintendo logo, correct header CRC, etc.), in
most cases it'd be recommended just to transfer a copy of the master cartridges
header from 8000000h-80000BFh.



## Multiboot Slave Program/Data


The transferred main program/data block is written to 20000C0h and up (max
203FFFFh) in slave RAM, note that absolute addresses in the program must be
then originated at 2000000h rather than 8000000h. In case that the master
cartridge is 256K or less, it could just transfer a copy of the whole cartridge
at 80000C0h and up, the master should then copy & execute its own ROM data
into RAM as well.



## Multiboot Slave Extended Header


For Multiboot slaves, separate Entry Point(s) must be defined at the beginning
of the Program/Data block (the Entry Point in the normal header is ignored),
also some reserved bytes in this section are overwritten by the Multiboot
procedure. For more information see chapter about Cartridge Header.



## Multiboot Slave with Cartridge


Beside for slaves without cartridge, multiboot can be also used for slaves
which do have a cartridge inserted, if so, SELECT and START must be kept held
down during power-on in order to switch the slave GBA into Multiboot mode (ie.
to prevent it from starting the cartridge as normally).

The general idea is to enable newer programs to link to any existing older GBA
programs, even if these older programs originally didn't have been intended to
support linking.

The uploaded program may access the slaves SRAM, Flash ROM, or EEPROM (if any,
allowing to read out or modify slave game positions), as well as cartridge ROM
at 80000A0h-8000FFFh (the first 4KBytes, excluding the nintendo logo, allowing
to read out the cartridge name from the header, for example).

The main part of the cartridge ROM is meant to be locked out in order to
prevent software pirates from uploading "intruder" programs which would send
back a copy of the whole cartridge to the master, however, for good or evil, at
present time, current GBA models and GBA carts do not seem to contain any such
protection.



## Uploading Programs from PC


Beside for the ability to upload a program from one GBA to another, this
feature can be also used to upload small programs from a PC to a GBA. For more
information see chapter about External Connectors.



## Nintendo DS


The GBA multiboot function requires a link port, and so, works on GBA and GBA
SP only. The Nintendo DS in GBA mode does include the multiboot BIOS function,
but it won't be of any use as the DS doesn't have a link port.




# <a name="biossoundfunctions"></a>BIOS Sound Functions



MidiKey2Freq

SoundBias

SoundChannelClear

SoundDriverInit

SoundDriverMain

SoundDriverMode

SoundDriverVSync

SoundDriverVSyncOff

SoundDriverVSyncOn

SoundWhatever0..4

SoundGetJumpList



## SWI 1Fh (GBA) - MidiKey2Freq


Calculates the value of the assignment to ((SoundArea)sa).vchn[x].fr when
playing the wave data, wa, with the interval (MIDI KEY) mk and the fine
adjustment value (halftones=256) fp.

```
  r0  WaveData* wa
  r1  u8 mk
  r2  u8 fp
```

Return:

```
  r0  u32
```

This function is particularly popular because it allows to read from BIOS
memory without copy protection range checks. The formula to read one byte (a)
from address (i, 0..3FFF) is:

a = (MidiKey2Freq(i-(((i AND 3)+1)OR 3), 168, 0) \* 2) SHR 24



## SWI 19h (GBA) or SWI 08h (NDS7/DSi7) - SoundBias


Increments or decrements the current level of the SOUNDBIAS register (with
short delays) until reaching the desired new level. The upper bits of the
register are kept unchanged.

```
  r0   BIAS level (0=Level 000h, any other value=Level 200h)
  r1   Delay Count (NDS/DSi only) (GBA uses a fixed delay count of 8)
```

Return: No return value.



## SWI 1Eh (GBA) - SoundChannelClear


Clears all direct sound channels and stops the sound.

This function may not operate properly when the library which expands the sound
driver feature is combined afterwards. In this case, do not use it.

No parameters, no return value.



## SWI 1Ah (GBA) - SoundDriverInit


Initializes the sound driver. Call this only once when the game starts up.

It is essential that the work area already be secured at the time this function
is called.

You cannot execute this driver multiple times, even if separate work areas have
been prepared.

```
  r0  Pointer to work area for sound driver, SoundArea structure as follows:
       SoundArea (sa) Structure
        u32    ident      Flag the system checks to see whether the
                          work area has been initialized and whether it
                          is currently being accessed.
        vu8    DmaCount   User access prohibited
        u8     reverb     Variable for applying reverb effects to direct sound
        u16    d1         User access prohibited
        void   (*func)()  User access prohibited
        int    intp       User access prohibited
        void*  NoUse      User access prohibited
        SndCh  vchn[MAX]  The structure array for controlling the direct
                          sound channels (currently 8 channels are
                          available). The term "channel" here does
                          not refer to hardware channels, but rather to
                          virtual constructs inside the sound driver.
        s8     pcmbuf[PCM_BF*2]
       SoundChannel Structure
        u8         sf     The flag indicating the status of this channel.
                          When 0 sound is stopped.
                          To start sound, set other parameters and
                          then write 80h to here.
                          To stop sound, logical OR 40h for a
                          release-attached off (key-off), or write zero
                          for a pause. The use of other bits is
                          prohibited.
        u8         r1     User access prohibited
        u8         rv     Sound volume output to right side
        u8         lv     Sound volume output to left side
        u8         at     The attack value of the envelope. When the
                          sound starts, the volume begins at zero and
                          increases every 1/60 second. When it
                          reaches 255, the process moves on to the
                          next decay value.
        u8         de     The decay value of the envelope. It is
                          multiplied by "this value/256" every 1/60
                          sec. and when sustain value is reached, the
                          process moves to the sustain condition.
        u8         su     The sustain value of the envelope. The
                          sound is sustained by this amount.
                          (Actually, multiplied by rv/256, lv/256 and
                          output left and right.)
        u8         re     The release value of the envelope. Key-off
                          (logical OR 40h in sf) to enter this state.
                          The value is multiplied by "this value/256"
                          every 1/60 sec. and when it reaches zero,
                          this channel is completely stopped.
        u8         r2[4]  User access prohibited
        u32        fr     The frequency of the produced sound.
                          Write the value obtained with the
                          MidiKey2Freq function here.
        WaveData*  wp     Pointer to the sound's waveform data. The waveform
                          data can be generated automatically from the AIFF
                          file using the tool (aif2agb.exe), so users normally
                          do not need to create this themselves.
        u32        r3[6]  User access prohibited
        u8         r4[4]  User access prohibited
       WaveData Structure
        u16   type    Indicates the data type. This is currently not used.
        u16   stat    At the present time, non-looped (1 shot) waveform
                      is 0000h and forward loop is 4000h.
        u32   freq    This value is used to calculate the frequency.
                      It is obtained using the following formula:
                      sampling rate x 2^((180-original MIDI key)/12)
        u32   loop    Loop pointer (start of loop)
        u32   size    Number of samples (end position)
        s8    data[]  The actual waveform data. Takes (number of samples+1)
                      bytes of 8bit signed linear uncompressed data. The last
                      byte is zero for a non-looped waveform, and the same
                      value as the loop pointer data for a looped waveform.
```

Return: No return value.



## SWI 1Ch (GBA) - SoundDriverMain


Main of the sound driver.

Call every 1/60 of a second. The flow of the process is to call
SoundDriverVSync, which is explained later, immediately after the V-Blank
interrupt.

After that, this routine is called after BG and OBJ processing is executed.

No parameters, no return value.



## SWI 1Bh (GBA) - SoundDriverMode


Sets the sound driver operation mode.

```
  r0  Sound driver operation mode
       Bit    Expl.
       0-6    Direct Sound Reverb value (0-127, default=0) (ignored if Bit7=0)
       7      Direct Sound Reverb set (0=ignore, 1=apply reverb value)
       8-11   Direct Sound Simultaneously-produced (1-12 channels, default 8)
       12-15  Direct Sound Master volume (1-15, default 15)
       16-19  Direct Sound Playback Frequency (1-12 = 5734,7884,10512,13379,
              15768,18157,21024,26758,31536,36314,40137,42048, def 4=13379 Hz)
       20-23  Final number of D/A converter bits (8-11 = 9-6bits, def. 9=8bits)
       24-31  Not used.
```

Return: No return value.



## SWI 1Dh (GBA) - SoundDriverVSync


An extremely short system call that resets the sound DMA. The timing is
extremely critical, so call this function immediately after the V-Blank
interrupt every 1/60 second.

No parameters, no return value.



## SWI 28h (GBA) - SoundDriverVSyncOff


Due to problems with the main program if the V-Blank interrupts are stopped,
and SoundDriverVSync cannot be called every 1/60 a second, this function must
be used to stop sound DMA.

Otherwise, even if you exceed the limit of the buffer the DMA will not stop and
noise will result.

No parameters, no return value.



## SWI 29h (GBA) - SoundDriverVSyncOn


This function restarts the sound DMA stopped with the previously described
SoundDriverVSyncOff.

After calling this function, have a V-Blank occur within 2/60 of a second and
call SoundDriverVSync.

No parameters, no return value.



## SWI 20h..24h (GBA) - SoundWhatever0..4 (Undocumented)


Whatever undocumented sound-related BIOS functions.



## SWI 2Ah (GBA) - SoundGetJumpList (Undocumented)


Receives pointers to 36 additional sound-related BIOS functions.

```
  r0  Destination address (must be aligned by 4) (120h bytes buffer)
```




# <a name="biossha1functionsdsionly"></a>BIOS SHA1 Functions (DSi only)



SHA1\_Init(struct)

SHA1\_Update(struct,src,srclen)

SHA1\_Finish(dst,struct)

SHA1\_Init\_Update\_Finish(dst,src,srclen)

SHA1\_Init\_Update\_Finish\_Mess(dst,dstlen,src,srclen)

SHA1\_Compare\_20\_Bytes(src1,src2)

SHA1\_Default\_Callback(struct,src,len)



## SWI 24h (DSi9/DSi7) - SHA1\_Init(struct)


Initializes a 64h-byte structure for SHA1 calculations:

```
  [struct+00h] = 67452301h      ;\
  [struct+04h] = EFCDAB89h      ;
  [struct+08h] = 98BADCFEh      ; initial SHA1 checksum value
  [struct+0Ch] = 10325476h      ;
  [struct+10h] = C3D2E1F0h      ;/
  [struct+14h] = 00000000h ;lsw ;\total len in bits, initially zero
  [struct+18h] = 00000000h ;msw ;/
  [struct+1Ch] = uninitialzed   ;-buffer for incomplete fragment (40h bytes)
  [struct+5Ch] = 00000000h      ;-incomplete fragment size
  if [struct+60h] = 00000000h then [struct+60h] = SHA1_Default_Callback
```

Observe that the incoming [struct+60h] value should be 00000000h, otherwise the
default callback isn't installed (using a different callback doesn't make too
much sense, and it's probably not done by any DSi programs) (the callback
feature might be intended to mount hardware accelleration, or to hook,
customize, encrypt, or replace the SHA1 functionality).



## SWI 25h (DSi9/DSi7) - SHA1\_Update(struct,src,srclen)


This function should be placed between Init and Finish. The Update function can
be called multiple times if the source data is split into separate blocks.
There's no alignment requirement (though the function works faster if src is
4-byte aligned).

```
  [struct+14h]=[struct+14h]+len*8  ;64bit value ;-raise total len in bits
  if [struct+5Ch]<>0 and [struct+5Ch]+len>=40h  ;\
    for i=[struct+5Ch] to 3Fh                   ; merge old incomplete chunk
      [struct+1Ch+i]=[src], src=src+1, len=len-1; with new data and process it
    SHA1_Callback(struct,struct+1Ch,40h)        ; (if it gives a full chunk)
    [struct+5Ch]=0                              ;/
  if len>=40h then                              ;\process full 40h-byte chunks
    SHA1_Callback(struct,src,len AND NOT 3Fh)   ; (if src isn't 4-byte aligned
    src=src+(len AND NOT 3Fh)                   ; then the DSi BIOS internally
    len=len AND 3Fh                             ;/copies all chunks to struct)
  if len>0 then                                 ;\
    for i=[struct+5Ch] to [struct+5Ch]+len-1    ; memorize remaining bytes
      [struct+1Ch+i]=[src], src=src+1, len=len-1; as incomplete chunk
      [struct+5Ch]=[struct+5Ch]+1               ;/
```



## SWI 26h (DSi9/DSi7) - SHA1\_Finish(dst,struct)


```
  [total_len]=bswap8byte([struct+14h]) ;get total len in bits in big-endian
  SHA1_Update(struct,value_80h,1)                            ;append end byte
  while [struct+5Ch]<>38h do SHA1_Update(struct,value_00h,1) ;append padding
  SHA1_Update(struct,total_len,8)                            ;append 64bit len
  [struct+14h]=bswap8byte([total_len]) ;restore total len, exclude above update
  [dst+00h]=bswap([struct+00h]  ;msw   ;\
  [dst+04h]=bswap([struct+04h]         ; store SHA1 result at dst
  [dst+08h]=bswap([struct+08h]         ; (in big-endian)
  [dst+0Ch]=bswap([struct+0Ch]         ;
  [dst+10h]=bswap([struct+10h]  ;lsw   ;/
```



## SHA1\_Default\_Callback(struct,src,len)


```
  for j=1 to len/40h
    a=[struct+0], b=[struct+4], c=[struct+8], d=[struct+0Ch], e=[struct+10h]
    for i=0 to 79
      if i=0..15  then w[i] = bswap([src]), src=src+4
      if i=16..79 then w[i] = (w[i-3] xor w[i-8] xor w[i-14] xor w[i-16]) rol 1
      if i=0..19  then f=5A827999h + e + (d xor (b and (c xor d)))
      if i=20..39 then f=6ED9EBA1h + e + (b xor c xor d)
      if i=40..59 then f=8F1BBCDCh + e + ((b and c) or (d and (b or c)))
      if i=60..79 then f=CA62C1D6h + e + (b xor c xor d)
      e=d, d=c, c=(b ror 2), b=a, a=f + (a rol 5) + w[i]
    [struct+0]=[struct+0]+a, [struct+4]=[struct+4]+b, [struct+8]=[struct+8]+c
    [struct+0Ch]=[struct+0Ch]+d, [struct+10h]=[struct+10h]+e
```



## SWI 27h (DSi9/DSi7) - SHA1\_Init\_Update\_Finish(dst,src,srclen)


```
  [struct+60h]=00000000h  ;want Init to install the default SHA1 callback
  SHA1_Init(struct)
  SHA1_Update(struct,src,srclen)
  SHA1_Finish(dst,struct)
```

Always returns r0=1.



## SWI 29h (DSi9/DSi7) - SHA1\_Init\_Update\_Finish\_Mess(dst,dstlen,src,srclen)


```
  if dst=0 then exit(r0=1)  ;uh, that's same return value as when okay
  if src=0 and srclen<>0 then exit(r0=0)
  [struct+60h]=00000000h                ;\
  SHA1_Init(struct)                     ; first compute normal SHA1
  SHA1_Update(struct,src,srclen)        ; (same as SHA1_Init_Update_Finish)
  SHA1_Finish(first_sha1,struct)        ;/
 @@lop1:
  i=13h  ;start with LSB of big-endian 20-byte value  ;\increment SHA1 value
 @@lop2:                                              ; by one (with somewhat
  [first_sha1+i]=[first_sha1+i]+1, i=i-1              ; uncommon/bugged carry-
  if i>=0 and [first_sha1+i+1]=01h then goto @@lop2   ;/out to higher bytes)
  SHA1_Update(struct,first_sha1,14h)    ;\compute 2nd SHA1 across 1st SHA1,
  SHA1_Finish(second_sha1,struct)       ;/done without re-initializing struct
  for i=0 to min(14h,dstlen)-1, [dst]=[second_sha1+i], dst=dst+1
  dstlen=dstlen-min(14h,dstlen)
  if dstlen<>0 then goto @@lop1 else exit(r0=1)
```



## SHA1\_Init\_Update\_Finish\_HMAC(dst,key,src,srclen)


```
  if len(key)>40h then key=SHA1(key) ;convert LONG keys to 14h-bytes length
  if len(key)<40h then zero-pad key to 40h-bytes length
  for i=0 to 3Fh, [inner_key+i]=[key+i] xor 36h ;\
  [struct+60h]=00000000h                        ;
  SHA1_Init(struct)                             ; compute 1st SHA1
  SHA1_Update(struct,inner_key,40h)             ; across inner key and data
  SHA1_Update(struct,src,srclen)                ;
  SHA1_Finish(first_sha1,struct)                ;/
  for i=0 to 3Fh, [outer_key+i]=[key+i] xor 5Ch ;\
  [struct+60h]=00000000h                        ;
  SHA1_Init(struct)                             ; compute final SHA1
  SHA1_Update(struct,outer_key,40h)             ; across outer key and 1st SHA1
  SHA1_Update(struct,first_sha1,14h)            ;
  SHA1_Finish(dst,struct)                       ;/
```



## SWI 28h (DSi9/DSi7) - SHA1\_Compare\_20\_Bytes(src1,src2)


Out: r0=1=match, r0=0=mismatch/error (error occurs if src1=0 or src2=0).




# <a name="biosrsafunctionsdsionly"></a>BIOS RSA Functions (DSi only)



RSA\_Init\_crypto\_heap(heap\_nfo,heap\_start,heap\_size)

RSA\_Decrypt(heap\_nfo,struct,dest4)

RSA\_Decrypt\_Unpad(heap\_nfo,dst,src,key)

RSA\_Decrypt\_Unpad\_OpenPGP\_SHA1(heap\_nfo,dst,src,key)


RSA is important because the DSi cartridge header and system files do contain
RSA signatures. Which makes it impossible to create unlicensed software
(without knowing Nintendo's private key).

- [BIOS RSA Basics](#biosrsabasics)
- [BIOS RSA Pseudo Code](#biosrsapseudocode)


## SWI 20h (DSi9/DSi7) - RSA\_Init\_crypto\_heap(heap\_nfo,heap\_start,heap\_size)


Initializes the heap for use with SWI 21h..23h. heap\_nfo is a 0Ch-byte
structure, which gets set to:

```
  [heap_nfo+0] = heap_start (rounded-up to 4-byte boundary)
  [heap_nfo+4] = heap_end   (start+size, rounded-down to 4-byte boundary)
  [heap_nfo+8] = heap_size  (matched to above rounded values)
```

heap\_start should point to a free memory block which will be used as heap,
heap\_size should be usually 1000h.



## SWI 21h (DSi9/DSi7) - RSA\_Decrypt(heap\_nfo,ptr\_nfo,len\_dest)


```
  [ptr_nfo+0] = dst (usually 7Fh bytes, max 80h bytes)
  [ptr_nfo+4] = src (80h bytes)
  [ptr_nfo+8] = key (80h bytes)
```

This is a subfunction for SWI 22h/23h. It's returning raw decrypted data
without unpadding (aside from stripping leading 00h bytes; most (or all)
signatures are containing one leading 00h byte, so the returned [len\_dest]
value will be usually 7Fh).

Return value (r0) is: 0=failed, 1=okay. The length of the decrypted data is
returned at [len\_dest].



## SWI 22h (DSi9/DSi7) - RSA\_Decrypt\_Unpad(heap\_nfo,dst,src,key)


Same as SWI 21h, and additionally removes the "01h,min eight FFh,00h" padding.
src,dst,key should be 80h-bytes. The output at dst can be theoretically max
80h-bytes (or shorter due to removed padding). In practice, the DSi is often
using only the first 14h-bytes at dst (aka the last 14h-bytes from src) as SHA1
or SHA1-HMAC value (RSA-SHA1). Return value (r0) is: 0=failed, 1=okay.



## SWI 23h (DSi9/DSi7) - RSA\_Decrypt\_Unpad\_OpenPGP\_SHA1(heap\_nfo,dst,src,key)


Same as SWI 22h, but with some extra processing for extracting a SHA1 value
from an OpenPGP header: The data must consist of five ASN.1 (DER) chunks (with
Tag IDs 30h,30h,06h,05h,04h), the last chunk (with ID=04h) must be 14h bytes in
size, and the 14h-byte chunk data is then copied to dst. The other four chunks
must exist, but their content is just skipped.

```
  00h 1    Leading zero       (00h)                           ;\
  01h 1    Block type         (01h)                           ; padding
  02h 5Ah  Padding Bytes      (FFh-filled)                    ;
  5Ch 1    Padding End        (00h)                           ;/
  5Dh 2    30h,junk(1)        (30h,21h)                       ;-sequence
  5Fh 2    30h,junk(1)        (30h,09h)                       ;-sequence
  61h 7    06h,len,junk(len)  (06h,05h, 2Bh,0Eh,03h,02h,1Ah)  ;-OID for SHA1
  68h 2    05h,junk(1)        (05h,00h)                       ;-null
  6Ah 16h  04h,len,sha1(len)  (04h,14h, sha1[14h bytes])      ;-SHA1
```

The "junk" bytes aren't actually used/verified by the DSi. Handling chunks with
len>7Fh looks quite weird/bugged. The DSi firmware contains some functions
where it could optionally use SWI 22h or SWI 23h for RSA signatures (although,
there aren't any known cases where it would actually use SWI 23h). Note: The
DSi's Flipnote ".ppm" files are using the SWI 23h style SHA1 format (but
Flipnote contains it's own RSA functions instead of using the BIOS SWI
functions). DS Download Play (and equivalent code in NDS Firmware) is SWI 23h
style, too (but also uses its own RSA function instead of the BIOS SWI).

Note: The format is based on the OpenPGP Message Format (RFC 4880), that format
allows to use similar headers for MD5, SHA256, etc. (the DSi supports only SHA1
though).



## Unencrypted Signatures


Emulators can hook the RSA BIOS functions to copy unencrypted signatures as-is
(instead of trying to decrypt them). That allows to create "authentic" files
that are fully compatible with the DSi firmware, and which would be actually
working on real hardware (when knowing the private key).

Unencrypted 80h-byte signatures should be stored in following format (as
defined in RFC 2313):

```
  00h   1      "00" Leading zero (00h)
  01h   1      "BT" Block type (always 01h on DSi)
  02h   8+n    "PS" Padding (FFh-filled, min 8 bytes, usually 69h bytes on DSi)
  0Ah+n 1      "00" Padding end (00h)
  0Bh+n 75h-n  "D"  Data (max 75h bytes, usually a 14h-byte SHA1 value on DSi)
```

If the hooked BIOS function sees a RSA source to contain "00h, 01h, at least
eight FFh, followed by 00h", then it could treat it as unencrypted data (it's
nearly impossible that an encrypted signature could contain those values).



## Key.Bit0


The DSi BIOS contains two different RSA core modes (either one of them is used,
depending on whether BIT0 of the FIRST BYTE of the "key" is set or cleared).
The purpose & difference between those two modes is unknown. Also, dunno if
that BIT0 thing is something common in the RSA world?



## DSi Public RSA Keys (for verifying signatures)


```
  TWL_FIRM       (F1,F5,1A,FF..) eMMC Boot Info (same key for retail+debug)
  BIOS:FFFF87F4h (C3,02,93,DE..) Key0: System Menu (Launcher) of Retail version
  BIOS:FFFF8874h (B6,18,D8,61..) Key1: System Fun Tools and Wifi Firmware
  BIOS:FFFF88F4h (DA,94,09,01..) Key2: System Base Tools (Settings, Shop)
  BIOS:FFFF8974h (95,6F,79,0D..) Key3: DSiWare and DSi ROM Cartridges
  BIOS:FFFF89F4h (D4,30,E3,7D..) Key4: Unknown ;\probably more/unused RSA keys
  BIOS:FFFF8A74h (BD,29,02,38..) Key5: Unknown ; (DSi only)
  BIOS:FFFF8AF4h (CF,8A,4B,15..) Key6: Unknown ; (doesn't exist on 3DS)
  BIOS:FFFF8B74h (A3,BC,C1,7C..) Key7: Unknown ;/
  BIOS:FFFF9920h (30,33,26,D5..) Unknown (probably NOT a RSA key)
  Launcher       (BA,F1,98,A4..) HWINFO_S.dat (with RSA-SHA1-HMAC)
  Launcher       (9F,80,BC,5F..) Version Data and TWLFontTable.dat
  Launcher       (C7,F4,1D,27..) DS Cart Whitelist (missing RSA in v1.4E)
  Launcher+NDS   (9E,C1,CC,C0..) For wifi-booted NDS titles (DsDownloadPlay)
  Flipnote       (C2,3C,BC,13..) Public key for Flipnote .ppm files
  Unknown        (?)             HWID.sgn
  Unknown        (?)             Newer NDS ROM Cartridges (have RSA, too?)
  DSi Shop       (9D,69,36,28..) Unknown, seems to be RSA        (100h bytes)
  Launcher       (F8,24,6C,58..) Root key for cert.sys CA00000001(200h bytes)
  cert.sys       (B2,79,C9,E2..) CA00000001 key for cert.sys keys(100h bytes)
  cert.sys       (93,BC,0D,1F..) CP00000007 key for tmd's        (100h bytes)
  cert.sys       (AD,07,A9,37..) XS00000003 key for shop-tickets (100h bytes)
  cert.sys       (92,FF,96,40..) XS00000006 key for free-tickets (100h bytes)
  cert.sys       (01,93,6D,08..) MS00000008 key for dev.kp       (ECC, non-RSA)
  dev.kp         (per-console)   TWxxxxxxxx... key for tad files (ECC, non-RSA)
  *.bin          (random?)       AP00030015484e42gg in tad files (ECC, non-RSA)
  Launcher+Boot  (BC,FD,A1,FF..) Debug0: System Menu (Launcher, Debug version)
  Launcher       (E9,9E,A7,9F..) Debug1:
  Launcher       (A7,9F,54,A0..) Debug2:
  Launcher       (AC,93,BB,3C..) Debug3: Public key for Debug DSiware/ROMs
  Debug Updater  (E5,1C,BF,C7..) Debug Public key for HWInfo
  Debug Updater  (C8,4B,38,2C..) Debug Public key for HWID.sgn    (100h bytes)
  Launcher       (D0,1F,E1,00..) Debug Root key for CA00000002 key(200h bytes)
  debug cert.sys (...)           Debug CA00000002 key for cert.sys(100h bytes)
  debug cert.sys (...)           Debug CP00000005 key for ...?    (100h bytes)
  debug cert.sys (...)           Debug CP00000007 key for ...     (100h bytes)
  debug cert.sys (...)           Debug XS00000006 key for ...     (100h bytes)
  verdata        (...)           Public keys in Version Data file?
  Unknown        (?)             further keys...?
```



## DSi Private RSA Keys (for creating signatures)


Nintendo's private keys should be known only by Nintendo. However, the console
does have a few "own" private keys (for sending signed data to Nintendo
(verdata), for storing signed data on SD card (flipnote), plus some more keys
for the developer's debug version).

```
  Flipnote       (26,A7,53,7E..) Private key for Flipnote .ppm files
  dev.kp         (per-console)   TWxxxxxxxx... key for tad files (ECC, non-RSA)
  (temp/unsaved?)(random?)       AP00030015484e42gg    tad files (ECC, non-RSA)
  verdata        (...)           Private keys in Version Data file?
  Debug Updater  (77,FC,77,9E..) Private key for Debug HWID.sgn (100h bytes)
  Debug Updater  (B5,7C,C2,85..) Private key for Debug HWInfo
  Debug SDK      (95,DC,C8,18..) Private key for Debug DSiware/ROMs (Debug3)
  Unknown        (?)             further keys...?
```

The private keys are usually stored in DER format; that's including entries for
the public key/exponent, and the original prime numbers that the key was
generated from, plus some numbers that were temporarily used during key
creation, and with all entries preceeded by tag/length values, whereas values
with MSB set are preceeded by a 00h byte to make them "unsigned" (eg. most
80h-byte keys will occupy 81h bytes in DER format).




# <a name="biosrsabasics"></a>BIOS RSA Basics




## RSA Basics


The RSA formulas are quite simple: Applying an exponent and modulus to the
source data. There are two formulas used for encryption/decryption. The first
formula requires only the Public Key (and an exponent, which is usually some
fixed constant; on the DSi it's always 10001h aka 65537 decimal). The second
formula is almost same, but requires the Private Key instead of the constant
exponent (and also requires the Public Key as modulus):

```
  Public Key formula:      dest = src^10001h mod pubkey
  Private Key formula:     dest = src^prvkey mod pubkey
```

That formulas can be used for encrypting secret messages, as so:

```
  Recipient's Public Key   --> Encrypt a message
  Recipient's Private Key  --> Decrypt a message
```

Or, using the formulas the other way around, to create digital signatures:

```
  Sender's Private Key     --> Encrypt/create a signature
  Sender's Public Key      --> Decrypt/verify a signature
```

The overall idea is that only the owner of the Private Key can decrypt
messages, or create signatures. The Public Key can be shared freely, so that
everybody can encrypt messages, or verify signatures.



## RSA Big Number Maths


The exponent/modulus can be implemented with simple unsigned multiply/divide
operations. However, RSA requires dealing with big 1024bit integers (or even
bigger numbers when using larger keys), this does usually require some software
functions since regular CPUs cannot directly deal with such large numbers.



## RSA Byte Order


The DSi is storing all RSA keys and signatures in Big-Endian format, so one
will need to reverse the byte order before doing the actual maths on
Little-Endian CPUs.



## RSA Signatures (used on DSi)


Digital signatures can be used for signing documents or other binaries. The
signature does usually consist of a secure checksum (SHA-1, MD5, SHA256, etc.)
computed on the document/binary, and then encrypted via the RSA Private Key
formula.

The checksum can be then decrypted via Public Key, if the decrypted checksum
does match up, then one can be sure that the document/binary hasn't been
modified, and that it was really created by the Private Key owner.



## RSA Encrypted Messages (not used on DSi)


Encrypted RSA messages are restricted to the size of the Public Key (eg. with a
1024bit key, the message should be smaller than 128 bytes). For bigger
messages, one could either split the message into smaller snippets, or, one
could combine RSA with some other encryption mechanism (eg. store an AES key in
the RSA message, and decrypt the actual document via AES; that would add
private/public key security to AES).



## RSA Padding


RSA can be weak if the message is a small number (especially very small values
like "0" or "1" obviously wouldn't work well with the "msg^exp" maths; other
small values can be also weak, eg. with the common/small public exponent
10001h). To avoid that problem, the MSBs of the message should be padded with
nonzero bytes, typically as so (as defined in RFC 2313):

```
  00h   1      "00" Leading zero (00h)
  01h   1      "BT" Block type (always 01h on DSi)
  02h   8+n    "PS" Padding (FFh-filled, min 8 bytes, usually 69h bytes on DSi)
  0Ah+n 1      "00" Padding end (00h)
  0Bh+n 75h-n  "D"  Data (max 75h bytes, usually a 14h-byte SHA1 value on DSi)
```

That, for 80h-byte messages. For other sizes replace "75h" by "F5h, 1F5h, etc."



## RSA Key Generation


Generating a RSA key pair is more difficult than the encryption/decryption
part. First of, one needs two unsigned random prime numbers; for a 1024bit key,
that would be usually two large 512bit prime numbers (whereas, finding real
prime numbers is complicated, and it's more common to use values that have a
"high probability" of being prime numbers).

The public key is then simply generated by multiplying the two prime numbers (P
and Q) with each other:

```
  pubkey = P * Q
```

The private key is also based on the same prime numbers, but the maths there
are more complicated (and not described here).

When knowing one prime number, one could theoretically compute the other as
"Q=pubkey/P", however, prime numbers aren't as rare as one might think, and
it's quite impossible to guess (or brute-force) one of the prime numbers.




# <a name="biosrsapseudocode"></a>BIOS RSA Pseudo Code




## rsa\_mpi\_pow\_mod(dst,src,pubkey,exp,num\_exp\_bits) ;[dst]=[src]^[exp] mod [key]


```
  base(rsa__number_size), bigbuf(rsa_number_size*2)
  [base]=[src], [dst]=1, pow8bit=01h                ;-init base, result, powbit
  for i=1 to num_exp_bits
    if [exp] AND pow8bit then rsa_mpi_mul_mod(dst,base)   ;-mul result
    rsa_mpi_mul_mod(base,base)                            ;-square base
    pow8bit=pow8bit ROL 1, exp=exp+carry                  ;-next exp bit
  next i
  return
```

This is the RSA main function. The exponent is applied by squaring the "src"
several times, and, if the corresponding exponent bit is set, multiplying the
result by the squared value. To avoid the numbers to become incredible large,
the modulus is applied after each multiplication (rather than applying it only
on the final result).

```
  For the Private Key formula: Use exp=prvkey, num_exp_bits=rsa_number_size*8
  For the Public Key formula:  Use exp=ptr_to_10001h, num_exp_bits=17
```

The parameters and result for "rsa\_mpi\_pow\_mod" must be in little-endian. Ie.
for DSi, reverse the byte order of the incoming/outgoing values. And, on DSi,
use rsa\_number\_size=80h (aka 128 bytes, aka for 1024bit RSA).



## rsa\_mpi\_mul\_mod(dst,src):


```
  rsa_mpi_mul(bigbuf,dst,src)           ;-multiply
  rsa_mpi_mod(bigbuf,pubkey)            ;-modulus
  [dst]=[bigbuf+0..rsa_number_size-1]   ;-copy to dst
  return
```



## rsa\_mpi\_mul(dst,src1,src2):    ;[dst]=[src1]\*[src2]


```
  [dst+0]=0, oldmsw=0                   ;-init first word and oldmsw
  for i=0 to rsa_number_size-4 step 4   ;\
    call @@inner_loop                   ; compute LSWs of destination
    src2=src2+4                         ;
  next i                                ;/
  src2=src2-4
  for i=rsa_number_size-8 to 0 step -4  ;\
    src1=src1+4                         ; compute MSWs of destination
    call @@inner_loop                   ;
  next i                                ;/
  return
 ;---
 @@inner_loop:
  [dst+4]=oldmsw, oldmsw=0
  for j=0 to i step 4
    msw:lsw = [src1+j]*[src2-j]
    [dst+0]=[dst+0]+lsw
    [dst+4]=[dst+4]+msw+cy
    oldmsw=oldmsw+cy
  next j
  dst=dst+4
  ret
```



## rsa\_mpi\_mod(dst,src):   ;[dst]=[dst] mod [src]  ;aka division remainder


;Double/Single -> Single modulo division (mpi/mpi)

;Divisor's MSW must be >= 80000000h

```
  ebx=rsa_number_size, dst=dst+ebx, i=ebx+4
 @@type0_lop:                                                           ;\
  if [dst+ebx-4]=0 then goto @@type0_next                               ;
  rsa_mpi_cmp(dst,src), if borrow then goto @@type1_next                ; type0
  rsa_mpi_sub(dst,src), if [dst+ebx-4]<>0 then goto @@type1_next        ; loop
 @@type0_next:                                                          ;
  dst=dst-4, i=i-4, if i>0 then goto @@type0_lop                        ;/
  goto @@done
 ;--- --- ---
 @@type1_lop:                                                           ;\
  lsw=[dst+ebx-4], msw=[dst+ebx-0]                                      ;
  if msw>=[src+ebx-4] then fac=FFFFFFFFh else fac=msw:lsw / [src+ebx-4] ;
  rsa_mpi_mulsub(dst,src,fac), if carry=0 then goto @@skip_add          ; type1
 @@add_more:                                                            ; loop
  rsa_mpi_add(dst,src)                                                  ;
  [dst+ebx]=[dst+ebx]+carry, if carry=0 then goto @@add_more            ;
 @@skip_add:                                                            ;
  if [dst+ebx-4]=0 then goto @@type0_next                               ;
 @@type1_next:                                                          ;
  dst=dst-4, i=i-4, if i>0 then goto @@type1_lop                        ;/
 @@done:
  return
```



## rsa\_mpi\_mulsub(dst,src,fac):    ;[dst]=[dst]-[src]\*fac


```
  oldborrow=0, oldmsw=0                                       ;\
  for i=0 to rsa_number_size-4 step 4                         ; process
    msw:lsw = [src+i]*fac, lsw=lsw+oldmsw, oldmsw=msw+carry   ; rsa_number_size
    [dst+i]=[dst+i]-lsw-oldborrow, oldborrow=borrow           ; bytes, plus...
  next i                                                      ;/
  [dst+rsa_number_size]=[dst+rsa_number_size]-oldmsw-oldborrow ;-one extra word
  return borrow   ;(unlike "rsa_embedded" which returns INVERTED borrow)
```



## rsa\_mpi\_add(dst,src):   ;out: [dst]=[dst]+[src], carry


```
  carry = 0
  for i=0 to rsa_number_size-4 step 4
    [dst+i]=[dst+i]+[src+i]+carry
  next i
  return carry
```



## rsa\_mpi\_sub(dst,src):   ;out: [dst]=[dst]-[src], borrow/unused


```
  borrow = 0
  for i=0 to rsa_number_size-4 step 4
    [dst+i]=[dst+i]-[src+i]-borrow
  next i
  return borrow
```



## rsa\_mpi\_cmp[dst,src]:   ;compare [dst]-[src], out: borrow


```
  for i=rsa_number_size-4 to 0 step -4
    temp=[dst+i]-[src+i], if not equal then return borrow
  next i
  return borrow
```

This is about same as "sub", but faster (because it can abort the loop upon
first difference).




# <a name="bios3dsexceptionvectors"></a>BIOS 3DS Exception Vectors




## Exception Vectors


The 3DS bootroms do mostly contain bootcode, unlike GBA/NDS without any public
functions, apart from some small stubs for redirecting the exception vectors to
RAM locations:

```
  08000000h 8  arm9_irq                ldr r15,[$+4] // dd vector  ;\
  08000008h 8  arm9_fiq                ldr r15,[$+4] // dd vector  ;
  08000010h 8  arm9_svc                ldr r15,[$+4] // dd vector  ; ARM9
  08000018h 8  arm9_undef              ldr r15,[$+4] // dd vector  ; exceptions
  08000020h 8  arm9_prefetch_abort     ldr r15,[$+4] // dd vector  ;
  08000028h 8  arm9_data_abort         ldr r15,[$+4] // dd vector  ;/
  1FFFFFA0h 8  arm11_irq               ldr r15,[$+4] // dd vector  ;\
  1FFFFFA8h 8  arm11_fiq               ldr r15,[$+4] // dd vector  ;
  1FFFFFB0h 8  arm11_svc               ldr r15,[$+4] // dd vector  ; ARM11
  1FFFFFB8h 8  arm11_undef             ldr r15,[$+4] // dd vector  ; exceptions
  1FFFFFC0h 8  arm11_prefetch_abort    ldr r15,[$+4] // dd vector  ;
  1FFFFFC8h 8  arm11_data_abort        ldr r15,[$+4] // dd vector  ;/
  1FFFFFDCh 4  arm11_core1_entrypoint  dd vector                   ;-CPU1 entry
  (OVERLAY) 4  arm11_core23_entry      dd vector                   ;-CPU2/3
```

The ARM11 exception vectors are shared for all CPU cores (so one must manually
redirect them to core specific handlers).

Alternately, instead of using the above RAM vectors, one could use ARM11
virtual memory or ARM9 ITCM for custom vectors.

Moreover, the CFG11\_BOOTROM\_OVERLAY\_CNT/VAL feature allows to redirect ARM11
vectors on New3DS (mainly intended for booting CPU2/CPU3 cores).



## SWI/SVC Functions


There aren't any built-in functions in the bootroms, however, SVC opcodes can
be used to obtain entrypoints to OS functions. That is, once when the OS is
booted (FIRM files are started without any OS functions, but .code files can
use OS functions).

Note: ARM has renamed SWI opcodes to SVC opcodes.




# <a name="biosramusage"></a>BIOS RAM Usage



Below contains info about RAM contents at cartridge boot time (as initialized
by the BIOS/Firmware), plus info about RAM locations used by IRQ handlers and
SWI functions.



## GBA BIOS RAM Usage


Below memory at 3007Fxxh is often accessed directly, or via mirrors at
3FFFFxxh.

```
  3000000h 7F00h User Memory and User Stack              (sp_usr=3007F00h)
  3007F00h A0h   Default Interrupt Stack (6 words/time)  (sp_irq=3007FA0h)
  3007FA0h 40h   Default Supervisor Stack (4 words/time) (sp_svc=3007FE0h)
  3007FE0h 10h   Debug Exception Stack (4 words/time)    (sp_xxx=3007FF0h)
  3007FF0h 4     Pointer to Sound Buffer (for SWI Sound functions)
  3007FF4h 3     Reserved (unused)
  3007FF7h 1     Reserved (intro/nintendo logo related)
  3007FF8h 2     IRQ IF Check Flags (for SWI IntrWait/VBlankIntrWait functions)
  3007FFAh 1     Soft Reset Re-entry Flag (for SWI SoftReset function)
  3007FFBh 1     Reserved (intro/multiboot slave related)
  3007FFCh 4     Pointer to user IRQ handler (to 32bit ARM code)
```



## NDS BIOS RAM Usage


Below memory at 27FFxxxh is mirrored to 23FFxxxh (on retail consoles with 4MB
RAM), however, it should be accessed via address 27FFxxxh (for compatibility
with debug consoles with 8MB RAM). Accessing it via mirrors at 2FFFxxxh is also
valid (this is done by DSi enhanced games; even when running in non-DSi mode;
this allows DSi games to use the same memory addresses in NDS and DSi mode).

```
  2000000h ...   ARM7 and ARM9 bootcode can be loaded here (2000000h..23BFDFFh)
  2400000h ...   Debug bootcode can be loaded here (2400000h..27BFDFFh)
  23FEE00h 168h  Fragments of NDS9 firmware boot code
  27FF800h 4     NDS Gamecart Chip ID 1
  27FF804h 4     NDS Gamecart Chip ID 2
  27FF808h 2     NDS Cart Header CRC (verified)            ;hdr[15Eh]
  27FF80Ah 2     NDS Cart Secure Area CRC (not verified ?) ;hdr[06Ch]
  27FF80Ch 2     NDS Cart Missing/Bad CRC (0=Okay, 1=Missing/Bad)
  27FF80Eh 2     NDS Cart Secure Area Bad (0=Okay, 1=Bad)
  27FF810h 2     Boot handler task number (usually FFFFh at cart boot time)
  27FF812h 2     Secure disable (0=Normal, 1=Disable; Cart[078h]=BIOS[1088h])
  27FF814h 2     SIO Debug Connection Exists (0=No, 1=Yes)
  27FF816h 2     RTC Status?                 (0=Okay, 1=Bad)
  27FF818h 1     Random RTC    ;random LSB from SIO debug detect handshake
  27FF819h 37h   Zerofilled by firmware
  27FF850h 2     NDS7 BIOS CRC (5835h)
  27FF860h 4     Somewhat copy of Cart[038h], nds7 ram addr (?)
  27FF864h 4     Wifi FLASH User Settings Bad (0=Okay, 1=Bad)
  27FF868h 4     Wifi FLASH User Settings FLASH Address (fmw[20h]*8)
                   maybe recommended to use above RAM cell instead FLASH entry?
  27FF86Ch 4     Whatever (seems to be zero at cart boot time)
  27FF870h 4     Whatever (seems to be zero at cart boot time)
  27FF874h 2     Wifi FLASH firmware part5 crc16 (359Ah) (fmw[026h])
  27FF876h 2     Wifi FLASH firmware part3/part4 crc16 (fmw[004h] or ZERO)
                   Above is usually ZERO at cart boot (set to fmw[004h] only
                   when running pictochat, or maybe also when changing user
                   settings)
  27FF878h 08h   Not used
  27FF880h 4     Message from NDS9 to NDS7  (=7 at cart boot time)
  27FF884h 4     NDS7 Boot Task (also checked by NDS9) (=6 at cart boot time)
  27FF888h ..    Whatever (seems to be zero at cart boot time)
  27FF890h 4     Somewhat boot flags (somewhat B0002A22h)
                   bit10 part3/part4 loaded/decoded (bit3 set if bad crc)
                   bit28 part5 loaded/decoded with good crc
  27FF894h 36Ch  Not used (zero)
  27FFC00h 4     NDS Gamecart Chip ID 1   (copy of 27FF800h)
  27FFC04h 4     NDS Gamecart Chip ID 2   (copy of 27FF804h)
  27FFC08h 2     NDS Cart Header CRC      (copy of 27FF808h)
  27FFC0Ah 2     NDS Cart Secure Area CRC (copy of 27FF80Ah)
  27FFC0Ch 2     NDS Cart Missing/Bad CRC (copy of 27FF80Ch)
  27FFC0Eh 2     NDS Cart Secure Area Bad (copy of 27FF80Eh)
  27FFC10h 2     NDS7 BIOS CRC (5835h)    (copy of <27FF850h>)
  27FFC12h 2     Secure Disable           (copy of 27FF812h)
  27FFC14h 2     SIO Debug Exist          (copy of 27FF814h)
  27FFC16h 1     RTC Status?              (<8bit> copy of 27FF816h)
  27FFC17h 1     Random 8bit              (copy of <27FF818h>)
  27FFC18h 18h   Not used (zero)
  27FFC30h 2     GBA Cartridge Header[BEh], Reserved
  27FFC32h 3     GBA Cartridge Header[B5h..B7h], Reserved
  27FFC35h 1     Whatever flags ?
  27FFC36h 2     GBA Cartridge Header[B0h], Maker Code
  27FFC38h 4     GBA Cartridge Header[ACh], Gamecode
  27FFC3Ch 4     Frame Counter (eg. 00000332h in no$gba with original firmware)
  27FFC40h 2     Boot Indicator (1=normal; required for some NDS games, 2=wifi)
  27FFC42h 3Ch   Not used (zero) (or boot info if booted via DS Download Play)
  27FFC7Eh 2     Not used (zero)
  27FFC80h 70h   Wifi FLASH User Settings (fmw[newest_user_settings])
  27FFCF0h 10h   Not used (zero)
  27FFDxxh ..    NDS9 Debug Exception Stack (stacktop=27FFD9Ch)
  27FFD9Ch 4     NDS9 Debug Exception Vector (0=None)
  27FFDA0h ..    ...
  27FFE00h 170h  NDS Cart Header at 27FFE00h+0..16Fh
  27FFF70h ..    Not used (zerofilled at cart boot time)
  27FFFF8h 2     NDS9 Scratch addr for SWI IsDebugger check
  27FFFFAh 2     NDS7 Scratch addr for SWI IsDebugger check
  27FFFFCh ..    ...
  27FFFFEh 2     Main Memory Control (on-chip power-down I/O port)
  DTCM+3FF8h 4   NDS9 IRQ IF Check Bits (hardcoded RAM address)
  DTCM+3FFCh 4   NDS9 IRQ Handler (hardcoded RAM address)
  37F8000h FE00h ARM7 bootcode can be loaded here (37F8000h..3807DFFh)
  380F700h 1D4h  Fragments of NDS7 firmware boot code
  380F980h 4     Unknown/garbage (set to FBDD37BBh, purpose unknown)
                   NOTE: Cooking Coach is doing similar crap at 37FCF1Ch ?!?!
  380FFC0h 4     DSi7 IRQ IF2 Check Bits (hardcoded RAM address) (DSi only)
  380FFDCh ..    NDS7 Debug Stacktop / Debug Vector (0=None)
  380FFF8h 4     NDS7 IRQ IF Check Bits (hardcoded RAM address)
  380FFFCh 4     NDS7 IRQ Handler (hardcoded RAM address)
  ---
  summary of nds memory used at cartridge boot time:
  (all other memory zero-filled unless containing cartridge data)
  37F8000h..3807E00h  ;cartridge area (nds7 only)
  2000000h..23BFE00h  ;cartridge area (nds9 and nds7)
  2400000h..27BFE00h  ;cartridge area (debug ver)
  23FEE00h..23FEF68h  ;fragments of NDS9 firmware boot code
  27FF800h..27FF85Fh  ;various values (from BIOS boot code)
  27FF860h..27FF893h  ;various values (from Firmware boot code)
  27FFC00h..27FFC41h  ;various values (from Firmware boot code)
  27FFC80h..27FFCE6h  ;firmware user settings
  27FFE00h..27FFF6Fh  ;cart header
  380F700h..380F8D4h  ;fragments of NDS7 firmware boot code
  380F980h            ;set to FBDD37BBh
  ---
  register settings at cartridge boot time:
  nds9 r0..r11     = zero
  nds9 r12,r14,r15 = entrypoint
  nds9 r13         = 3002F7Ch (!)
  nds9 r13_irq     = 3003F80h
  nds9 r13_svc     = 3003FC0h
  nds9 r14/spsr_irq= zero
  nds9 r14/spsr_svc= zero
  ---
  nds7 r0..r11     = zero
  nds7 r12,r14,r15 = entrypoint
  nds7 r13         = 380FD80h
  nds7 r13_irq     = 380FF80h
  nds7 r13_svc     = 380FFC0h
  nds7 r14/spsr_irq= zero
  nds7 r14/spsr_svc= zero
  ---
  Observe that SWI SoftReset applies different stack pointers:
  Host  sp_svc    sp_irq    sp_sys    zerofilled area       return address
  NDS7  380FFDCh  380FFB0h  380FF00h  [380FE00h..380FFFFh]  Addr[27FFE34h]
  NDS9  0803FC0h  0803FA0h  0803EC0h  [DTCM+3E00h..3FFFh]   Addr[27FFE24h]
```



## DSi BIOS RAM


```
  2000000h 8     AutoParam Old Title ID (former title)    ;carthdr[230h]
  2000008h 1     AutoParam Unknown/Unused
  2000009h 1     AutoParam Flags (03h=Stuff is used?)
  200000Ah 2     AutoParam Old Maker code                 ;carthdr[010h]
  200000Ch 2     AutoParam Unknown (02ECh) ;\counter/length/indices/whatever?
  200000Eh 2     AutoParam Unknown (0000h) ;/
  2000010h 2     AutoParam CRC16 on [000h..2FFh], initial=FFFFh, [010h]=0000h
  2000012h 2     AutoParam Unknown/Unused (000Fh = want Internet Settings?)
  2000014h 2ECh  AutoParam Unknown... some buffer... string maybe?
  2000300h 4     AutoLoad ID ("TLNC") (also requires BPTWL[70h]=01h)
  2000304h 1     AutoLoad Unknown/unused (usually 01h)
  2000305h 1     AutoLoad Len of data at 2000308h (01h..18h, for CRC, 18h=norm)
  2000306h 2     AutoLoad CRC16 of data at 2000308h (with initial value FFFFh)
  2000308h 8     AutoLoad Old Title ID (former title) (can be 0=anonymous)
  2000310h 8     AutoLoad New Title ID (new title to be started, 0=none)
  2000318h 4     AutoLoad Flags (bit0,1-3,4,5,6,7) ;usually 16bit, once 32bit
  200031Ch 4     AutoLoad Unused (but within checksummed area when CRC len=18h)
  2000320h E0h   AutoLoad Unused (but zerofilled upon erasing autoload area)
  2000400h 128h  System Settings from TWLCFGn.dat file (bytes 088h..1AFh)
  20005E0h 1     WlFirm Type (1=DWM-W015, 2=W024, 3=W028)    ;wifi_flash[1FDh]
  20005E1h 1     WlFirm Unknown (zero)
  20005E2h 2     WlFirm CRC16 with initial value FFFFh on [20005E4h..20005EFh]
  20005E4h 4     WlFirm RAM vars (500400h)  ;\
  20005E8h 4     WlFirm RAM base (500000h)  ; as from "Wifi Firmware" file
  20005ECh 4     WlFirm RAM size (02E000h)  ;/
  20005F0h 10h   WlFirm Unknown (zero)
  2000600h 14h   Hexvalues from HWINFO_N.dat
  2000800h ...   Unlaunch Auto-load feature (via "device:/Path/Filename.ext")
  23FEE00h 200h  DSi9 bootstrap relict
  ---
  2FEE120h 4     "nand"   <--- passed as so to launcher
  2FF80xxh
  2FF82xxh
  2FF83xxh
  2FF89xxh
  2FF8Axxh
  2FF8Bxxh
  2FF8Cxxh
  2FF8Dxxh       ... Wifi MAC address, channel mask, etc.
  2FF8Fxxh
  2FF90xxh
  2FF91xxh
  2FF9208h       FBDD37BBh (that odd "garbage" value occurs also on NDS)
  2FFA1xxh
  2FFA2xxh
  2FFA5xxh
  2FFA6xxh
  2FFA680h 12    02FD4D80h,00000000h,00001980h
  2FFA68Ch ..    Zerofilled
  ---
  2FFC000h 1000h Full Cart Header (as at 2FFE000h, but, FOR NDS ROM CARTRIDGE)
  2FFD000h 7B0h  Zerofilled
  2FFD7B0h 8+1   Version Data Filename (eg. 30,30,30,30,30,30,30,34,00)
  2FFD7B9h 1     Version Data Region   (eg. 50h="P"=Europe)
  2FFD7BAh 1     Unknown  (00)   ;bit0 = warmboot-flag-related
  2FFD7BBh 1     Unknown  (00)
  2FFD7BCh 15+1  eMMC CID (dd,ss,ss,ss,ss,03,4D,30,30,46,50,41,00,00,15), 00
  2FFD7CCh 15+1  eMMC CSD (40,40,96,E9,7F,DB,F6,DF,01,59,0F,2A,01,26,90), 00
  2FFD7DCh 4     eMMC OCR (80,80,FF,80)                                  ;20h
  2FFD7E0h 8     eMMC SCR (00,04,00,00,00,00,00,00) (for MMC: dummy/4bit);24h
  2FFD7E8h 2     eMMC RCA (01,00)                                        ;2Ch
  2FFD7EAh 2     eMMC Typ (01,00) (0=SD Card, 1=MMC Card)                ;2Eh
  2FFD7ECh 2     eMMC HCS (00,00) ;copy of OCR.bit30 (sector addressing) ;30h
  2FFD7EEh 2     eMMC ?   (00,00)                                        ;32h
  2FFD7F0h 4     eMMC ?   (00,00,00,00)                                  ;34h
  2FFD7F4h 4     eMMC CSR (00,09,00,00)   ;card status (state=tran)      ;38h
  2FFD7F8h 2     eMMC Port 4004824h setting  (00,01)  ;SD_CARD_CLK_CTL   ;3Ch
  2FFD7FAh 2     eMMC Port 4004828h setting  (E0,40)  ;SD_CARD_OPTION    ;3Eh
  2FFD7FCh 2     eMMC ?   (00,00)                                        ;40h
  2FFD7FEh 2     eMMC Device (usually 0001h=eMMC) (0000h=SD/MMC Slot?)   ;42h
  2FFD800h 1     Titles: Number of titles in below lists (max 76h)
  2FFD801h 0Fh   Titles: Zerofilled
  2FFD810h 10h   Titles: Pub Flags (1bit each) ;same maker plus public.sav
  2FFD820h 10h   Titles: Prv Flags (1bit each) ;same maker plus private.sav
  2FFD830h 10h   Titles: Jmp Flags (1bit each) ;jumpable or current-title
  2FFD840h 10h   Titles: Mkr Flags (1bit each) ;same maker
  2FFD850h 3B0h  Titles: Title IDs (8 bytes each)
  2FFDC00h 400h  Zerofilled
  2FFE000h 1000h DSi Full Cart Header (additionally to short headers)
  2FFF000h 0Ch   Zerofilled
  2FFF00Ch 4     ? 0000007Fh
  2FFF010h 4     ? 550E25B8h
  2FFF014h 4     ? 02FF4000h
  2FFF018h A68h  Zerofilled
  2FFFA80h 160h  Short Cart header (as at 2FFFE00h, but, FOR NDS ROM CARTRIDGE)
  2FFFBE0h 20h   Zerofilled
```

Below resembles NDS area at 27FFC00h (with added/removed stuff)...

```
  2FFFC00h 4     NDS Gamecart Chip ID
  2FFFC04h 20h   Zerofilled
  2FFFC24h 5     ? (04 00 73 01 03)
  2FFFC29h 7     Zerofilled
  2FFFC30h 12    GBA Cartridge Header (FF FF FF FF FF 00 FF FF FF FF FF FF)
  2FFFC3Ch 4     Frame Counter maybe? (eg. 1F 01 00 00 in cooking coach)
  2FFFC40h 2     Boot Indicator (1=ROM Cartridge,2=Wifi/tmp?,3=SD/MMC DSiware)
  2FFFC42h 3Ch   Not used (zero) (or boot info if booted via DS Download Play)
  2FFFC7Eh 2     Not used (zero)
  2FFFC80h 70h   Wifi FLASH User Settings (fmw[newest_user_settings])
  2FFFCF0h 4     ?           (3D 00 01 6E) (update counter and crc16 ?)
  2FFFCF4h 6     Wifi MAC Address (00 23 CC xx xx xx)              (fmw[036h])
  2FFFCFAh 2     Wifi Channels (usually 1041h = ch1+7+13) (based on fmw[03Ch])
  2FFFCFCh 4     Zero
  2FFFD00h 68h   Zerofilled
  2FFFD68h 4     Bitmask for Supported Languages (3Eh for Europe);\
  2FFFD6Ch 4     Unknown (00,00,00,00)                           ; from
  2FFFD70h 1     Console Region (0=JP,1=US,2=EU,3=AU,4=CHN,5=KOR); HWINFO_S.dat
  2FFFD71h 12    Serial/Barcode (ASCII, 11-12 characters)        ;
  2FFFD7Dh 3     ? (00 00 3C)                                    ;/
  2FFFD80h 0Ch   Zerofilled
  2FFFD8Ch 10h   ARM9 debug exception stack (stacktop 2FFFD9Ch)
  2FFFD9Ch 4     ARM9 debug exception vector (020D3E64h)
  2FFFDA0h 4     02F80000h                                ;\
  2FFFDA4h 4     02FFA674h                                ;
  2FFFDA8h 4     00000000h zero                           ; start addresses?
  2FFFDACh 4     01FF86E0h itcm?                          ;
  2FFFDB0h 4     027C00C0h                                ;
  2FFFDB4h 4     02FFF000h                                ;
  2FFFDB8h 4     03040000h wram?                          ;
  2FFFDBCh 4     03800000h wram?                          ;
  2FFFDC0h 4     0380C3B4h wram?                          ;/
  2FFFDC4h 4     02F80000h                                ;\
  2FFFDC8h 4     02FFC000h ptr to DSi Full Cart Header    ;
  2FFFDCCh 4     00000000h zero                           ; end addresses?
  2FFFDD0h 4     02000000h ram bottom?                    ; (for above nine
  2FFFDD4h 4     027C0780h                                ; start addresses)
  2FFFDD8h 4     02FFF680h                                ;
  2FFFDDCh 4     03040000h wram?                          ;
  2FFFDE0h 4     03800000h wram?                          ;
  2FFFDE4h 4     0380F780h wram?                          ;/
  2FFFDE8h 4     RTC Date at Boot (BCD) (yy,mm,dd,XX) (XX=maybe day-of-week?)
  2FFFDECh 4     RTC Time at Boot (BCD) (hh,ss,mm,0) (hh.bit6=maybe PM or 24h?)
  2FFFDF0h 4     Initial ARM7 Port 4004008h bits (13FBFB06h) (SCFG_EXT7)
  2FFFDF4h 1     Initial ARM7 Port 40040xxh bits (C4h)       (SCFG_xxx)
  2FFFDF5h 1     Initial ARM7 Port 400400xh bits (F0h)       (SCFG_xxx)
  2FFFDF6h 2+2   Zerofilled
  2FFFDFAh 1     Warmboot Flag (bptwl[70h] OR 80h, ie. 80h=cold or 81h=warm)
  2FFFDFBh 1     01h
  2FFFDFCh 4     Pointer to TWLCFGn.dat (usually 2000400h) (or 0=2000400h)
  2FFFE00h 160h  Short Cart header (unlike NDS, only 160h, not 170h)
  2FFFF60h A0h   Zerofilled
 37FA414h       "nand:/title/....app"  <-- [1D4h]+3C0h  (without Device List!)
 380C400h 22E4h BIOS Keys (as from Boot Stage 1, see there)
  380F010h 10h   AES key for dev.kp (E5,CC,5A,8B,...) (optional/for launcher)
  380F600h 200h  DSi7 bootstrap relict (at 3FFF600h aka mirrored to 380F600h)
  380FFC0h 4     DSi7 IRQ IF2 Check Bits (hardcoded RAM address) (DSi only)
  380FFC4h 4     DSi7 SCFG_EXT7 setting
  380FFC8h 2     DSi7 SCFG_misc bits
  380FFDCh ..    DSi7 Debug Stacktop / Debug Vector (0=None)
  380FFF8h 4     DSi7 IRQ IF Check Bits (hardcoded RAM address)
  380FFFCh 4     DSi7 IRQ Handler (hardcoded RAM address)
  xxxxxxxh ?     ARM7i and ARM9 bootcode can be loaded WHERE and WHERE?
  cart_header[1D4h] 400h SD/MMC Device List ARM7 RAM; initialized by firmware
```

Initial state after DSi BIOS ROM bootcode (when starting eMMC bootcode)
requires only a few memory blocks in ITCM, ARM7 WRAM, and AES keyslots:

```
  1FFC400h 400h  BIOS Keys from FFFF87F4h (C3 02 93 DE ..) RSA keys (8x80h)
  1FFC800h 80h   BIOS Keys from FFFF9920h (30 33 26 D5 ..) Whatever
  1FFC880h 14h   Whatever, should/may be zerofilled?
  1FFC894h 1048h BIOS Keys from FFFF99A0h (99 D5 20 5F ..) Blowfish/NDS-mode
  1FFD8DCh 1048h BIOS Keys from FFFFA9E8h (D8 18 FA BF ..) Blowfish/unused?
  3FFC400h 200h  BIOS Keys from 00008188h (CA 13 31 79 ..) Whatever, 32x10h AES?
  3FFC600h 40h   BIOS Keys from 0000B5D8h (AF 1B F5 16 ..) Whatever, AES?
  3FFC640h 14h   Whatever, must be zerofilled
  3FFC654h 1048h BIOS Keys from 0000C6D0h (59 AA 56 8E ..) Blowfish/DSi-mode
  3FFD69Ch 1048h BIOS Keys from 0000D718h (54 86 13 3B ..) Blowfish/unused?
  3FFE6E4h 44h   eMMC Info (to be relocated to 2FFD7BCh, see there for details)
  4004450h 8     AES Key0.X ("Nintendo") for modcrypt
  4004480h 10h   AES Key1.X (CPU/Console ID and constants) for dev.kp and Tad
  40044B0h 10h   AES Key2.X ("Nintendo DS",...)            for Tad
  40044E0h 1Ch   AES Key3.X/Y (CPU/Console ID and constants) for eMMC
  2000000h ...   Warmboot Param (optional, passed on to New Title)
  2000300h 20h   Warmboot Info  (optional, passed on to Launcher)
```



## 3DS Initial RAM content (from bootrom's FIRM loader)


```
  01FFB800h 90h    ARM9 ITCM+3800h Decrypted OTP
  01FFB900h 200h   ARM9 ITCM+3900h NCSD header
  01FFBB00h 200h   ARM9 ITCM+3B00h FIRM header
  01FFBD00h 4*100h ARM9 ITCM+3D00h rsa keys ? private key 0..3 (modulo)
  01FFC100h 4*200h ARM9 ITCM+4100h rsa keys ? private key 4..7 (modulo+exp)
  01FFC900h 400h   ARM9 ITCM+4900h ??? ... copied from 1FFFD000h or 1FFFD400h
  01FFCD00h 40h    ARM9 ITCM+4D00h keys?
  01FFCD40h 40h    ARM9 ITCM+4D40h keys?
  01FFCD80h 64h    ARM9 ITCM+4D80h sdmmc info?
  01FFD000h 2470h  ARM9 ITCM+5000h Bootrom DSi Keys (RSA/AES/Blowfish)
  08000000h 30h    ARM9 Exception Vectors (ldr pc,[$+4] opcodes)
  1FFFFFA0h 30h    ARM11 Exception Vectors (ldr pc,[$+4] opcodes)
  1FFFFFDCh 4      ARM11 Core 1 Entrypoint (32bit word)
  20000000h 1000h  FCRAM FIRM Launch Params (on warmboot only)
```




# <a name="biosdumping"></a>BIOS Dumping




## BIOSes


```
  GBA BIOS 16K  (fully dumpable, CRC32=81977335 on GBA with [3F0Ch]=00h)
  GBA BIOS 16K  (fully dumpable, CRC32=A6473709 on NDS/3DS with [3F0Ch]=01h)
  NDS7 BIOS 16K (fully dumpable, CRC32=1280F0D5)
  NDS9 BIOS 4K  (fully dumpable, CRC32=2AB23573)
  DSi7 BIOS 64K (about 41K dumpable) (new: fully dumped now)
  DSi9 BIOS 64K (about 41K dumpable)
  3DS9 BIOS 64K (fully dumpable)
  3DS11 BIOS 64K (fully dumpable)
  DSiWifi BIOS 80K on older DSi (fully dumpable)
  DSiWifi BIOS Unknown size on newer DSi (fully dumpable)
  3DSWifi BIOS Unknown size on 3DS (fully dumpable)
```



## GBA BIOS


Contains SWI Functions and Bootcode (for starting cartridges, or booting via
Serial Port). The GBA BIOS can be read only by opcodes executed in BIOS area,
for example, via the MidiKey2Freq function (most other SWI Functions (like
CpuSet) are refusing source addresses within BIOS area).



## NDS BIOSes


Contains SWI Functions and Bootcode (for booting from SPI Bus Firmware FLASH
memory). The NDS9 BIOS can be dumped without restrictions (eg. via CpuSet, or
via LDR opcodes in RAM). The NDS7 BIOS has same restrictions as GBA, ie.
reading works only by BIOS opcodes, and not by functions like CpuSet. The
GetCRC16 functions does work though (at least for memory at 1204h..3FFFh). As
an additional obstacle, memory at 0000h..1203h can be dumped only by opcodes
within 0000h..1203h (that memory does mainly contain data, but some of the data
values can serve as THUMB LDR opcodes). For details see:

- [DS Memory Control - BIOS](ds.html#dsmemorycontrolbios)
Note: DSi consoles are containing a copy of the NDS BIOSes, but with BIOSPROT
set to 0020h (even when running in NDS mode), so the first 20h bytes of the
DSi's NDS7 BIOS aren't dumpable (except via tracing, see below), that 20h bytes
should be just same as on original NDS7 though.



## DSi BIOSes - Lower 32K-halves (SWI Functions)


The lower 32K of DSi9 doesn't have any restricions. The lower 32K of DSi7 has
similar restrictions as NDS7, but with BIOSPROT set to 0020h (instead of
1204h), this is making it more easy to dump memory at 0020h..7FFFh (eg. via
GetCRC16), but makes it impossible to dump the exception vectors at
0000h..001Fh, however, they can be deduced by tracing (with timer IRQs):

```
  ROM:00000000h  EA000006  b 20h   ;dsi7_reset_vector
  ROM:00000004h  EA000006  b 24h   ;dsi7_undef_handler
  ROM:00000008h  EA00001F  b 8Ch   ;dsi7_swi_handler
  ROM:0000000Ch  EA000004  b 24h   ;dsi7_prefetch_abort_handler
  ROM:00000010h  EA000003  b 24h   ;dsi7_data_abort_handler
  ROM:00000014h  EAFFFFFE  b 14h   ;reserved_vector
  ROM:00000018h  EA000013  b 6Ch   ;dsi7_irq_handler
  ROM:0000001Ch  EA000000  b 24h   ;dsi7_fiq_handler
```

Aside from branch opcodes, above could theoretically contain ALU opcodes that
modify R15 (but that would be very unlikely, and would make no difference)
(and, the above 20h bytes are dumped/confirmed on 3DS in DSi mode).



## DSi BIOSes - Upper 32K-halves (Bootcode, for booting from eMMC memory)


The upper 32K of the DSi9 and DSi7 BIOSes are locked at some point during
booting, and there's no known way to dump them directly. However, portions of
that memory are relocated to RAM/TCM before locking, and that relocated copies
can be dumped.

On a DSi, the following DSi ROM data can be dumped (originally done via Main
Memory hacks, ie. with complex external hardware soldered to the mainboard, but
it's now also possible via Unlaunch.dsi exploit):

```
  ROM:FFFF87F4h / TCM:1FFC400h (400h)  (C3 02 93 DE ..) RSA keys (8x80h)
  ROM:FFFF9920h / TCM:1FFC800h (80h)   (30 33 26 D5 ..) Whatever
  ROM:FFFF99A0h / TCM:1FFC894h (1048h) (99 D5 20 5F ..) Blowfish/NDS-mode
  ROM:FFFFA9E8h / TCM:1FFD8DCh (1048h) (D8 18 FA BF ..) Blowfish/unused?
  ROM:00008188h / RAM:3FFC400h (200h)  (CA 13 31 79 ..) Whatever, 32x10h AES?
  ROM:0000B5D8h / RAM:3FFC600h (40h)   (AF 1B F5 16 ..) Whatever, "common key"?
  ROM:0000C6D0h / RAM:3FFC654h (1048h) (59 AA 56 8E ..) Blowfish/DSi-mode
  ROM:0000D718h / RAM:3FFD69Ch (1048h) (54 86 13 3B ..) Blowfish/unused?
```

On a 3DS, the following "DSi ROM data" can be dumped from the 2470h-byte DSi
key area in 3DS memory at ARM9 ITCM 01FFD000h..01FFF46F (via 3DS exploits that
are capable of executing code on ARM9 side):

```
  ROM:FFFF87F4h / 3DS:01FFD000h  200h  RSA keys 0..3 (4x80h)
  ROM:00008308h / 3DS:01FFD200h  80h   some AES keys
  ROM:FFFF9920h / 3DS:01FFD280h  80h   whatever
  ROM:0000B5D8h / 3DS:01FFD300h  40h   AES keys and values (common etc)
  ROM:?         / 3DS:01FFD340h  A0h   misc "Nintendo" string etc.
  ROM:0000C6D0h / 3DS:01FFD3E0h  1048h Blowfish for DSi-mode
  ROM:FFFF99A0h / 3DS:01FFE428h  1048h Blowfish for DS-mode
```

The 3DS does have only half of the DSi keys (the extra keys might be used for
DSi debug version, but aren't needed for normal DSi software).

The 40h-byte area for ROM:0000B5D8h can be fully dumped from 3DS ITCM, the same
vales should also exist in DSi ITCM, but the DSi zerofills a 10h-byte fraction
of that area after initialization, and it doesn't seem be possible to read that
values via Main Memory hacks (most of that erased values can be found in AES
keyslots though).

The A0h-byte area is found only in 3DS ITCM, it should also exist somewhere in
DSi ROM, but isn't relocated to DSi ITCM (however, the relevant values can be
found in AES keyslots, eg. the "Nintendo" string).



## Checksums for BiosDSi.rom (20000h bytes)


```
  Offset Size  CRC32
  00000h 8000h 5434691Dh                ;\
  08000h 188h  ?                        ;
  08188h 180h  E5632151h  (not 3ds)     ;
  08308h 80h   64515306h                ;
  08388h 3250h ?                        ;
  0B5D8h 20h   85BE2749h                ; ARM7
  0B5F8h 10h   25A46A54h  (3ds only)    ;
  0B608h 10h   E882B9A9h                ;
  0B618h 10B8h ?                        ;
  0C6D0h 1048h 3B5CDF06h                ;
  0D718h 1048h 5AC363F9h  (not 3ds)     ;
  0E860h 18A0h ?                        ;/
  10000h 8000h 11E7C1EAh                ;\
  18000h 7F4h  ?                        ;
  187F4h 200h  4405D4BAh                ;
  189F4h 200h  2A32F2E7h  (not 3ds)     ;
  18BF4h D2Ch  ?                        ; ARM9
  19920h 80h   2699A10Fh                ;
  199A0h 1048h A8F58AE7h                ;
  1A9E8h 1048h E94759ACh  (not 3ds)     ;
  1BA30h 45D0h ?                        ;/
  ?      A0h   180DF59Bh  (3ds only)    ;-whatever, "Nintendo" string etc.
  ?      80h   ........h  (TWL-FIRM)    ;-RSA key for eMMC boot info
```

Checksums for the 'whole' 20000h-byte file (with unknown/missing areas
zerofilled):

```
  180DF59Bh (tcm/ram dump)  (missing 10h bytes)
  03A21235h (3ds dump)      (missing 180h+200h+1048h+1048h bytes)
  CDAA8FF6h (combined dump) (missing only the unknown "?" areas)
```



## 3DS BIOS


Can be dumped by crashing the 3DS with wrong voltages, or by using sighax to
crash the bootrom before disabling the upper 32Kbyte halves of the ARM9/ARM11
ROMs.



## DSiWifi BIOS


The Wifi BIOS can be dumped by using the WINDOW\_DATA register via SDIO CMD53.



## Further DSi BIOSes


The DSi cameras and several other I2C/SPI devices are probably having BIOS
ROMs, too. Unknown if/how that ROMs are dumpable.



## DSi BIOS Dumping via voltage errors


Lowering VDD12 for a moment does work quite reliable for crashing the ARM9 and
trapping the 2FFFD9Ch vector in Main RAM. The problem is that Main RAM seems to
be disabled during bootstage 1 (it gets enabled at begin of bootstage 2 via
EXMEMCNT, that is, shortly after the upper BIOS 32Kbyte areas are disabled).
More on that here:

- [http://4dsdev.kuribo64.net/thread.php?id=130](http://4dsdev.kuribo64.net/thread.php?id=130)

One theory/idea (from dark\_samus) is that EXMEMCNT controls the CE2 pin on the
Main RAM chip, so one could try to rewire that pin to get Main RAM enabled
regardless of EXMEMCNT, if that's actually working, then trapping the 2FFFD9Ch
vector should work even while BIOS ROMs are fully readable.




# <a name="externalconnectors"></a>External Connectors




## External Connectors


- [AUX GBA Game Pak Bus](#auxgbagamepakbus)
- [AUX DS Game Card Slot](#auxdsgamecardslot)
- [AUX Link Port](#auxlinkport)
- [AUX Sound/Headphone Socket and Battery/Power Supply](#auxsoundheadphonesocketandbatterypowersupply)
- [AUX DSi SD/MMC Pin-Outs](#auxdsisdmmcpinouts)


## Getting access to Internal Pins


- [AUX Opening the GBA](#auxopeningthegba)
- [AUX Mainboard](#auxmainboard)
- [AUX DSi Component Lists](#auxdsicomponentlists)
- [AUX DSi Signals/Testpoints](#auxdsisignalstestpoints)
- [AUX DSi Internal Connectors](#auxdsiinternalconnectors)
- [AUX DSi Chipset Pinouts](#auxdsichipsetpinouts)


## More Internal Stuff


- [Pinouts - CPU - Signal Summary](#pinoutscpusignalsummary)
- [Pinouts - CPU - Pinouts](#pinoutscpupinouts)
- [Pinouts - Audio Amplifiers](#pinoutsaudioamplifiers)
- [Pinouts - LCD Cables](#pinoutslcdcables)
- [Pinouts - Power Switches, DC/DC Converters, Reset Generators](#pinoutspowerswitchesdcdcconvertersresetgenerators)
- [Pinouts - Wifi](#pinoutswifi)
- [Pinouts - Various](#pinoutsvarious)


## Xboo Multiboot Cable


- [AUX Xboo PC-to-GBA Multiboot Cable](#auxxboopctogbamultibootcable)
- [AUX Xboo Flashcard Upload](#auxxbooflashcardupload)
- [AUX Xboo Burst Boot Backdoor](#auxxbooburstbootbackdoor)
- [DS Xboo](ds.html#dsxboo)



# <a name="auxgbagamepakbus"></a>AUX GBA Game Pak Bus




## Game Pak Bus - 32pin cartridge slot


The cartridge bus may be used for both CGB and GBA game paks. In GBA mode, it
is used as follows:

```
 Pin    Name    Dir  Expl.
 1      VDD     O    Power Supply 3.3V DC
 2      PHI     O    System Clock (selectable none, 4.19MHz, 8.38MHz, 16.78MHz)
 3      /WR     O    Write Select    ;\latched address to be incremented on
 4      /RD     O    Read Select     ;/rising edges of /RD or /WR signals
 5      /CS     O    ROM Chip Select ;-A0..A15 to be latched on falling edge
 6-21   AD0-15  I/O  lower 16bit Address    and/or  16bit ROM-data (see below)
 22-29  A16-23  I/O  upper 8bit ROM-Address   or    8bit SRAM-data (see below)
 30     /CS2    O    SRAM Chip Select
 31     /REQ    I    Interrupt request (/IREQ) or DMA request (/DREQ)
 32     GND     O    Ground 0V
```

When accessing game pak SRAM, a 16bit address is output through AD0-AD15, then
8bit of data are transferred through A16-A23.

When accessing game pak ROM, a 24bit address is output through AD0-AD15 and
A16-A23, then 16bit of data are transferred through AD0-AD15.

The 24bit address is formed from the actual 25bit memory address (byte-steps),
divided by two (halfword-steps).

Pin Pitch is 1.5mm.



## 8bit-Gamepak-Switch (GBA, GBA SP only) (not DS)


A small switch is located inside of the cartridge slot, the switch is pushed
down when an 8bit cartridge is inserted, it is released when a GBA cartridge is
inserted (or if no cartridge is inserted).

The switch mechanically controls whether VDD3 or VDD5 are output at VDD35; ie.
in GBA mode 3V power supply/signals are used for the cartridge slot and link
port, while in 8bit mode 5V are used.

The switch additionally drags IN35 to 3V when an 8bit cart is inserted, the
current state of IN35 can be determined in GBA mode via Port 4000204h
(WAITCNT), if the switch is pushed, then CGB mode can be activated via Port
4000000h (DISPCNT.3), this bit can be set ONLY by opcodes in BIOS region (eg.
via CpuSet SWI function).

In 8bit mode, the cartridge bus works much like for GBA SRAM, however, the 8bit
/CS signal is expected at Pin 5, while GBA SRAM /CS2 at Pin 30 is interpreted
as /RESET signal by the 8bit MBC chip (if any). In practice, this appears to
result in 00h being received as data when attempting to read-out 8bit
cartridges from inside of GBA mode.




# <a name="auxdsgamecardslot"></a>AUX DS Game Card Slot



```
  Pin    Dir  Name  Connection in cartridge
  1   >  -    GND   (ROM all unused Pins, EEPROM Pin 4 = VSS)
  2      Out  CLK   (4MB/s, ROM Pin 5, EEPROM Pin 6 = CLK)
  3   N  -    ?     (ROM Pin 17) (Seems to be not connected in console)
  4   i  Out  /CS1  (ROM Pin 44) ROM Chipselect
  5   n  Out  /RES  (ROM Pin 42) Reset, switches ROM to unencrypted mode
  6   t  Out  /CS2  (EEPROM Pin 1) EEPROM Chipselect
  7   e  In   IRQ   (GND)
  8   n  -    3.3V  (ROM Pins 2, 23, EEPROM Pins 3,7,8 = /W,/HOLD,VCC)
  9   d  I/O  D0    (ROM Pin 18)
  10  o  I/O  D1    (ROM Pin 19)
  11     I/O  D2    (ROM Pin 20)
  12  C  I/O  D3    (ROM Pin 21)
  13  0  I/O  D4    (ROM Pin 24)
  14  1  I/O  D5    (ROM Pin 25)
  15  -  I/O  D6    (ROM Pin 26, EEPROM Pin 2 = Q = Data EEPROM to NDS)
  16  0  I/O  D7    (ROM Pin 27, EEPROM Pin 5 = D = Data NDS to EEPROM)
  17  1  -    GND   (ROM all unused Pins, EEPROM Pin 4 = VSS)
```


Chipselect High-to-Low transitions are invoking commands, which are transmitted
through data lines during next following eight CLK pulses, after the command
transmission, further CLK pulses are used to transfer data, the data transfer
ends at chipselect Low-to-High transition.

Data should be stable during CLK=LOW period throughout CLK rising edge.

Note: Supply Pins (1,8,17) are slightly longer than other pins. Pin pitch is
1.5mm.


The DS does also have a 32pin cartridge slot, that slot is used to run GBA
carts in GBA mode, it can be also used as expansion port in DS mode.




# <a name="auxlinkport"></a>AUX Link Port




## Serial Link Port Pin-Out (GBA:"EXT" - GBA SP:"EXT.1")


```
  Pin  Name  Cable
  1    VDD35 N/A       GBA Socket     GBA Plug   Old "8bit" Plug
  2    SO    Red       ___________    _________    ___________
  3    SI    Orange   |  2  4  6  |  / 2  4  6 \  |  2  4  6  |
  4    SD    Brown     \_1_ 3 _5_/   \_1_ 3 _5_/   \_1__3__5_/
  5    SC    Green         '-'           '-'
  6    GND   Blue      Socket Outside View / Plug Inside View
  Shield     Shield
```

Note: The pin numbers and names are printed on the GBA mainboard, colors as
used in Nintendo's AGB-005 and older 8bit cables.



## Serial Link/Power Supply Port (GBA-Micro: "EXT.")


```
  1  In  DC (Supply 5.2VDC)  ___________________
  2  Out V3 (SIO 3.3VDC)    |  1 2 3 4 5 6 7 8  |
  3  I/O SO (SIO RCNT.3)    | ================= |
  4  I/O SI (SIO RCNT.2)     \_________________/
  5  I/O SD (SIO RCNT.1)
  6  I/O SC (SIO RCNT.0)
  7  OUT DG (SIO GROUND)
  8  In  DG (Supply GROUND)
  -  -   -  (Shield not connected)
```



## Cable Diagrams (Left: GBA Cable, Right: 8bit Gameboy Cable)


```
  Big Plug  Middle Socket  Small Plug    Plug 1         Plug 2
   SI _________________     ____ SI       SI ______  ______SI
   SO ____________SO   |__ | ___ SO       SO ______><______SO
   GND____________GND______|____GND       GND_____________GND
   SD ____________SD____________ SD       SD               SD
   SC ____________SC____________ SC       SC _____________ SC
   Shield_______Shield_______Shield       Shield_______Shield
```



## Normal Connection


Just connect the plugs to the two GBAs and leave the Middle Socket
disconnected, in this mode both GBAs may behave as master or slave, regardless
of whether using big or small plugs.

The GBA is (NOT ???) able to communicate in Normal mode with MultiPlay cables
which do not have crossed SI/SO lines.



## Multi-Play Connection


Connect two GBAs as normal, for each further GBAs connect an additional cable
to the Middle socket of the first (or further) cable(s), up to four GBAs may be
connected by using up to three cables.

The GBA which is connected to a Small Plug is master, the slaves are all
connected to Large Plugs. (Only small plugs fit into the Middle Socket, so it's
not possible to mess up something here).



## Multi-Boot Connection


MultiBoot (SingleGamepak) is typically using Multi-Play communication, in this
case it is important that the Small plug is connected to the master/sender (ie.
to the GBA that contains the cartridge).



## Non-GBA Mode Connection


First of all, it is not possible to link between 32bit GBA games and 8bit
games, parts because of different cable protocol, and parts because of
different signal voltages.

However, when a 8bit cartridge is inserted (the GBA is switched into 8bit
compatibility mode) it may be connected to other 8bit games (monochrome
gameboys, CGBs, or to other GBAs which are in 8bit mode also, but not to GBAs
in 32bit mode).

When using 8bit link mode, an 8bit link cable must be used. The GBA link cables
won't work, see below modification though.



## Using a GBA 32bit cable for 8bit communication


Open the middle socket, and disconnect Small Plugs SI from GND, and connect SI
to Large Plugs SO instead. You may also want to install a switch that allows to
switch between SO and GND, the GND signal should be required for MultiPlay
communication only though.

Also, cut off the plastic ledge from the plugs so that they fit into 8bit
gameboy sockets.



## Using a GBA 8bit cable for 32bit communication


The cable should theoretically work as is, as the grounded SI would be required
for MultiPlay communication only. However, software that uses SD for
Slave-Ready detection won't work unless when adding a SD-to-SD connection (the
8bit plugs probably do not even contain SD pins though).




# <a name="auxsoundheadphonesocketandbatterypowersupply"></a>AUX Sound/Headphone Socket and Battery/Power Supply




## GBA, GBA-Micro, NDS, and NDS-Lite: Stereo Sound Connector (3.5mm, female)


```
  Tip     Audio Left         ___ ___ _____.-----------.
  Middle  Audio Right       (___|___|_____|           |
  Base    Ground              L   R   GND '-----------'
```

The NDS socket doesn't fully match regular 3.5mm plugs, one needs to cut-off a
portion of the DS case to be able to fully insert the plug, which still
requires a lot of pressure, furthermore, when fully inserted, left/right become
shortcut to mono, so one needs to pull-back the plug a bit to gain stereo
output.



## GBA SP and NDS - Power/Headphone Socket (EXT.2)


```
  Pin SP   NDS  Expl.
  1   P31  SL   Audio LOUT                          _____________
  2   P32  VIN  Supply Input (DC 5.2V)           SW| 5   ___   1 |SL
  3   P33  SR   Audio ROUT                         | ----   ---- |
  4   P34  SG   Audio GND (via 100uF to GND)       |_6__4   3__2_|
  5   P35  SW   Audio Speaker Disable (GND=Dis)    GND SG\_/SR VIN
  6        GND  Supply GND
  Shield        GND
```

External power input is used to charge the built-in battery, it cannot be used
to run the SP without that battery.



## NDS-Lite - Power Socket


```
  Pin  Expl.                                          __________
  1    Supply Input (DC 5.2V)                        /  ======  \
  2    Supply GND                                GND |___2__1___| VIN
```



## GBA-Micro - Power Socket


Uses an 8pin socket (which combines SIO and Power), for pin-outs, see

- [AUX Link Port](#auxlinkport)


## External Power Supply


GBA: DC 3.3V (no separate power socket, requires 2xAA-battery-shaped adapter)

GBA-SP/NDS: DC 5.2V (or DC 5V) (special connector on power/headphone socket)

NDS-Lite: DC 5.2V (or DC 5V) (another special connector on power socket)



## Internal Battery Supply


GBA: 2xAA (3V)

GBA-SP: Li-ion 3.7V, 600mAh (built-in, recharge-able)

GBA-Micro: Li-ion 3.8V, 460mAh (built-in, recharge-able)

NDS: Li-ion 3.7V, 850mAh (built-in, recharge-able)

NDS-Lite: Li-ion 3.7V, 1000mAh (built-in, recharge-able)



## Using PC +5V DC as Power Supply


Developers whom are using a PC for GBA programming will probably want to use
the PC power supply (gained from disk drive power supply cable) for the GBA as
well rather than dealing with batteries or external power supplies.

GBA: To lower the voltage to approximately 3 Volts use two diodes, type 1N 4004
or similar, the ring printed onto the diodes points towards the GBA side,
connected as such:

```
  PC +5V (red)   --------|>|---|>|--------  GBA BT+
  PC GND (black) -------------------------  GBA BT-
```

GBA SP, GBA Micro, NDS, and NDS-Lite: Works directly at +5V connected to EXT.2
socket (not to the internal battery pins), without any diodes.




# <a name="auxdsisdmmcpinouts"></a>AUX DSi SD/MMC Pin-Outs




## SD/MMC Transfer Modes


```
  Transfer Modes        SPI-Mode    1-bit-Bus  4-bit-Bus   SDIO
  MMC Cards             Optional    Yes        MMCplus     No
  SD Cards              Yes         Yes        Optional??  Optional
```

Note: SDIO is an extension to the SD protocol, allowing to access other
non-memory-card hardware (such like cameras or network adaptors) via SD
connectors.

Note: Original MMC cards don't support 4-bit bus, but there are revisions like
MMCplus and MMCmobile (with extra pin rows) which do support 4-bit and 8bit
bus.



## SD/MMC Pin-Outs


```
  MMC  MMCplus SD  miniSD microSD SPI-Mode    1-bit-Bus   4-bit/8bit-Bus
  1    1       1   1      2       /CS         CardDetect  Data3
  2    2       2   2      3       DataIn      CMD/REPLY   CMD/REPLY
  3    3       3   3      --      GND         GND         GND
  4    4       4   4      4       VDD         VDD         VDD
  5    5       5   5      5       CLK         CLK         CLK
  6    6       6   6      6       GND         GND         GND
  7    7       7   7      7       DataOut     Data        Data0
  --   8       8   8      8       /IRQ (SDIO) /IRQ (SDIO) Data1 or /IRQ (SDIO)
  --   9       9   9      1       NC          NC          Data2
  --   10      --  --     --      NC          NC          Data4  ;\
  --   11      --  --     --      NC          NC          Data5  ; MMCplus
  --   12      --  --     --      NC          NC          Data6  ; 8bit
  --   13      --  --     --      NC          NC          Data7  ;/
  --   --      --  10     --      Reserved    Reserved    Reserved
  --   --      --  11     --      Reserved    Reserved    Reserved
```

Moreover, the card sockets (not the cards themselves) are usually containing a
Card Detect switch, and, for SD card sockets, also a write protect switch:

```
  --   --      CD  CD     CD      Card Detect (senses if card is inserted)
  --   ---     WP  --     --      Write Protect (senses position of LOCK tab)
```

Note that the LOCK tab on SD cards is just a small piece of plastic without any
electronics attached to it, the actual switch/sensor is located in the SD card
socket (ie. the LOCK works much like the write-protect tabs on audio tapes,
video tapes, and floppy discs).

Card detect can be an actual switch (however, some sockets are simply having
dual contacts for Pin 3 (GND), one being GND, and the other becoming GNDed when
a cartridge is inserted).



## SD/MMC Card Shapes


```
     ______________________________
    /    __ __ __ __ __ __ __      |
   /    |  |  |  |  |  |  |  |     |
  |     | 1| 2| 3| 4| 5| 6| 7|     |
  | MMC |__|__|__|__|__|__|__|     |
  |    ______________________________
  |   /    __ __ __ __ __ __ __ _    |
  |  /  __|  |  |  |  |  |  |  | |   |
  | |  |  | 1| 2| 3| 4| 5| 6| 7|8|   | MMCplus:
  | |  | 9|__|__|__|__|__|__|__| |   | pinout is same as 9pin SD cards,
  | |  |_ |_ __ __       __ __ | |   | with extra DAT4-7 on pin10-13
  | |    |  | 1| 1|     | 1| 1|  |   |
  | |MMC | 9| 0| 1|     | 2| 3| 8|   |
  | |plus|__|__|__|     |__|__|__|   |
  | |    ______________________________
  | |   /    __ __ __ __ __ __ __ _    |
  | |  /  __|  |  |  |  |  |  |  | |   |
  | | |  |  | 1| 2| 3| 4| 5| 6| 7|8|   |
  | | |  | 9|__|__|__|__|__|__|__|_|   |
  | | '. |__| SD                      .'      SD Write Protect Tab
  | |  |  _________________________   |   <-- Unlock position
  | | .' |  _ _ _ _ _ _ _ _ _ _ _  |  |#  <-- LOCK position
  | | |  | | | | | |1|1| | | | | | |  '.
  | | |  | |9|1|2|3|0|1|4|5|6|7|8| |   |
  |_| |  | |_|_|_|_|_|_|_|_|_|_|_| |   |
    | |  |   miniSD                 \  |
    | |  |     _________________     | |
    | |  |    | _ _ _ _ _ _ _ _ |    | |
    | |  |    || | | | | | | | ||    | |
    |_|  |    ||1|2|3|4|5|6|7|8||    | |
      |  |    ||_|_|_|_|_|_|_|_||    | |
      |  |   /                  |    | |
      |  |  |_  microSD         |    | |
      |  |    |                 |    | |
      |  |   /                  |    | |
      |  |  |                   |    | |
      |  |  |                   |    | |
      |  |_ |                   | ___| |
      |____ |___________________| _____|
```



## SD/MMC Signals for on-board eMMC chip on DSi Mainboard "C/TWL-CPU-01"


Below are the required eMMC signals. Low-end hardware may get away with using
Data0 as single data line (eg. small microprocessors with few I/O pins), but
higher quality hardware should support 4bit data mode (eg. off-the-shelve SD
card interfaces may insist on all four data lines being connected). Data3 (aka
CardDetect) might be also needed even in 1bit data mode.

```
                         _______ CLK (SPI: CLK)    _______ Data3 (SPI: /CS)
                        |                         | ______ Data0 (SPI: DataOut)
                 <#>  <#>  <#>                    || _____ Data1
                EM14  R113 C130                   ||| ____ Data2
        .------------------------.                ||||
        |                        |          <#>  <####> #  #
        |                        |     U5   C57   RA4  C54 C55
        |  Shielding-plate       '---.  o .------------------.
        |                            |    |o                 |
        |                            |    |                  |
        |                            |    |  Samsung     834 |
        |                            |    |  KMAPF0000M-S998 |
        |  CPU              RAM      |    |                  |
        |                            |    |                  |
        |                            |    '------------------'
        '----------------------------'     R94 R54  C50 C51
             |                             <#> <#>  <#> <#>
             |                               |
             |___ shield = GND               |___ CMD/REPLY (SPI: DataIn)
```

For other DSi mainboards, see:

- [http://www.dsibrew.org/wiki/Hardware](http://www.dsibrew.org/wiki/Hardware)

The KMAPF0000M chip does probably NOT support SPI mode, and it does probably
support only MMC protocol (not SD protocol). That, assuming that the chip does
have similar capabilities as in KMCEN0000M datasheet (there's no KMAPF0000M
datasheet online).



## SD/MMC Signals and 3DS Hardmodding


Pictures with eMMC solder pads for 3DS mainboards boards can be found at:

- [https://www.3dbrew.org/wiki/Hardware](https://www.3dbrew.org/wiki/Hardware)

However, hardmodding is much more difficult for 3DS as than for DSi. Three
3DS-specific problems:


## CLK resistor

  - The 3DS tends to hold CLK low, and there is no resistor installed between
  - 3DS CPU and eMMC chip, so the card reader may be unable to pull CLK high.
  - As workaround, install a resistor in the CLK line (the mainboard has square
  - solder pads just for that purpose) (330..660 ohms have worked for me, I am
  - currently using 470 ohms; smaller resistors like 0..120 ohm won't work with
  - the card reader, and much larger resistors won't work with the 3DS itself).

## Incompatible Card readers

  - Newer 3DS/New3DS models seem to contain eMMC chips that are incompatible with
  - older card readers, maybe because their firmware/driver treats the CSD
  - version number as unknown, despite of the eMMC chip being nothing special.
  - As workaround, try to use some newer card readers, or use a DSi console as
  - card reader (ie. write you software that can happily ignore CSD version).

## Mechanical problems

  - Most solder points are on the wrong PCB side (unlike as on DSi, you will
  - need to remove the 3DS mainboard). Whereas, the connector for the top screen
  - backlight is rated to survive max 10 mating cycles (mine died after 5-7).
  - Part of the problem was that I wasn't aware of the other problems mentioned
  - above (so I had removed the mainboard several times to check my soldering),
  - and, the connector is 'underneath' of the PCB, making it difficult to insert
  - the cable straight and smoothly, causing pins in the connector may get bent,
  - making it impossible to insert the ribbon cable (maybe it will help if you
  - take time, and don't impatiently try to push the cable into the socket).



## Soldering Notes


Connect CLK/CMD to the pins on right side of R113/R94 (as shown in the
drawing). Connect Data3/0/1/2 to the LOWER pins of RA4 (unlike as shown in the
drawing, ie. NOT to the upper pins), or alternately, connect them to the four
vias below of RA4. Connect GND somewhere to shielding plate, for example.

My own setup is: A 8pin ribbon cable soldered to a spare SD-to-SDmicro adapter
(used as connector for SD/MMC slots), the ribbon cable is wired to a small
circuit board, which is soldered to the shielding of the DSi's game cartridge
slot (just for mechanical stability). Next, some fragile wires are forwarded
from the circuit board to the actual mainboard pins.



## Software Notes


Remove the DSi wifiboard (not absolutely required, but doing so will hang the
DSi before accessing the eMMC, which ensures that the eMMC won't be accessed
simultaneously by the DSi and PC). Switch on the DSi. Connect it to SD/MMC card
reader. Under Windows, the eMMC should show up as MMC-storagedevice in Windows
Explorer (alongsides with your HDD drives), due to the encryption it isn't
possible to access the filesystem or logical partitions of the chip. However,
the physical sectors can be accessed.

For example, using HxD hex editor: Click Extras, Open Disk, and select the MMC
(in HxD it shows up under Physical Discs: as Removeable Disk). Click Edit,
Select All, Copy. Click File, New. Then Edit, Paste. And finally File, Save As
for saving an image of the whole 240MByte FLASH chip.

I've tried accessing the eMMC on two PCs, one worked, the other didn't:

```
  Win98 with External Card reader: Windows didn't recognize the MMC chip
  Win7  with External Card reader: Okay (recognized as "unformatted" disk)
  Win7  with Internal Card reader: Okay (recognized as "unformatted" disk)
```

For testing the Operating System/Card Reader side: Connect a normal SD card to
the card reader. If HxD is showing it as both Logical Disc and Physical Disc,
then you are fine. If it shows up as Logical Disc only, then your setup won't
work for accessing the eMMC chip.




# <a name="auxopeningthegba"></a>AUX Opening the GBA



Since Nintendo uses special screws with Y-shaped heads to seal the GBA (as well
as older 8bit gameboys), it's always a bit difficult to loosen these screws.



## Using Screwdrivers


One possible method is to use a small flat screwdriver, which might work, even
though it'll most likely damage the screwdriver.

Reportedly, special Y-shaped screwdrivers for gameboys are available for sale
somewhere (probably not at your local dealer, but you might find some in the
internet or elsewhere).



## Destroying the Screws


A more violent method is to take an electric drill, and drill-off the screw
heads, this might also slightly damage the GBA plastic chase, also take care
that the metal spoons from the destroyed screws don't produce shortcuts on the
GBA mainboard.



## Using a selfmade Screwdriver


A possible method is to take a larger screw (with a normal I-shaped, or
X-shaped head), and to cut the screw-tip into Y-shape, you'll then end up with
an "adapter" which can be placed in the middle between a normal screwdriver and
gameboy screws.

Preferably, first cut the screw-tip into a shape like a "sharp three sided
pyramid", next cut notches into each side. Access to a grinding-machine will be
a great benefit, but you might get it working by using a normal metal-file as
well.



## Opening the GBA Micro


- open the case with appropriate screwdriver or drilling machine or whatever

- remove the plastic front-plate (there are two snap-ins inside at ONE side)

- remove the mainboard and screen and plastic skeleton from the metal case

- remove the start/select daughter-board from the plastic skeleton

- remove the plastic skeleton (move the screen through the skeleton)

- remove the screen (lift lcd socket front-side, backlight socket rear-side)



## Opening the NDS-Lite


- open the case with appropriate screwdriver or drilling machine or whatever

- remove the RFU unit, and the 4-pin touch-screen cable (under the RFU unit)

- remove the mainboard together with the lower screen

- remove the upper/lower screen cables (on the rear-side of the mainboard)




# <a name="auxmainboard"></a>AUX Mainboard



Other possibly useful signals on the mainboard...



## FIQ Signal


The FIQ (Fast Interrupt) signal (labeled FIQ on the mainboard) could be used as
external interrupt (or debugging break) signal.

Caution: By default, the FIQ input is directly shortcut to VDD35 (+3V or +5V
power supply voltage), this can be healed by scratching off the CL1 connection
located close to the FIQ pin (FIQ still appears to have an internal pull-up, so
that an external resistor is not required).

The GBA BIOS rejects FIQs if using normal ROM cartridge headers (or when no
cartridge is inserted). When using a FIQ-compatible ROM header, Fast Interrupts
can be then requested by pulling FIQ to ground, either by a push button, or by
remote controlled signals.



## RESET Signal


The RESET signal (found on the mainboard) could be used to reset the GBA by
pulling the signal to ground for a few microseconds (or longer). The signal can
be directly used (it is not shortcut to VDD35, unlike FIQ).

Note: A reset always launches Nintendo's time-consuming and annoying boot/logo
procedure, so that it'd be recommend to avoid this "feature" when possible.



## Joypad Signals


The 10 direction/button signals are each directly shortcut to ground when
pressed, and pulled up high otherwise (unlike 8bit gameboys which used a 2x4
keyboard matrix), it'd be thus easy to connect a remote keyboard, keypad,
joypad, or read-only 12bit parallel port.




# <a name="auxdsicomponentlists"></a>AUX DSi Component Lists




## DSi Mainboard "C/TWL-CPU-01" Components


```
  U1 352pin   CPU TWL       (under shielding plate)                    ;\under
  U2   ?pin   RAM 8Mx16, Fujitsu MB82DBS08164D-70L, NEC uPD46128512AF1 ;/shield
  U3  56pin   "TexasIns 72071B0" or "Mitsumi 3317A" (Powerman) (right of NAND)
  U4  48pin   "AIC3000D, TI 89K, EXDK G4" (PAIC3 codec? above headphone socket)
  U5   ?pin   Samsung KMAPF0000M-S998 (eMMC, 256Mbyte NAND FLASH)
  U6  36pin   "BPTWL, K007K, 0902KM00D" (small/square, left of cartridge slot)
  U7   8pin   "AOK, S8BXS" (ISL95810, I2C potentiometer)         ;\on PCB
  U8   8pin   "7BDS" (PCA9306, I2C voltage translator)           ;/backside
  U9  12pin   "199A, 01IU" (Seiko S-35199A01) (RTC) ;under shielding plate (A)
  U10  4pin   "6800" or "688F" Hinge Magnet Sensor (PCB backside, near A/B/X/Y)
  U11 10pin   ",\\ 908, 335A" or "2005D, 8350" (right of cartridge slot)
  U12  5pin   "L8NX" or "C7JHN" (upper-right of PCB back-side) ;text layer (B)
  U13  5pin   Backlight 1, "U01" or "KER"  ;\lower-right board edge
  U14  5pin   Backlight 2, "U01" or "KER"  ;/see text-layer (B)
  U15  4pin   VIN to EXB+ control via Q1, ",\\ T34" (near external power input)
  U16     -   N/A
  U17  6pin   Headphone Mute, "VY" or "Z198" (in lower-right, on PCB backside)
  U18  6pin   Mic Bias, "YJ" (above headphone socket, not always installed)
  U19  5pin   "E30H6" or "L2SX" (at lower right of cartridge slot)
  U20  3pin   Whatever, DSi XL only (maybe magnet Sensor instead of U10?)
  Q1   6pin   external power supply related (VIN to EXTB+) (controlled by U15)
  Q2    pin   N/A ?
  Q3   6pin   ... above battery plug
  Q4   3pin   Headphone MUTE for SR  ;\old TWL-CPU-01 mainboard only
  Q5   3pin   Headphone MUTE for SL  ;/(replaced by Q17/Q18 on newer boards)
  Q6   6pin   MC1_VDD power ON (supply)
  Q7   3pin   MC1_VDD power OFF (pulldown)
  Q8    pin   N/A ?
  Q9    pin   N/A ?
  Q10   pin   N/A ?
  Q11  3pin   BLUE (LED)   ;\LEDs (note: the other LEDs, ORANGE
  Q12  3pin   YELLOW (LED) ;      and YELLOW, are driven directly)
  Q13  3pin   CAM_LED      ;/
  Q14  3pin   not installed (above powerman chip)
  Q15  3pin   not installed (above powerman chip)
  Q16  3pin   VDD-5 related, near DPAD socket (P15)
  Q17  6pin   Headphone MUTE for ..?  ;\new TWL-CPU-10 mainboard only
  Q18  6pin   Headphone MUTE for ..?  ;/(formerly Q4/Q5 on older boards)
  X1   4pin   16.756  (rectangular oscillator)        ;\under shielding plate
  X2   4pin   CB837 or CB822 (long slim osc) for RTC? ;/text layer: see (A)
  F1   2pin   Fuse for external power input
  SW1  2pin   Button A (right)
  SW2  2pin   Button B (lower)
  SW3  2pin   Button X (upper)
  SW4  2pin   Button Y (left)
  SW5  2pin   Button Select (lower)
  SW6  2pin   Button Start  (upper)
  P1  19pin   NDS/DSi cartridge slot (17pin slot + 2pin switch at right side)
  P2      -   N/A
  P3      -   N/A
  P4   8pin   External microphone/headphone combo socket
  P5  50pin   Wifi-Daughterboard
  P6      -   N/A
  P7  47pin   To UPPER lcd screen (video+backlight+speakers) (on PCB backside)
  P8  37pin   To LOWER lcd screen (video signals)
  P9  25pin   To UPPER lcd screen (signals for both cameras, and camera led)
  P10  4pin   To LOWER lcd screen (touchpad X-,Y-,X+,Y+)
  P11  2pin   External Power Supply input (4.6V DC IN)
  P12     -   N/A
  P13     -   N/A
  P14     -   N/A
  P15 15pin   To battery/DPAD/PowerButton board (and onwards to 3xLEDs)
  P16 26pin   To bottom cover (SD Slot and L/R/VOL+/- buttons)
  P17  2pin   Battery cable (lower-right) ;see text-layer (B)
  P18  4pin   To LOWER lcd screen (backlight cathode/anode)
  P19  1pin   Shielding-Plate for CPU (lower clip)
  P20  1pin   Shielding-Plate for CPU (upper clip)
  P21  1pin   Shielding-Plate for CPU (right clip)
  P22     -   N/A
  P23  2pin   To Internal Microphone (via orange shielded wire)
  P24 14pin   DSi XL only (L/VOL+/-, which were formerly on P16 in old DSi)
```

DSi Front/bottom-Side Text Layer sections (in upper left of mainboard)

```
  (A)  For components underneath of shielding plate
  (B)  For components in lower-right board edge (near battery connector)
  (C)  For components at middle/right of cartridge slot
  (D)  For components left of U4 (left board edge)
  (E)  For components right of U4 (above headphone socket)
  (F)  For components at lower/right of cartridge slot
```

DSi Back/top-Side Text Layer sections (at various places)

```
  (A)  at top/middle,  for components at upper right edge
  (B)  at middle/left, for components near upper right edge
  (C)  at lower/left,  for components left of Y-button
  (D)  at lower/righz, for components at right edge
```



## DSi Wifi Daughterboard (DWM-W015) (older DSi version)


PCB Text: "RU (S)-717V 01 ,\\" or "RU (S)-717V 03 ,\\" or "FK RU 06 ,\\"

```
  U   56pin "Mitsumi, Japan, 844L, MM3218" (same as in DS Lite)
  U  132pin "ROCm, Atheros, AR6002G-AC1B, E19077.1B, 0844, Taiwan"
  U    8pin I2C EEPROM "408F, B837" (HN58X2408F; 1Kx8 for atheros calibration)
  U    8pin SPI FLASH big chip "45PE10V6, HPASC VS, KOR 8364, ST"  ;\either one
  U    8pin SPI FLASH tiny chip "5A32, 8936?"                      ;/installed
  U    8pin "4P, K" or "S6, K" (odd 3+1+3+1 pin package, near antenna)
  U    4pin "3VP, OT" or "3VB, OS" (at board edge, near 50pin connector)
  X    4pin "26.000, 9848" (bigger oscillator, for atheros chip)
  X    4pin "22.000, xxxx" (smaller oscillator, for mitsumi chip)
  P   50pin Connector to Mainboard
  P    2pin Connector for Antenna (shielded white cable)
```

The "3VP/3VP" thing is some 1.2V voltage regulator (sth like LP3983 or TPS799xx
or similar).

White PCB sticker (underneath of the black isolation sticker): "DWM-W015,
IC:4250A-DWMW015, FCC ID:EW4DWMW015, [R]003WWA080444, [T]D080261003, MADE IN
PHILIPINES, MITSUMI ELEC. CO., LTD."



## DSi Wifi Daughterboard (DWM-W024) (newer DSi version)


PCB Text: "FB RU 06 ,\\"

```
  U   76pin "ROCm, Atheros, AR6013G-AL1C" (or 80pin, with 4pins at edges?)
  U    8pin I2C EEPROM? "4DA?, D940?"   ;maybe I2C eeprom for atheros
  U    8pin SPI FLASH "5A32, 8937?"   ;FLASH (small solder pads)
  U    8pin SPI FLASH not installed (alternate bigger solder pads for FLASH?)
  U    4pin "?" (at board edge, near 50pin connector)
  X    4pin "??" (oscillator, near ROCm chip)
  P   50pin Connector to Mainboard
  P    2pin Connector for Antenna (shielded white cable)
```



## DSi Wifi Daughterboard (J27H020) (alternate newer DSi version)


This seems to be functionally same as DWM-W024, but the PCB is different, and
the circuit seems to use a different voltage regulator, and includes solder
pads for some additional uninstalled components. And the shielding is difficult
to remove because it's soldered all way round (rather than just at some solder
points).

- Sticker 1:
  - HON HAI PRECISION IND.CO.,LTD.        ;aka Foxconn
  - MODEL: J27H020
  - [R] 003WWA100195
  - [T] D100196003
  - FCC ID: MCLJ27H020
  - IC: 2878D-27H020
- Sticker 2:
  - (barcode)
  - \<mac.address> J27H020.00 LF, 4xxxxxx-xxx
- Board:
  - U1   76pin atheros ... "ATHEROS, AR6013G-AL1C, N2U586.00C, 1035, KOREA"
  - U2    -    -
  - U3    -    -
  - U4    8pin I2C EEPROM "G80, 8, G02"
  - U5    8pin SPI FLASH big chip "26FV032T, OGK01"  (installed)
  - U6    -    -
  - U7    8pin SPI FLASH small chip (not installed)
  - U?    5pin voltage regulator or so "IG19P"
  - U?    6pin whatever near antenna (not installed)
  - Y1    4pin crystal "H400K"
  - J3    2pin Connector for Antenna (shielded white cable)
  - J6   50pin Connector to Mainboard



## 3DS Wifi Daughterboard (DWM-W028, or J27H023, or J27H023.01)


Component list is unknown. The thing is said to use a "Atheros AR6014" chip.

Later 3DS models have the Wifi unit (with AR6014G chip) on the mainboard
(instead of using a removeable DWM board).



## DSi Battery/DPAD Daughterboard "C/TWL-SUB-01"


```
  TH1  2pin  Battery Thermistor Thermal Sensor (ca. 10kOhm at room temperature)
  F1   2pin  Battery Fuse
  SW1  2pin  DPAD Up Button
  SW2  2pin  DPAD Down Button
  SW3  2pin  DPAD Left Button
  SW4  2pin  DPAD Right Button
  SW5  2pin  Power/Reset Button
  P1  15pin  To Mainboard (P15) (button/led signals) (wire "15P-01")
  P2   6pin  To 3xLEDs
  P3   3pin  To battery (TWL-003 3.6V 840mAh 3Wh C/TWL-A-BP, Li-ion 00"
  Wire 2pin  To Mainboard (P17) (battery supply) (red=vcc, black=gnd)
```



## DSi LED Board/Foil "LED-01, (DF)"


```
  D    2pin  Left LED      ;-wifi
  D    2pin  Middle LED    ;-charge
  D    2pin  Right LED 1   ;\power "two-color-LED"
  D    2pin  Right LED 2   ;/composed of 2 single LEDs
  Wire 6pin  To Battery/DPAD Daughterboard
```



## DSi Lower Screen with Touchpad


```
  Wire 4pin  Touchpad
  Wire 4pin  Backlight (actually 2pins, each 2 pins are same)
  Wire xxpin Video Signals
  LCD  "LS033A1DG48R, 8X16Q U0003986"
```



## DSi Upper Screen with Speakers & Cameras & LED & Microphone


```
  Orange Ribbon Cable: Video Signals, Backlight, and Speakers
  Black Ribbon Cable: Cameras and Camera LED
  Shielded Orange 2pin Wire: Microphone
  Shielded White 2pin Wire: Wifi PCB Antenna
  LCD  "LS033A1DG38R, BX16Q L0005532"
  The speakers use red/black wires, which connect to the orange ribbon cable
```



## DSi Upper Screen Area Extra Components: Speakers & Cameras & LED & Microphone




## DSi Lower Case (SD Slot, L/R and VOL+/- Buttons, and screen calibration)


```
  Whatever, not checked yet
```



## DSi Disassembly Notes


Bottom Cover Screws:

```
  7 screws (two are under battery cover)
```

Remove bottom cover, and:

```
  P16: To bottom cover (SD Slot and L/R/VOL+/-) --> pull (away from board)
```

Remove Wifi Daughterboard:

```
  P5:  Wifi-board (without cable)               --> pull (away from board)
  WHITE: Wifi-Antenna (shielded 2pin)           --> pull (away from wifi-board)
```

Remove mainboard:

```
  ORANGE P24 (shielded 2pin)  --> pull (away from board)
  WHITE SUPPLY                --> lift (use screwdriver & push away from board)
  3x bigger white/black connectors --> lift black lid (at cable-side)
  2x smaller black connectors      --> lift black lid (at cable-end) (!!!)
```

Turn mainboard over, then unlock the connector at back side:

```
  1x bigger white/black connector  --> lift black lid (at cable-side)
```

Remove Battery board:

```
  1x smaller black connector       --> lift black lid (at cable-end) (!!!)
  1x bigger white/black connectors --> lift black lid (at cable-side)
  (don't disconnect bigger connector if the other cable end is already
  disconnected from mainboard) (or if you did do, reassemble as follows:
  longer cable end to battery board, short cable end to mainboard)
  1x battery cable (disconnect at mainboard side, see there)
```

Top Cover Disassembly:

```
  Disconnect upper LCD and mic/antenna from mainboard (see above)
  Remove 4 screws (all hidden under square rubber pieces)
  slide rear bezel upwards by two millimeters?
  push metal hinge inwards by three millimeters (under LED unit)
```




# <a name="auxdsisignalstestpoints"></a>AUX DSi Signals/Testpoints



The DSi has testpoints with friendly names. The DSi XL has "nameless"
testpoints (numbered TP1 thru TP194, as described on 
- [https://www.dsibrew.org/wiki/DSi\_XL\_testpoints](https://www.dsibrew.org/wiki/DSi_XL_testpoints) which may include some bugs for the Supply and BGA pins though).


```
  ADPO       TP... U3,U6        Charger connected? (Low=Connected, High=No)
  AOUT       TP... U3,U6        Analog battery level? (max=2.35V?)
  PM_SLP     TP... U1,U3,U6     Sleep or so? (Low=Normal, High=Sleep?)
  PMOFF      TP... U1,U3,U6,U17 Powerman/Headphone off? (Low=Normal)
  PWSWO      TP... U3,U6        Power? (Low=Off, or Reset-tapping, High=On)
  VCNT5      TP... U1,U3,U4,U19 Control for VSHD for Both Screens ?
  mFE        TP... U6          BPTWL Flash Enable (aka FLMD0)    (Low=Normal)
  /mRST      TP... U6          BPTWL Flash Invoke transfer?      (High=Normal)
  /IRQ_O     TP... U1,U6       BPTWL Interrupt (eg. Power Button)
  GPIO330    TP... U1          General Purpose GPIO bit (?)
  RESET      TP... P5,U1,U3..  Reset WifiFlash,MainCpu,PowerMan,TSC from BPTWL
  P00        TP... U1,SW1      Button A
  P01        TP... U1,SW2      Button B
  P02        TP... U1,SW5,U6   Button SELECT (also SELECT+VOLP/VOLN=Backlight)
  P03        TP... U1,SW6      Button START
  P04        TP... U1,P15      Button DPAD right
  P05        TP... U1,P15      Button DPAD left
  P06        TP... U1,P15      Button DPAD up
  P07        TP... U1,P15      Button DPAD down
  P08        TP... U1,P16      Button Shoulder R
  P09        TP... U1,P16      Button Shoulder L
  R00        TP... U1,SW3      Button X
  R01        TP... U1,SW4      Button Y
   -?-       TP... -           Button Debug aka R03 (if any exists)
  VOLP       TP... P16,U6      Button Volume Plus  (Backlight when SELECT+VOLP)
  VOLN       TP... P16,U6      Button Volume Minus (Backlight when SELECT+VOLN)
  PWSW       TP... P15,U3      Button Power (High=Released)
  PENIRQ     TP... U1,U4       Touchscreen Pen Down
  R7         TP... U1,U10      Shell Open Magnet Sensor (HINGE)
  RTC_INT    TP... U9,U1       RTC Real-Time Clock Interrupt
  -          TP... U9,U1       RTC Real-Time Clock CS
  -          TP... U9,U1       RTC Real-Time Clock /SCK
  -          TP... U9,U1       RTC Real-Time Clock SIO
  SCL        TP... P9,U1,..    Primary I2C Bus Clock (Both Cameras,etc.)
  SDA        TP... P9,U1,..    Primary I2C Bus Data  (Both Cameras,etc.)
  -          TP... U8,U6       Primary I2C Bus Clock (for BPTWL xlated to 3.3V)
  -          TP... U8,U6       Primary I2C Bus Data  (for BPTWL xlated to 3.3V)
  SCL1       TP... U3,U6,U7    Secondary IC2 bus Clock (BPTWL/Wiper/Powerman)
  SDA1       TP... U3,U6,U7    Secondary IC2 bus Data  (BPTWL/Wiper/Powerman)
  SD10_CLK   TP... U1,P16      SD/MMC SD Slot Clock (typ 16MHz)
  SD10_CMD   TP... U1,P16      SD/MMC SD Slot Cmd
  SD10_DATA0 TP... U1,P16      SD/MMC SD Slot Data0
  SD10_DATA1 TP... U1,P16      SD/MMC SD Slot Data1
  SD10_DATA2 TP... U1,P16      SD/MMC SD Slot Data2
  SD10_DATA3 TP... U1,P16      SD/MMC SD Slot Data3
  SD10_WP    TP... U1,P16      SD/MMC SD Slot Write Protect Switch
  SD10_CD    TP... U1,P16      SD/MMC SD Slot Card Detect Switch
  SD10_VDD   TP... P16         SD/MMC SD Slot Supply (aka VDD33)
  SD11_CLK   TP... U1,U5       SD/MMC eMMC Clock (typ 16MHz)
  -          TP... U1,U5       SD/MMC eMMC Cmd/Data0..3/CD/WP/VDDI
  SPI_CS1    TP... U1,U3,U4    SPI Chip Select Powerman
  SPI_CS2    TP... U1,P5       SPI Chip Select Wifi FLASH chip select
  SPI_CS3    TP... U1,U4       SPI Chip Select Touchscreen/Sound TSC
  -          TP... P5,R122     SPI Wifi FLASH_WP (R122)
  SPI_SCK    TP... U1,P5,U3,U4 SPI Wifi/Pow/TSC Clock
  SPI_MISO   TP... U1,P5,U3,U4 SPI Wifi/Pow/TSC Master In Slave Out (Reply)
  SPI_MOSI   TP... U1,P5,U3,U4 SPI Wifi/Pow/TSC Master Out Slave In (Command)
  MC1_CLK    TP... U1,P1       NDS/DSi cartridge slot
  MC1_CS     TP... U1,P1       NDS/DSi cartridge slot
  MC1_RES    TP... U1,P1       NDS/DSi cartridge slot
  MC1_CS2    TP... U1,P1       NDS/DSi cartridge slot
  MC1_IREQ   TP... U1,P1       NDS/DSi cartridge slot
  MC1_VDD    TP... P1,Q6/Q7    NDS/DSi cartridge slot Power Supply (when on)
  -          TP... U1,R28,Q7   NDS/DSi cartridge slot Power Enable
  MC1_IO0    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO1    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO2    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO3    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO4    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO5    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO6    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_IO7    TP... U1,P1       NDS/DSi cartridge slot Data
  MC1_DET    TP... U1,P1       NDS/DSi cartridge slot Cart Insert Switch
  -          TP... U1          NDS/DSi 2nd cart slot (NC in retail console)
  -          TP... U1,U2       Main RAM Address A0..A22
  -          TP... U1,U2       Main RAM Data DQ0..DQ15
  -          TP... U1,U2       Main RAM /OE
  -          TP... U1,U2       Main RAM /WE
  -          TP... U1,U2       Main RAM CE1
  -          TP... U1,U2       Main RAM CE2
  -          TP... U1,U2       Main RAM CLK
  -          TP... U1,U2       Main RAM /ADV
  -          TP... U1,U2       Main RAM /LB
  -          TP... U1,U2       Main RAM /UB
  -          TP... U1          Main RAM A23 or Chipsel for debug ver 32Mbyte
  -          TP... P5,U1       Wifi Atheros SDIO Clock (typ 16MHz?)
  -          TP... P5,U1       Wifi Atheros SDIO Command
  -          TP... P5,U1       Wifi Atheros SDIO Data 1
  -          TP... P5,U1       Wifi Atheros SDIO Data 2
  -          TP... P5,U1       Wifi Atheros SDIO Data 3
  -          TP... P5,U1       Wifi Atheros SDIO Data 3
  -          TP... P5,U9       Wifi Atheros 32kHz (from RTC_FOUT to Wifi)
  /WIFI_RST  TP... P5,U6       Wifi Atheros Reset?
  ATH_TX_H   TP... P5,U6       Wifi Atheros TX (for Wifi LED?)
  -          TP... P5,U1       Wifi SEL_ATH_L (near PENIRQ pin)
  WL_RXPE    TP... P5,U1,U6    Wifi RX something (for CPU and Wifi LED)
  WL_TXPE    TP... P5,U1,U6    Wifi TX something (for CPU and Wifi LED)
  -          TP... P5,U1       Wifi Mitsumi BBP_SLEEP
  -          TP... P5,U1       Wifi Mitsumi RF_SLEEP
  -          TP... P5,U1       Wifi Mitsumi BBP_SCS
  -          TP... P5,U1       Wifi Mitsumi RF_SCS
  -          TP... P5,U1       Wifi Mitsumi RB_RF_SDO
  -          TP... P5,U1       Wifi Mitsumi RB_RF_SDI
  -          TP... P5,U1       Wifi Mitsumi RB_RF_SCLK
  -          TP... P5,U1       Wifi Mitsumi CCA
  -          TP... P5,U1       Wifi Mitsumi MCLK
  -          TP... P5,U1       Wifi Mitsumi TRDATA
  -          TP... P5,U1       Wifi Mitsumi TRCLK
  -          TP... P5,U1       Wifi Mitsumi TRRDY
  -          TP... P5          Wifi JTAG_TDO    (NC in console)
  -          TP... P5          Wifi JTAG_TDI    (NC in console)
  -          TP... P5          Wifi JTAG_TMS    (NC in console)
  -          TP... P5          Wifi JTAG_TCK    (NC in console)
  -          TP... P5          Wifi JTAG_TRST_L (NC in console)
  -          TP... P5          Wifi to MM3218.pin47 (0ohm)     (NC in console)
  .          TP... P5          Wifi to MM3218.pin28 (0ohm+cap) (NC in console)
  SND_SDI    TP... U1,U4       Audio I2S Data In   (Microphone)
  SND_SDO    TP... U1,U4       Audio I2S Data Out  (Speaker/Headphone)
  SND_WS     TP... U1,U4       Audio I2S LR Clock  (typ 32kHz)
  SND_SCLK   TP... U1,U4       Audio I2S Bit Clock (typ 1MHz, aka SND_WS*2*16)
  SND_MCLK   TP... U1,U4       Audio TSC Master Clock (typ 8MHz)
  -          TP... U4,U7       Audio Volume Wiper (from I2C potentiometer)
  SPLN       TP... P7,U4       Audio Speaker Left Minus
  SPLP       TP... P7,U4       Audio Speaker Left Plus
  SPRN       TP... P7,U4       Audio Speaker Right Minus
  SPRP       TP... P7,U4       Audio Speaker Right Plus
  HP#SP      TP... P4,U1,U4    Audio Headphone/Speaker switch
  SL         TP... P4,Q5,U4    Audio Headphone Left
  SR         TP... P4,Q4,U4    Audio Headphone Right
  -          TP... U1,U17      Audio Headphone Mute (from Main CPU GPIO)
  -          TP... U4,U17,Q..  Audio Headphone Mute (via Q4/Q5 to SR/SL)
  MIC        TP... P4,P23      Audio Microphone (via .. to LIN)
  LIN        TP... U4,U18      Audio Microphone Input
  -          TP... U4,U18      Audio Microphone Bias (to U18)
  BLA2       TP... P7,U3,U14   LCD Upper Screen Backlight Anode (ca. 15V)
  BLC2       TP... P7,U3       LCD Upper Screen Backlight Cathode
  LDB20      TP... P7,U1       LCD Upper Screen Blue Bit0
  LDB21      TP... P7,U1       LCD Upper Screen Blue Bit1
  LDB22      TP... P7,U1       LCD Upper Screen Blue Bit2
  LDB23      TP... P7,U1       LCD Upper Screen Blue Bit3
  LDB24      TP... P7,U1       LCD Upper Screen Blue Bit4
  LDB25      TP... P7,U1       LCD Upper Screen Blue Bit5
  LDR20      TP... P7,U1       LCD Upper Screen Red Bit0
  LDR21      TP... P7,U1       LCD Upper Screen Red Bit1
  LDR22      TP... P7,U1       LCD Upper Screen Red Bit2
  LDR23      TP... P7,U1       LCD Upper Screen Red Bit3
  LDR24      TP... P7,U1       LCD Upper Screen Red Bit4
  LDR25      TP... P7,U1       LCD Upper Screen Red Bit5
  LDG20      TP... P7,U1       LCD Upper Screen Green Bit0
  LDG21      TP... P7,U1       LCD Upper Screen Green Bit1
  LDG22      TP... P7,U1       LCD Upper Screen Green Bit2
  LDG23      TP... P7,U1       LCD Upper Screen Green Bit3
  LDG24      TP... P7,U1       LCD Upper Screen Green Bit4
  LDG25      TP... P7,U1       LCD Upper Screen Green Bit5
  COM2       TP... P7,P16      LCD Upper Screen VCOM Calibration
  DCLK       TP... P7,P8,U1    LCD Both Screens Dot Clock  ;2.8MHz
  LS         TP... P7,P8,U1    LCD Both Screens Latch Pulse;15.6kHz, 0.3us high
  SPL        TP... P7,P8,U1    LCD Both Screens Start Pulse;15.6kHz, 0.3us high
  GCK        TP... P7,P8,U1    LCD Both Screens Gate Clock ;15.6kHz, 9us low
  REV        TP... P7,P8,U1    LCD Both Screens VcomFlicker;7kHz, toggle@15kHz
  GSP        TP... P7,P8,U1    LCD Both Screens Gate Start ;60Hz, short low
  INI        TP... P7,P8,U1    LCD Both Screens Init? MOD? ;usually high?
  VSHD       TP... P7,P8,U19   LCD Both Screens Supply 3.3V
  VDD5       TP... P7,P8,P16   LCD Both Screens Supply 5.0V
  VDD-5      TP... P7,P8       LCD Both Screens Supply -5.0V
  VDD10      TP... P7,P8       LCD Both Screens Supply 10.0V
  BLA1       TP... P18,U3,U13  LCD Lower Screen Backlight Anode (ca. 14V)
  BLC1       TP... P18,U3      LCD Lower Screen Backlight Cathode (ca. 0V)
  LDB10      TP... P8,U1       LCD Lower Screen Blue Bit0
  LDB11      TP... P8,U1       LCD Lower Screen Blue Bit1
  LDB12      TP... P8,U1       LCD Lower Screen Blue Bit2
  LDB13      TP... P8,U1       LCD Lower Screen Blue Bit3
  LDB14      TP... P8,U1       LCD Lower Screen Blue Bit4
  LDB15      TP... P8,U1       LCD Lower Screen Blue Bit5
  LDR10      TP... P8,U1       LCD Lower Screen Red Bit0
  LDR11      TP... P8,U1       LCD Lower Screen Red Bit1
  LDR12      TP... P8,U1       LCD Lower Screen Red Bit2
  LDR13      TP... P8,U1       LCD Lower Screen Red Bit3
  LDR14      TP... P8,U1       LCD Lower Screen Red Bit4
  LDR15      TP... P8,U1       LCD Lower Screen Red Bit5
  LDG10      TP... P8,U1       LCD Lower Screen Green Bit0
  LDG11      TP... P8,U1       LCD Lower Screen Green Bit1
  LDG12      TP... P8,U1       LCD Lower Screen Green Bit2
  LDG13      TP... P8,U1       LCD Lower Screen Green Bit3
  LDG14      TP... P8,U1       LCD Lower Screen Green Bit4
  LDG15      TP... P8,U1       LCD Lower Screen Green Bit5
  COM1       TP... P8,P16      LCD Lower Screen VCOM Calibration
  X-         TP... P10,U4      LCD Lower Screen Touchpad X-
  Y-         TP... P10,U4      LCD Lower Screen Touchpad Y-
  X+         TP... P10,U4      LCD Lower Screen Touchpad X+
  Y+         TP... P10,U4      LCD Lower Screen Touchpad Y+
  VIN        TP2   P11         Supply 4.6V External Power Supply Input (DC IN)
  VGND       TP5   P11         Supply 0.0V External Power Supply Ground
  VDET-      TP... U3,Q3       Supply 4.xV Shortcut with BT+ (via 0 ohm R71)
  BT+        TP... P17,U3,U11  Supply 4.xV Battery Plus (red wire) 3.6V 800mAh
  BT-        TP... P17         Supply 0.0V Battery GND  (black wire)
  DET        TP... P15,U3      Supply Battery Middle Pin  (Low=Normal)
  TH         TP... P15,U11     Supply Battery Thermal Sensor (on DPAD board)
  PVDD       TP... U3,U9       Supply 4.xV Permanent Power for RTC
  VDD12      TP... U3,U1       Supply 1.2V (Main CPU)
  VDD18      TP... P9,U3,U1,.. Supply 1.8V (Main CPU and many things)
  VDD28      TP... P9,U12,U6   Supply 2.8V (for Cameras and BPTWL)
  VDD33      TP... U3,U1,..    Supply 3.3V (Main CPU and many things)
  VDD42      TP... P15,P9,U3.. Supply 4.2V (for LEDs) (and U19)
  B+         TP... U3,U4,U19.. Supply 4.xV (for Speakers, or everything?) U11
  SG         TP... U3          Supply 0.0V Ground or Audio GND or so
  GND1       TP... ..          Supply 0.0V Ground (near COM1/COM2 testpoints)
  GND2       TP... ..          Supply 0.0V Ground (near wifi connector)
  EXTB+      TP... U3,U11,Q1   Charger 4.xV or 0V (EXTB+ comes from VIN via Q1)
  Rosc       TP... U3,U11      Charger 1.0V ?
  RICHG      TP... U3,U11      Charger 0.4V ?
  ORANGE     TP... P15,U11     LED Battery Charge
  BLUE       TP... P15,U6,Q11  LED Power On/Good
  YELLOW     TP... P15,U6,Q12  LED Wifi
  RED        TP... P15,U6      LED Power On/Low
  CAM_LED    TP... P9,U6,Q13   LED Camera
  CAM_D0     TP... P9,U1       Both Cameras Data Bit0
  CAM_D1     TP... P9,U1       Both Cameras Data Bit1
  CAM_D2     TP... P9,U1       Both Cameras Data Bit2
  CAM_D3     TP... P9,U1       Both Cameras Data Bit3
  CAM_D4     TP... P9,U1       Both Cameras Data Bit4
  CAM_D5     TP... P9,U1       Both Cameras Data Bit5
  CAM_D6     TP... P9,U1       Both Cameras Data Bit6
  CAM_D7     TP... P9,U1       Both Cameras Data Bit7
  CAM_RST    TP... P9,U1       Both Cameras Reset?
  HSYNC      TP... P9,U1       Both Cameras Hsync
  VSYNC      TP... P9,U1       Both Cameras Vsync
  RCLK       TP... P9,U1       Both Cameras Data Clock (8MHz, unless blanking)
  CKI        TP... P9,U1       Both Cameras Master Clock (8MHz) (or off)
```



## Cut-Links


```
  CL1   VDD12    to/near Powerman
  CL2
  CL3   VDD18    to/near Powerman
  CL4    ?       near Powerman (above L3)
  CL5   VDD33    PCB top side (near Powerman)
  CL6   VDD5     near lower screen connector (F)
  CL7   BLC1     near lower screen connector
  CL8
  CL9   GNDed     to/near Powerman (near ADPO pin)
  CL10  GNDed     to/near Powerman (near VDD5 pin)
  CL11
  CL12
  CL13
  CL14  SDIO CLK   to/near wifi connector
  CL15  BB RF CLK  to/near wifi connector
  CL16  Wifi MCLK  to/near wifi connector
  CL17
  CL18
  CL19
  CL20
  CL21  VDD42    to/near Powerman
  CL22  VDD10    to DA1 and C5, near DPAD board connector
  CL23  VDD-5    near lower screen connector
  CL24
  CL25  ?        PCB top side (near Button B)
  CL26
  CL27  ?        PCB top side (near Button B) (D)
  CL28  ?        PCB top side (near Button B) (D)
  CL29
  CL30
  CL31
  CL32
  CL33
  CL34  SND_SDI   near TSC chip
  CL35  SND_SDO   near TSC chip
  CL36  SND_WS    near TSC chip
  CL37  SND_SCLK  near TSC chip
  CL38  SND_MCLK  near TSC chip
  CL39
```



## Short-Links


```
  R122  FLASH_WP to VDD33 (not installed)
```




# <a name="auxdsiinternalconnectors"></a>AUX DSi Internal Connectors




## P1 - 19pin - NDS/DSi cartridge slot (17pin slot + 2pin switch at right side)


```
  1   GND
  2   MC1_CLK
  3   -
  4   MC1_CS
  5   MC1_RES
  6   MC1_CS2
  7   MC1_IREQ
  8   MC1_VDD via Q6 to VDD33 (cpu signal preamplified from Q7)
  9   MC1_IO0
  10  MC1_IO1
  11  MC1_IO2
  12  MC1_IO3
  13  MC1_IO4
  14  MC1_IO5
  15  MC1_IO6
  16  MC1_IO7
  17  GND
  18  MC1_DET           ;\switch closed when cart inserted
  19  GND               ;/
  Shield GND
```



## P4 - 8pin - External microphone/headphone combo socket


```
  1  GND    ;\      ;\
  2  SL     ; head- ; headphone gnd/left/right
  3  SR     ; phone ;/
  4  GND    ;       ;\headphone/speaker switch (pin 4+5 shortcut with each
  5  HP#SP  ;/      ;/other when no headphone connected)
  6  MIC    ;\    ;\microphone switch (pin6+7 shortcut when no mic connected)
  7  Switch ; mic ;/(internal mic from P23 is then passed from pin7 to pin6)
  8  GND    ;/
```



## P5 - 50pin - DSi Wifi-Daughterboard (DWM-W015, DWM-W024, or J27H020)


```
                           GND   2  1   SDIO.CLK       ;\
                         VDD18   4  3   GND            ; SDIO for
                         VDD18   6  5   SDIO.DAT0      ; Atheros Wifi
                           GND   8  7   SDIO.DAT3      ; (CLK, CMD, DATA0-3)
                         VDD33  10  9   SDIO.DAT1      ;
                         VDD33  12  11  SDIO.CMD       ;
                           GND  14  13  SDIO.DAT2      ;/
                      ATH_TX_H  16  15  DSi: NC (DWM: JTAG_TDO)
                     /WIFI_RST  18  17  DSi: NC (DWM: JTAG_TMS)
     (DWM:JTAG_TDI)    DSi: NC  20  19  GND
     (DWM:JTAG_TCK)    DSi: NC  22  21  RTC_FOUT (or RTC_F32K?)  ;for Atheros?
     (DWM:JTAG_TRST_L) DSi: NC  24  23  GND
 (near CPU irq pins) SEL_ATH_L  26  25  DSi: NC (wifi: via 0 ohm MM3218.pin47)
               /FLASH_WP (R122) 28  27  SPI_CS2 (wifi FLASH memory)
                       SPI_SCK  30  29  BBP_SLEEP    to MM3218.pin42
                      SPI_MISO  32  31  RF_SLEEP     to MM3218.pin41
                      SPI_MOSI  34  33  RF_SCS       to MM3218.pin38
       to MM3218.pin15     CCA  36  35  BBP_SCS      to MM3218.pin37
                       WL_RXPE  38  37  BB_RF_SDO    to MM3218.pin36
       to MM3218.pin19  TRDATA  40  39  BB_RF_SDI    to MM3218.pin35
                           GND  42  41  BB_RF_SCLK   to MM3218.pin34
       to MM3218.pin21   TRCLK  44  43  NC(VDD18_TP) to MM3218.pin28 (0ohm+cap)
       to MM3218.pin18   TRRDY  46  45  GND
                       WL_TXPE  48  47  MCLK    to MM3218.pin23 (via XX & CLxx)
                         RESET  50  49  GND
```

- [https://fccid.io/EW4DWMW024/Label/Label-format-and-location-1137926.pdf](https://fccid.io/EW4DWMW024/Label/Label-format-and-location-1137926.pdf)
- [https://fccid.io/EW4DWMW015/Label/Label-Location-1031953.pdf](https://fccid.io/EW4DWMW015/Label/Label-Location-1031953.pdf)
Pin 25 and 43 are VDD test points on DWM-W015 wifiboard (but are NC on DSi
mainboard & DWM-W024 wifiboard).



## P7 - 47pin - To UPPER lcd screen (video+backlight+speakers) (on PCB backside)


```
                          BLA2   1  2   BLC2    ;-backlight
                          SPLN   3  4   SPLN    ;\left speaker
                          SPLP   5  6   SPLP    ;/
                          SPRN   7  8   SPRN    ;\right speaker
                          SPRP   9  10  SPRP    ;/
                         VDD-5  11  12  VDD10
                          VDD5  13  14  GND
                          VSHD  15  16  VSHD
                           INI  17  18  GSP
                           GCK  19  20  LDB20
                         LDB21  21  22  LDB22
                         LDB23  23  24  LDB24
                         LDB25  25  26  LDG20
                         LDG21  27  28  LDG22
                           GND  29  30  LDG23
                         LDG24  31  32  LDG25
                         LDR20  33  34  LDR21
                         LDR22  35  36  LDR23
                         LDR24  37  38  LDR25
                           GND  39  40  DCLK
                           SPL  41  42  LS
                           GND  43  44  via C79 to COM2
                           REV  45  46  GND
                          COM2  47
```



## P8 - 37pin - To LOWER lcd screen (video signals)


```
                         VDD-5   1  2   VDD10
                          VDD5   3  4   GND
                          VSHD   5  6   VSHD
                           INI   7  8   GSP
                           GCK   9  10  LDB10
                         LDB11  11  12  LDB12
                         LDB13  13  14  LDB14
                         LDB15  15  16  LDG10
                         LDG11  17  18  LDG12
                           GND  19  20  LDG13
                         LDG14  21  22  LDG15
                         LDR10  23  24  LDR11
                         LDR12  25  26  LDR13
                         LDR14  27  28  LDR15
                           GND  29  30  DCLK
                           SPL  31  32  LS
                           GND  33  34  via C93 to COM1
                           REV  35  36  GND
                          COM1  37
```



## P9 - 25pin - To UPPER lcd screen (signals for both cameras, and camera led)


```
                           GND   1  2   CAM_LED
                         VDD42   3  4   GND
                  R100    RCLK   5  6   GND
                           GND   7  8   HSYNC
                         VSYNC   9  10  CAM_D5  RA7
                   RA7  CAM_D6  11  12  CAM_D4  RA7
                       CAM_RST  13  14  SCL
                           SDA  15  16  CAM_D7  RA7
                   RA6  CAM_D0  17  18  CAM_D3  RA6
                   RA6  CAM_D1  19  20  CAM_D2  RA6
                         VDD28  21  22  GND
                           CKI  23  24  GND
                         VDD18  25
```



## P10 - 4pin - To LOWER lcd screen (touchpad X-,Y-,X+,Y+)


```
  1 X-
  2 Y-
  3 X+
  4 Y+
```



## P11  2pin   External Power Supply input (4.6V DC IN)


```
  1 VIN  (+4.6V)
  2 VGND (GND)
  Shield (GND)
```



## P15 - 15pin - To battery/DPAD/PowerButton board (and onwards to 3xLEDs)


```
  dpad up button      P06  1  2  ORANGE (LED) Battery Charge
  dpad right button   P04  3  4  BLUE   (LED) Power On/Good
  dpad left button    P05  5  6  YELLOW (LED) Wifi
  dpad down button    P07  7  8  RED    (LED) Power On/Low
                      GND  9  10 VDD42 (to LEDs)
                      GND 11  12 TH on DPAD board (via R102 to TH on main)
  middle battery pin  DET 13  14 GND
  power button       PWSW 15
```

Note: On Daughterboard, pins are mirrored (eg. PWSW=Pin1 instead Pin15)



## P16 - 26pin - To bottom cover (SD Slot and L/R/VOL+/- buttons)


On original DSi:

```
                      GND  2  1  SD10_CLK                   ;\
               SD10_DATA0  4  3  SD10_VDD (aka VDD33)       ;
               SD10_DATA1  6  5  SD10_VDD (aka VDD33)       ; pin 1..18
                  SD10_WP  8  7  GND                        ; to RIGHT side:
                      GND 10  9  SD10_CMD                   ; R-button, and
  shoulder button R   P08 12  11 GND                        ; SD-card slot
                      GND 14  13 SD10_DATA3                 ;
                  SD10_CD 16  15 SD10_DATA2                 ;
                      GND 18  17 GND                        ;/
                      GND 20  19 GND                        ;\pin 19..20
  maybe display ;\   VDD5 22  21 VOLP  volume plus          ; to LEFT side:
  calibration?  ;    COM1 24  23 VOLN  volume minus         ; L-button, VOL +/-
  (at battery)  ;/   COM2 26  25 P09   shoulder button L    ;/and calibration
```

On DSi XL (changed pins, COM1/COM2 removed, and L/VOL+- moved to P24):

```
       GND  1  2  GND
       GND  3  4  GND
       GND  5  6  GND
  SD10_CLK  7  8  GND
     VDD33  9  10 SD10_D0
     VDD33 11  12 SD10_D1
       GND 13  14 SD10_WP
  SD10_CMD 15  16 VDD33
       GND 17  18 P08 (should button R)
   SD10_D3 19  20 VDD33
   SD10_D2 21  22 SD10_CD
       GND 23  24 GND
       GND 25  26 GND
```



## P17 - 2pin - Battery cable (lower-right) ;see text-layer (B)


```
  + BT+ (plus) (red wire)
  - BT- (GND)  (black wire)
```



## P18 - 4pin - To LOWER lcd screen (backlight cathode/anode)


```
  1 BLC1  ;\both same
  2 BLC1  ;/
  3 BLA1  ;\both same
  4 BLA1  ;/
```



## P19 - 1pin - Shielding-Plate for CPU (lower clip)



## P20 - 1pin - Shielding-Plate for CPU (upper clip)



## P21 - 1pin - Shielding-Plate for CPU (right clip)


```
  Shield GND
```



## P23 - 2pin - To Internal Microphone (via orange shielded wire)


```
  Pin  MIC (from P4.Pin7, disconnected when external microphone connected)
  Shield GND
```



## P24 - 14pin - DSi XL only (L/VOL+/- were formerly on P16 in old DSi)


```
               GND 1  2  GND
               N/C 3  4  VOLP
  P09 (shoulder-L) 5  6  VOLN
            GND 7-13  8-14 GND
```



## DPAD-BOARD - P2 - 6pin - To LEDs


```
  1  YELLOW  Wifi
  2  BLUE    Power On/Good
  3  ORANGE  Battery Charge
  4  GND   (for red+orange)
  5  RED     Power On/Low
  6  VDD42 (for yellow+blue)
```




# <a name="auxdsichipsetpinouts"></a>AUX DSi Chipset Pinouts



A photo of the DSi mainboard (with extra text layer on vias and solderpads) can
be found at:

- [http://problemkaputt.de/twl-core.jpg](http://problemkaputt.de/twl-core.jpg)



## DSi U1 - TWL-CPU (19x19 pin grid) (352 pins, aka 19x19 minus middle 3x3 pins)


```
      Wifi    MC2 maybe      MC1           SD/MMC  eMMC  SPI     RTC   IRQs
  .---.---.---------------.---------------.-------.---.-------.-------.---.---.
  |NC |CSB|NC  NC  NC  NC |D7  D3  IRQ CLK|D0  CLK|CLK|CS3 SCK|CS  SCK|R7 |NC |
  +---'   |               |               |       |   |       |   .---'   '---+
  |SDI CSR|NC  NC  NC  NC |D6  D2  DET CS |D1  CMD|D0 |CS2 MIS|SIO|PEN NC  WIF|
  |       |               |               |       |   |       +---+---.   .---+
  |SCK SDO|NC  NC  NC  NC |D5  D1  PWR CS2|D2  CD |D1 |CS1 MOS|R00 R01|RTC|P09|
  |       '---.       .---+           .---'   .---'   '---.---+       '---'   |
  |TCK TDA RDY|NC  NC |V33|D4  D0  RES|D3  WP |D3  D2  CMD| ? |P08 P07 P06 P05|
  |           '---+---'   '-----------'-------'-----------'---+               |
  |MCK CCA RXP TXP|GND V12 V33 GND V12 V33 G?  V12 V33 GND V33|P04 P03 P02 P01|
  | . .           |                                           +-----------.   |
  |DT3|SLR SLB ?  |GND V33 V12 GND GND GND V33 G?  GND V33 V12| ?  RES NC |P00|
  |   '. . . . . .|                                           |           '---+
  |CLK DT2 DT1 DT0|V33 GND V12 V33 GND V12 G?  V12 GND GND V33|PMO VC5 PMS X  |
  +-----------.   |                                           '-----------.   |
  |V33 NC  GND|CMD|V12 GND V33 GND V12 V33 GND V33 V12 V12 V33 GND GND GND|X  |
  |           '---'               .-----------.               .-----------'   |
  |V33 NC  V33 V33 GND V33 GND V33|-   -   -  |GND GND GND GND|HP# IRQ mut GND|
  +---.   .---.                   |           |               |               |
  |B15|V33|NC |V33 V12 GND V12 V12|-   -   -  |V12 V18 GND V12|NC  NC  NC  GP |
  |   '---'---'---.               |           |               +---------------+
  |B14 B13 B12 B11|V33 GND V33 GND|-   -   -  |V18 GND V18 GND|A1  D1  A0  D0 |
  |               |               '-----------'               |               |
  |B10 G15 G14 G13|GND V33 V12 GND V18 V12 V18 GND V18 V12 V18|A3  D3  A2  D2 |
  |               |                                           |               |
  |G12 G11 G10 R15|V33 V12 GND V12 GND V18 GND V12 GND V18 GND|A5  D5  A4  D4 |
  |               |                                           |               |
  |R14 R13 R12 R11|GND V33 V18 GND V18 V12 V18 GND V18 V12 V18|A7  D7  A6  D6 |
  +-----------.   |                                           |               |
  |DCK GSP SPL|R10|V33 V12 GND V18 GND V18 GND V12 GND V18 GND|A9  D9  A8  D8 |
  |       .---'---'-------.-------.---.---.-------.-----------'               |
  |LS  REV|B22 G24 G20 R22|D7  D3 |NC |RST|SCK WS |CE1 /OE A20 A11 D11 A10 D10|
  |   .---'               |       |   |   |       |                           |
  |GCK|B25 B21 G23 R25 R21|D6  D2 |NC |VSY|MCK SDO|NC  CE2 A19 A13 D13 A12 D12|
  |   |                   |       |   |   +---.   |                           |
  |INI|B24 B20 G22 R24 R20|D5  D1 |NC |HSY|SDA|SDI|/LB CLK A18 A21 A14 D15 D14|
  +---+               .---+       '---'   |   +---'                       .---+
  |NC |B23 G25 G21 R23|NC |D4  D0  CKI RCK|SCL|/UB ADV /WE A15 A17 A22 A16|NC |
  '---'---------------'---'---------------'---'---------------------------'---'
          LCD                  CAM         I2C SND          RAM               o
```



## DSi U2 - Main RAM (8Mx16) (Fujitsu MB82DBS08164, or NEC uPD46128512)


DSi mainboard solder pads (12x8 grid, within 14x10 grid):

```
       A    B    C    D    E    F    G    H    J    K    L    M    N    P
  10   -    -    -    -    -    -    -    -    -    -    -    -    -    -
  9    -    NC   NC   -    A15  A21  A22  A16  NC   VSS  -    NC   NC   -
  8    -    NC   NC   A11  A12  A13  A14  NC   DQ15 DQ7  DQ14 NC   NC   -
  7    -    -    -    A8   A19  A9   A10  DQ6  DQ13 DQ12 DQ5  -    -    -
  6    -    -    -    /WE  CE2  A20  -    -    DQ4  VDD  NC   -    -    -
  5    -    -    -    CLK  /ADV (W)  -    -    DQ3  VDD  DQ11 -    -    -
  4    -    -    -    /LB  /UB  A18  A17  DQ1  DQ9  DQ10 DQ2  -    -    -
  3    -    NC   -    A7   A6   A5   A4   VSS  /OE  DQ0  DQ8  NC   NC   -
  2    -    NC   NC   -    A3   A2   A1   A0   NC   /CE1 -    NC   NC   -
  1 o  -    -    -    -    -    -    -    -    -    -    -    -    -    -
```

DSi RAM - Fujitsu MB82DBS08164 (14x10 grid):

```
       A    B    C    D    E    F    G    H    J    K    L    M    N    P
  10   NC   NC   NC   NC   NC   NC   VDD  VSS  NC   NC   NC   NC   NC   NC
  9    NC   NC   NC   NC   A15  A21  A22  A16  NC   VSS  NC   NC   NC   NC
  8    -    -    NC   A11  A12  A13  A14  NC   DQ15 DQ7  DQ14 NC   -    -
  7    -    -    NC   A8   A19  A9   A10  DQ6  DQ13 DQ12 DQ5  NC   -    -
  6    -    -    NC   /WE  CE2  A20  NC   NC   DQ4  VDD  NC   NC   -    -
  5    -    -    NC   CLK  /ADV /WAI NC   VDD  DQ3  VDD  DQ11 VDD  -    -
  4    -    -    NC   /LB  /UB  A18  A17  DQ1  DQ9  DQ10 DQ2  VSS  -    -
  3    -    -    VSS  A7   A6   A5   A4   VSS  /OE  DQ0  DQ8  NC   -    -
  2    NC   NC   NC   NC   A3   A2   A1   A0   NC   /CE1 NC   NC   NC   NC
  1 o  NC   NC   -    NC   NC   NC   VDD  VSS  NC   NC   NC   NC   NC   NC
```

DSi RAM - NEC uPD46128512 (14x10 grid):

```
       A    B    C    D    E    F    G    H    J    K    L    M    N    P
  10   NC   NC   NC   -    -    -    NC   NC   -    -    -    NC   NC   NC
  9    -    NC   NC   -    A15  A21  A22  A16  NC   VSS  -    NC   NC   -
  8    -    -    NC   A11  A12  A13  A14  NC   DQ15 DQ7  DQ14 NC   -    -
  7    -    -    -    A8   A19  A9   A10  DQ6  DQ13 DQ12 DQ5  -    -    -
  6    -    -    NC   /WE  CE2  A20  NC   NC   DQ4  VCC  NC   NC   -    -
  5    -    -    NC   CLK  /ADV /WAI NC   NC   DQ3  VCC  DQ11 NC   -    -
  4    -    -    -    /LB  /UB  A18  A17  DQ1  DQ9  DQ10 DQ2  -    -    -
  3    -    -    NC   A7   A6   A5   A4   GND  /OE  DQ0  DQ8  NC   -    -
  2    -    NC   NC   NC   A3   A2   A1   A0   NC   /CE1 -    NC   NC   -
  1 o  NC   NC   NC   -    -    -    NC   NC   -    -    -    NC   NC   NC
```

The Fujitsu & NEC datasheets are specifing 14x10 grid (the chips are
essentially pin-compatible, except that: NEC has removed some NC pins, changed
some supply pins to NC, and (attempted to) rename VSS to GND).

However, the DSi mainboard has 12x8 grid solder pads (and the installed
NEC/Fujitsu chip/packages are actually only 12x8, too).

The DSi debug version is said to have 32Mbyte RAM, there is no provision for
that feature on DSi retail boards; dev boards are probably somewhere having an
extra address line (or an extra chip select for a 2nd RAM chip).



## DSi U3 - Power Managment (Texas Instruments 72071B0, or Mitsumi 3317A)


```
  1   GND (via CL9)
  2   ADPO
  3   EXTB+
  4   VDD33
  5   RESET         ;\
  6   SPI_SCK       ; main cpu bus
  7   SPI_MOSI      ; (reset and spi)
  8   SPI_MISO      ;
  9   SPI_CS1       ;/  <-- powerman (this does ALSO connect to U4)
  10  GND
  11  PMOFF
  12  PWSWO
  13  VCNT5
  14  PM_SLP
  ---
  15  B+
  16  VDD12 via L1
  17  VDD12
  18  GND
  19  BLC1          ;\
  20  BLA1 via U13  ; backlight 1+2
  21  BLA2 via U14  ; anode/cathode
  22  BLC2          ;/
  23  GND
  24  B+
  25  B+
  26  VDD18 via L2
  27  VDD18 via L2
  28  VDD18
  ---
  29  DET     ;\battery contacts
  30  BT+     ;/    ;\these are almost shortcut
  31  VDET-         ;/with each other (via 0 ohm R71)
  32  PVDD
  33  PWSW (when off: very few ohms to PVDD ???)
  34        ... via R104 (100K) to Q3 (B+ enable or so?)
  35  B+
  36        ... via to C18 to GND (seems to have no other connection)
  37  GND
  38  AOUT  ;\to U6
  39  GND   ;
  40  SCL1  ;   ;\secondary IC2 bus (to U6)
  41  SDA1  ;/  ;/
  42  VDD33
  ---
  43  GND via CL10
  44  VDD5 input (sense if VDD5/C16 has reached voltage)
  45  charge-pump for VDD5 (L7 and via DA3 to VDD5/C16)
  46  charge-pump for VDD5 (L5 and C14)
  47  VDD33 (via CL5)
  48  VDD33 (via L3)
  49  VDD33
  50  VDD33
  51  B+
  52  B+
  53  B+
  54  charge-pump for VDD42 (L7 and C23)
  55  charge-pump for VDD42 (L7 and via D3 to VDD42/C22)
  56  VDD42 input (sense if VDD42/C22 has reached voltage)
```



## DSi U4 - Sound and Touchscreen controller (AIC3000D)


AIC3000D pinout is same as in TSC2117 datasheet (aside from GPIx/GPIOx pins).

```
  Pin TSC2117 AIC3000D
  1   MISO    SPI_MISO
  2   MOSI    SPI_MOSI
  3   /SS     SPI_CS1 (powerman, this does ALSO connect to U3)
  4   SCLK    SPI_SCLK
  5   GPIO1   SPI_CS3 (touchscreen)
  6   GPIO2   PENIRQ
  7   IOVSS   GND
  8   IOVDD   VDD33
  9   DVDD    VDD18
  10  SDOUT   SND_SDI              ;\
  11  SDIN    SND_SDO              ;
  12  WCLK    SND_WS               ; serial sound input from main cpu
  ---                              ; (and serial output? microphone maybe?)
  13  BCLK    SND_SCLK             ;
  14  MCLK    SND_MCLK             ;/
  15  SDA       ... via R107 to VDD18  ;\unused I2C bus (?)
  16  SCL       ... via R106 to VDD18  ;/
  17  VOL/M   wiper (sound volume, from I2C potentiometer) "VOL/MICDET?"
  18  MICBIAS micbias (to U18)                  ;\
  19  MIC     LIN (to U18) (and to MIC via C31) ; microphone related
  20  AUX1    aux  ;\via 0ohm R108 to U18       ;
  21  AUX2    aux  ;/(that is almost GND)       ;/
  22  AVSS    GND
  23  AVDD    VDD33
  24  VBAT    GND
  ---
  25  VREF    VDD33
  26  TSVSS   GND
  27  YN      Y-                   ;\
  28  XN      X-                   ;
  29  DVSS    GND                  ; touchscreen input
  30  YP      Y+                   ;
  31  XP      X+                   ;/
  32  TSVDD   VDD33
  33  SPLN    SPLN                 ;\
  34  SLVSS   GND                  ;
  35  SLVDD   B+                   ; speaker output
  36  SPLP    SPLP                 ;
  ---                              ;
  37  SPRN    SPRN                 ;
  38  SRVDD   B+                   ;
  39  SRVSS   GND                  ;
  40  SPRP    SPRP                 ;/
  41  HPL     SL via CP2 and R88      ;\
  42  HVDD    VDD33                   ;
  43  HVSS    GND                     ; headphone output
  44  HPR     SR via CP3 and R89      ;
  45  GPI3    MUTE via Q4/Q5 to SR/SL ;
  46  GPI2    HP#SP switch            ;/
  47  GPI1    VCNT5
  48  /RESET  RESET
```



## DSi U5 - 256Mbyte eMMC NAND (14x14 grid)


```
       1    2    3    4    5    6    7    8    9    10   11   12   13   14
  o A  NC   NC   DAT0 DAT1 DAT2 NC   NC   NC   NC   NC   NC   NC   NC   NC
    B  NC   DAT3 DAT4 DAT5 DAT6 DAT7 NC   NC   NC   NC   NC   NC   NC   NC
    C  NC   VDDI NC   VSSQ NC   VCCQ NC   NC   NC   NC   NC   NC   NC   NC
    D  NC   NC   NC   NC   -    -    -    -    -    -    -    NC   NC   NC
    E  NC   NC   NC   -    NC   VCC  VSS  NC   NC   NC   -    NC   NC   NC
    F  NC   NC   NC   -    VCC  -    -    -    -    NC   -    NC   NC   NC
    G  NC   NC   NC   -    VSS  -    -    -    -    NC   -    NC   NC   NC
    H  NC   NC   NC   -    NC   -    -    -    -    VSS  -    NC   NC   NC
    J  NC   NC   NC   -    NC   -    -    -    -    VCC  -    NC   NC   NC
    K  NC   NC   NC   -    NC   NC   NC   VSS  VCC  NC   -    NC   NC   NC
    L  NC   NC   NC   -    -    -    -    -    -    -    -    NC   NC   NC
    M  NC   NC   NC   VCCQ CMD  CLK  NC   NC   NC   NC   NC   NC   NC   NC
    N  NC   VSSQ NC   VCCQ VSSQ NC   NC   NC   NC   NC   NC   NC   NC   NC
    P  NC   NC   VCCQ VSSQ VCCQ VSSQ NC   NC   NC   NC   NC   NC   NC   NC
```

Note: The pinout follows JEDEC's eMMC standard. The "NC" pins are GNDed in DSi,
the "DAT4..DAT7" pins are not connected in DSi. The "VDDI" pin isn't wired to
VDD, instead it goes to a capacitor (0.1uF min) "for internal power stability".



## DSi U6 - "BPTWL" - I2C bus(ses), LEDs, volume, power, wifi (6x6 grid)


```
       1           2           3            4           5           6
  o A  GND         WL_TXPE     P02 button   SDA'33      ADPO        GND
    B  ATH_TX_H    BLUE(LED)   RED(LED)     SCL'33      V33         GND
    C  YELLOW(LED) VOLP button VOLN button  PM_SLP      V33'        /WIFI_RST
    D  SDA1        RESET       SCL1         to C46      GND         PMOFF
    E  VDD28       GND         CAM_LED      PWSWO       mFE         /mRST
    F  GND         AOUT        mFE'(R79)    WL_RXPE     /IRQ_O      GND
```

Aka from 78K0/KB2 datasheet...

```
       1           2           3            4           5           6
  o A  NC          P32/INT3    P30/INT1     P61/SDA     P33/INT4/T  NC
    B  P31/INT2    P16/INT5/T  P15/TOH0     P60/SCL0    EVDD        EVSS
    C  P17/T'50    P14/RxD6    P13/TxD6     P00/TI000   VDD         P121/X1
    D  P11/SI10/Rx P12/SO10    P10/SCK10/Tx REGC        VSS      P122/X2/EXCLK
    E  AVREF       AVSS        P22/ANI2     P21/ANI1    FLMD0       RESET
    F  NC          P23/ANI3    P20/ANI0  P01/TI010/TO00 P120/INT0/EXLVI  NC
```



## DSi U7 - I2C bus potentiometer (ISL95810) (Device 50h)


```
  1  /WP (DSi: VDD33)  writeprotect
  2  SCL (DSi: SCL1)   i2c bus   ;\from U6
  3  SDA (DSi: SDA1)   i2c bus   ;/
  4  GND (DSi: GND)    ground
  5  RW  (DSi: wiper)  pot.wiper ;-to U4
  6  RL  (DSi: VDD18)  pot.L
  7  RH  (DSi: GND)    pot.H
  8  VCC (DSi: VDD33)  supply
```



## DSi U8 - bidirectional I2C voltage translator (PCA9306)


```
  1  GND   (DSi: GND)
  2  VREF1 (DSi: VDD18)
  3  SCL1  (DSi: SCL)     ;\to U1 (CPU)
  4  SDA1  (DSi: SDA)     ;/
  5  SDA2  (DSi: SDA'33)  ;\to U6 (BPTWL)
  6  SCL2  (DSi: SCL'33)  ;/
  7  VREF2 (DSi: VDD33)
  8  EN    (DSi: VDD33)
```



## DSi U9 - RTC - Seiko S-35199A01 (4x3 grid)


```
       A    B    C    D
  3    CS   /SCK VDD  F32K
  2    SIO  CTRL /INT FOUT
  1 o  VSS  XIN  XOUT VDDL
```



## DSi U10 - Magnet Sensor (for hinge, aka shell opened/closed)


```
  1  VDD33
  2  R7 (HINGE)  ;to U1
  3  GND
  4  GND
```



## DSi U11 - Charge


```
  1  EXTB+
  2  Rosc
  3  ORANGE (via R2)              ;-charge LED
  4  GND
  5  TH' (via R76 to TH)          ;\thermal sensor
  6  TH  (via R102 to DPAD board) ;/for battery?
  7  B+
  8  RICHG
  9  BT+
  10 BT+
```

Note: Seems to resemble Mitsumi MM3358 datasheet.



## DSi U12 - 5pin, VDD33 to VDD28 converter? (near upper screen socket)


```
  1  VDD28
  2  GND
  3  VDD33
  4  VDD33
  5  (no pin)
  6  ... to DA6/DA7
```



## DSi U13 - 5pin, Backlight 1 (Lower Screen), near power managment chip



## DSi U14 - 5pin, Backlight 2 (Upper Screen), near power managment chip


```
  1  from U3 (wanted backlight pulse/level or so?)
  2  GND
  3  B+
  4  BLA1/BLA2 backlight anode output
  5  (no pin)
  6  B+
```



## DSi U15 - 4pin, near external power input


```
  1  via R4 to C100,Q1 (controls Q1 for forwarding VIN to EXTB+)
  2  VIN'' (from VIN via EM1 and F1)
  3  ?
  4  via C132,R14 to GND, and via D1 to VIN''
```

VIN power-input related.



## DSi U16 - N/A


None such.



## DSi U17 - Headphone Mute logic, near headphone socket


```
  1  mute (from main CPU GPIO)
  2  GND
  3  PMOFF
  4  MUTE (to Q4/Q5/TSC)
  5  VDD33
  6  ?
```



## DSi U18 - Microphone Bias, near headphone socket (not always installed)


```
  1  NC
  2  GND
  3  LIN (via C31/EM5 to MIC)
  4  aux (via 0 ohm to TSC AUX1/AUX2)
  5  VDD33
  6  micbias (from TSC micbias)
```



## DSi U19 - 5pin, something near dpad socket


```
  1  via D5/D6 from B+/VDD42
  2  GND
  3  VCNT5
  4  ?
  5  (no pin)
  6  VSHD
```

Note: Pin-numbering is unclear, here is no Pin 1 marker.



## DSi U20 - Whatever, DSi XL only (maybe magnet Sensor instead of U10?)


```
  1  ?
  2  ?
  3  ?
```



## DSi LEDs:


```
  CAM_LED (via R68 and Q13) ;Camera LED    ;\
  BLUE    (via R21 and Q11) ;Power On/Good ; from U6
  YELLOW  (via R22 and Q12) ;Wifi LED      ;/
  RED     (via R20)         ;Power On/Low  ;-from U6 (or to U6 ?)
  ORANGE  (via R2)          ;Charge LED    ;-from U11
```



## MM3218 chip (same chip as in DS Lite) (on DSi Wifi Daughterboard)


```
  1  VDD18
  2  GND
  3  VDD18
  4  Antenna signal
  5  Antenna shield
  6  VDD18
  7  VDD18
  8  GND
  9  NC
  10 GND
  11 GND
  12 GND
  13 NC
  14 /RESET
  ---
  15 CCA    ... to DSi mainboard connector pin 36
  16 WL_TXPE
  17 WL_RXPE
  18 TRRDY  ... to DSi mainboard connector pin 46 !!!
  19 TRDATA ... to DSi mainboard connector pin 40
  20 VDD33
  21 TRCLK  ... to DSi mainboard connector pin 44
  22 GND
  23 MCLK   ... via nearby big component ... to DSi mainboard connector pin 47
  24 VDD18
  25 NC
  26 22MHz
  27 22MHz'
  28 ... to DSi mainboard connector pin 43 NC? (with cap to GND and via 0 ohm)
  ---
  29 VDD33
  30 via capacitor to VDD33
  31 via 1K2 + 120K to GND (aka via 121.2K to GND)
  32 VDD18
  33 VDD18
  34 BB_RF_SCLK   ... to DSi mainboard connector pin 41
  35 BB_RF_SDI    ... to DSi mainboard connector pin 39
  36 BB_RF_SDO    ... to DSi mainboard connector pin 37
  37 BBP_SCS      ... to DSi mainboard connector pin 35
  38 RF_SCS       ... to DSi mainboard connector pin 33
  39 GND
  40 VDD33
  41 RF_SLEEP     ... to DSi mainboard connector pin 31
  42 BBP_SLEEP    ... to DSi mainboard connector pin 29
  ---
  43 VDD18
  44 ... shortcut to MM3218.pin50, and via resistor to MM3218.pin46
  45 VDD33
  46 ... via resistor to MM3218.pin44+50
  47 ... to DSi mainboard connector pin 25 (via 0 ohm) (+cap) (NC in DSi)
  48 VDD33
  49 GND
  50 ... shortcut to MM3218.pin44, and via resistor to MM3218.pin46
  51 ... via resistor to GND
  52 VDD18
  53 NC
  54 NC
  55 NC
  56 NC
```



## ROCm Atheros AR6002G-AC1B chip (on DSi DWM-W015 Wifi Daughterboard)


```
   1     2     3     4     5     6     7    8    9     10    11    12    13
 A AGND  RF2   RF2   RF2   RF2   PDET  NC   NC   VDD18 VDD12 XTAL  XTAL  BT_CLK
         OUTN  OUTP  INP   INN                   BIAS  XTAL  I     O     OUT
 B RF5   AGND  VDD18 VDD12 VDD12 BIAS  NC   NC   VDD12 VDD12 VDD18 BT_   DVDD12
   INP         FE    LNA   BIAS  REF             D_SYN BB    XTAL  CLKEN
 C RF5   VDD12 -     -     AGND  AGND  AGND AGND AGND  -     -     GPIO  GPIO
   INN   FE                                                        17    16
 D PA5   NC    -     -     -     -     -    -    -     -     -     GPIO  GPIO
   BIAS                                                            14    15
 E RF5   VDD18 AGND  -     AGND  AGND  AGND AGND DVSS  -     -     DVDD  DVDD
   OUT   VCO                                                       GPIO1 GPIO0
 F VDD12 VDD12 AGND  -     AGND  AGND  AGND AGND DVSS  -     DVSS  GPIO  GPIO
   TX5   SYNTH                                                     12    13
 G XPA   XPA   AGND  -     AGND  AGND  AGND AGND DVSS  -     DVSS  GPIO  GPIIO
   BIAS2 BIAS5                                                     10    11
 H VCCFE LDO_  AGND  -     AGND  AGND  AGND AGND DVSS  -     DVSS  GPIO  DVDD
   M     OUT                                                       9     12
 J ANTA  VDDIO AGND  -     DVSS  DVSS  DVSS DVSS DVSS  -     DVSS  CLK_  DVDD
         ANT                                                       REQ   12
 K ANTC  ANTB  -     -     -     -     -    -    -     -     -     SYS_  CHIP_
                                                                   RST_L PWD_L
 L ANTD  ANTE  -     -     DVSS  DVSS  DVSS  DVSS DVSS -     -     DVDD  DVDD_
                                                                   12    SDIO
 M AGND  GPIO0 GPIO2 DVDD  GPIO4 GPIO6 GPIO8 JTAG JTAG JTAG  SDIO_ SDIO_ SDIO_
                     12                      TMS  TCK  TDO   DATA3 DATA2 CLK
 N DVDD  GPIO1 GPIO3 DVDD_ GPIO5 GPIO7 DVDD_ DVDD JTAG DVDD_ SDIO_ SDIO_ SDIO_
   12                BT                SDIO  12   TDI  SDIO  CMD   DATA1 DATA0
```



## Atheros AR6013 chip (on DSi DWM-W024 Wifi Daughterboard)


```
  1  1.2V
  2  VDD18
  3  NC
  4  NC
  5  VDD18
  6  NC
  7  1.2V
  8  VDD18
  9  1.2V
  10 NC
  11 NC (except, wired to testpoint)
  12 VDD33
  13 via 0 ohm to ATH_TX_H  ;<--with 0 ohm connection
  14 via (N/A) to ATH_TX_H  ;<--connection not installed
  15 to a dead-end-via
  16 to a dead-end-via
  17 1.2V
  18 P5.pin24       JTAG_TRST_L     (plus testpoint)
  19 NC (except, wired to testpoint)
  ---
  20 VDD18
  21 1.2V
  22 I2C.SCL
  23 I2C.SDA
  24 P5.pin21 RTC 32KHZ via 0 ohm
  25 /WIFI_RST
  26 ATH_TX_H
  27 SDKI.CMD
  28 SDIO.CLK
  29 VDD33
  30 SDIO.DAT0
  31 SDIO.DAT2
  32 SDIO.DAT1
  33 SDIO.DAT3
  34 P5.pin22       JTAG_TCK
  35 P5.pin20       JTAG_TDI
  36 P5.pin17       JTAG_TMS
  37 1.2V
  38 P5.pin15       JTAG_TDO
  ---
  39 P5.pin36       CCA
  40 VDD33
  41 1.2V
  42 P5.pin46       TRRDY
  43 P5.pin44       TRCLK
  44 P5.pin40       TRDATA
  45 WL_TXPE
  46 WL_RXPE
  47 P5.pin47       MCLK
  48 P5.pin33       RF_SCS      ... and 6.9ohm to P5.47 ?
  49 P5.pin35       BBP_SCS
  50 P5.pin39       BB_RF_SDI
  51 VDD33
  52 P5.pin37       BB_RF_SDO
  53 P5.pin41       BB_RF_SCLK
  54 P5.pin29       BBP_SLEEP
  55 P5.pin31       RF_SLEEP
  56 P5.pin26       SEL_ATH_L   ... IRQ?
  57 1.2V
  ---
  58 VDD18
  59 XTALx
  60 XTALx
  61 1.2V
  62 1.2V
  63 VDD18
  64 NC (except, wired to testpoint)
  65 NC (except, wired to testpoint)
  66 NC (except, wired to testpoint)
  67 NC (except, wired to testpoint)
  68 NC
  69 via 6.1K to GND
  70 1.2V
  71 NC
  72 NC
  73 VDD18
  74 RF2.OUTx
  75 RF2.OUTx
  76 VDD18
  ---
  GND center plates
```

Note: /RESET connects to Wifi FLASH only, ie. NOT to the MM3218 clone within
AR6013G (that's unlike as real MM3218).



## Atheros AR6014 chip (on 3DS DWM-W028 Wifi Daughterboard) (and J27H023?)


Pinouts unknown.


There is a 3rd part number, J27H020, made by hon hai (Foxconn) instead of
Mitsumi.




# <a name="pinoutscpusignalsummary"></a>Pinouts - CPU - Signal Summary




## Advance Gameboy CPU Signal Summary


Cart Bus: D0-D7, A0-A15, /CS, /RD, /WR (different usage in GBA/DMG mode)

WRAM Bus: WA0-WA16, WD0-WD15, /WLB, /WUB, /WWE, /WOE (used in GBA mode only)

LCD Bus : LDR1-5, LDG1-5, LDB1-5, DCK, LP, PS, SPL, CLS, SPS, MOD, REVC

Joypad: TP0-3 (Buttons), TP4-7 (Directions), TP8-9 (L/R-Buttons, via R43/R44)

Serial Link: SC, SD (aka P14?), SI, SO - Audio: SO1-2, Vin

Other: CK1-2, PHI, IN35, VCNT5, /FIQ (via CL1 to VDD3), /RESET (IN), /RES (OUT)

Supply: VDD35, VDD3, VDD2, GND (some are probably undoc inputs)

GBA SP: Same as GBA, plus VDD1, plus duplicated supply pins, plus pin 152.




# <a name="pinoutscpupinouts"></a>Pinouts - CPU - Pinouts




## Advance Gameboy CPU Pinouts (CPU AGB)


```
  1 VDD3  17 D0   33 A0    49 WA4   65 VDD2  81 WD9   97 LDB5   113 CK1
  2 IN35  18 A15  34 /CS   50 WA5   66 WD5   82 WD1   98 LDB4   114 CK2
  3 TP8   19 A14  35 /RD   51 WA6   67 WD13  83 /WOE  99 LDB3   115 VDD2
  4 TP0   20 A13  36 /WR   52 WA7   68 WD6   84 DCK   100 LDB2  116 GND
  5 TP1   21 A12  37 PHI   53 /WLB  69 WD14  85 LP    101 LDB1  117 VDD2
  6 SO1   22 A11  38 VDD35 54 /WUB  70 WD7   86 PS    102 GND   118 VCNT5
  7 SO2   23 A10  39 GND   55 /WWE  71 WD15  87 LDR5  103 VDD3  119 TP9
  8 Vin   24 A9   40 SC    56 WA8   72 WD8   88 LDR4  104 SPL   120 TP6
  9 /RES  25 A8   41 SD    57 WA9   73 WD16  89 LDR3  105 CLS   121 TP5
  10 D7   26 A7   42 SI    58 WA10  74 WA16  90 LDR2  106 SPS   122 TP7
  11 D6   27 A6   43 SO    59 WA11  75 WD12  91 LDR1  107 MOD   123 TP4
  12 D5   28 A5   44 VDD2  60 WA12  76 WD4   92 LDG5  108 REVC  124 /FIQ
  13 D4   29 A4   45 WA0   61 WA13  77 WD11  93 LDG4  109 GNDed 125 /RESET
  14 D3   30 A3   46 WA1   62 WA14  78 WD3   94 LDG3  110 GNDed 126 TP2
  15 D2   31 A2   47 WA2   63 WA15  79 WD10  95 LDG2  111 GNDed 127 TP3
  16 D1   32 A1   48 WA3   64 GND   80 WD2   96 LDG1  112 GNDed 128 GND
```



## GBA SP CPU Pinouts (CPU AGB B)


```
  1 IN35   21 D0    41 A0    61 WA4   81 WD13  101 GND   121 LDB4  141 GND
  2 TP8    22 A15   42 /CS   62 WA5   82 WD6   102 VDD1  122 LDB3  142 VDD3
  3 TP0    23 A14   43 /RD   63 WA6   83 WD14  103 GND   123 LDB2  143 GND
  4 TP1    24 A13   44 /WR   64 WA7   84 WD7   104 VDD3  124 LDB1  144 VCNT5
  5 SO1    25 A12   45 PHI   65 /WLB  85 WD15  105 DCK   125 GND   145 TP9
  6 SO2    26 A11   46 VDD35 66 /WUB  86 WD8   106 LP    126 VDD3  146 TP6
  7 Vin    27 GND   47 GND   67 GND   87 WD16  107 PS    127 SPL   147 TP5
  8 VDD1   28 VDD35 48 SC    68 VDD2  88 WA16  108 LDR5  128 CLS   148 TP7
  9 GND    29 A10   49 SD    69 /WWE  89 VDD2  109 LDR4  129 SPS   149 TP4
  10 VDD35 30 A9    50 SI    70 WA8   90 GND   110 LDR3  130 MOD   150 /FIQ
  11 /RES  31 A8    51 SO    71 WA9   91 WD12  111 LDR2  131 REVC  151 /RESET
  12 D7    32 A7    52 VDD35 72 WA10  92 WD4   112 LDR1  132 GND   152 ?
  13 D6    33 A6    53 GND   73 WA11  93 WD11  113 LDG5  133 GND   153 TP3
  14 D5    34 A5    54 VDD1  74 WA12  94 WD3   114 LDG4  134 GND   154 TP2
  15 D4    35 A4    55 GND   75 WA13  95 WD10  115 LDG3  135 GND   155 VDD3
  16 D3    36 GND   56 VDD2  76 WA14  96 WD2   116 LDG2  136 VDD1  156 GND
  17 D2    37 VDD35 57 WA0   77 WA15  97 WD9   117 LDG1  137 GND
  18 GND   38 A3    58 WA1   78 GND   98 WD1   118 GND   138 CK1
  19 VDD35 39 A2    59 WA2   79 VDD2  99 /WOE  119 VDD3  139 CK2
  20 D1    40 A1    60 WA3   80 WD5   100 VDD2 120 LDB5  140 VDD2
```

Pin 152 seems to be not connected on the mainboard, maybe an undoc output.



## GBA-Micro, NDS, NDS-Lite Pinouts


Unknown. The CPU BGA Pins are hidden underneath of the CPU. And, in NDS and
NDS-Lite, the CPU itself hides underneath of the DS Cartridge Slot.




# <a name="pinoutsaudioamplifiers"></a>Pinouts - Audio Amplifiers




## Advance Gameboy Audio Amplifier (AMP AGB IR3R60N) (U6)


```
  1   2   3   4   5   6   7   8   9   10  11  12   13   14   15   16   17  18
  C38 FR1 FR2 FL1 FL2 GND RIN LIN C39 VOL SW  VDD5 LOUT VCC3 ROUT VCC3 SP  GND
```

SW=Headphone Switch (grounded when none connected).



## GBA SP Audio Amplifier (uses AMB AGB IR3R60N, too) (U3)


Same connection as in GBA, except that pin14/16 connect to VR21 (instead VCC3),
and pin1/9 connect to different capacitors.



## NDS - National Semiconductor LM4880M Dual 250mW Audio Power Amplifier (U12)


```
  1-OUT A  2-IN A  3-BYPASS  4-GND  5-SHUTDOWN  6-IN B  7-OUT A  8-VDD.VQ5
```

NDS-Lite: No external amplifier (Mitsumi 3205B Powermanagment Device contains
internal amplifier).




# <a name="pinoutslcdcables"></a>Pinouts - LCD Cables




## Advance Gameboy Display Socket


```
  1 ?     6 GND    11 LDR2   16 LDG2   21 LDB3   26 SPS     31 P2-VSS  36 V4
  2 VSHD  7 VSHD   12 LDR1   17 LDG1   22 LDB2   27 ?       32 P2-VCC  37 V3
  3 DCK   8 LDR5   13 LDG5   18 GND    23 LDB1   28 MOD     33 ?       38 V2
  4 LP    9 LDR4   14 LDG4   19 LDB5   24 SPL    29 VCOM    34 VDD5    39 V1
  5 PS    10 LDR3  15 LDG3   20 LDB4   25 CLS    30 P2-VEE  35 GND     40 V0
```



## GBA SP Display Socket


```
  1 VSHD 5 VSHD  9 LDR3   13 LDG4  17 GND   21 LDB2  25 SPS   29 P2VSS 33 U83
  2 DCK  6 GND   10 LDR2  14 LDG3  18 LDB5  22 LDB1  26 MOD   30 COM   34 VDD5
  3 LP   7 LDR5  11 LDR1  15 LDG2  19 LDB4  23 SPL   27 REVC  31 VDD5
  4 PS   8 LDR4  12 LDG5  16 LDG1  20 LDB3  24 CLS   28 P2VDD 32 GND
```



## GBA Micro Display Sockets


```
  __GBA Mirco display socket (P1)____________________________________
  1-PS    6-5bit  11-MD    16-5bit  21-5bit  26-CL             31-GND
  2-RV    7-5bit  12-SL    17-5bit  22-5bit  27-SS             32-GND
  3-GND   8-5bit  13-CK    18-5bit  23-5bit  28-via C5 to VR1  33-V10
  4-5bit  9-LP    14-GND   19-5bit  24-5bit  29-V5             34-V-5
  5-5bit  10-VD   15-5bit  20-GND   25-5bit  30-to VR1
  __GBA Mirco backlight socket (P3)__________________________________
  1-LC  2-LC  3-LA  4-LA
```



## NDS Upper/Lower Display Sockets


```
  ___NDS upper screen/upper backlight/speakers socket (P3)_____________________
  1-SPLO 7-PS2   13-LDR2  19-GND   25-LDG2  31-LDB2  37-MOD2  43-VDD15  49-SPRO
  2-SPLO 8-REV2  14-LDR1  20-DCLK2 26-LDG1  32-LDB1  38-GND   44-VDD-5  50-GND
  3-SSC2 9-GND   15-LDR0  21-GND   27-LDG0  33-LDB0  39-VDD5  45-VDD-10 51-GND
  4-ASC2 10-LDR5 16-LS2   22-LDG5  28-LDB5  34-GCK2  40-VDD10 46-LEDC2
  5-GND  11-LDR4 17-VSHD  23-LDG4  29-LDB4  35-GSP2  41-COM2  47-LEDA2
  6-SPL2 12-LDR3 18-DISP1 24-LDG3  30-LDB3  36-GND   42-GND   48-SPRO
  ___NDS lower screen socket (P4)______________________________________________
  1-SSC1 6-REV1  11-LDR2  16-DISP0 21-LDG4  26-LDB5  31-LDB0  36-GND  41-VDD15
  2-ASC1 7-GND   12-LDR1  17-SPL1  22-LDG3  27-LDB4  32-GCK1  37-?    42-VDD10
  3-GND  8-LDR5  13-LDR0  18-DCLK1 23-LDG2  28-LDB3  33-GSP1  38-VDD5 43-GND
  4-?    9-LDR4  14-LS1   19-GND   24-LDG1  29-LDB2  34-VSHD  39-COM1 44-VDD-5
  5-PS1  10-LDR3 15-VSHD  20-LDG5  25-LDG0  30-LDB1  35-MOD1  40-GND  45-VDD-10
  ___NDS lower backlight socket (P5)____   ___NDS touchscreen socket (P6)______
  1:LEDA1  2:LEDA1  3:LEDC1  4:LEDC1       1:Y-    2:X-    3:Y+    4:X+
```



## NDS-Lite Upper/Lower Display Sockets


```
  ___NDS-Lite upper screen/upper backlight/speakers socket (P3)________________
  1-VDD-5 6-MOD    11-LD2xx 16-LD2xx 21-LD2xx 26-LD2xx 31-LS   36-GND   41-SPRO
  2-VDD10 7-GSP    12-LD2xx 17-LD2xx 22-LD2xx 27-LD2xx 32-VSHD 37-COM2  42-SG
  3-VDD5  8-GCK    13-LD2xx 18-GND   23-LD2xx 28-GND   33-GND  38-LEDA2 43-SG
  4-GND   9-LD2xx  14-LD2xx 19-LD2xx 24-LD2xx 29-DCLK  34-xx2? 39-LEDC2 44-SPLO
  5-VSHD  10-LD2xx 15-LD2xx 20-LD2xx 25-LD2xx 30-SPL   35-REV  40-SPRO  45-SPLO
  ___NDS-Lite lower screen/lower backlight (P4)________________________________
  1-VDD-5 6-MOD    11-LD1xx 16-LD1xx 21-LD1xx 26-LD1xx 31-LS   36-GND
  2-VDD10 7-GSP    12-LD1xx 17-LD1xx 22-LD1xx 27-LD1xx 32-VSHD 37-COM1
  3-VDD5  8-GCK    13-LD1xx 18-GND   23-LD1xx 28-GND   33-GND  38-LEDA1
  4-GND   9-LD1xx  14-LD1xx 19-LD1xx 24-LD1xx 29-DCLK  34-xx1? 39-LEDC1
  5-VSHD  10-LD1xx 15-LD1xx 20-LD1xx 25-LD1xx 30-SPL   35-REV
  ___NDS-Lite touchscreen socket (P6)______   ___NDS-Lite white coax (P12)_____
  1:X-  2:Y-  3:X+  4:Y+                      Center:MICIN  Shield:GND
```




# <a name="pinoutspowerswitchesdcdcconvertersresetgenerators"></a>Pinouts - Power Switches, DC/DC Converters, Reset Generators




## Advance Gameboy Power Switch (2-position slider, with two common pins)



## GBA SP Power Switch (same as GBA)


```
  1 via resistor to GND (OFF)
  2 VS (BT+) (ON)
  C VCC (to board)
```



## GBA Micro Power Switch


Same as GBA and GBA SP, but Pin 1 and 2 exchanged.



## Advance Gameboy Cartridge Slot Switch (integrated 4pin micro switch)



## GBA SP Cartridge Slot Switch (separate 4pin micro switch)


```
  C1 VDD35  (to S2 when PRESSED, to S1 when RELEASED)
  S1 VDD3   (to C2 when PRESSED, to C1 when RELEASED)
  C2 IN35   (to S1 when PRESSED)
  S2 VDD5   (to C1 when PRESSED)
```

Pressed=8bit DMG/MGB/CGB cart, Released=32bit GBA cart (or no cart inserted)

GBA: switch integrated in cart socket, GBA-SP: separate switch next to socket.



## Advance Gameboy Power Controller (M 121 514X) (U4)


```
  1-VIN    2-VOUT5  3-CSS5    4-VDRV5  5-GND    6-VDRV3    7-CSS3    8-VOUT3
  9-VCNT5  10-CSCP  11-REGEXT 12-VDD3  13-VDD2  14-/RESET  15-LOWBAT 16-VDD13
```

/RESET is passed to the CPU, and then forwarded to /RES pin on cart slot.



## Advance Gameboy LCD Regulator (AGB-REG IR3E09N) (U3)


```
  1  2  3    4      5   6   7   8   9   10   11  12  13  14  15  16   17     18
  ?  ?  REVC U3-COM V0  V1  ?   ?   ?   GND  ?   V2  ?   V3  V4  VDD5 U3-VDD ?
```



## GBA SP Power Controller 1 (S6403 AU227 9276) (U4)


```
  1-VCC    2-SCP1   3-SCP2 4-VDRV3 5-VOUT3/VDD3 6-VDD2  7-VOUT1/VDD1  8-VDRV1
  9-LOWBAT 10-VCNT5 11-LS5 12-?    13-GND       14-?    15-VOUT5/VDD5 16-VDRV5
```



## GBA SP Power Controller 2 (2253B 2808) (U5)


```
  1-TIN     2-U5C3     3-ADJ  4-U5VDD  5-VIN   6-?    7-U57  8-?
  9-to-C29  10-to-C30  11-?   12-GND   13-VS   14-S-  15-S+  16-U5OUT
```



## GBA Micro - Power Managment Device (U2)


- 1 via C43 to GND
- 2 via R24 to C34 to R25 back to U2.2
- 3 via C35 to GND
- 4 via C36 to GND
- 5
- 6 audio.in ? (see BP)
- 7   via C48 to GND
- 8   via R21 to C46 to C47 to C38 to R23 to phones
- 9  VL (to U4)
- 10  via R27 to C33 to C44 to C49 to R22 to phones
- 11  via C45 to GND
- 12 audio.in ? (see BP)
- 13  via C41 to GND
- 14 phones (switch)
- 15 phones (tip via R22)
- 16 phones (mid via R23)
- 17 VCS
- 18 SP
- 19 GND
- 20 LB
- 21 via C52 to GND
- 22 via C53 to GND
- 23 RS         (looks like RESET output)
- 24 to R37/C56 (looks like RESET input)
- 25
- 26
- 27 via C54 to V3
- 28 V3
- 29 GND
- 30 V3
- 31 VC
- 32 to C58
- 33 to R41/C58
- 34 GND
- 35
- 36 VC
- 37 VC
- 38
- 39 V5
- 40 GND
- 41 GND
- 42
- 43
- 44
- 45 B+
- 46 S-
- 47 S+
- 48



## GBA Micro - Volume/Backlight Level Up/Down Controller (U5)


```
  1-     5-GND  9-      13-XD      17-     21-     25-    29-
  2-     6-GND  10-     14-to U4.7 18-XR   22-CN   26-    30-
  3-     7-     11-XC   15-        19-V+   23-CNS  27-    31-BP
  4-LN   8-     12-GND  16-        20-V-   24-     28-V3  32-
```



## NDS Powermanagment Device (Mitsumi 3152A) (U3)


```
  1 R50-EXTB+        17               33 LEDC1           49 VCNT5
  2 R39-ORANGE       18               34 GND             50
  3 GND              19 VQ5           35 LEDC2           51 RST
  4                  20               36                 52
  5 Rxx-Q4           21               37 U10-LEDA2       53
  6 INS+             22 GND           38                 54
  7 INS-             23 VQ5           39 MIC.C53-AIN     55 VQ5
  8                  24               40 MIC.TSC.AUX     56 R24-SR
  9 VDET             25 VDD3.3        41 GND             57
  10 PVDD            26 GND           42 R38-RED         58 R22-SL
  11                 27 CL60-VDD3.3   43 R37-GREEN       59 GND
  12 PWSW            28 VSHD          44 VDD3.3          60 VR3.PIN2
  13                 29               45 PWM.SPI.CLK     61
  14 GND             30 VDD5          46 PWM.SPI.D       62
  15 GND             31 U9-LEDA1      47 PWM.SPI.Q       63
  16 VQ5             32               48 PWM.SPI.SEL     64 GND
```



## NDS-LITE Powermanagment Device (Mitsumi 3205B) (U3)


```
  1 SW               17               33 LEDC1           49 VCNT5
  2 R50-EXTB+        18               34 GND             50
  3 R39-ORANGE       19 VQ5           35 LEDC2           51 RST
  4 GND              20               36                 52
  5                  21               37 U10-LEDA2       53
  6 R30-Q4           22 GND           38                 54
  7 INS+             23 VQ5           39 MIC.C53-AIN     55 CL63-VQ5
  8 INS-             24               40 MIC.TSC.AUX     56 R24-SR
  9 VDET             25 VDD3.3        41 GND             57 SPRO
  10 PVDD            26 GND           42 R38-RED         58 SPLO
  11                 27 CL60-VDD3.3   43 R37-GREEN       59 R22-SL
  12 PWSW            28 VSHD          44 VDD3.3          60 GND
  13 GND             29               45 PWM.SPI.CLK     61 R79-VR3.PIN2
  14 GND             30 VDD5          46 PWM.SPI.D       62
  15 GND             31 U9-LEDA1      47 PWM.SPI.Q       63
  16 VQ5             32               48 PWM.SPI.SEL     64
```



## NDS-LITE Power Switch


```
  1 PWSW (grounded when switch is pulled)
  2 GND
  3 GND
  4 NC? (grounded when switch is not pulled)
```




# <a name="pinoutswifi"></a>Pinouts - Wifi




## NDS RFU Daughter Board (Firmware FLASH, Wifi BB/RF Chips)


```
  1 N/A      6 FMW.CLK    11 ENABLE 16 RX.DTA? 21 BB./CS     26 22MHz   31 GND
  2 GND      7 FMW./SEL   12 GND    17 TX.MAIN 22 RF./CS     27 GND     32 GND
  3 high?    8 FMW.DTA.Q  13 GND    18 GND     23 BB.RF.CLK  28 VDD3.3  33 GND
  4 RXTX.ON  9 FMW.DTA.D  14 TX.ON  19 TX.CLK  24 BB.RF.RD   29 VDD1.8
  5 FMW./WP  10 FMW./RES  15 RX.ON  20 TX.DTA  25 BB.RF.WR   30 GND
```



## NDS-Lite RFU Daughter Board DWM-W006 (Firmware FLASH, Wifi BB/RF Chip)


```
  1 GND      6 GND       11 BB.RF.WR  16 VDD3.3   21 RF.SLEEP  26 FMW.Q
  2 TXPE     7 TRCLK     12 BB.RF.CLK 17 GND      22 FMW./RES  27 FMW./WP
  3 RXPE     8 TRDATA    13 GND       18 RF./CS   23 GND       28 FMW./CS
  4 CCA      9 GND       14 MCLK      19 BB.SLEEP 24 FMW.CLK   29 LD  ;hi?
  5 TRRDY    10 BB.RF.RD 15 GND       20 BB./CS   25 FMW.D     30 GND
```

- [https://fccid.io/EW4DWMW006/Label/ID-label-format-and-location-706511.pdf](https://fccid.io/EW4DWMW006/Label/ID-label-format-and-location-706511.pdf)


## Wifi RF Chip: RF9008, 0441, E0121Q (32 pin)


```
  1       5       9       13      17         21 RF.CLK  25        29
  2       6       10      14 GND  18         22         26        30
  3       7       11      15      19 RF.RD   23         27        31
  4       8       12      16      20 RF./CS  24         28        32
```

Pin19 RF.RD (oops, should be WR, maybe I've exchanged RD-WR?)

Pin20 RF./CS (via 10ohm)

Pin21 RF.CLK (via 10ohm)

Resembles RF2958 datasheet...?



## Wifi BB Chip: Mitsumi, Japan, 4418, MM3155 (48 pins)


```
  1 GND   7       13 GND     19      25       31         37 TX.MAIN 43
  2       8       14         20      26       32 BB./CS  38 RX.DTA? 44
  3       9       15 BB.CLK  21      27       33 TX.DTA  39 RX.ON   45 GND
  4       10      16 BB.WR   22      28 RST   34 RXTX.ON 40 TX.ON   46
  5       11      17 BB.RD   23      29       35 TX.CLK  41         47
  6       12      18 22MHz   24      30       36         42         48
```

Pin15 BB.CLK (via 10ohm to RFU.23)

Pin16 BB.WR (RFU.25)

Pin17 BB.RD (RFU.24)

Pin18 22MHz (via 50ohm)

Pin28 RST (same as FMW./RES)

Pin32 BB./CS (RFU.21)

The chip is roughly resembling HSP3824/HFA3824A/HFA3860A/HFA3860B (though
pinout and registers aren't compatible).



## NDS-LITE BB/RF-Chip Mitsumi MM3218 (56 pins)


```
  1-VDD18 8-GND   15-CCA    22-GND   29-VDD33     36-BBRF_SDO 43-VDD18 50-
  2-GND   9-NC    16-TXPE   23-MCLK  30-          37-BB_SCS   44-...   51-
  3-VDD18 10-GND  17-RXPE   24-VDD18 31-          38-RF_SCS   45-VDD33 52-VDD18
  4-Ant   11-GND  18-TRRDY  25-NC    32-VDD18     39-GND      46-...   53-NC
  5-Ant'  12-GND  19-TRDATA 26-22MHz 33-VDD18     40-VDD33    47-...   54-NC
  6-VDD18 13-NC   20-VDD33  27-22MHz 34-BBRF_SCLK 41-RF_SLEEP 48-VDD33 55-NC
  7-VDD18 14-/RES 21-TRCLK  28-      35-BBRF_SDI  42-BB_SLEEP 49-GND   56-NC
```

Note: Pinout should be same as in DSi (see DSi pinout for details).



## TX Signal/Timing Chart (Host Game)


```
  RX.DTA?  __________________________________________________________
  RXTX.ON  __-----------------------_________________________________
  RX.ON    __---_______-------------_________________________________
  TX.ON    _____-------______________________________________________
  TX.MAIN  ________----______________________________________________
  TX.CLK   _____#__####______________________________________________
  TX.DTA   _____#__####______________________________________________
```

This example shows a host sending beacons. The pre-beacon receive period is
probably to sense conflicts with other transmitters. The post-beacon receive
period is to get responses from other players. The two transmit parts are: The
hardware header, followed by inactivity on the tx pins during the rest of the
preamble period, then followed by the actual IEEE frame. The rest of the time
is spent in idle mode to reduce power consumption.



## RX Signal/Timing Chart (Join Game)


```
  RX.DTA?  __________________________________________________________
  RXTX.ON  -----------------------------------------------______-----
  RX.ON    -----------------------------------------------_________--
  TX.ON    __________________________________________________________
  TX.MAIN  __________________________________________________________
  TX.CLK   __________________________________________________________
  TX.DTA   _______________________________________________---________
```

This example shows a client trying to receive beacons, so most of the time is
spent in receive mode (the short idle periods are probably occuring when it is
switching to another channel). Once when it has associated with a host, the
client may spend more time in idle mode, and needs to be in receive mode only
when expecting to receive beacons or other data.




# <a name="pinoutsvarious"></a>Pinouts - Various




## Advance Gameboy 256Kbytes RAM 128Kx16bit (NEC D442012LGY-B85x-MJH) (wide)



## GBA SP 256Kbytes RAM 128Kx16bit (F 82D12160-10FN) (square)


```
  1 A15   7 A9    13 IC    19 A6   25 A0   31 D2   37 VCC  43 D15
  2 A14   8 A8    14 /UB   20 A5   26 /CE1 32 D10  38 D5   44 D8
  3 A13   9 NC    15 /LB   21 A4   27 GND  33 D3   39 D13  45 D16
  4 A12   10 NC   16 NC    22 A3   28 /OE  34 D11  40 D6   46 GND
  5 A11   11 /WE  17 NC    23 A2   29 D1   35 D4   41 D14  47 NC
  6 A10   12 CE2  18 A7    24 A1   30 D9   36 D12  42 D7   48 A16
```

Connection in GBA and GBA SP: IC-GND, /CE1-GND, CE2-VDD2, VCC-VDD2, Pin16-VDD2,
the other NC pins seem to be actually not connected, all other pins connect to
the corresponding Wxx CPU pins. Note: Both GBA and GBA SP have soldering points
for wide (12x18mm) and square (12x14mm) RAMs, so either could be used.

The GBA additionally contains 32K built-in WRAM, and built-in VRAM, so the
above 256K RAM chip is probably not used in 8bit classic/color gameboy mode.

Note: In the GBA Micro, the 256K RAM are contained on-chip in the CPU.



## Advance Gameboy Schematic Fragments


P2-VSS = VDD-15

VIN = VCC3 via R33

REGEXT (on my modified board, REGEXT underneath of my diodes)

/RES (OUT) (via R40)

/CS (via R39)

/WR (via R38)

SC (via Rxx)

SD (via Rxx)

SI (via Rxx)

SO (via Rxx)

DCK (via R36)

A-GND via CP4 (100uF) to GND (used speaker, and on headphone socket)



## GBA SP Schematic Fragments


P2VDD = VDD13

P2VSS = VDD15

/RES via R46

/CS via R45

/WR via R44

DCK via R20

VS=BT+

In my repaired GBA-SP: CK1 test-point is disconnected (instead GND'ed).

In my repaired GBA-SP: broken oscillator replaced

In my repaired GBA-SP: broken r1 1mOhm replaced (near oscillator)

In my repaired GBA-SP: broken EXT2 socket metal-spring/snapper removed

CL1 FIQ (near SW4)

CL2 ?

CL3 ?

CL4 VOUT1/VDD1 (near U4)

CL5 VOUT3/VDD3 (near U4)

CL6 VOUT5/VDD5 (near U4)

DL1-red (power low) ---R32--Q4--R6--

DL2-green (power good) ---Q6--LOWBAT/R34-VDD3

DL3-orange (charge) --R24--Q2--VIN/U57

P2VDD--VDD13

P2VSS--VDD15

S+ and S- are (almost) shortcut by R23 (1.0 ohm)

S+ via Q1 to VIN

VS via D1 to S-

A-GND via CP1 (100uF) to GND

U4 pin 12 to r6 (towards red led)

U4 pin 14 to D6---to U7

SC (CPU pin48) with R7 100K ohm pullup to VDD35

P35 via Q11 to SW (speaker disable)



## GBA SP Backlight-Button Schematic (U6,U8,Q12)


```
        ______                _____
  GND--|1 U8 6|-- U85        |     |--VDD5
  U82--|2    5|-- U85    U61-| Q12 |         U83  ------> to display
  U83--|3____4|-- U82        |_____|--Q12B   Q12B <------ from button
  U61--|1 U6 8|--VDD5    (X)---R51--VDD5    (X)---C70--GND
  U62--|2    7|--VDD5    U62---R49--VDD5    U61---R40--GND
  U62--|3    6|--(X)     Q12B--R39--VDD5    U82---R38--GND
  GND--|4____5|--NC?     Q12B--C69--VDD5    U85---R50--U62
```




# <a name="auxxboopctogbamultibootcable"></a>AUX Xboo PC-to-GBA Multiboot Cable



Below describes how to connect a PC parallel port to the GBA link port,
allowing to upload small programs (max 256 KBytes) from no$gba's Utility menu
into real GBAs.


This is possible because the GBA BIOS includes a built-in function for
downloading & executing program code even when no cartridge is inserted.
The program is loaded to 2000000h and up in GBA memory, and must contain
cartridge header information just as for normal ROM cartridges (nintendo logo,
checksum, etc., plus some additional multiboot info).



## Basic Cable Connection


The general connection is very simple (only needs four wires), the only problem
is that you need a special GBA plug or otherwise need to solder wires directly
to the GBA mainboard (see Examples below).

```
  GBA  Name  Color                 SUBD CNTR Name
  2    SO    Red     ------------- 10   10   /ACK
  3    SI    Orange  ------------- 14   14   /AUTOLF
  5    SC    Green   ------------- 1    1    /STROBE
  6    GND   Blue    ------------- 19   19   GND
```

Optionally, also connect the following signals (see notes below):

```
  4    SD    Brown   ------------- 17   36   /SELECT  (double speed burst)
  3    SI    Orange  ----[===]---- 2..9 2..9 D0..7    (pull-up, 560 Ohm)
  5    SC    Green   ----[===]---- 2..9 2..9 D0..7    (pull-up, 560 Ohm)
  4    SD    Brown   ----[===]---- 2..9 2..9 D0..7    (pull-up, 560 Ohm)
  START  (mainboard) -----|>|----- 16   31   /INIT    (auto-reset, 1N4148)
  SELECT (mainboard) -----|>|----- 16   31   /INIT    (auto-reset, 1N4148)
  RESET  (mainboard) -----||------ 16   31   /INIT    (auto-reset, 300nF)
```

Notes: The GBA Pins are arranged from left to right as 2,4,6 in upper row, and
1,3,5 in lower row; outside view of GBA socket; flat side of socket upside. The
above "Colors" are as used in most or all standard Nintendo link cables, note
that Red/Orange will be exchanged at one end in cables with crossed SO/SI
lines. At the PC side, use the SUBD pin numbers when connecting to a 25-pin
SUBD plug, or CNTR pin numbers for 36-pin Centronics plug.



## Optional SD Connection (Double Speed Burst)


The SD line is used for Double Speed Burst transfers only, in case that you are
using a gameboy link plug for the connection, and if that plug does not have a
SD-pin (such like from older 8bit gameboy cables), then you may leave out this
connection. Burst Boot will then only work half as fast though.



## Optional Pull-Ups (Improves Low-to-High Transition Speed)


If your parallel port works only with medium or slow delay settings, try to
connect 560 Ohm resistors to SI/SC/SD inputs each, and the other resistor pin
to any or all of the parallel port data lines (no$gba outputs high to pins
2..9).



## Optional Reset Connection (CAUTION: Connection changed September 2004)


The Reset connection allows to automatically reset & upload data even if a
program in the GBA has locked up (or if you've loaded a program that does not
support nocash burst boot), without having to reset the GBA manually by
switching it off and on (and without having to press Start+Select if a
cartridge is inserted).

The two diodes should be 1N4148 or similar, the capacitor should be 300nF (eg.
three 100nF capacitors in parallel). The signals are labeled on the mainboard,
and can be found at following names / CPU pin numbers: RESET/CPU.125,
SELECT/TP2/CPU.126, START/TP3/CPU.127.



## Optional Power Supply Connection


Also, you may want to connect the power supply to parallel port data lines, see
chapter Power Supply for details.



## Transmission Speed


The first transfer will be very slow, and the GBA BIOS will display the boot
logo for at least 4 seconds, even if the transfer has completed in less time.
Once when you have uploaded a program with burst boot backdoor, further
transfers will be ways faster. The table below shows transfer times for 0KByte
- 256KByte files:

```
  Boot Mode_____Delay 0_______Delay 1_______Delay 2_____
  Double Burst  0.1s - 1.8s   0.1s - 3.7s   0.1s - 5.3s
  Single Burst  0.1s - 3.6s   0.1s - 7.1s   0.1s - 10.6s
  Normal Bios   4.0s - 9.0s   4.0s - 12.7s  4.0s - 16.3s
```

All timings measured on a 66MHz computer, best possible transmission speed
should be 150KBytes/second. Timings might slightly vary depending on the CPU
speed and/or operating system. Synchronization is done by I/O waitstates, that
should work even on faster computers. Non-zero delays are eventually required
for cables without pull-ups.



## Requirements


Beside for the cable and plugs, no special requirements.

The cable should work with all parallel ports, including old-fashioned
one-directional printer ports, as well as modern bi-directional EPP ports.
Transfer timings should work stable regardless of the PCs CPU speed (see above
though), and regardless of multitasking interruptions.

Both no$gba and the actual transmission procedure are using some 32bit code, so
that either one currently requires 80386SX CPUs or above.



## Connection Examples


As far as I can imagine, there are four possible methods how to connect the
cable to the GBA. The first two methods don't require to open the GBA, and the
other methods also allow to connect optional power supply and reset signal.

```
  1) Connect it to the GBA link port. Advantage: No need to
     open/modify the GBA. Disadvantage: You need a special plug,
     (typically gained by removing it from a gameboy link cable).
  2) Solder the cable directly to the GBA link port pins. Advantages:
     No plug required & no need to open the GBA. Disadvantages:
     You can't remove the cable, and the link port becomes unusable.
  3) Solder the cable directly to the GBA mainboard. Advantage: No
     plug required at the GBA side. Disadvantage: You'll always
     have a cable leaping out of the GBA even when not using it,
     unless you put a small standard plug between GBA and cable.
  4) Install a Centronics socket in the GBA (between power switch
     and headphone socket). Advantage: You can use a standard
     printer cable. Disadvantages: You need to cut a big hole into
     the GBAs battery box (which cannot be used anymore), the big
     cable might be a bit uncomfortable when holding the GBA.
```

Personally, I've decided to use the lastmost method as I don't like ending up
with hundreds of special cables for different purposes, and asides, it's been
fun to damage the GAB as much as possible.



## Note


The above used PC parallel port signals are typically using 5V=HIGH while GBA
link ports deal with 3V=HIGH. From my experiences, the different voltages do
not cause communication problems (and do not damage the GBA and/or PC
hardware), and after all real men don't care about a handful of volts, however,
use at own risk.




# <a name="auxxbooflashcardupload"></a>AUX Xboo Flashcard Upload




## Flashcard Upload


Allows to write data to flashcards which are plugged into GBA cartridge slot,
cartridge is automatically started after writing. On initial power-up, hold
down START+SELECT to prevent the GBA from booting the old program in the
flashcard.

The Upload function in Utility menu uses flashcard mode for files bigger than
256KB (otherwise uses multiboot mode automatically). Also, there's a separate
Upload to Flashcard function in Remote Access submenu, allowing to write files
of 256KB or less to flashcard if that should be desired.



## Supported Flashcards


Function currently tested with Visoly Flash Advance (FA) 256Mbit (32MB) Turbo
cartridge. Should also work with older FA versions. Please let me know if you
are using other flashcards which aren't yet supported.



## Flashcard Performance


Writing to flashcards may become potentially slow because of chip erase/write
times, cable transmission time, and the sheer size of larger ROM-images.
However, developers whom are testing different builts of their project usually
won't need to rewrite the complete flashcard, Xboo uses a highspeed checksum
mechanism (16MB/sec) to determine which flashcard sector(s) have changed, and
does then re-write only these sector(s).

To eliminate transmission time, data transfer takes place in the erase phases.
Erase/write time depends on the flashcard type, should be circa 1-2 seconds per
256KB sector. Because the cartridge is programmed directly in the GBA there's
no need to remove it from the GBA when writing to it.



## Developers Advice


Locate your program fragments at fixed addresses, for example, code and data
blocks each aligned to 64K memory boundaries, so that data remains at the same
location even when the size of code changes. Fill any blank spaces by value FFh
for faster write time. Reduce the size of your ROM-image by efficient memory
use (except for above alignment trick). Include the burst boot backdoor in your
program, allowing to re-write the flashcard directly without resetting the GBA.



## Lamers Advice


Xboo Flashcard support does not mean to get lame & to drop normal multiboot
support, if your program fits into 256KB then make it \<both> flashcard
\<and> multiboot compatible - multiboot reduces upload time, increases
your flashcard lifetime, and will also work for people whom don't own
flashcards.




# <a name="auxxbooburstbootbackdoor"></a>AUX Xboo Burst Boot Backdoor



When writing Xboo compatible programs, always include a burst boot "backdoor",
this will allow yourself (and other people) to upload programs much faster as
when using the normal GBA BIOS multiboot function. Aside from the improved
transmission speed, there's no need to reset the GBA each time (eventually
manually if you do not have reset connect), without having to press
Start+Select (if cartridge inserted), and, most important, the time-consuming
nintendo-logo intro is bypassed.



## The Burst Boot Protocol


In your programs IRQ handler, add some code that watches out for burst boot IRQ
requests. When sensing a burst boot request, download the actual boot
procedure, and pass control to that procedure.

```
  Send (PC)    Reply (GBA)
  "BRST"       "BOOT"        ;request burst, and reply <prepared> for boot
  <wait 1/16s> <process IRQ> ;long delay, allow slave to enter IRQ handler
  llllllll     "OKAY"        ;send length in bytes, reply <ready> to boot
  dddddddd     --------      ;send data in 32bit units, reply don't care
  cccccccc     cccccccc      ;exchange crc (all data units added together)
```

Use normal mode, 32bit, external clock for all transfers. The received
highspeed loader (currently approx. 180h bytes) is to be loaded to and started
at 3000000h, which will then handle the actual download operation.


Below is an example program which works with multiboot, burstboot, and as
normal rom/flashcard. The source can be assembled with a22i (the no$gba
built-in assembler, see no$gba utility menu). When using other/mainstream
assemblers, you'll eventually have to change some directives, convert numbers
from NNNh into 0xNNN format, and define the origin somewhere in linker/makefile
instead of in source code.


```
 .arm            ;select 32bit ARM instruction set
 .gba            ;indicate that it's a gameboy advance program
 .fix            ;automatically fix the cartridge header checksum
 org 2000000h    ;origin in RAM for multiboot-cable/no$gba-cutdown programs
 ;------------------
 ;cartridge header/multiboot header
  b     rom_start                ;-rom entry point
  dcb   ...insert logo here...   ;-nintento logo (156 bytes)
  dcb   'XBOO SAMPLE '           ;-title (12 bytes)
  dcb   0,0,0,0,  0,0            ;-game code (4 bytes), maker code (2 bytes)
  dcb   96h,0,0                  ;-fixed value 96h, main unit code, device type
  dcb   0,0,0,0,0,0,0            ;-reserved (7 bytes)
  dcb   0                        ;-software version number
  dcb   0                        ;-header checksum (set by .fix)
  dcb   0,0                      ;-reserved (2 bytes)
  b     ram_start                ;-multiboot ram entry point
  dcb   0,0                      ;-multiboot reserved bytes (destroyed by BIOS)
  dcb   0,0                      ;-blank padded (32bit alignment)
 ;------------------
 irq_handler:  ;interrupt handler (note: r0-r3 are pushed by BIOS)
  mov    r1,4000000h             ;\get I/O base address,
  ldr    r0,[r1,200h] ;IE/IF     ; read IE and IF,
  and    r0,r0,r0,lsr 16         ; isolate occurred AND enabled irqs,
  add    r3,r1,200h   ;IF        ; and acknowledge these in IF
  strh   r0,[r3,2]               ;/
  ldrh   r3,[r1,-8]              ;\mix up with BIOS irq flags at 3007FF8h,
  orr    r3,r3,r0                ; aka mirrored at 3FFFFF8h, this is required
  strh   r3,[r1,-8]              ;/when using the (VBlank-)IntrWait functions
  and    r3,r0,80h ;IE/IF.7 SIO  ;\
  cmp    r3,80h                  ; check if it's a burst boot interrupt
  ldreq  r2,[r1,120h] ;SIODATA32 ; (if interrupt caused by serial transfer,
  ldreq  r3,[msg_brst]           ; and if received data is "BRST",
  cmpeq  r2,r3                   ; then jump to burst boot)
  beq    burst_boot              ;/
  ;... insert your own interrupt handler code here ...
  bx     lr                      ;-return to the BIOS interrupt handler
 ;------------------
 burst_boot:     ;requires incoming r1=4000000h
  ;... if your program uses DMA, disable any active DMA transfers here ...
  ldr   r4,[msg_okay]            ;\
  bl    sio_transfer             ; receive transfer length/bytes & reply "OKAY"
  mov   r2,r0 ;len               ;/
  mov   r3,3000000h   ;dst       ;\
  mov   r4,0  ;crc               ;
 @@lop:                          ;
  bl    sio_transfer             ; download burst loader to 3000000h and up
  stmia [r3]!,r0      ;dst       ;
  add   r4,r4,r0      ;crc       ;
  subs  r2,r2,4       ;len       ;
  bhi   @@lop                    ;/
  bl    sio_transfer             ;-send crc value to master
  b     3000000h  ;ARM state!    ;-launch actual transfer / start the loader
 ;------------------
 sio_transfer:  ;serial transfer subroutine, 32bit normal mode, external clock
  str   r4,[r1,120h]  ;siodata32 ;-set reply/send data
  ldr   r0,[r1,128h]  ;siocnt    ;\
  orr   r0,r0,80h                ; activate slave transfer
  str   r0,[r1,128h]  ;siocnt    ;/
 @@wait:                         ;\
  ldr   r0,[r1,128h]  ;siocnt    ; wait until transfer completed
  tst   r0,80h                   ;
  bne   @@wait                   ;/
  ldr   r0,[r1,120h]  ;siodata32 ;-get received data
  bx    lr
 ;---
 msg_boot dcb 'BOOT'     ;\
 msg_okay dcb "OKAY"     ; ID codes for the burstboot protocol
 msg_brst dcb "BRST"     ;/
 ;------------------
 download_rom_to_ram:
  mov  r0,8000000h  ;src/rom     ;\
  mov  r1,2000000h  ;dst/ram     ;
  mov  r2,40000h/16 ;length      ; transfer the ROM content
 @@lop:                          ; into RAM (done in units of 4 words/16 bytes)
  ldmia [r0]!,r4,r5,r6,r7        ; currently fills whole 256K of RAM,
  stmia [r1]!,r4,r5,r6,r7        ; even though the proggy is smaller
  subs  r2,r2,1                  ;
  bne   @@lop                    ;/
  sub   r15,lr,8000000h-2000000h ;-return (retadr rom/8000XXXh -> ram/2000XXXh)
 ;------------------
 init_interrupts:
  mov  r4,4000000h               ;-base address for below I/O registers
  ldr  r0,=irq_handler           ;\install IRQ handler address
  str  r0,[r4,-4]   ;IRQ HANDLER ;/at 3FFFFFC aka 3007FFC
  mov  r0,0008h                  ;\enable generating vblank irqs
  strh r0,[r4,4h]   ;DISPSTAT    ;/
  mrs  r0,cpsr                   ;\
  bic  r0,r0,80h                 ; cpu interrupt enable (clear i-flag)
  msr  cpsr,r0                   ;/
  mov  r0,0                      ;\
  str  r0,[r4,134h] ;RCNT        ; init SIO normal mode, external clock,
  ldr  r0,=5080h                 ; 32bit, IRQ enable, transfer started
  str  r0,[r4,128h] ;SIOCNT      ; output "BOOT" (indicate burst boot prepared)
  ldr  r0,[msg_boot]             ;
  str  r0,[r4,120h] ;SIODATA32   ;/
  mov  r0,1                      ;\interrupt master enable
  str  r0,[r4,208h] ;IME=1       ;/
  mov  r0,81h                    ;\enable execution of vblank IRQs,
  str  r0,[r4,200h] ;IE=81h      ;/and of SIO IRQs (burst boot)
  bx   lr
 ;------------------
 rom_start:   ;entry point when booted from flashcart/rom
  bl   download_rom_to_ram       ;-download ROM to RAM (returns to ram_start)
 ram_start:   ;entry point for multiboot/burstboot
  mov  r0,0feh                   ;\reset all registers, and clear all memory
  swi  10000h ;RegisterRamReset  ;/(except program code in wram at 2000000h)
  bl   init_interrupts           ;-install burst boot irq handler
  mov  r4,4000000h               ;\enable video,
  strh r4,[r4,000h] ;DISPCNT     ;/by clearing the forced blank bit
 @@mainloop:
  swi  50000h ;VBlankIntrWait    ;-wait one frame (cpu in low power mode)
  mov  r5,5000000h               ;\increment the backdrop palette color
  str  r8,[r5]                   ; (ie. display a blinking screen)
  add  r8,r8,1                   ;/
  b    @@mainloop
 ;------------------
 .pool
 end
```




# <a name="aboutthisdocument"></a>About this Document




## About


GBATEK written 2001-2014 by Martin Korth, programming specs for the GBA and NDS
hardware, I've been trying to keep the specs both as short as possible, and as
complete as possible. The document is part of the no$gba debuggers built-in
help text.



## Updates


The standalone docs in TXT and HTM format are updated when having added any
major changes to the document. The no$gba built-in version will be updated more
regularly, including for minor changes, along with all no$gba updates.



## Homepage


- [http://problemkaputt.de/gba.htm](http://problemkaputt.de/gba.htm) - no$gba emulator homepage (freeware)

- [http://problemkaputt.de/gba-dev.htm](http://problemkaputt.de/gba-dev.htm) - no$gba debugger homepage

- [http://problemkaputt.de/gbapics.htm](http://problemkaputt.de/gbapics.htm) - no$gba debugger screenshots

- [http://problemkaputt.de/gbatek.htm](http://problemkaputt.de/gbatek.htm) - gbatek html version

- [http://problemkaputt.de/gbatek.txt](http://problemkaputt.de/gbatek.txt) - gbatek text version



## Feedback


If you find any information in this document to be misleading, incomplete, or
incorrect, please say something! My spam-shielded email address is found at:

- [http://problemkaputt.de/email.htm](http://problemkaputt.de/email.htm) - contact

Mail from programmers only, please. No gaming questions, thanks.



## Credits


Thanks for GBATEK fixes, and for info about GBA and NDS hardware,

- Jasper Vijn

- Remi Veilleux (DS video details)

- Randy Linden

- Sebastian Rasmussen

- Stephen Stair (DS Wifi)

- Cue (DS Firmware bits and bytes)

- Tim Seidel (DS Wifi RF2958 datasheet)

- Damien Good (DS Bios Dumping, and lots of e-Reader info)

- Kenobi and Dualscreenman (lots of ARDS/CBDS cheat info)

- Flubba (GBA X/Y-Axis tilt sensor, and GBA Gameboy Player info)

- DarkFader (DS Key2)

- Dstek by neimod (DS Sound)

- Christian Auby

- Jeff Frohwein

- NDSTech Wiki, 
- [http://www.bottledlight.com/ds/](http://www.bottledlight.com/ds/) (lots of DS info)

- wwylele (xpertteak cpu and mmio findings)



## Formatting


TXT is 80 columns, TXT is 80 columns, TXT is 80 columns.

Don't trust anything else. Never.






# AMD64 Instruction Set Mastery — Week 5
## Logical and Bit Manipulation Instructions

**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 3  

---

# Lecture Notes

## 1. Basic Logical Operations

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Logical Instructions                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  AND — Bitwise AND                                                     │
│  ────────────────────────────────────────────────────────────────────  │
│  24 ib       AND AL, imm8     │  20-23 /r  AND r/m, r / r, r/m       │
│  25 id       AND rAX, imm32   │  80/4, 81/4, 83/4  AND r/m, imm      │
│                                                                         │
│  OR — Bitwise OR                                                       │
│  ────────────────────────────────────────────────────────────────────  │
│  0C, 0D      OR accum, imm    │  08-0B /r  OR r/m, r / r, r/m        │
│  80/1, 81/1, 83/1  OR r/m, imm                                        │
│                                                                         │
│  XOR — Bitwise Exclusive OR                                           │
│  ────────────────────────────────────────────────────────────────────  │
│  34, 35      XOR accum, imm   │  30-33 /r  XOR r/m, r / r, r/m       │
│  80/6, 81/6, 83/6  XOR r/m, imm                                       │
│                                                                         │
│  NOT — One's Complement                                               │
│  ────────────────────────────────────────────────────────────────────  │
│  F6 /2, F7 /2  NOT r/m        │  Flags: None affected                │
│                                                                         │
│  TEST — Logical Compare                                               │
│  ────────────────────────────────────────────────────────────────────  │
│  A8, A9      TEST accum, imm  │  84-85 /r  TEST r/m, r               │
│  F6/0, F7/0  TEST r/m, imm    │  Like AND but result discarded       │
│                                                                         │
│  Flag behavior (AND, OR, XOR, TEST):                                  │
│  - CF, OF always cleared                                              │
│  - SF, ZF, PF set according to result                                 │
│  - AF undefined                                                        │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. Shift Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Shift Instructions                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  SHL/SAL — Shift Left (Logical/Arithmetic, identical)                 │
│  ────────────────────────────────────────────────────────────────────  │
│  D0 /4       SHL r/m8, 1      │  C0 /4 ib   SHL r/m8, imm8           │
│  D1 /4       SHL r/m, 1       │  C1 /4 ib   SHL r/m, imm8            │
│  D2 /4       SHL r/m8, CL     │  D3 /4      SHL r/m, CL              │
│                                                                         │
│  SHR — Shift Right Logical (zero-fill)                                │
│  D0 /5, D1 /5, D2 /5, D3 /5 (shift 1 or CL)                          │
│  C0 /5, C1 /5 (shift imm8)                                            │
│                                                                         │
│  SAR — Shift Right Arithmetic (sign-fill)                             │
│  D0 /7, D1 /7, D2 /7, D3 /7 (shift 1 or CL)                          │
│  C0 /7, C1 /7 (shift imm8)                                            │
│                                                                         │
│  Flags:                                                                │
│  - CF = last bit shifted out                                          │
│  - OF = undefined (except shift by 1)                                 │
│  - SF, ZF, PF set according to result                                 │
│                                                                         │
│  64-bit mode: Shift count masked to 6 bits (0-63)                     │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 3. Rotate Instructions

```
ROL — Rotate Left           │  ROR — Rotate Right
RCL — Rotate Left with CF   │  RCR — Rotate Right with CF

Forms same as shifts: /0 (ROL), /1 (ROR), /2 (RCL), /3 (RCR)

Rotation: Bits wrap around, not lost
CF included in rotation for RCL/RCR (9-bit/17-bit/33-bit/65-bit rotation)
```

## 4. Double-Precision Shifts

```
SHLD — Shift Left Double
  0F A4 /r ib   SHLD r/m, r, imm8
  0F A5 /r      SHLD r/m, r, CL
  
  Shifts r/m left, filling from high bits of r

SHRD — Shift Right Double
  0F AC /r ib   SHRD r/m, r, imm8
  0F AD /r      SHRD r/m, r, CL
  
  Shifts r/m right, filling from low bits of r

Use: Multi-precision shifts, bit field extraction
```

## 5. Bit Test and Manipulation

```
BT  — Bit Test:        CF ← bit[r/m, index]
BTS — Bit Test and Set: CF ← bit[...], bit ← 1
BTR — Bit Test and Reset: CF ← bit[...], bit ← 0
BTC — Bit Test and Complement: CF ← bit[...], bit ← NOT bit

Opcodes: 0F A3/AB/B3/BB /r (register index)
         0F BA /4-7 ib (immediate index)

BSF — Bit Scan Forward: Find lowest set bit
BSR — Bit Scan Reverse: Find highest set bit
  0F BC /r    BSF r, r/m
  0F BD /r    BSR r, r/m
  ZF set if source is 0
```

## 6. BMI1/BMI2 Instructions

```
Modern bit manipulation (require CPUID feature flags):

ANDN r, r, r/m    — r ← NOT(r) AND r/m
BEXTR r, r/m, r   — Extract bit field
BLSI r, r/m       — Isolate lowest set bit
BLSMSK r, r/m     — Mask up to lowest set bit
BLSR r, r/m       — Reset lowest set bit

BZHI r, r/m, r    — Zero high bits
PDEP r, r, r/m    — Parallel deposit bits
PEXT r, r, r/m    — Parallel extract bits

LZCNT r, r/m      — Count leading zeros (AMD extension)
TZCNT r, r/m      — Count trailing zeros
POPCNT r, r/m     — Population count (number of 1 bits)
```

---

# Exercises

1. Implement `popcount` using only basic logical operations
2. Extract bits 15:8 using BEXTR and using shifts/masks
3. Implement sign extension using SAR

---

# AMD64 Instruction Set Mastery — Week 6
## Control Flow Instructions

---

# Lecture Notes

## 1. Unconditional Jump

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    JMP — Jump                                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Near Jump (same segment):                                             │
│  EB cb       JMP rel8        Short jump (-128 to +127)                │
│  E9 cd       JMP rel32       Near jump (±2GB in 64-bit mode)          │
│  FF /4       JMP r/m64       Indirect jump through register/memory    │
│                                                                         │
│  Far Jump (different segment):                                         │
│  FF /5       JMP m16:32/64   Indirect far jump                        │
│  EA cp       JMP ptr16:32    Direct far jump (invalid in 64-bit!)     │
│                                                                         │
│  Flags: None affected                                                  │
│                                                                         │
│  64-bit mode: RIP-relative addressing default for near jumps          │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. Conditional Jumps

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Jcc — Conditional Jump                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Short form (7X cb): ±128 bytes                                       │
│  Near form (0F 8X cd): ±2GB                                           │
│                                                                         │
│  Unsigned comparisons:                                                 │
│  JA/JNBE  (77/0F 87)  — Above: CF=0 AND ZF=0                         │
│  JAE/JNB  (73/0F 83)  — Above or Equal: CF=0                         │
│  JB/JNAE  (72/0F 82)  — Below: CF=1                                  │
│  JBE/JNA  (76/0F 86)  — Below or Equal: CF=1 OR ZF=1                │
│                                                                         │
│  Signed comparisons:                                                   │
│  JG/JNLE  (7F/0F 8F)  — Greater: ZF=0 AND SF=OF                     │
│  JGE/JNL  (7D/0F 8D)  — Greater or Equal: SF=OF                     │
│  JL/JNGE  (7C/0F 8C)  — Less: SF≠OF                                 │
│  JLE/JNG  (7E/0F 8E)  — Less or Equal: ZF=1 OR SF≠OF               │
│                                                                         │
│  Equality/Zero:                                                        │
│  JE/JZ    (74/0F 84)  — Equal/Zero: ZF=1                            │
│  JNE/JNZ  (75/0F 85)  — Not Equal/Zero: ZF=0                        │
│                                                                         │
│  Single flags:                                                         │
│  JC/JNC, JO/JNO, JS/JNS, JP/JNP                                      │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 3. CALL and RET

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    CALL and RET                                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  CALL — Call Procedure                                                 │
│  E8 cd       CALL rel32      Near call, push return address           │
│  FF /2       CALL r/m64      Indirect near call                       │
│  FF /3       CALL m16:64     Indirect far call                        │
│                                                                         │
│  Operation: PUSH RIP; RIP ← target                                    │
│                                                                         │
│  RET — Return from Procedure                                          │
│  C3          RET             Near return                              │
│  C2 iw       RET imm16       Near return, pop imm16 extra bytes       │
│  CB          RETF            Far return                               │
│  CA iw       RETF imm16      Far return with stack adjustment         │
│                                                                         │
│  Operation: POP RIP; [RSP += imm16]                                   │
│                                                                         │
│  Flags: None affected                                                  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 4. LOOP Instructions

```
LOOP rel8  (E2 cb):  RCX--; if RCX≠0, jump
LOOPE rel8 (E1 cb):  RCX--; if RCX≠0 AND ZF=1, jump
LOOPNE rel8 (E0 cb): RCX--; if RCX≠0 AND ZF=0, jump

Note: These are slow on modern CPUs!
Prefer: DEC RCX; JNZ loop_start
```

## 5. SETcc — Set Byte on Condition

```
0F 9X /0    SETcc r/m8

Sets r/m8 to 1 if condition true, 0 otherwise.
Same conditions as Jcc.

Example:
  cmp rax, rbx
  setl cl         ; CL = 1 if RAX < RBX (signed)
```

---

# AMD64 Instruction Set Mastery — Week 7
## String and Block Instructions

---

# Lecture Notes

## 1. String Instruction Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    String Instructions                                 │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Register usage:                                                       │
│  RSI = Source index (DS:RSI or specified segment)                     │
│  RDI = Destination index (ES:RDI, ES not overridable)                │
│  RCX = Count for REP prefix                                           │
│  Direction Flag (DF): 0 = increment, 1 = decrement                    │
│                                                                         │
│  Base instructions (single element):                                   │
│  MOVS — Move string element                                           │
│  CMPS — Compare string elements                                       │
│  SCAS — Scan string for value in AL/AX/EAX/RAX                       │
│  LODS — Load string element into accumulator                          │
│  STOS — Store accumulator into string                                 │
│                                                                         │
│  Size suffixes: B (byte), W (word), D (dword), Q (qword)             │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. REP Prefixes

```
REP (F3):    Repeat RCX times
REPE/REPZ (F3):   Repeat while equal/zero AND RCX≠0
REPNE/REPNZ (F2): Repeat while not equal/zero AND RCX≠0

REP MOVSB:  Copy RCX bytes from [RSI] to [RDI]
REP STOSB:  Fill RCX bytes at [RDI] with AL
REPE CMPSB: Compare until mismatch or RCX=0
REPNE SCASB: Scan until found or RCX=0
```

## 3. Fast String Operations

```
Modern CPUs have "fast strings" microcode:
- REP MOVSB can be as fast as or faster than SSE copy
- Requires: DF=0, aligned addresses (ideally)
- Processor may use wider internal transfers

ERMSB (Enhanced REP MOVSB/STOSB):
- Check CPUID for support
- Optimized for small-to-medium copies
```

---

# AMD64 Instruction Set Mastery — Week 8
## Flag and Processor Control Instructions

---

# Lecture Notes

## 1. Flag Manipulation

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Flag Instructions                                   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Carry Flag:                                                           │
│  F8    CLC    Clear CF                                                │
│  F9    STC    Set CF                                                  │
│  F5    CMC    Complement CF                                           │
│                                                                         │
│  Direction Flag:                                                       │
│  FC    CLD    Clear DF (strings increment)                           │
│  FD    STD    Set DF (strings decrement)                             │
│                                                                         │
│  Interrupt Flag:                                                       │
│  FA    CLI    Clear IF (disable interrupts) — Ring 0 only            │
│  FB    STI    Set IF (enable interrupts) — Ring 0 only               │
│                                                                         │
│  LAHF/SAHF — Load/Store AH from/to flags                             │
│  9F    LAHF   AH ← SF:ZF:0:AF:0:PF:1:CF                              │
│  9E    SAHF   SF:ZF:_:AF:_:PF:_:CF ← AH                              │
│                                                                         │
│  PUSHF/POPF — Push/Pop flags register                                │
│  9C    PUSHF  Push FLAGS/EFLAGS/RFLAGS                               │
│  9D    POPF   Pop FLAGS/EFLAGS/RFLAGS                                │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. Processor Control

```
NOP — No Operation
  90          NOP              (actually XCHG EAX, EAX)
  0F 1F /0    NOP r/m (multi-byte NOP, 2-9 bytes)

HLT — Halt (Ring 0)
  F4          Halts CPU until interrupt

PAUSE — Spin-Loop Hint
  F3 90       Hints CPU we're in spin-wait loop
              Saves power, improves performance

CPUID — CPU Identification
  0F A2       Returns CPU info based on EAX input

RDTSC — Read Time-Stamp Counter
  0F 31       EDX:EAX ← TSC

RDTSCP — Read TSC and Processor ID
  0F 01 F9    EDX:EAX ← TSC, ECX ← IA32_TSC_AUX
```

## 3. Memory Ordering

```
MFENCE (0F AE F0) — Full memory fence
SFENCE (0F AE F8) — Store fence
LFENCE (0F AE E8) — Load fence

LOCK prefix (F0) — Atomic memory operation
  Only valid with: ADD, ADC, AND, BTC, BTR, BTS, 
                   CMPXCHG, CMPXCHG8B/16B, DEC, INC,
                   NEG, NOT, OR, SBB, SUB, XOR, XADD, XCHG
```

---

# Combined Exercises (Weeks 5-8)

1. Implement `strlen` using REPNE SCASB
2. Write branchless `abs()` using logical operations
3. Implement a spinlock using LOCK XCHG
4. Create a function dispatcher using indirect CALL

---

**Next Week**: System Instructions Part 1—segments, descriptors, protection.

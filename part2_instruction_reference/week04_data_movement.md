# AMD64 Instruction Set Mastery — Week 3
## Data Movement Instructions

**Course**: AMD64 Instruction Set Mastery  
**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 3  
**Week Duration**: ~15-20 hours of focused study  

---

# Lecture Notes

## 1. MOV — The Fundamental Transfer

### MOV Instruction Forms (from Volume 3)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    MOV Instruction Family                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode        Instruction         Description                         │
│  ────────────────────────────────────────────────────────────────────  │
│  88 /r         MOV r/m8, r8        Move r8 to r/m8                    │
│  89 /r         MOV r/m16/32/64, r  Move r16/32/64 to r/m              │
│  8A /r         MOV r8, r/m8        Move r/m8 to r8                    │
│  8B /r         MOV r16/32/64, r/m  Move r/m to r16/32/64              │
│  8C /r         MOV r/m16, Sreg     Move segment register to r/m16     │
│  8E /r         MOV Sreg, r/m16     Move r/m16 to segment register     │
│  A0           MOV AL, moffs8       Move byte at moffs8 to AL          │
│  A1           MOV rAX, moffs       Move word/dword/qword to rAX       │
│  A2           MOV moffs8, AL       Move AL to moffs8                  │
│  A3           MOV moffs, rAX       Move rAX to moffs                  │
│  B0+rb ib     MOV r8, imm8         Move imm8 to r8                    │
│  B8+rw/rd/rq  MOV r, imm           Move imm to r16/32/64              │
│  C6 /0 ib     MOV r/m8, imm8       Move imm8 to r/m8                  │
│  C7 /0        MOV r/m, imm         Move imm to r/m16/32/64            │
│                                                                         │
│  Key insight: Direction bit (bit 1 of opcode):                        │
│    88, 89 = r/m ← reg (direction = 0)                                 │
│    8A, 8B = reg ← r/m (direction = 1)                                 │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### MOV Does NOT Affect Flags

```
Flags Affected: None

This is critical! MOV is one of the few instructions that:
- Performs data transfer
- Does NOT modify any flags

Implication: Safe to use MOV between operations that depend on flags
```

> **AMD Manual Reference**: Volume 3, "MOV" instruction entry (varies by revision)

### The 64-bit Immediate Exception

```
MOV r64, imm64 (opcode B8+rd with REX.W):
  - ONLY instruction that accepts true 64-bit immediate
  - 10 bytes total: REX (1) + opcode (1) + imm64 (8)

All other instructions with REX.W:
  - Use 32-bit immediate, sign-extended to 64 bits

Example:
  mov rax, 0x123456789ABCDEF0  ; Valid: 48 B8 F0 DE BC 9A 78 56 34 12
  add rax, 0x123456789ABCDEF0  ; Invalid! Immediate too large
```

---

## 2. MOVSX and MOVZX — Sign and Zero Extension

### Extension Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Extension Instructions                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  MOVZX — Move with Zero-Extend                                        │
│  ────────────────────────────────────────────────────────────────────  │
│  0F B6 /r    MOVZX r16/32/64, r/m8    Zero-extend byte               │
│  0F B7 /r    MOVZX r32/64, r/m16      Zero-extend word               │
│                                                                         │
│  MOVSX — Move with Sign-Extend                                        │
│  ────────────────────────────────────────────────────────────────────  │
│  0F BE /r    MOVSX r16/32/64, r/m8    Sign-extend byte               │
│  0F BF /r    MOVSX r32/64, r/m16      Sign-extend word               │
│                                                                         │
│  MOVSXD — Move with Sign-Extend Doubleword (64-bit mode)             │
│  ────────────────────────────────────────────────────────────────────  │
│  63 /r       MOVSXD r64, r/m32        Sign-extend dword to qword     │
│                                                                         │
│  Note: In 64-bit mode, MOV to 32-bit register automatically          │
│        zero-extends to 64 bits. MOVZX r64, r/m32 doesn't exist!      │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### The Zero-Extension Rule

```
Critical 64-bit mode behavior:

Writing to a 32-bit register ALWAYS zero-extends to 64 bits:
  mov eax, 0xFFFFFFFF    ; RAX = 0x00000000FFFFFFFF (not 0xFFFFFFFFFFFFFFFF)
  
Writing to 8-bit or 16-bit registers does NOT extend:
  mov al, 0xFF           ; Only AL modified, upper RAX unchanged
  mov ax, 0xFFFF         ; Only AX modified, upper RAX unchanged
  
This is why MOVZX r64, r/m32 doesn't exist—use MOV r32, r/m32 instead.
```

---

## 3. LEA — Load Effective Address

### LEA Instruction

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    LEA — Load Effective Address                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode      Instruction         Description                           │
│  8D /r       LEA r, m            Load effective address of m into r   │
│                                                                         │
│  Key insight: LEA computes address but does NOT access memory         │
│                                                                         │
│  Common uses:                                                          │
│  1. Address calculation:                                               │
│     lea rax, [rbx + rcx*8 + 0x100]  ; rax = rbx + rcx*8 + 0x100      │
│                                                                         │
│  2. Arithmetic without flags (3-operand add):                         │
│     lea rax, [rbx + rcx]            ; rax = rbx + rcx, no flags      │
│     lea rax, [rbx + rbx*2]          ; rax = rbx * 3                   │
│     lea rax, [rbx*4]                ; rax = rbx * 4                   │
│     lea rax, [rbx + rbx*4]          ; rax = rbx * 5                   │
│     lea rax, [rbx + rbx*8]          ; rax = rbx * 9                   │
│                                                                         │
│  3. Copy with offset:                                                  │
│     lea rax, [rbx + 1]              ; rax = rbx + 1, flags unchanged  │
│                                                                         │
│  Flags Affected: None                                                  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### LEA vs. MOV

```
lea rax, [rbx]       ; rax = rbx (address)
mov rax, [rbx]       ; rax = *rbx (contents at address)

lea rax, [label]     ; rax = address of label
mov rax, [label]     ; rax = contents at label
```

---

## 4. PUSH and POP

### Stack Operations

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    PUSH and POP Instructions                           │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  PUSH — Push onto Stack                                               │
│  ────────────────────────────────────────────────────────────────────  │
│  50+rd       PUSH r64            Push r64 onto stack                  │
│  FF /6       PUSH r/m64          Push r/m64 onto stack                │
│  6A ib       PUSH imm8           Push sign-extended imm8              │
│  68 id       PUSH imm32          Push sign-extended imm32             │
│                                                                         │
│  POP — Pop from Stack                                                  │
│  ────────────────────────────────────────────────────────────────────  │
│  58+rd       POP r64             Pop into r64                         │
│  8F /0       POP r/m64           Pop into r/m64                       │
│                                                                         │
│  In 64-bit mode:                                                       │
│  - Default operand size is 64 bits (not 32!)                          │
│  - Cannot PUSH/POP 32-bit values                                      │
│  - Can PUSH/POP 16-bit values with 66h prefix (rarely useful)        │
│                                                                         │
│  Operation:                                                            │
│  PUSH: RSP = RSP - 8; [RSP] = operand                                 │
│  POP:  operand = [RSP]; RSP = RSP + 8                                 │
│                                                                         │
│  Flags Affected: None                                                  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### PUSHA/POPA — Invalid in 64-bit Mode

```
The following are invalid in 64-bit mode:
  PUSHA/PUSHAD — Push all general registers
  POPA/POPAD   — Pop all general registers

These instructions still work in compatibility mode (32-bit).
```

---

## 5. XCHG — Exchange

### Exchange Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    XCHG — Exchange                                     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode      Instruction         Description                           │
│  90+rd       XCHG rAX, r         Exchange rAX with r16/32/64          │
│  86 /r       XCHG r/m8, r8       Exchange r8 with r/m8                │
│  87 /r       XCHG r/m, r         Exchange r with r/m16/32/64          │
│                                                                         │
│  CRITICAL: XCHG with memory operand has implicit LOCK!                │
│  ────────────────────────────────────────────────────────────────────  │
│  xchg [rax], rbx   ; Atomic! Implicit LOCK prefix                     │
│  xchg rax, rbx     ; Not atomic (register-only, no memory)           │
│                                                                         │
│  Flags Affected: None                                                  │
│                                                                         │
│  NOP Encoding:                                                         │
│  XCHG EAX, EAX (90h) is encoded as NOP                                │
│  But: 66 90 (XCHG AX, AX) is also valid NOP variant                  │
│       48 90 (XCHG RAX, RAX) would zero upper RAX — use 90h for NOP   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 6. CMOVcc — Conditional Move

### Conditional Move Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    CMOVcc — Conditional Move                           │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Form: 0F 4X /r   CMOVcc r, r/m                                       │
│                                                                         │
│  Condition    Opcode  Test                                             │
│  ──────────────────────────────────────────────────────────────────    │
│  CMOVO        0F 40   OF=1 (overflow)                                 │
│  CMOVNO       0F 41   OF=0                                            │
│  CMOVB/C/NAE  0F 42   CF=1 (below, carry)                            │
│  CMOVAE/NB/NC 0F 43   CF=0                                            │
│  CMOVE/Z      0F 44   ZF=1 (equal, zero)                             │
│  CMOVNE/NZ    0F 45   ZF=0                                            │
│  CMOVBE/NA    0F 46   CF=1 OR ZF=1                                   │
│  CMOVA/NBE    0F 47   CF=0 AND ZF=0                                  │
│  CMOVS        0F 48   SF=1 (sign)                                    │
│  CMOVNS       0F 49   SF=0                                            │
│  CMOVP/PE     0F 4A   PF=1 (parity even)                             │
│  CMOVNP/PO    0F 4B   PF=0 (parity odd)                              │
│  CMOVL/NGE    0F 4C   SF≠OF (less, signed)                          │
│  CMOVGE/NL    0F 4D   SF=OF                                          │
│  CMOVLE/NG    0F 4E   ZF=1 OR SF≠OF                                 │
│  CMOVG/NLE    0F 4F   ZF=0 AND SF=OF                                 │
│                                                                         │
│  Flags Affected: None                                                  │
│                                                                         │
│  Use case: Branchless code                                            │
│    cmp rax, rbx                                                       │
│    cmovl rax, rbx    ; rax = min(rax, rbx)                           │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Other Data Movement

### BSWAP — Byte Swap

```
Opcode: 0F C8+rd
BSWAP r32/r64 — Reverse byte order

Example:
  mov eax, 0x12345678
  bswap eax           ; EAX = 0x78563412

64-bit:
  mov rax, 0x123456789ABCDEF0
  bswap rax           ; RAX = 0xF0DEBC9A78563412

Flags Affected: None
```

### MOVBE — Move with Byte Swap

```
Opcode: 0F 38 F0 /r (load), 0F 38 F1 /r (store)
MOVBE r, m / MOVBE m, r — Move and swap bytes

Combines MOV and BSWAP in one instruction.
Useful for big-endian data conversion.

Requires: MOVBE CPUID feature flag
```

### CBW/CWDE/CDQE — Sign-Extend Accumulator

```
CBW   (98h):  AX  ← sign-extend(AL)
CWDE  (98h):  EAX ← sign-extend(AX)    ; with 66h absent
CDQE  (98h):  RAX ← sign-extend(EAX)   ; with REX.W

CWD   (99h):  DX:AX   ← sign-extend(AX)
CDQ   (99h):  EDX:EAX ← sign-extend(EAX)
CQO   (99h):  RDX:RAX ← sign-extend(RAX)  ; with REX.W

Use: Prepare for IDIV which uses extended dividend
```

---

# Key Readings

| Section | Topic | Time |
|---------|-------|------|
| MOV entry | All MOV forms | 2 hours |
| MOVSX/MOVZX | Extension moves | 1 hour |
| LEA entry | Address computation | 1 hour |
| PUSH/POP | Stack operations | 1.5 hours |
| CMOVcc | Conditional moves | 1.5 hours |
| XCHG, BSWAP | Specialized transfers | 1 hour |

---

# Exercises

## Exercise 1: MOV Encoding Practice

Determine the shortest encoding for:
1. `MOV RAX, 0` (hint: there's a trick)
2. `MOV RAX, 1`
3. `MOV RAX, -1`
4. `MOV RAX, 0x7FFFFFFF`
5. `MOV RAX, 0x80000000`

## Exercise 2: LEA Arithmetic

Use LEA to compute (show encoding):
1. `RAX = RBX * 3`
2. `RAX = RBX * 5 + 10`
3. `RAX = RBX + RCX + 8`
4. `RAX = RBX * 9`

## Exercise 3: Zero-Extension Verification

Write assembly to prove:
1. `MOV EAX, -1` zero-extends to RAX
2. `MOV AX, -1` does NOT affect upper RAX
3. `MOVZX EAX, r/m16` zero-extends to RAX

## Mini-Project: Memory Copy

Implement `memcpy` using only instructions from this week.

---

# Quiz

**Q1**: What's the only instruction that can load a full 64-bit immediate?

**Q2**: Why doesn't MOVZX r64, r/m32 exist?

**Q3**: What implicit prefix does XCHG have when used with memory?

**Q4**: What's the difference between `LEA RAX, [RBX]` and `MOV RAX, [RBX]`?

**Q5**: Does PUSH RAX modify any flags?

---

**Next Week**: Arithmetic Instructions—ADD, SUB, MUL, DIV, and their variants.

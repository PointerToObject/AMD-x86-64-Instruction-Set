# AMD64 Instruction Set Mastery — Week 2
## Instruction Encoding Deep Dive

**Course**: AMD64 Instruction Set Mastery  
**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 1  
**Week Duration**: ~15-20 hours of focused study  

---

# Lecture Notes

## 1. Instruction Byte Format

### Complete Instruction Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                 x86-64 Instruction Byte Order                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────┬─────────┬─────────┬────────┬─────┬──────────┬──────────┐  │
│  │ Legacy  │   REX   │ Opcode  │ ModRM  │ SIB │ Displace │ Immediate│  │
│  │Prefixes │ Prefix  │  1-3B   │  0-1B  │0-1B │  0/1/2/4 │ 0/1/2/4/8│  │
│  │  0-4B   │  0-1B   │         │        │     │   bytes  │   bytes  │  │
│  └─────────┴─────────┴─────────┴────────┴─────┴──────────┴──────────┘  │
│                                                                         │
│  Total maximum: 15 bytes                                               │
│  If exceeded: #UD exception                                            │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

> **AMD Manual Reference**: Volume 3, Chapter 1 "Instruction Encoding" (pp. 1-34)

---

## 2. Legacy Prefixes

### The Five Prefix Groups

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Legacy Prefix Groups                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Group 1 — Lock and Repeat:                                           │
│    F0h = LOCK (atomic memory access)                                  │
│    F2h = REPNE/REPNZ (repeat string, not equal)                       │
│    F3h = REP/REPE/REPZ (repeat string, equal)                         │
│                                                                         │
│  Group 2 — Segment Override:                                          │
│    2Eh = CS segment override / Branch not taken hint                  │
│    36h = SS segment override                                          │
│    3Eh = DS segment override / Branch taken hint                      │
│    26h = ES segment override                                          │
│    64h = FS segment override (used in 64-bit mode)                    │
│    65h = GS segment override (used in 64-bit mode)                    │
│                                                                         │
│  Group 3 — Operand-Size Override:                                     │
│    66h = Toggle between 16/32-bit operand size                        │
│          Also used as mandatory prefix for some SSE instructions      │
│                                                                         │
│  Group 4 — Address-Size Override:                                     │
│    67h = Toggle address size (64→32 in 64-bit mode)                  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Prefix Ordering and Multiple Prefixes

```
Rules:
1. Multiple prefixes from DIFFERENT groups allowed
2. Multiple prefixes from SAME group: only last one effective
3. Order doesn't matter (generally), but convention: Group 1-2-3-4

Example encoding with multiple prefixes:
  F0 64 67 01 00    = LOCK ADD DWORD PTR FS:[EAX], EAX
  │  │  │  └─────── Opcode + ModRM
  │  │  └────────── Group 4: Address-size (32-bit addressing)
  │  └───────────── Group 2: FS segment override
  └──────────────── Group 1: LOCK prefix
```

---

## 3. REX Prefix

### REX Byte Format

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    REX Prefix (40h-4Fh)                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│     7   6   5   4   3   2   1   0                                      │
│   ┌───┬───┬───┬───┬───┬───┬───┬───┐                                   │
│   │ 0 │ 1 │ 0 │ 0 │ W │ R │ X │ B │                                   │
│   └───┴───┴───┴───┴───┴───┴───┴───┘                                   │
│     └─────────────┘   │   │   │   │                                    │
│     Fixed pattern     │   │   │   │                                    │
│     (0100 binary)     │   │   │   │                                    │
│                       │   │   │   │                                    │
│   W = 1: 64-bit operand size (REX.W)                                  │
│   R = 1: Extend ModRM.reg to 4 bits (access R8-R15)                   │
│   X = 1: Extend SIB.index to 4 bits (access R8-R15)                   │
│   B = 1: Extend ModRM.r/m or SIB.base to 4 bits                       │
│                                                                         │
│   REX values: 40h (no bits), 41h (B), 42h (X), 44h (R), 48h (W)      │
│               4Fh (all bits set)                                       │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### When REX is Required

```
REX is REQUIRED when:
  - Using 64-bit operand size (REX.W = 1)
  - Accessing R8-R15 registers (need REX.R, REX.X, or REX.B)
  - Accessing SPL, BPL, SIL, DIL (uniform byte registers)

REX is OPTIONAL when:
  - Any 64-bit mode instruction can have REX prefix
  - REX.W=0 defaults to 32-bit operand size (usually)

REX CANNOT be used:
  - With VEX/EVEX prefixed instructions
  - Before legacy prefixes (must come after)
```

### REX Register Extension Examples

```
Without REX:           With REX.R=1:
ModRM.reg = 000 → RAX   ModRM.reg = 000 → R8
ModRM.reg = 001 → RCX   ModRM.reg = 001 → R9
ModRM.reg = 010 → RDX   ModRM.reg = 010 → R10
...                     ...
ModRM.reg = 111 → RDI   ModRM.reg = 111 → R15

Example: MOV R8, RAX
  Opcode: 89 (MOV r/m64, r64)
  REX: 49h (W=1 for 64-bit, R=1 for R8 in reg field)
  ModRM: C0h (mod=11, reg=000/R8, r/m=000/RAX)
  Full encoding: 49 89 C0
```

> **AMD Manual Reference**: Volume 3, Section 1.2.7 "REX Prefix" (pp. 14-17)

---

## 4. ModRM Byte

### ModRM Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    ModRM Byte Format                                   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│     7   6   5   4   3   2   1   0                                      │
│   ┌───────┬───────────┬───────────┐                                   │
│   │  mod  │    reg    │    r/m    │                                   │
│   │ 2 bits│  3 bits   │  3 bits   │                                   │
│   └───────┴───────────┴───────────┘                                   │
│                                                                         │
│  mod field:                                                            │
│    00 = Memory, no displacement (or disp32 if r/m=101)                │
│    01 = Memory, 8-bit signed displacement                             │
│    10 = Memory, 32-bit displacement                                   │
│    11 = Register (no memory access)                                   │
│                                                                         │
│  reg field:                                                            │
│    - Second operand (usually destination for reg←r/m)                 │
│    - OR opcode extension (/0 through /7)                              │
│                                                                         │
│  r/m field:                                                            │
│    - First operand (memory or register)                               │
│    - If mod≠11 and r/m=100: SIB byte follows                         │
│    - If mod=00 and r/m=101: disp32 only (RIP-relative in 64-bit)     │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Register Encoding (mod=11)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                  Register Encoding (with REX extension)                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  r/m │ No REX │ REX.B=1 │ 8-bit (no REX) │ 8-bit (w/REX) │            │
│  ────┼────────┼─────────┼────────────────┼───────────────┤            │
│  000 │  RAX   │   R8    │      AL        │      AL       │            │
│  001 │  RCX   │   R9    │      CL        │      CL       │            │
│  010 │  RDX   │   R10   │      DL        │      DL       │            │
│  011 │  RBX   │   R11   │      BL        │      BL       │            │
│  100 │  RSP   │   R12   │      AH        │      SPL      │ ◄── Note! │
│  101 │  RBP   │   R13   │      CH        │      BPL      │ ◄── Note! │
│  110 │  RSI   │   R14   │      DH        │      SIL      │ ◄── Note! │
│  111 │  RDI   │   R15   │      BH        │      DIL      │ ◄── Note! │
│                                                                         │
│  Important: AH, CH, DH, BH not accessible if ANY REX prefix present   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Memory Addressing (mod≠11)

```
32/64-bit Addressing Modes (mod + r/m):

mod=00:
  r/m=000: [RAX]
  r/m=001: [RCX]
  r/m=010: [RDX]
  r/m=011: [RBX]
  r/m=100: [SIB]           ← SIB byte follows
  r/m=101: [RIP+disp32]    ← RIP-relative (64-bit mode only)
  r/m=110: [RSI]
  r/m=111: [RDI]

mod=01:
  r/m=000: [RAX+disp8]
  r/m=001: [RCX+disp8]
  ...
  r/m=100: [SIB+disp8]
  r/m=101: [RBP+disp8]
  ...

mod=10:
  r/m=000: [RAX+disp32]
  ...
  r/m=100: [SIB+disp32]
  r/m=101: [RBP+disp32]
  ...
```

> **AMD Manual Reference**: Volume 3, Section 1.4 "ModRM and SIB Bytes" (pp. 17-22)

---

## 5. SIB Byte

### SIB Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    SIB Byte Format                                     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│     7   6   5   4   3   2   1   0                                      │
│   ┌───────┬───────────┬───────────┐                                   │
│   │ scale │   index   │   base    │                                   │
│   │ 2 bits│  3 bits   │  3 bits   │                                   │
│   └───────┴───────────┴───────────┘                                   │
│                                                                         │
│  Effective Address = base + (index × scale) + displacement            │
│                                                                         │
│  scale:                                                                │
│    00 = ×1                                                            │
│    01 = ×2                                                            │
│    10 = ×4                                                            │
│    11 = ×8                                                            │
│                                                                         │
│  index (with REX.X extension):                                        │
│    000-111 = RAX-RDI (or R8-R15 with REX.X=1)                        │
│    100 = No index (index × scale = 0)                                │
│                                                                         │
│  base (with REX.B extension):                                         │
│    000-111 = RAX-RDI (or R8-R15 with REX.B=1)                        │
│    101 = Special case depends on mod:                                 │
│          mod=00: no base, disp32 only                                 │
│          mod≠00: RBP (or R13)                                         │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### SIB Examples

```
Example 1: [RAX + RCX*4]
  ModRM: mod=00, r/m=100 (SIB follows)
  SIB: scale=10 (×4), index=001 (RCX), base=000 (RAX)
  SIB byte = 10 001 000 = 88h
  
Example 2: [RBX + RDX*8 + 0x100]
  ModRM: mod=10 (disp32), r/m=100 (SIB follows)
  SIB: scale=11 (×8), index=010 (RDX), base=011 (RBX)
  SIB byte = 11 010 011 = D3h
  Full addressing: ModRM=94h, SIB=D3h, disp32=00010000h
  
Example 3: [R12 + R13*2]  (needs REX)
  REX: 43h (REX.X=1 for R13 index, REX.B=1 for R12 base)
  ModRM: mod=00, r/m=100
  SIB: scale=01 (×2), index=101 (R13 with REX.X), base=100 (R12 with REX.B)
```

---

## 6. Displacement and Immediate Fields

### Displacement

```
Displacement size determined by ModRM mod field:
  mod=00: No displacement (exceptions: disp32 for [RIP+disp32], [disp32])
  mod=01: 8-bit signed displacement (-128 to +127)
  mod=10: 32-bit signed displacement

In 64-bit mode:
  - Displacements are sign-extended to 64 bits
  - RIP-relative addressing uses 32-bit displacement
  
Storage: Little-endian byte order
```

### Immediate Operands

```
Immediate sizes (from opcode notation):
  ib = 8-bit immediate
  iw = 16-bit immediate
  id = 32-bit immediate
  io = 64-bit immediate (rare, only MOV r64, imm64)

In 64-bit mode with REX.W:
  - Most 32-bit immediates are sign-extended to 64 bits
  - Only MOV supports true 64-bit immediate

Storage: Little-endian byte order
```

---

## 7. VEX and EVEX Prefixes (Overview)

### VEX Prefix (AVX)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    VEX Prefix Formats                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  2-byte VEX (C5h):                                                     │
│  ┌────────┬────────┐                                                   │
│  │   C5   │R vvvv L pp│                                                │
│  └────────┴────────┘                                                   │
│    R = inverted REX.R                                                  │
│    vvvv = additional operand (inverted)                                │
│    L = vector length (0=128-bit, 1=256-bit)                           │
│    pp = implied prefix (00=none, 01=66h, 10=F3h, 11=F2h)              │
│                                                                         │
│  3-byte VEX (C4h):                                                     │
│  ┌────────┬────────────┬────────────┐                                 │
│  │   C4   │R X B mmmmm │W vvvv L pp │                                 │
│  └────────┴────────────┴────────────┘                                 │
│    R, X, B = inverted REX.R, REX.X, REX.B                            │
│    mmmmm = opcode map (01=0F, 02=0F38, 03=0F3A)                       │
│    W = REX.W equivalent                                               │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### EVEX Prefix (AVX-512)

```
4-byte EVEX (62h):
  ┌────────┬────────────┬────────────┬────────────┐
  │   62   │ R X B R' 00│ W vvvv 1 pp│ z L'L b V' │
  │        │ mm         │            │ aaa        │
  └────────┴────────────┴────────────┴────────────┘
  
Additional features:
  - 512-bit vectors (L'L encoding)
  - Mask registers (aaa = k0-k7)
  - Zeroing/merging (z bit)
  - Broadcast (b bit)
  - Extended registers (32 SIMD registers)
```

> **AMD Manual Reference**: Volume 3, Section 1.2 "Instruction Prefixes" (pp. 3-17)

---

## 8. Complete Encoding Examples

### Example 1: `ADD RAX, RBX`

```
Instruction: ADD RAX, RBX (64-bit register-to-register)

Step 1: Find opcode
  ADD r64, r/m64 → 03 /r (from Volume 3)
  Need REX.W for 64-bit

Step 2: Build REX
  REX.W = 1 (64-bit operand)
  REX.R = 0 (RAX in reg, doesn't need extension)
  REX.B = 0 (RBX in r/m, doesn't need extension)
  REX = 0100 1000 = 48h

Step 3: Build ModRM
  mod = 11 (register mode)
  reg = 000 (RAX)
  r/m = 011 (RBX)
  ModRM = 11 000 011 = C3h

Final encoding: 48 03 C3
```

### Example 2: `MOV QWORD PTR [RBX+RCX*8+0x10], RAX`

```
Instruction: MOV [RBX+RCX*8+0x10], RAX

Step 1: Find opcode
  MOV r/m64, r64 → 89 /r
  Need REX.W for 64-bit

Step 2: Build REX
  REX.W = 1, R = 0, X = 0, B = 0
  REX = 48h

Step 3: Build ModRM
  mod = 01 (8-bit displacement)
  reg = 000 (RAX)
  r/m = 100 (SIB follows)
  ModRM = 01 000 100 = 44h

Step 4: Build SIB
  scale = 11 (×8)
  index = 001 (RCX)
  base = 011 (RBX)
  SIB = 11 001 011 = CBh

Step 5: Displacement
  disp8 = 10h

Final encoding: 48 89 44 CB 10
```

### Example 3: `PUSH R13`

```
Instruction: PUSH R13

Step 1: Find opcode
  PUSH r64 → 50+rd (register encoded in opcode)
  Base opcode 50h + register code

Step 2: R13 encoding
  R13 = 1101 binary = 13 decimal
  Low 3 bits: 101 → opcode = 50h + 5 = 55h
  High bit: 1 → need REX.B

Step 3: Build REX
  REX.B = 1 (for R13)
  REX = 0100 0001 = 41h

Final encoding: 41 55
```

---

# Key Readings

## Required

| Section | Topic | Pages | Time |
|---------|-------|-------|------|
| Chapter 1.1 | Instruction format overview | 1-3 | 1 hour |
| Chapter 1.2 | All prefix types | 3-17 | 3 hours |
| Chapter 1.3-1.4 | ModRM and SIB | 17-22 | 2 hours |
| Chapter 1.5-1.6 | Displacement and Immediate | 22-23 | 1 hour |
| Appendix A.1-A.3 | Opcode maps introduction | varies | 2 hours |

---

# Exercises

## Exercise 1: Manual Encoding

Encode these instructions by hand, showing all steps:

1. `SUB RCX, RDX`
2. `MOV AL, [RSI]`
3. `ADD DWORD PTR [RAX+RBX*4], 0x12345678`
4. `JMP QWORD PTR [R8+0x20]`
5. `XOR R10, R11`

## Exercise 2: Decode Byte Sequences

Decode these byte sequences to assembly:

1. `48 8B 04 25 00 10 00 00`
2. `41 FF 14 C5 00 00 00 00`
3. `48 83 C4 20`
4. `0F 84 F0 FF FF FF`
5. `49 89 5C 24 08`

## Exercise 3: Build Encoding Tables

Create your own reference tables for:
- All REX prefix values and their meanings
- ModRM bytes for register-to-register operations
- Common SIB bytes for array indexing

## Mini-Project: Simple Assembler

Write a program that encodes these specific instructions:
- `MOV r64, r64`
- `ADD r64, imm32`
- `PUSH r64`
- `POP r64`

---

# Quiz

**Q1**: What is the valid range of REX prefix bytes?

**Q2**: What does mod=00, r/m=101 mean in 64-bit mode?

**Q3**: Why can't you access AH, BH, CH, DH when a REX prefix is present?

**Q4**: What SIB byte encodes [RAX + RCX*4]?

**Q5**: How many bytes is the instruction `MOV RAX, 0x123456789ABCDEF0`?

---

# Priority Guide

## CRITICAL
- REX prefix format and when required
- ModRM byte structure
- SIB byte structure
- Register encoding with/without REX

## IMPORTANT
- Legacy prefix groups
- VEX prefix overview
- RIP-relative addressing

## OPTIONAL
- EVEX prefix details
- Full opcode map memorization

---

**Next Week**: Data Movement Instructions—MOV, PUSH, POP, LEA, and data transfer operations.

# AMD64 Instruction Set Mastery — Week 4
## Arithmetic Instructions

**Course**: AMD64 Instruction Set Mastery  
**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 3  
**Week Duration**: ~15-20 hours of focused study  

---

# Lecture Notes

## 1. ADD and SUB

### Addition and Subtraction

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    ADD — Add                                           │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode        Instruction         Notes                               │
│  04 ib         ADD AL, imm8        Accumulator short form             │
│  05 id         ADD rAX, imm32      Accumulator short form             │
│  80 /0 ib      ADD r/m8, imm8      ModRM with opcode extension        │
│  81 /0 id      ADD r/m32, imm32    ModRM with opcode extension        │
│  83 /0 ib      ADD r/m, imm8       Sign-extended imm8                 │
│  00 /r         ADD r/m8, r8        r/m ← r/m + reg                    │
│  01 /r         ADD r/m, r          r/m ← r/m + reg                    │
│  02 /r         ADD r8, r/m8        reg ← reg + r/m                    │
│  03 /r         ADD r, r/m          reg ← reg + r/m                    │
│                                                                         │
│  Flags: OF, SF, ZF, AF, PF, CF set according to result               │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                    SUB — Subtract                                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Same forms as ADD, but opcodes:                                       │
│  2C/2D (accum), 80/5, 81/5, 83/5, 28-2B (/r forms)                    │
│                                                                         │
│  Operation: dest ← dest - src                                         │
│  Flags: Same as ADD                                                    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### ADC and SBB — With Carry/Borrow

```
ADC (Add with Carry):
  dest ← dest + src + CF

SBB (Subtract with Borrow):
  dest ← dest - src - CF

Use case: Multi-precision arithmetic
  ; Add 128-bit: RDX:RAX + RCX:RBX → RDX:RAX
  add rax, rbx      ; Low 64 bits, sets CF
  adc rdx, rcx      ; High 64 bits, includes carry
```

---

## 2. INC and DEC

### Increment and Decrement

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    INC and DEC                                         │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  INC r/m8       FE /0                                                  │
│  INC r/m16/32/64   FF /0                                              │
│  DEC r/m8       FE /1                                                  │
│  DEC r/m16/32/64   FF /1                                              │
│                                                                         │
│  CRITICAL: INC/DEC do NOT affect CF!                                  │
│  ────────────────────────────────────────────────────────────────────  │
│  Flags affected: OF, SF, ZF, AF, PF                                   │
│  Flags NOT affected: CF                                               │
│                                                                         │
│  Why? To allow use in loops with multi-precision arithmetic           │
│                                                                         │
│  Note: In legacy 32-bit mode, 40-4F were INC/DEC r32                 │
│        In 64-bit mode, 40-4F are REX prefixes                        │
│        Must use FF /0 and FF /1 forms                                 │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 3. MUL and IMUL

### Unsigned and Signed Multiplication

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    MUL — Unsigned Multiply                             │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode    Instruction    Operation                                    │
│  F6 /4     MUL r/m8       AX ← AL × r/m8                             │
│  F7 /4     MUL r/m16      DX:AX ← AX × r/m16                        │
│  F7 /4     MUL r/m32      EDX:EAX ← EAX × r/m32                     │
│  F7 /4     MUL r/m64      RDX:RAX ← RAX × r/m64                     │
│                                                                         │
│  Flags: CF, OF set if upper half non-zero; SF, ZF, AF, PF undefined  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                    IMUL — Signed Multiply                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  One-operand (like MUL):                                              │
│  F6 /5     IMUL r/m8      AX ← AL × r/m8 (signed)                   │
│  F7 /5     IMUL r/m       DX:AX/EDX:EAX/RDX:RAX ← signed product    │
│                                                                         │
│  Two-operand:                                                          │
│  0F AF /r  IMUL r, r/m    r ← r × r/m (truncated to operand size)   │
│                                                                         │
│  Three-operand:                                                        │
│  6B /r ib  IMUL r, r/m, imm8   r ← r/m × sign-ext(imm8)            │
│  69 /r id  IMUL r, r/m, imm32  r ← r/m × sign-ext(imm32)           │
│                                                                         │
│  Two/three operand forms: result truncated to register size          │
│  Flags: CF, OF set if result truncated; SF, ZF, AF, PF undefined    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### IMUL Practical Forms

```asm
; One-operand (full precision)
imul rbx            ; RDX:RAX = RAX × RBX (signed)

; Two-operand (truncated)
imul rax, rbx       ; RAX = RAX × RBX (low 64 bits only)
imul rax, [mem]     ; RAX = RAX × [mem]

; Three-operand (truncated)
imul rax, rbx, 10   ; RAX = RBX × 10
imul rax, [mem], 5  ; RAX = [mem] × 5
```

---

## 4. DIV and IDIV

### Division Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    DIV — Unsigned Divide                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Opcode    Instruction    Operation                                    │
│  F6 /6     DIV r/m8       AL ← AX ÷ r/m8, AH ← remainder            │
│  F7 /6     DIV r/m16      AX ← DX:AX ÷ r/m16, DX ← remainder       │
│  F7 /6     DIV r/m32      EAX ← EDX:EAX ÷ r/m32, EDX ← remainder   │
│  F7 /6     DIV r/m64      RAX ← RDX:RAX ÷ r/m64, RDX ← remainder   │
│                                                                         │
│  Flags: CF, OF, SF, ZF, AF, PF all undefined                         │
│                                                                         │
│  Exceptions:                                                           │
│  #DE (Divide Error) if:                                               │
│    - Divisor is zero                                                  │
│    - Quotient too large for destination                               │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                    IDIV — Signed Divide                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Same forms as DIV, but F6/7 /7                                       │
│  Uses signed dividend and divisor                                      │
│                                                                         │
│  Before IDIV, extend dividend:                                        │
│    CDQ  — sign-extend EAX into EDX:EAX                               │
│    CQO  — sign-extend RAX into RDX:RAX                               │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Division Setup Pattern

```asm
; Unsigned 64-bit divide
xor edx, edx        ; Zero-extend RAX to RDX:RAX
mov rax, dividend
div divisor         ; RAX = quotient, RDX = remainder

; Signed 64-bit divide
mov rax, dividend
cqo                 ; Sign-extend RAX to RDX:RAX
idiv divisor        ; RAX = quotient, RDX = remainder
```

---

## 5. NEG and CMP

### Negation and Comparison

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    NEG — Two's Complement Negate                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  F6 /3     NEG r/m8      r/m8 ← -r/m8                                │
│  F7 /3     NEG r/m       r/m ← -r/m                                  │
│                                                                         │
│  Operation: dest ← 0 - dest                                           │
│  Flags: CF set unless operand is 0; OF, SF, ZF, AF, PF set normally  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                    CMP — Compare                                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Same forms as SUB (3C/3D, 80/7, 81/7, 83/7, 38-3B)                   │
│                                                                         │
│  Operation: computes dest - src, sets flags, discards result          │
│  Flags: Same as SUB                                                    │
│                                                                         │
│  Use: Sets flags for subsequent conditional jump/move                 │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Flag Behavior Summary

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Arithmetic Flags Summary                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Instruction  │ CF │ OF │ SF │ ZF │ AF │ PF │                         │
│  ─────────────┼────┼────┼────┼────┼────┼────┤                         │
│  ADD/SUB      │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │                         │
│  ADC/SBB      │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │                         │
│  INC/DEC      │ -  │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ CF unchanged!          │
│  NEG          │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │                         │
│  CMP          │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ ✓  │ result discarded       │
│  MUL/IMUL     │ ✓* │ ✓* │ ?  │ ?  │ ?  │ ?  │ *set if overflow       │
│  DIV/IDIV     │ ?  │ ?  │ ?  │ ?  │ ?  │ ?  │ all undefined          │
│                                                                         │
│  ✓ = set according to result                                          │
│  - = unchanged                                                         │
│  ? = undefined                                                         │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 7. MULX, ADCX, ADOX — Modern Extensions

### Multi-Precision Support (BMI2, ADX)

```
MULX r1, r2, r/m (BMI2):
  r1:r2 ← RDX × r/m
  - Does NOT affect flags
  - Allows parallel computation

ADCX r, r/m (ADX):
  r ← r + r/m + CF
  - Only affects CF

ADOX r, r/m (ADX):
  r ← r + r/m + OF
  - Only affects OF

Use: Parallel carry chains for big integer arithmetic
```

---

# Key Readings

| Section | Topic | Time |
|---------|-------|------|
| ADD/SUB/ADC/SBB | Basic arithmetic | 2 hours |
| MUL/IMUL | Multiplication forms | 2 hours |
| DIV/IDIV | Division | 1.5 hours |
| INC/DEC/NEG/CMP | Support operations | 1.5 hours |
| MULX/ADCX/ADOX | Modern extensions | 1 hour |

---

# Exercises

## Exercise 1: Multi-Precision Addition

Implement 128-bit addition: `R9:R8 + R11:R10 → R9:R8`

## Exercise 2: Multiplication Forms

Write code showing all three IMUL forms producing same result.

## Exercise 3: Division Edge Cases

Test and document behavior of:
1. Division by zero
2. Quotient overflow
3. Negative divisor handling

## Mini-Project: Big Integer Calculator

Implement 256-bit addition and multiplication.

---

# Quiz

**Q1**: Why doesn't INC/DEC modify the carry flag?

**Q2**: What's the difference between one-operand and two-operand IMUL?

**Q3**: What instruction must precede IDIV r64?

**Q4**: What exception does DIV raise for overflow?

**Q5**: What flags does MULX affect?

---

**Next Week**: Logical and Bit Manipulation—AND, OR, XOR, shifts, rotates, BMI.

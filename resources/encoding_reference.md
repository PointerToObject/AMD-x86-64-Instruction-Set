# x86-64 Encoding Quick Reference

A condensed reference for instruction encoding. For full details, see AMD64 Volume 3, Chapter 1.

---

## Instruction Format

```
┌─────────────┬─────────┬─────────┬────────┬─────┬──────────┬───────────┐
│   Legacy    │   REX   │ Opcode  │ ModRM  │ SIB │ Displace │ Immediate │
│  Prefixes   │ Prefix  │  1-3B   │  0-1B  │0-1B │  0/1/2/4 │ 0/1/2/4/8 │
│   0-4 B     │  0-1B   │         │        │     │   bytes  │   bytes   │
└─────────────┴─────────┴─────────┴────────┴─────┴──────────┴───────────┘
Maximum length: 15 bytes
```

---

## Legacy Prefixes

| Group | Byte | Name | Effect |
|-------|------|------|--------|
| 1 | `F0` | LOCK | Atomic memory operation |
| 1 | `F2` | REPNE | Repeat while not equal |
| 1 | `F3` | REP/REPE | Repeat / Repeat while equal |
| 2 | `2E` | CS | CS segment override |
| 2 | `36` | SS | SS segment override |
| 2 | `3E` | DS | DS segment override |
| 2 | `26` | ES | ES segment override |
| 2 | `64` | FS | FS segment override |
| 2 | `65` | GS | GS segment override |
| 3 | `66` | Operand-size | Toggle 16/32-bit operand |
| 4 | `67` | Address-size | Toggle address size |

---

## REX Prefix (40h-4Fh)

```
  7   6   5   4   3   2   1   0
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 0 │ 0 │ W │ R │ X │ B │
└───┴───┴───┴───┴───┴───┴───┴───┘
```

| Bit | Name | Effect |
|-----|------|--------|
| W | REX.W | 64-bit operand size |
| R | REX.R | Extends ModRM.reg to 4 bits |
| X | REX.X | Extends SIB.index to 4 bits |
| B | REX.B | Extends ModRM.r/m or SIB.base |

**Common Values**:
- `40` = Plain REX (for SPL, BPL, SIL, DIL access)
- `48` = REX.W (64-bit operand)
- `41` = REX.B (R8-R15 in r/m)
- `44` = REX.R (R8-R15 in reg)
- `4C` = REX.WR (64-bit + extended reg)

---

## ModRM Byte

```
  7   6   5   4   3   2   1   0
┌───────┬───────────┬───────────┐
│  mod  │    reg    │    r/m    │
│ 2 bits│  3 bits   │  3 bits   │
└───────┴───────────┴───────────┘
```

### mod Field

| mod | Meaning |
|-----|---------|
| 00 | [r/m], no displacement (special: r/m=101 → disp32/RIP+disp32) |
| 01 | [r/m + disp8] |
| 10 | [r/m + disp32] |
| 11 | r/m is register (no memory) |

### Register Encoding (reg and r/m fields)

| Value | 64-bit | 32-bit | 16-bit | 8-bit | 8-bit (REX) |
|-------|--------|--------|--------|-------|-------------|
| 000 | RAX | EAX | AX | AL | AL |
| 001 | RCX | ECX | CX | CL | CL |
| 010 | RDX | EDX | DX | DL | DL |
| 011 | RBX | EBX | BX | BL | BL |
| 100 | RSP | ESP | SP | AH | SPL |
| 101 | RBP | EBP | BP | CH | BPL |
| 110 | RSI | ESI | SI | DH | SIL |
| 111 | RDI | EDI | DI | BH | DIL |

With REX.R/REX.B = 1:

| Value | 64-bit | 32-bit |
|-------|--------|--------|
| 000 | R8 | R8D |
| 001 | R9 | R9D |
| ... | ... | ... |
| 111 | R15 | R15D |

---

## SIB Byte

```
  7   6   5   4   3   2   1   0
┌───────┬───────────┬───────────┐
│ scale │   index   │   base    │
│ 2 bits│  3 bits   │  3 bits   │
└───────┴───────────┴───────────┘
```

**Address = base + (index × scale) + displacement**

### scale Field

| scale | Multiplier |
|-------|------------|
| 00 | ×1 |
| 01 | ×2 |
| 10 | ×4 |
| 11 | ×8 |

### Special Cases

- index = 100: No index (index × scale = 0)
- base = 101 with mod=00: No base, disp32 only
- base = 101 with mod≠00: RBP/R13 as base

---

## Common Instruction Encodings

### MOV

| Encoding | Instruction |
|----------|-------------|
| `89 /r` | MOV r/m, reg |
| `8B /r` | MOV reg, r/m |
| `B8+rd id` | MOV r32, imm32 |
| `48 B8+rd iq` | MOV r64, imm64 |
| `C7 /0 id` | MOV r/m32, imm32 |

### ADD/SUB/AND/OR/XOR/CMP

| Opcode | Operation |
|--------|-----------|
| `00-05` | ADD |
| `08-0D` | OR |
| `10-15` | ADC |
| `18-1D` | SBB |
| `20-25` | AND |
| `28-2D` | SUB |
| `30-35` | XOR |
| `38-3D` | CMP |

Pattern: `X0-X3` = r/m,r / r,r/m; `X4-X5` = AL/rAX, imm

### JMP/CALL

| Encoding | Instruction |
|----------|-------------|
| `EB cb` | JMP rel8 |
| `E9 cd` | JMP rel32 |
| `FF /4` | JMP r/m64 |
| `E8 cd` | CALL rel32 |
| `FF /2` | CALL r/m64 |

### Jcc (Conditional Jump)

| Short | Near | Condition |
|-------|------|-----------|
| 70/71 | 0F 80/81 | O/NO |
| 72/73 | 0F 82/83 | B/AE (C/NC) |
| 74/75 | 0F 84/85 | E/NE (Z/NZ) |
| 76/77 | 0F 86/87 | BE/A |
| 78/79 | 0F 88/89 | S/NS |
| 7A/7B | 0F 8A/8B | P/NP |
| 7C/7D | 0F 8C/8D | L/GE |
| 7E/7F | 0F 8E/8F | LE/G |

---

## Opcode Notation (Volume 3)

| Notation | Meaning |
|----------|---------|
| `/r` | ModRM byte, reg field is register operand |
| `/0-/7` | ModRM byte, reg field is opcode extension |
| `ib` | Immediate byte |
| `iw` | Immediate word |
| `id` | Immediate dword |
| `io` | Immediate qword (rare) |
| `cb/cw/cd` | Relative branch offset |
| `+rb/+rw/+rd` | Register encoded in opcode low bits |

---

## Quick Encoding Examples

### `MOV RAX, RBX`
```
48 89 D8
│  │  └── ModRM: mod=11, reg=011(RBX), r/m=000(RAX)
│  └───── Opcode: MOV r/m, r
└──────── REX.W: 64-bit operand
```

### `ADD QWORD PTR [RCX+RDX*8+0x10], 5`
```
48 83 44 D1 10 05
│  │  │  │  │  └── imm8: 5
│  │  │  │  └───── disp8: 0x10
│  │  │  └──────── SIB: scale=11(×8), index=010(RDX), base=001(RCX)
│  │  └─────────── ModRM: mod=01, reg=000(/0), r/m=100(SIB)
│  └────────────── Opcode: ADD r/m, imm8 (sign-extended)
└───────────────── REX.W: 64-bit operand
```

### `JMP [RAX+RBX*4]`
```
FF 24 98
│  │  └── SIB: scale=10(×4), index=011(RBX), base=000(RAX)
│  └───── ModRM: mod=00, reg=100(/4), r/m=100(SIB)
└──────── Opcode: JMP r/m
```

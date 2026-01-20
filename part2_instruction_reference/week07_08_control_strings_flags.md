# AMD64 Instruction Set Mastery — Week 6
## Control Flow Instructions

**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 3  

---

# Lecture Notes

## 1. Unconditional Jump

```
JMP — Jump
────────────────────────────────────────────────────────────────────────
EB cb       JMP rel8        Short jump (-128 to +127)
E9 cd       JMP rel32       Near jump (±2GB in 64-bit mode)
FF /4       JMP r/m64       Indirect jump through register/memory

In 64-bit mode: Default is RIP-relative addressing
Far jumps (EA) invalid in 64-bit mode; use FF /5 for indirect far
```

## 2. Conditional Jumps (Jcc)

```
┌─────────────────────────────────────────────────────────────────────────┐
│  Condition      Short  Near    Test                                    │
├─────────────────────────────────────────────────────────────────────────┤
│  JO/JNO         70/71  0F80/81 Overflow                               │
│  JB/JC/JNAE     72     0F82    CF=1 (unsigned below)                  │
│  JAE/JNB/JNC    73     0F83    CF=0 (unsigned above/equal)            │
│  JE/JZ          74     0F84    ZF=1 (equal/zero)                      │
│  JNE/JNZ        75     0F85    ZF=0 (not equal)                       │
│  JBE/JNA        76     0F86    CF=1 OR ZF=1                           │
│  JA/JNBE        77     0F87    CF=0 AND ZF=0                          │
│  JS/JNS         78/79  0F88/89 Sign flag                              │
│  JP/JNP         7A/7B  0F8A/8B Parity                                 │
│  JL/JNGE        7C     0F8C    SF≠OF (signed less)                   │
│  JGE/JNL        7D     0F8D    SF=OF (signed greater/equal)          │
│  JLE/JNG        7E     0F8E    ZF=1 OR SF≠OF                         │
│  JG/JNLE        7F     0F8F    ZF=0 AND SF=OF                        │
└─────────────────────────────────────────────────────────────────────────┘
```

## 3. CALL and RET

```
CALL — Call Procedure
────────────────────────────────────────────────────────────────────────
E8 cd       CALL rel32      Push RIP, jump relative
FF /2       CALL r/m64      Push RIP, jump indirect
FF /3       CALL m16:64     Far call (indirect)

RET — Return
────────────────────────────────────────────────────────────────────────
C3          RET             Pop RIP
C2 iw       RET imm16       Pop RIP, RSP += imm16
CB/CA       RETF            Far return
```

## 4. SETcc and CMOVcc

```
SETcc r/m8 (0F 9X /0):  Set byte to 1 if condition, 0 otherwise
CMOVcc r, r/m (0F 4X /r): Move if condition true

Both use same condition codes as Jcc
```

## 5. INT, INTO, IRET

```
INT n (CD ib):    Software interrupt, vector n
INT3 (CC):        Breakpoint (single byte!)
INTO (CE):        Interrupt on overflow (invalid in 64-bit)
IRET (CF):        Return from interrupt

INT 3 vs INT 3:
  CC = one byte (debugger breakpoint)
  CD 03 = two bytes (same effect, but different encoding)
```

---

# AMD64 Instruction Set Mastery — Week 7
## String and Block Instructions

---

# Lecture Notes

## 1. String Primitives

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    String Instructions                                 │
├─────────────────────────────────────────────────────────────────────────┤
│  Registers: RSI (source), RDI (dest), RCX (count), DF (direction)    │
│                                                                         │
│  MOVS — Move String                                                    │
│  A4    MOVSB    Move byte [RSI] → [RDI]                              │
│  A5    MOVSW/D/Q Move word/dword/qword                                │
│                                                                         │
│  CMPS — Compare Strings                                               │
│  A6    CMPSB    Compare [RSI] with [RDI], set flags                  │
│  A7    CMPSW/D/Q                                                      │
│                                                                         │
│  SCAS — Scan String                                                   │
│  AE    SCASB    Compare AL with [RDI]                                │
│  AF    SCASW/D/Q Compare AX/EAX/RAX with [RDI]                       │
│                                                                         │
│  LODS — Load String                                                   │
│  AC    LODSB    AL ← [RSI]                                           │
│  AD    LODSW/D/Q                                                      │
│                                                                         │
│  STOS — Store String                                                  │
│  AA    STOSB    [RDI] ← AL                                           │
│  AB    STOSW/D/Q                                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. REP Prefixes

```
F3    REP       Repeat RCX times (MOVS, STOS, LODS)
F3    REPE      Repeat while equal (CMPS, SCAS)
F2    REPNE     Repeat while not equal (CMPS, SCAS)

Common patterns:
  REP MOVSB     — memcpy(rdi, rsi, rcx)
  REP STOSB     — memset(rdi, al, rcx)
  REPE CMPSB    — memcmp, stop on mismatch
  REPNE SCASB   — strchr, find byte in string
```

---

# AMD64 Instruction Set Mastery — Week 8
## Flag and Processor Control

---

# Lecture Notes

## 1. Flag Instructions

```
Carry: CLC (F8), STC (F9), CMC (F5)
Direction: CLD (FC), STD (FD)
Interrupt: CLI (FA), STI (FB) — Ring 0

LAHF (9F): AH ← flags (SF:ZF:0:AF:0:PF:1:CF)
SAHF (9E): flags ← AH
PUSHF/POPF (9C/9D): Push/pop RFLAGS
```

## 2. Processor Control

```
NOP (90): No operation
PAUSE (F3 90): Spin-loop hint
HLT (F4): Halt — Ring 0
CPUID (0F A2): CPU identification
RDTSC (0F 31): Read timestamp counter
RDTSCP (0F 01 F9): Read TSC + processor ID
```

## 3. Memory Barriers

```
MFENCE (0F AE F0): Full fence
SFENCE (0F AE F8): Store fence  
LFENCE (0F AE E8): Load fence
LOCK prefix (F0): Atomic operation
```

---

# AMD64 Instruction Set Mastery — Week 9
## System Instructions Part 1: Segments and Descriptors

**Primary Text**: AMD64 Architecture Programmer's Manual, Volume 3, Chapter 4  

---

# Lecture Notes

## 1. Descriptor Table Instructions

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Descriptor Table Management                         │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  LGDT m16&64 (0F 01 /2)   Load Global Descriptor Table Register       │
│  SGDT m (0F 01 /0)        Store GDTR                                  │
│  LLDT r/m16 (0F 00 /2)    Load Local Descriptor Table Register        │
│  SLDT r/m (0F 00 /0)      Store LDTR                                  │
│  LTR r/m16 (0F 00 /3)     Load Task Register                          │
│  STR r/m (0F 00 /1)       Store Task Register                         │
│  LIDT m16&64 (0F 01 /3)   Load Interrupt Descriptor Table Register    │
│  SIDT m (0F 01 /1)        Store IDTR                                  │
│                                                                         │
│  All Load instructions: Ring 0 only                                   │
│  All Store instructions: Any privilege level                          │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 2. Segment Access Instructions

```
LAR r, r/m16 (0F 02 /r): Load Access Rights
  - Returns access rights from segment descriptor
  
LSL r, r/m16 (0F 03 /r): Load Segment Limit
  - Returns segment limit

VERR r/m16 (0F 00 /4): Verify Readable
VERW r/m16 (0F 00 /5): Verify Writable
  - ZF=1 if segment is accessible with requested permission
```

## 3. Segment Load/Store

```
MOV Sreg, r/m16 (8E /r): Load segment register
MOV r/m16, Sreg (8C /r): Store segment register

In 64-bit mode:
  - CS, DS, ES, SS loads mostly ignored (flat model)
  - FS, GS base loaded from descriptor
  - Use WRFSBASE/WRGSBASE for direct base access (if enabled)
```

---

# AMD64 Instruction Set Mastery — Week 10
## System Instructions Part 2: Control and Virtualization

---

# Lecture Notes

## 1. Control Register Access

```
MOV CRn, r64 (0F 22 /r): Load control register
MOV r64, CRn (0F 20 /r): Read control register

CR0: System control (PE, PG, WP, etc.)
CR2: Page fault linear address
CR3: Page table base
CR4: Feature enables (PSE, PAE, SMEP, SMAP, etc.)
CR8: Task priority (APIC)

LMSW r/m16 (0F 01 /6): Load Machine Status Word (CR0[15:0])
SMSW r/m (0F 01 /4): Store Machine Status Word
CLTS (0F 06): Clear Task-Switched flag in CR0
```

## 2. Debug Registers

```
MOV DRn, r64 (0F 23 /r): Load debug register
MOV r64, DRn (0F 21 /r): Read debug register

DR0-DR3: Breakpoint addresses
DR6: Debug status
DR7: Debug control
```

## 3. MSR Access

```
RDMSR (0F 32): ECX = MSR address, result in EDX:EAX
WRMSR (0F 30): ECX = MSR address, value from EDX:EAX

Ring 0 only. #GP if invalid MSR or privilege violation.
```

## 4. Cache and TLB

```
INVD (0F 08):    Invalidate cache (no writeback!) — dangerous
WBINVD (0F 09):  Write back and invalidate cache
WBNOINVD (F3 0F 09): Write back, don't invalidate
INVLPG m (0F 01 /7): Invalidate TLB entry for address
INVPCID (66 0F 38 82 /r): Invalidate by PCID
```

## 5. Virtualization (SVM)

```
VMRUN (0F 01 D8):   Run virtual machine
VMLOAD (0F 01 DA):  Load state from VMCB
VMSAVE (0F 01 DB):  Save state to VMCB
VMMCALL (0F 01 D9): Call hypervisor
CLGI (0F 01 DD):    Clear global interrupt flag
STGI (0F 01 DC):    Set global interrupt flag
SKINIT (0F 01 DE):  Secure init and jump
INVLPGA (0F 01 DF): Invalidate TLB entry (with ASID)
```

---

# AMD64 Instruction Set Mastery — Week 11
## Advanced and Modern Instructions

---

# Lecture Notes

## 1. CET — Control-flow Enforcement

```
Shadow Stack:
INCSSPQ r64 (F3 0F AE /5): Increment shadow stack pointer
RDSSPQ r64 (F3 0F 1E /1): Read shadow stack pointer
SAVEPREVSSP (F3 0F 01 EA): Save previous SSP
RSTORSSP m64 (F3 0F 01 /5): Restore SSP
WRSSD/Q (0F 38 F6): Write to shadow stack

ENDBR32/ENDBR64 (F3 0F 1E): End branch marker
```

## 2. Memory Protection Keys

```
RDPKRU (0F 01 EE): Read PKRU register
WRPKRU (0F 01 EF): Write PKRU register

Allows fast per-page permission changes without modifying page tables.
```

## 3. Transactional Memory (TSX)

```
XBEGIN rel (C7 F8): Begin transaction
XEND (0F 01 D5): End transaction
XABORT imm8 (C6 F8): Abort transaction
XTEST (0F 01 D6): Test if in transaction
```

## 4. SEV Instructions

```
PVALIDATE (F2 0F 01 FF): Validate page
RMPADJUST (F3 0F 01 FE): Adjust RMP entry
RMPUPDATE (F2 0F 01 FE): Update RMP
PSMASH (F3 0F 01 FF): Destroy 2MB page entry
```

---

# AMD64 Instruction Set Mastery — Week 12
## Capstone Project: Build an Instruction Decoder

---

# Project Overview

Build a complete x86-64 instruction decoder that:
1. Parses instruction bytes
2. Identifies prefixes, opcode, ModRM, SIB
3. Determines instruction mnemonic
4. Formats operands
5. Outputs assembly syntax

## Requirements

```
Input: Byte stream
Output: Disassembled instructions

Must handle:
- All legacy prefixes
- REX prefix
- 1, 2, and 3-byte opcodes
- ModRM addressing modes
- SIB byte
- Displacement
- Immediate

Nice to have:
- VEX prefix decoding
- EVEX prefix decoding
```

## Suggested Implementation

```c
struct instruction {
    uint8_t prefixes[4];
    uint8_t rex;
    uint8_t opcode[3];
    uint8_t modrm;
    uint8_t sib;
    int64_t displacement;
    int64_t immediate;
    int length;
    char mnemonic[16];
    char operands[64];
};

int decode_instruction(const uint8_t *bytes, struct instruction *insn);
void format_instruction(struct instruction *insn, char *output);
```

## Testing

Verify against:
1. objdump output
2. Known instruction encodings from course
3. Edge cases (15-byte instruction, etc.)

---

# Final Exam Topics

1. Encode/decode any GP instruction by hand
2. Identify prefix combinations and their effects
3. Explain ModRM/SIB addressing modes
4. Describe flag effects for arithmetic/logical operations
5. Recognize system instruction privilege requirements
6. Apply CPUID feature detection

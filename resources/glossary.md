# AMD64 Architecture Glossary

Quick reference for terms used throughout the course and AMD manuals.

---

## A

**APIC** (Advanced Programmable Interrupt Controller): Hardware component for managing interrupts in multiprocessor systems. Local APIC is per-core; I/O APIC handles external interrupts.

**AVX** (Advanced Vector Extensions): SIMD instruction set using 256-bit YMM registers. AVX-512 extends to 512-bit ZMM registers.

---

## B

**BMI** (Bit Manipulation Instructions): Instruction extensions (BMI1, BMI2) for efficient bit operations like ANDN, BEXTR, PDEP, PEXT.

---

## C

**Canonical Address**: In 64-bit mode, a valid virtual address where bits 63:48 are copies of bit 47. Non-canonical addresses cause #GP.

**CET** (Control-flow Enforcement Technology): Security feature with shadow stacks (protect return addresses) and indirect branch tracking (IBT).

**CPL** (Current Privilege Level): The privilege level (0-3) of currently executing code, stored in CS[1:0].

**CR0-CR4** (Control Registers): System registers controlling CPU features like paging (CR0.PG), write protection (CR0.WP), and page table base (CR3).

---

## D

**Descriptor**: A structure in GDT/LDT/IDT that describes segments, gates, or interrupt handlers. 8 bytes in legacy mode, 16 bytes for some 64-bit entries.

**DPL** (Descriptor Privilege Level): The privilege level stored in a descriptor, determining who can access that segment or gate.

---

## E

**EFER** (Extended Feature Enable Register): MSR controlling long mode (LME/LMA), syscall (SCE), and NX bit (NXE).

**Exception**: A synchronous event caused by instruction execution (e.g., #GP, #PF, #UD). Contrast with interrupt (asynchronous).

---

## F

**Fault**: An exception that reports the failing instruction's address; can potentially be restarted after handling.

**Far Pointer**: A pointer containing both segment selector and offset. Format: `segment:offset`.

---

## G

**GDT** (Global Descriptor Table): System table containing segment descriptors. Located by GDTR register.

**GPR** (General-Purpose Register): RAX, RBX, RCX, RDX, RSI, RDI, RBP, RSP, R8-R15.

---

## H

**Huge Page**: Large page size (2MB or 1GB) instead of standard 4KB. Reduces TLB pressure for large allocations.

---

## I

**IBS** (Instruction-Based Sampling): AMD performance monitoring feature providing precise instruction attribution.

**IDT** (Interrupt Descriptor Table): Table of gate descriptors for interrupt/exception handlers. Located by IDTR.

**IOPL** (I/O Privilege Level): RFLAGS[13:12], controls which privilege levels can execute I/O instructions.

**IST** (Interrupt Stack Table): Mechanism in 64-bit TSS for dedicated interrupt stacks.

---

## L

**LAM** (Linear Address Masking): Feature allowing software to use upper address bits for metadata.

**LDT** (Local Descriptor Table): Per-process descriptor table (rarely used in modern systems).

**Long Mode**: AMD64's 64-bit operating mode. Has two sub-modes: 64-bit mode and compatibility mode.

---

## M

**ModRM**: Byte following opcode that specifies operands (addressing mode, registers, memory).

**MSR** (Model-Specific Register): Configuration registers accessed via RDMSR/WRMSR.

**MTRR** (Memory Type Range Registers): MSRs defining memory type (WB, UC, etc.) for physical address ranges.

---

## N

**NPT** (Nested Page Tables): AMD-V feature providing hardware-assisted guest physical to host physical translation.

**NX** (No-Execute): Page table bit preventing code execution from a page. Also called XD (Execute Disable).

---

## P

**PAT** (Page Attribute Table): Mechanism for specifying memory types via page table entries.

**PCID** (Process-Context Identifier): Tag for TLB entries allowing efficient context switches without full TLB flush.

**PML4** (Page Map Level 4): Top level of 4-level page table hierarchy. CR3 points to PML4 table.

**PMU** (Performance Monitoring Unit): Hardware for counting CPU events (cache misses, branches, etc.).

---

## R

**REX Prefix**: Byte (40h-4Fh) enabling 64-bit operand size and access to R8-R15 registers.

**RFLAGS**: 64-bit flags register containing status flags (CF, ZF, SF, OF) and system flags (IF, IOPL).

**Ring**: Privilege level. Ring 0 = most privileged (kernel), Ring 3 = least privileged (user).

**RIP-Relative**: Addressing mode where effective address is RIP + signed displacement. Default in 64-bit mode.

**RPL** (Requested Privilege Level): Privilege level in bits [1:0] of a segment selector.

---

## S

**SEV** (Secure Encrypted Virtualization): AMD feature encrypting guest VM memory with per-VM keys.

**SIB** (Scale-Index-Base): Byte specifying complex addressing: `[base + index*scale + displacement]`.

**SMAP** (Supervisor Mode Access Prevention): Prevents kernel from reading/writing user pages without explicit permission.

**SMEP** (Supervisor Mode Execution Prevention): Prevents kernel from executing code in user pages.

**SMM** (System Management Mode): Special CPU mode for firmware/BIOS functions, entered via SMI.

**SSE** (Streaming SIMD Extensions): SIMD instruction set using 128-bit XMM registers.

**SVM** (Secure Virtual Machine): AMD's hardware virtualization technology (AMD-V).

---

## T

**TLB** (Translation Lookaside Buffer): Cache for virtual-to-physical address translations.

**Trap**: An exception that reports the next instruction's address (e.g., breakpoint). The faulting instruction completed.

**TSC** (Time Stamp Counter): 64-bit counter incremented every cycle. Read via RDTSC/RDTSCP.

**TSS** (Task State Segment): Structure containing stack pointers for privilege level changes (RSP0-2) and IST pointers.

---

## V

**VEX Prefix**: 2 or 3-byte prefix for AVX instructions, replacing REX and enabling 3-operand forms.

**VMCB** (Virtual Machine Control Block): AMD-V structure containing guest state and control information.

**VMRUN**: Instruction that enters a virtual machine, using VMCB at address in RAX.

---

## X

**XCR0**: Extended control register defining which XSAVE components are enabled.

**XSAVE/XRSTOR**: Instructions for saving/restoring extended processor state (FPU, SSE, AVX, etc.).

---

## Numbers

**#DE**: Divide Error exception (vector 0).

**#DB**: Debug exception (vector 1).

**#GP**: General Protection exception (vector 13).

**#PF**: Page Fault exception (vector 14).

**#UD**: Invalid Opcode exception (vector 6).

---

## See Also

- AMD64 Architecture Programmer's Manual, Volume 1, Appendix A (Definitions)
- AMD64 Architecture Programmer's Manual, Volume 2, Appendix A (MSRs)
- AMD64 Architecture Programmer's Manual, Volume 3, Preface (Notation)

<p align="center">
  <img src="https://img.shields.io/badge/AMD64-Architecture%20Mastery-ED1C24?style=for-the-badge&logo=amd&logoColor=white" alt="AMD64 Architecture Mastery"/>
</p>

<h1 align="center">AMD64 Architecture Mastery</h1>

<p align="center">
  <strong>The Definitive University-Level Curriculum for x86-64 Systems Programming</strong>
</p>

<p align="center">
  <em>Based on AMD64 Architecture Programmer's Manual, Volumes 1-3</em>
</p>

<p align="center">
  <a href="#-quick-start"><img src="https://img.shields.io/badge/Quick%20Start-5%20min-success?style=flat-square" alt="Quick Start"/></a>
  <a href="#-course-structure"><img src="https://img.shields.io/badge/Duration-16%20Weeks-blue?style=flat-square" alt="Duration"/></a>
  <a href="#-prerequisites"><img src="https://img.shields.io/badge/Level-Advanced-red?style=flat-square" alt="Level"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="License"/></a>
</p>

<p align="center">
  <a href="#-about">About</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-course-structure">Structure</a> •
  <a href="#-documentation">Docs</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

<br/>

<p align="center">
  <img src="https://img.shields.io/badge/Volume%201-Application%20Programming-353535?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Volume%202-System%20Programming-ED1C24?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Volume%203-Instruction%20Reference-76B900?style=for-the-badge"/>
</p>

<br/>

## 📋 Table of Contents

<details>
<summary><strong>Click to expand full navigation</strong></summary>

- [About](#-about)
  - [Overview](#overview)
  - [Key Features](#key-features)
  - [Who Is This For](#who-is-this-for)
- [Quick Start](#-quick-start)
  - [Prerequisites Check](#prerequisites-check)
  - [Installation](#installation)
  - [First Steps](#first-steps)
- [Course Structure](#-course-structure)
  - [Architecture Overview](#architecture-overview)
  - [Part 1: Foundations](#part-1-foundations-weeks-1-3)
  - [Part 2: Instruction Reference](#part-2-instruction-reference-weeks-4-8)
  - [Part 3: System Programming](#part-3-system-programming-weeks-9-13)
  - [Part 4: Advanced Topics](#part-4-advanced-topics-weeks-14-16)
- [Documentation](#-documentation)
  - [Weekly Modules](#weekly-modules)
  - [Reference Materials](#reference-materials)
  - [AMD Manual Mapping](#amd-manual-mapping)
- [Learning Path](#-learning-path)
  - [Time Investment](#time-investment)
  - [Recommended Schedule](#recommended-schedule)
  - [Assessment Strategy](#assessment-strategy)
- [Development Environment](#-development-environment)
  - [System Requirements](#system-requirements)
  - [Toolchain Setup](#toolchain-setup)
  - [Verification](#verification)
- [Learning Outcomes](#-learning-outcomes)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgments](#-acknowledgments)

</details>

---

## 📖 About

### Overview

**AMD64 Architecture Mastery** is a comprehensive, university-level curriculum designed to provide complete understanding of the x86-64 processor architecture. Developed using AMD's official Architecture Programmer's Manuals as the authoritative source, this course bridges the gap between academic theory and professional systems programming practice.

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                                                                 │
│    "The objective is not to memorize instructions, but to develop the          │
│     ability to read, interpret, and apply authoritative technical              │
│     documentation—the foundational skill of every systems programmer."         │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

This curriculum represents **300-400 hours** of structured learning, equivalent to a two-semester university sequence in computer architecture and systems programming.

### Key Features

<table>
<tr>
<td width="50%">

**📚 Authoritative Source Material**
- Based exclusively on AMD64 Architecture Programmer's Manual
- Direct citations to manual sections
- No secondhand interpretations

**🎯 Comprehensive Coverage**
- All general-purpose instructions
- Complete system programming model
- Modern security features

</td>
<td width="50%">

**💻 Hands-On Learning**
- 100+ practical exercises
- 16 weekly mini-projects
- Capstone: Build an instruction decoder

**📊 Professional Quality**
- University-level rigor
- Industry-relevant skills
- Interview preparation ready

</td>
</tr>
</table>

### Who Is This For

| Audience | Benefit |
|----------|---------|
| **Computer Science Students** | Deep understanding of processor architecture beyond typical coursework |
| **Systems Programmers** | Authoritative reference for low-level development |
| **Compiler Engineers** | Complete instruction encoding and behavior documentation |
| **OS Developers** | Comprehensive system programming model coverage |
| **Security Researchers** | Hardware security features and exploitation prerequisites |
| **Embedded Engineers** | Bare-metal programming fundamentals |
| **Technical Interviewees** | Systems programming interview preparation |

---

## 🚀 Quick Start

### Prerequisites Check

Before beginning, verify you have the required background:

```bash
# You should be comfortable with:
✓ C programming (pointers, structs, memory management)
✓ Basic assembly concepts (registers, memory, instructions)  
✓ Binary and hexadecimal number systems
✓ Linux command line operations
✓ Fundamental OS concepts (processes, memory, files)
```

### Installation

**1. Clone the Repository**

```bash
git clone https://github.com/yourusername/amd64-architecture-mastery.git
cd amd64-architecture-mastery
```

**2. Download Required Materials**

```bash
# AMD64 Architecture Programmer's Manual (Required - Free)
# Visit: https://developer.amd.com/resources/developer-guides-manuals/

# Download:
#   • Volume 1: Application Programming (Publication 24592)
#   • Volume 2: System Programming (Publication 24593)  
#   • Volume 3: General-Purpose and System Instructions (Publication 24594)
```

**3. Set Up Development Environment**

```bash
# Ubuntu/Debian
sudo apt update && sudo apt install -y \
    nasm gcc gdb qemu-system-x86 build-essential binutils

# Fedora/RHEL
sudo dnf install nasm gcc gdb qemu-system-x86 binutils

# Arch Linux
sudo pacman -S nasm gcc gdb qemu binutils
```

**4. Verify Installation**

```bash
nasm --version && gcc --version && gdb --version && qemu-system-x86_64 --version
```

### First Steps

```bash
# Begin with Week 1
cat part1_foundations/week01_cpu_fundamentals.md

# Or use your preferred markdown viewer
code part1_foundations/week01_cpu_fundamentals.md
```

---

## 📁 Course Structure

### Architecture Overview

```
amd64-architecture-mastery/
│
├── 📄 README.md                                    # This document
├── 📄 LICENSE                                      # MIT License
├── 📄 CONTRIBUTING.md                              # Contribution guidelines
│
├── 📁 part1_foundations/                           # Weeks 1-3
│   ├── week01_cpu_fundamentals.md                  # Architecture overview
│   ├── week02_instruction_encoding.md              # Encoding mechanics
│   └── week03_memory_model.md                      # Memory architecture
│
├── 📁 part2_instruction_reference/                 # Weeks 4-8
│   ├── week04_data_movement.md                     # MOV, LEA, PUSH, POP
│   ├── week05_arithmetic.md                        # ADD, SUB, MUL, DIV
│   ├── week06_logical_and_bits.md                  # AND, OR, XOR, shifts
│   └── week07_08_control_strings_flags.md          # Control flow, strings
│
├── 📁 part3_system_programming/                    # Weeks 9-13
│   ├── week09_memory_management.md                 # Paging, TLB
│   ├── week10_interrupts.md                        # IDT, exceptions
│   ├── week11_protection.md                        # Privilege levels
│   ├── week12_system_calls.md                      # SYSCALL/SYSRET
│   └── week13_system_instructions.md               # System instructions
│
├── 📁 part4_advanced/                              # Weeks 14-16
│   ├── week14_simd_programming.md                  # SSE, AVX
│   ├── week15_virtualization.md                    # AMD-V, SVM
│   └── week16_security_capstone.md                 # Security + Project
│
└── 📁 resources/                                   # Reference materials
    ├── encoding_reference.md                       # Encoding quick reference
    ├── glossary.md                                 # Terminology
    ├── caching_reference.md                        # Cache architecture
    ├── performance_reference.md                    # PMU, profiling
    └── capstone_ideas.md                           # Project suggestions
```

---

### Part 1: Foundations (Weeks 1-3)

> **Objective**: Establish core understanding of AMD64 architecture and instruction encoding.

<table>
<tr>
<th width="15%">Week</th>
<th width="25%">Topic</th>
<th width="35%">Key Concepts</th>
<th width="25%">Manual Reference</th>
</tr>
<tr>
<td><strong>Week 1</strong></td>
<td>CPU Fundamentals</td>
<td>
• Operating modes (Long, Legacy, Real)<br/>
• Register architecture (GPR, segment, system)<br/>
• System V AMD64 ABI<br/>
• First assembly program
</td>
<td>Vol 1: Ch 1-3<br/>Vol 2: Ch 1</td>
</tr>
<tr>
<td><strong>Week 2</strong></td>
<td>Instruction Encoding</td>
<td>
• Instruction byte format (1-15 bytes)<br/>
• Legacy prefixes (Group 1-4)<br/>
• REX prefix (W, R, X, B bits)<br/>
• ModRM and SIB bytes<br/>
• VEX/EVEX overview
</td>
<td>Vol 3: Ch 1<br/>Vol 3: App A</td>
</tr>
<tr>
<td><strong>Week 3</strong></td>
<td>Memory Model</td>
<td>
• Segmentation in 64-bit mode<br/>
• Virtual address space (canonical)<br/>
• Paging introduction<br/>
• Memory types overview
</td>
<td>Vol 2: Ch 1, 4-5</td>
</tr>
</table>

---

### Part 2: Instruction Reference (Weeks 4-8)

> **Objective**: Master reading and applying the instruction reference manual.

<table>
<tr>
<th width="15%">Week</th>
<th width="25%">Topic</th>
<th width="35%">Key Concepts</th>
<th width="25%">Manual Reference</th>
</tr>
<tr>
<td><strong>Week 4</strong></td>
<td>Data Movement</td>
<td>
• MOV (all forms, encoding)<br/>
• MOVSX, MOVZX, MOVSXD<br/>
• LEA (address computation)<br/>
• PUSH, POP, XCHG<br/>
• CMOVcc (conditional move)
</td>
<td>Vol 3: Ch 3</td>
</tr>
<tr>
<td><strong>Week 5</strong></td>
<td>Arithmetic</td>
<td>
• ADD, ADC, SUB, SBB<br/>
• MUL, IMUL (1/2/3 operand)<br/>
• DIV, IDIV<br/>
• INC, DEC, NEG, CMP<br/>
• Flag behavior analysis
</td>
<td>Vol 3: Ch 3</td>
</tr>
<tr>
<td><strong>Week 6</strong></td>
<td>Logical & Bit Manipulation</td>
<td>
• AND, OR, XOR, NOT, TEST<br/>
• SHL, SHR, SAR, ROL, ROR<br/>
• BT, BTS, BTR, BTC<br/>
• BSF, BSR, POPCNT, LZCNT<br/>
• BMI1/BMI2 instructions
</td>
<td>Vol 3: Ch 3</td>
</tr>
<tr>
<td><strong>Week 7-8</strong></td>
<td>Control, Strings, Flags</td>
<td>
• JMP, Jcc (all conditions)<br/>
• CALL, RET (near/far)<br/>
• REP MOVS/STOS/CMPS/SCAS<br/>
• CPUID, RDTSC, RDTSCP<br/>
• Memory barriers (MFENCE, etc.)
</td>
<td>Vol 3: Ch 3</td>
</tr>
</table>

---

### Part 3: System Programming (Weeks 9-13)

> **Objective**: Understand OS-level hardware interaction and privilege mechanisms.

<table>
<tr>
<th width="15%">Week</th>
<th width="25%">Topic</th>
<th width="35%">Key Concepts</th>
<th width="25%">Manual Reference</th>
</tr>
<tr>
<td><strong>Week 9</strong></td>
<td>Memory Management</td>
<td>
• 4-level page tables (PML4→PT)<br/>
• Page table entry format<br/>
• TLB operation and invalidation<br/>
• Huge pages (2MB, 1GB)<br/>
• Page fault handling
</td>
<td>Vol 2: Ch 5</td>
</tr>
<tr>
<td><strong>Week 10</strong></td>
<td>Interrupts & Exceptions</td>
<td>
• IDT structure (16-byte gates)<br/>
• Exception classification<br/>
• TSS and IST mechanism<br/>
• APIC overview<br/>
• IRET operation
</td>
<td>Vol 2: Ch 8</td>
</tr>
<tr>
<td><strong>Week 11</strong></td>
<td>Protection & Privilege</td>
<td>
• Ring model (CPL, DPL, RPL)<br/>
• Segment-based protection<br/>
• Page-level protection<br/>
• SMEP, SMAP, Protection Keys<br/>
• CET (Shadow Stacks, IBT)
</td>
<td>Vol 2: Ch 4-5</td>
</tr>
<tr>
<td><strong>Week 12</strong></td>
<td>System Calls</td>
<td>
• SYSCALL/SYSRET mechanism<br/>
• MSR configuration (STAR, LSTAR)<br/>
• Linux syscall ABI<br/>
• Kernel entry/exit sequence<br/>
• vDSO
</td>
<td>Vol 2: Ch 6</td>
</tr>
<tr>
<td><strong>Week 13</strong></td>
<td>System Instructions</td>
<td>
• Descriptor table management<br/>
• Control register access (CRn)<br/>
• MSR access (RDMSR/WRMSR)<br/>
• Cache control (CLFLUSH, WBINVD)<br/>
• TLB control (INVLPG)
</td>
<td>Vol 3: Ch 4</td>
</tr>
</table>

---

### Part 4: Advanced Topics (Weeks 14-16)

> **Objective**: Explore modern processor features and complete capstone project.

<table>
<tr>
<th width="15%">Week</th>
<th width="25%">Topic</th>
<th width="35%">Key Concepts</th>
<th width="25%">Manual Reference</th>
</tr>
<tr>
<td><strong>Week 14</strong></td>
<td>SIMD Programming</td>
<td>
• Register evolution (MMX→AVX-512)<br/>
• SSE packed operations<br/>
• AVX 3-operand form<br/>
• Feature detection (CPUID)<br/>
• XSAVE/XRSTOR
</td>
<td>Vol 1: Ch 4-5<br/>Vol 4</td>
</tr>
<tr>
<td><strong>Week 15</strong></td>
<td>Virtualization</td>
<td>
• AMD-V (SVM) architecture<br/>
• VMCB structure<br/>
• VMRUN/VMEXIT flow<br/>
• Nested Page Tables (NPT)<br/>
• Intercept configuration
</td>
<td>Vol 2: Ch 15</td>
</tr>
<tr>
<td><strong>Week 16</strong></td>
<td>Security & Capstone</td>
<td>
• SME (memory encryption)<br/>
• SEV, SEV-ES, SEV-SNP<br/>
• CET implementation<br/>
• <strong>Capstone Project</strong>
</td>
<td>Vol 2: Ch 15-17</td>
</tr>
</table>

---

## 📚 Documentation

### Weekly Modules

Each weekly module follows a consistent pedagogical structure:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        Weekly Module Structure                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  1. LECTURE NOTES                                               │   │
│  │     Detailed technical exposition with diagrams,                │   │
│  │     code examples, and cross-references                         │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│                                    ▼                                    │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  2. KEY READINGS                                                │   │
│  │     Specific AMD manual sections with page numbers              │   │
│  │     and estimated reading time                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│                                    ▼                                    │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  3. EXERCISES                                                   │   │
│  │     Progressive hands-on problems ranging from                  │   │
│  │     basic comprehension to advanced application                 │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│                                    ▼                                    │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  4. MINI-PROJECT                                                │   │
│  │     Substantial implementation challenge integrating            │   │
│  │     week's concepts into practical application                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│                                    ▼                                    │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  5. QUIZ                                                        │   │
│  │     Self-assessment questions for knowledge verification        │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│                                    ▼                                    │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  6. PRIORITY GUIDE                                              │   │
│  │     Classification of topics: Critical / Important / Optional   │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Reference Materials

| Document | Description | Location |
|----------|-------------|----------|
| **Encoding Reference** | Quick lookup tables for instruction encoding | `resources/encoding_reference.md` |
| **Glossary** | Comprehensive terminology definitions (A-Z) | `resources/glossary.md` |
| **Caching Reference** | Memory types, MTRR, PAT configuration | `resources/caching_reference.md` |
| **Performance Reference** | PMU, profiling techniques, RDTSC | `resources/performance_reference.md` |
| **Capstone Ideas** | Project suggestions and requirements | `resources/capstone_ideas.md` |

### AMD Manual Mapping

<details>
<summary><strong>📕 Volume 1: Application Programming (Publication 24592)</strong></summary>

| Chapter | Topic | Course Coverage |
|---------|-------|-----------------|
| Chapter 1 | Long Mode | Week 1 |
| Chapter 2 | Memory Model | Week 3 |
| Chapter 3 | General-Purpose Programming | Weeks 1, 4-8 |
| Chapter 4 | 128-Bit Media Programming | Week 14 |
| Chapter 5 | 64-Bit Media Programming | Week 14 |
| Chapter 6 | x87 Floating-Point Programming | Reference |
| Appendix A | Definitions | Glossary |
| Appendix B | EFLAGS Register | Week 4-5 |
| Appendix D | Instruction Prefixes | Week 2 |
| Appendix E | Instruction Effects | Week 4-8 |

</details>

<details>
<summary><strong>📗 Volume 2: System Programming (Publication 24593)</strong></summary>

| Chapter | Topic | Course Coverage |
|---------|-------|-----------------|
| Chapter 1 | System Programming Overview | Week 1, 3 |
| Chapter 2 | x86 and AMD64 Architecture | Week 1 |
| Chapter 3 | System Resources | Week 10, 13 |
| Chapter 4 | Segmented Virtual Memory | Week 3, 11 |
| Chapter 5 | Page Translation | Week 9 |
| Chapter 6 | System-Management Mode | Reference |
| Chapter 7 | Memory System | Week 9, Caching Ref |
| Chapter 8 | Exceptions and Interrupts | Week 10 |
| Chapter 9 | Machine Check | Reference |
| Chapter 10 | System Calls | Week 12 |
| Chapter 11 | Task Management | Week 10 |
| Chapter 12 | Debug and Performance | Performance Ref |
| Chapter 13 | Floating-Point Support | Reference |
| Chapter 14 | 128-Bit State Support | Week 14 |
| Chapter 15 | Secure Virtual Machine | Week 15 |
| Chapter 16 | Secure Encrypted Virtualization | Week 16 |
| Chapter 17 | Encryption Extensions | Week 16 |
| Appendix A | MSR Cross-Reference | Reference |
| Appendix B | Implementation Differences | Reference |

</details>

<details>
<summary><strong>📘 Volume 3: Instruction Reference (Publication 24594)</strong></summary>

| Chapter | Topic | Course Coverage |
|---------|-------|-----------------|
| Chapter 1 | Instruction Encoding | Week 2 |
| Chapter 2 | Instruction Overview | Week 2 |
| Chapter 3 | General-Purpose Instructions (A-Z) | Weeks 4-8 |
| Chapter 4 | System Instructions (A-Z) | Weeks 10-13 |
| Appendix A | Opcode and Operand Encoding | Week 2, Encoding Ref |
| Appendix B | 64-Bit Mode Instructions | Week 2 |
| Appendix C | Feature Identification | Week 14 |
| Appendix D | Instruction Subsets | Reference |
| Appendix E | CPUID Specification | Week 14 |

</details>

---

## 📈 Learning Path

### Time Investment

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     Weekly Time Allocation                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Lecture Notes & Manual Study  ████████████████░░░░░░░░  4-6 hrs (30%) │
│  Hands-on Exercises            ████████████░░░░░░░░░░░░  4-5 hrs (25%) │
│  Mini-Project Development      ██████████░░░░░░░░░░░░░░  3-4 hrs (20%) │
│  Practice & Code Review        ██████░░░░░░░░░░░░░░░░░░  2-3 hrs (15%) │
│  Quiz & Self-Assessment        ████░░░░░░░░░░░░░░░░░░░░  1-2 hrs (10%) │
│                                                                         │
│  ─────────────────────────────────────────────────────────────────────  │
│  Weekly Total:     15-20 hours                                         │
│  Course Total:     300-400 hours (16 weeks)                            │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Recommended Schedule

<table>
<tr>
<th>Day</th>
<th>Activity</th>
<th>Duration</th>
<th>Focus</th>
</tr>
<tr>
<td><strong>Monday</strong></td>
<td>Lecture notes study</td>
<td>2-3 hrs</td>
<td>Concept introduction</td>
</tr>
<tr>
<td><strong>Tuesday</strong></td>
<td>AMD manual reading</td>
<td>2-3 hrs</td>
<td>Source material study</td>
</tr>
<tr>
<td><strong>Wednesday</strong></td>
<td>Exercises (first half)</td>
<td>2-3 hrs</td>
<td>Guided practice</td>
</tr>
<tr>
<td><strong>Thursday</strong></td>
<td>Exercises (completion)</td>
<td>2-3 hrs</td>
<td>Independent practice</td>
</tr>
<tr>
<td><strong>Friday</strong></td>
<td>Mini-project start</td>
<td>2-3 hrs</td>
<td>Design & implementation</td>
</tr>
<tr>
<td><strong>Saturday</strong></td>
<td>Mini-project completion</td>
<td>2-3 hrs</td>
<td>Testing & refinement</td>
</tr>
<tr>
<td><strong>Sunday</strong></td>
<td>Quiz & review</td>
<td>2-3 hrs</td>
<td>Assessment & planning</td>
</tr>
</table>

### Assessment Strategy

| Component | Frequency | Purpose | Weight |
|-----------|-----------|---------|--------|
| **Weekly Quiz** | Each week | Knowledge verification | 20% |
| **Exercises** | Each week | Skill application | 25% |
| **Mini-Projects** | Each week | Integration practice | 25% |
| **Capstone Project** | Week 16 | Comprehensive demonstration | 30% |

---

## 🛠 Development Environment

### System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **Processor** | x86-64 (AMD or Intel) | AMD Ryzen / Intel Core i5+ |
| **Memory** | 4 GB RAM | 8+ GB RAM |
| **Storage** | 10 GB free | 20+ GB SSD |
| **Operating System** | Linux (any distribution) | Ubuntu 22.04 LTS |

### Toolchain Setup

<details>
<summary><strong>Ubuntu / Debian</strong></summary>

```bash
sudo apt update
sudo apt install -y \
    nasm \
    gcc \
    g++ \
    gdb \
    qemu-system-x86 \
    build-essential \
    binutils \
    xxd \
    hexdump
```

</details>

<details>
<summary><strong>Fedora / RHEL / CentOS</strong></summary>

```bash
sudo dnf install -y \
    nasm \
    gcc \
    gcc-c++ \
    gdb \
    qemu-system-x86 \
    binutils
```

</details>

<details>
<summary><strong>Arch Linux</strong></summary>

```bash
sudo pacman -S \
    nasm \
    gcc \
    gdb \
    qemu \
    binutils
```

</details>

### Verification

```bash
#!/bin/bash
echo "════════════════════════════════════════════════════════════════"
echo "  AMD64 Architecture Mastery - Environment Verification"
echo "════════════════════════════════════════════════════════════════"
echo ""

check_tool() {
    if command -v $1 &> /dev/null; then
        echo "  ✓ $1: $($1 --version 2>&1 | head -1)"
    else
        echo "  ✗ $1: NOT FOUND"
    fi
}

echo "Required Tools:"
check_tool nasm
check_tool gcc
check_tool gdb
check_tool qemu-system-x86_64
check_tool objdump
check_tool ndisasm

echo ""
echo "════════════════════════════════════════════════════════════════"
echo "  Verification complete."
echo "════════════════════════════════════════════════════════════════"
```

---

## 🎓 Learning Outcomes

Upon successful completion of this curriculum, participants will demonstrate proficiency in:

### Conceptual Understanding

<table>
<tr>
<td width="50%">

**Architecture Fundamentals**
- [ ] Operating modes and transitions
- [ ] Complete register architecture  
- [ ] Memory model and addressing
- [ ] Privilege and protection mechanisms

</td>
<td width="50%">

**Instruction Set Mastery**
- [ ] All instruction encoding formats
- [ ] Opcode map navigation
- [ ] Flag behavior analysis
- [ ] Feature detection via CPUID

</td>
</tr>
</table>

### Practical Competencies

<table>
<tr>
<td width="50%">

**Development Skills**
- [ ] Assembly language programming
- [ ] Machine-level debugging
- [ ] Performance profiling
- [ ] Security feature implementation

</td>
<td width="50%">

**Professional Abilities**
- [ ] Technical documentation interpretation
- [ ] Independent problem solving
- [ ] System-level troubleshooting
- [ ] Architecture documentation contribution

</td>
</tr>
</table>

---

## ❓ FAQ

<details>
<summary><strong>Do I need AMD hardware specifically?</strong></summary>

No. The x86-64 instruction set architecture is compatible between AMD and Intel processors. All concepts covered in this curriculum apply to both manufacturers' implementations. For virtualization-specific topics (AMD-V/SVM), QEMU provides adequate emulation for learning purposes.

</details>

<details>
<summary><strong>Can this curriculum be used for academic credit?</strong></summary>

This curriculum is designed at university level and can supplement existing courses or serve as the foundation for new academic offerings. Contact your educational institution regarding credit transfer or course adoption possibilities.

</details>

<details>
<summary><strong>What differentiates this from online assembly tutorials?</strong></summary>

Traditional tutorials teach specific examples through memorization. This curriculum develops the fundamental skill of reading authoritative technical documentation—the approach used by professional compiler writers, kernel developers, and security researchers. You learn *how* to find any answer, not just specific solutions.

</details>

<details>
<summary><strong>Is the course self-paced?</strong></summary>

Yes. While designed as a 16-week structured curriculum, all materials are immediately accessible. Self-study students can proceed at their own pace, though a minimum of 15 hours per week is recommended for optimal concept retention.

</details>

<details>
<summary><strong>How should I handle difficulties?</strong></summary>

1. Review the relevant AMD manual sections directly
2. Consult the glossary and reference materials
3. Open a Discussion on the GitHub repository
4. Search existing Issues for similar questions

</details>

<details>
<summary><strong>Is this suitable for interview preparation?</strong></summary>

Yes. This curriculum comprehensively covers topics commonly assessed in systems programming interviews at major technology companies, including instruction encoding, memory management, synchronization primitives, and low-level optimization techniques.

</details>

---

## 🤝 Contributing

Contributions are welcome and appreciated. Please review the contribution guidelines before submitting.

### Contribution Categories

| Type | Description | Label |
|------|-------------|-------|
| 🐛 **Bug Reports** | Errors, inaccuracies, broken links | `bug` |
| 📝 **Content Improvements** | Clarifications, additional explanations | `enhancement` |
| ➕ **New Content** | Additional exercises, examples, projects | `new-content` |
| 🌐 **Translations** | Localization to other languages | `translation` |
| 📊 **Diagrams** | Visual aids and illustrations | `documentation` |
| 🔧 **Tooling** | Scripts, automation, utilities | `tooling` |

### Process

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/improvement`)
3. **Implement** your changes
4. **Test** thoroughly
5. **Commit** with clear messages (`git commit -am 'Add detailed explanation of X'`)
6. **Push** to your fork (`git push origin feature/improvement`)
7. **Submit** a Pull Request

---

## 📜 License

This curriculum is released under the MIT License.

```
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> **Attribution Note**: AMD64 Architecture Programmer's Manual is © Advanced Micro Devices, Inc. This educational curriculum references AMD documentation under fair use for educational purposes.

---

## 🙏 Acknowledgments

<table>
<tr>
<td align="center" width="25%">
<strong>AMD</strong><br/>
<sub>Comprehensive public documentation</sub>
</td>
<td align="center" width="25%">
<strong>OSDev Community</strong><br/>
<sub>Practical implementation knowledge</sub>
</td>
<td align="center" width="25%">
<strong>OpenSecurityTraining</strong><br/>
<sub>Educational methodology inspiration</sub>
</td>
<td align="center" width="25%">
<strong>Agner Fog</strong><br/>
<sub>Decades of optimization research</sub>
</td>
</tr>
</table>

---

<br/>

<p align="center">
  <strong>📊 Curriculum Statistics</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Weekly%20Modules-16-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Reference%20Docs-5-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Exercises-100+-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Total%20Hours-300--400-red?style=for-the-badge"/>
</p>

<br/>

---

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-course-structure">Structure</a> •
  <a href="#-documentation">Documentation</a> •
  <a href="../../issues">Report Issue</a> •
  <a href="../../discussions">Discussions</a>
</p>

<br/>

<p align="center">
  <sub>Developed with rigorous attention to technical accuracy and pedagogical effectiveness.</sub>
</p>

<p align="center">
  <sub>
    <em>"Give someone an instruction, they write one program.<br/>
    Teach them to read the manual, they write any program."</em>
  </sub>
</p>

<br/>

<p align="center">
  <strong>⭐ Star this repository if you find it valuable!</strong>
</p>

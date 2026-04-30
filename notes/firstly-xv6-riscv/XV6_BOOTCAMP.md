# xv6-RISCV Boot Camp

Goal: Rapidly solidify OS foundations in 10 days. Do not treat this as a destination.
After this boot camp, you must transition to Linux immediately.

---

## Why xv6?

At 20, your OS foundations are fragile. xv6 is ~10K lines and small enough to hold in your head. It teaches the mental models you need before Linux `mm/mmap.c` makes sense.

**Rule:** If you finish the `pgtbl` lab and still don't intuitively understand page table flags (`PTE_V`, `PTE_R`, `PTE_W`, `PTE_X`) and page fault handling, do not move to Linux yet. That concept is load-bearing.

---

## The Book: xv6-riscv-book

Download `book-riscv-rev3.pdf` from the MIT 6.S081 course website.

| Chapter | Topic | Read? | Notes |
|---|---|---|---|
| 1 | OS interfaces | **Yes** | Syscalls, processes, file descriptors |
| 2 | Page tables | **Yes** | Virtual memory, SATP, `walk()`, `mappages()` |
| 3 | Traps | **Yes** | Interrupts, `usertrap()`, `kerneltrap()` |
| 4 | Locking | **Skim** | Know `spinlock` and deadlock. Skip code details. |
| 5 | Scheduling | **Yes** | Context switch, `scheduler()`, `yield()` |
| 6+ | File system, etc. | **Skip** | Not on critical path to AI infra |

**Time budget:** 3-4 days to read Ch 1-3 and 5 while tracing code.

---

## Lectures: Watch Only These 4

Search "MIT 6.S081 2023 lectures" on YouTube or MIT OCW.

1. **Lecture 3: OS Organization** — Why monolithic vs microkernel. Context for Linux and gvisor.
2. **Lecture 4: Page Tables** — Visual walkthrough of RISC-V page tables.
3. **Lecture 6: Isolation & System Call Entry/Exit** — Traps, `uservec`, `userret`.
4. **Lecture 8: Interrupts** — Timer interrupts, preemption, `yield()`.

**Skip:** File system, networking, RCU, meltdown/spectre lectures.

---

## Labs: Do Only These 3

| Lab | What You Learn | Time | Priority |
|---|---|---|---|
| **pgtbl** | Page table inspection, `pgaccess()`, flags | 2 days | **Critical** |
| **traps** | User-mode interrupts, backtrace | 1-2 days | **High** |
| **cow** | Copy-on-write fork, page faults | 2-3 days | **Medium** |

**Skip:** `util`, `syscall`, `primes`, `fs`, `net`, `lock`.

---

## 10-Day Schedule

| Day | Activity |
|---|---|
| 1 | Read book Ch 1. Read `kernel/proc.c`, `kernel/syscall.c`. |
| 2 | Read book Ch 2. Watch Lecture 4 (Page Tables). |
| 3 | Do **pgtbl lab**. |
| 4 | Read book Ch 3. Watch Lecture 6 (Traps). |
| 5 | Do **traps lab**. |
| 6 | Read book Ch 5. Watch Lecture 8 (Interrupts). |
| 7 | Trace `kernel/sched.c:scheduler()` + `swtch.S`. |
| 8 | Read book Ch 4 (skim). Start **cow lab**. |
| 9 | Finish **cow lab** if needed. |
| 10 | **Transition to Linux.** Open `linux-master/mm/mmap.c`. Find `do_mmap()`. Compare to `kernel/vm.c:mappages()`. |

---

## Core Concepts to Extract

After 10 days, you must be able to answer these without looking at code:

1. What is in a `struct proc`? (PCB, page table, trapframe, context, fds)
2. Timer interrupt sequence: CPU -> trap vector -> `usertrap()` -> `yield()` -> `scheduler()` -> `swtch()` -> `usertrapret()`
3. How does xv6 walk a 3-level RISC-V page table? (`walk()`)
4. What is the difference between a trap and an interrupt?

---

## What xv6 Does NOT Have (Don't Expect It)

- NUMA
- cgroups
- eBPF
- Modern block layer / NVMe
- GPU drivers
- RCU
- Complex locking (only spinlocks)

These are "Linux scale problems." Accept them as infrastructure, don't try to find them in xv6.

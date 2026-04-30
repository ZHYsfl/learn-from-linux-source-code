A Concrete Test for "Am I Ready to Leave xv6?"

You should be able to answer these without looking at code:
1. What is in a struct proc in xv6? (PCB, page table, trapframe, context, file descriptors)
2. When a timer interrupt fires, what is the exact sequence: CPU → trap vector → usertrap() → yield() → scheduler() → another process → swtch() → usertrapret()?
3. How does xv6 convert a virtual address to a physical address? (Walk the 3-level RISC-V page table)
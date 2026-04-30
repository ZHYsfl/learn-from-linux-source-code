# Linux Kernel Study Plan

Kernel version: 7.1.0 (`D:/start/OS/linux/linux-master`)
Goal: Understand Linux internals relevant to AI infrastructure.
Method: Trace-driven. Pick an operation, trace it top-to-bottom.

---

## Rules

1. **Do not read cover-to-cover.** The kernel is ~30M lines. You will drown.
2. **Use `git log` and `git blame`.** Find *why* code exists, not just *what* it does.
3. **Compare to xv6.** When confused, ask "What would xv6 do?" If xv6 doesn't have it, accept it as Linux infrastructure.
4. **Build and trace.** Use `bpftrace`, `perf`, or QEMU to observe behavior.

---

## Phase 1: Memory Management (Weeks 1-3)

*Why: LLM serving is memory-bound. KV-cache, huge pages, NUMA, OOM behavior determine P99 latency.*

### System Calls to Trace
- `mmap` -> `mm/mmap.c`, `mm/filemap.c`
- `mlock` -> `mm/mlock.c` (GPU memory pinning)
- `madvise(MADV_HUGEPAGE)` -> `mm/huge_memory.c`

### Files to Read (in order)
1. `include/linux/mm_types.h` — `struct mm_struct`, `struct vm_area_struct`
2. `mm/page_alloc.c` — Buddy allocator. Focus on `__alloc_pages_nodemask`
3. `mm/slub.c` — SLUB allocator (replaced slab)
4. `mm/memcontrol.c` — **cgroups v2 memory controller.** `memory.max`, `memory.high`, `memory.reclaim`
5. `mm/oom_kill.c` — OOM killer. Read `oom_badness()`

### Key Concept
Trace what happens when a process exceeds its cgroup memory limit while holding GPU memory. Does the OOM killer trigger? In what order?

---

## Phase 2: CPU Scheduling + cgroups (Weeks 4-5)

*Why: Inference batching, request scheduling, CPU-GPU pipeline overlap.*

### Files to Read
1. `kernel/sched/fair.c` — CFS. vruntime, `sched_entity`
2. `kernel/sched/core.c` — `schedule()`, `context_switch()`
3. `kernel/cgroup/cgroup.c` — cgroup v2 hierarchy. `cpu.max`, `cpu.weight`, `cpuset.cpus`
4. `kernel/sched/rt.c` — Real-time scheduling

### Exercise
Set `cpu.cfs_quota_us` and `cpu.cfs_period_us` in a cgroup, then trace `tg_set_cfs_bandwidth()` in `kernel/sched/fair.c`.

---

## Phase 3: I/O, Networking, eBPF (Weeks 6-8)

*Why: Data loading, distributed training (RDMA/NCCL), observability.*

### Files to Read
1. `block/blk-mq.c` — Multi-queue block layer (NVMe)
2. `io_uring/` — Modern async I/O
3. `net/core/`, `net/ipv4/` — Socket buffers, TCP congestion
4. `drivers/infiniband/` — RDMA
5. `kernel/bpf/` — **eBPF verifier and program loading**
6. `kernel/trace/bpf_trace.c` — bpftrace probes
7. `drivers/gpu/drm/` — DRM subsystem for GPU memory management

### Key Concept
Build a simple `bpftrace` probe: `kprobe:__alloc_pages_nodemask { printf("order=%d\n", arg0); }`

---

## What to Skip (For Now)

- `arch/` unless you need ARM64 GPU DMA specifics
- `fs/` except `fs/page_cache.c` and `fs/iomap/` (DAX)
- `sound/`, `drivers/media/`, most `drivers/usb/`
- `security/` unless working on confidential AI

---

## xv6 -> Linux Concept Map

| xv6 Concept | Linux Equivalent | Why It Matters |
|---|---|---|
| `proc.c:scheduler()` (round-robin) | `kernel/sched/core.c:__schedule()` | Inference thread pinning, CPU isolation |
| `vm.c:mappages()` | `mm/memory.c:handle_mm_fault()` | GPU memory mapping, page faults on mmap'd weights |
| `trap.c:usertrap()` | `arch/x86/entry/entry_64.S`, `do_syscall_64()` | eBPF kprobes attach here |
| `bio.c` (buffer cache) | `mm/page_cache.c`, `fs/iomap/` | Checkpoint loading, dataset mmap |

---

## First Week Assignment

**Day 1-2:** Read `mm/mmap.c`. Trace `do_mmap()` -> `vma_merge()` -> `find_vma()`. Draw the VMA tree for a simple process.

**Day 3-4:** Read `mm/page_alloc.c`. Understand how `__alloc_pages()` chooses a NUMA node. Critical for multi-socket training servers.

**Day 5-7:** Read `mm/memcontrol.c`. Find the function reading `memory.max` and triggering reclaim. Understand `memory.high` (throttle) vs `memory.max` (OOM kill).

---

## Grep Patterns for Exploration

```bash
# Find syscall entry points
grep -r "SYSCALL_DEFINE6.mmap" mm/

# Find huge page related commits
git log --oneline --grep="hugepage" mm/ | head -20

# Trace page allocation
bpftrace -e 'kprobe:__alloc_pages_nodemask { printf("order=%d\n", arg0); }'
```

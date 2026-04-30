# AI Infra Scientist Study Roadmap

Goal: Build deep systems knowledge for AI infrastructure. Focus on Linux internals, GPU/accelerator systems, container runtimes, and modern observability.

Started: 2026-04-30
Status: Phase 0 (Foundation building)

---

## Philosophy

1. **Trace-driven learning**: Pick one operation (e.g., `mmap`) and trace it top-to-bottom through the kernel. Do not read files in isolation.
2. **Production relevance**: Every topic must connect to a real AI infra problem (LLM serving, distributed training, container scheduling).
3. **Time-boxing**: Do not get stuck in toy projects. xv6 is a 2-week scaffold, not a destination.
4. **English-first**: All notes, commit messages, and documentation are written in English.

---

## Three-Phase Plan

### Phase 0: xv6 Boot Camp (Weeks 1-2)
Fix fragile OS foundations rapidly. Read the xv6 book, watch 4 MIT lectures, complete 3 labs.
See: [firstly-xv6-riscv/XV6_BOOTCAMP.md](firstly-xv6-riscv/XV6_BOOTCAMP.md)

### Phase 1: Linux Kernel Deep Dive (Weeks 3-8)
Trace-driven study of the real kernel (7.1.0). Priority: Memory management -> Scheduling -> I/O/Networking/eBPF.
See: [learn-from-linux/LINUX_STUDY_PLAN.md](learn-from-linux/LINUX_STUDY_PLAN.md)

### Phase 2: gvisor + Modern Runtime (Weeks 9-12)
Compare-and-contrast Linux with gvisor's userspace kernel. Focus on syscall interception, container sandboxing, and netstack.
See: [learn-from-gvisor/GVISOR_PLAN.md](learn-from-gvisor/GVISOR_PLAN.md) (to be written)

---

## Technology Context (2026 vs 2013)

Bojie's 2013 blogs emphasized Linux fundamentals. The core thesis still holds, but the landscape evolved:

| 2013 Focus | 2026 AI Infra Focus |
|---|---|
| General Linux internals | cgroup v2, NUMA, memory tiers (CXL, HBM) |
| Printf debugging | bpftrace, perf, NVIDIA Nsight Systems |
| CPU-centric scheduling | GPU scheduling (MIG, time-slicing), CPU affinity for inference |
| C only | Rust entering kernel and infra (vLLM, data loaders) |
| Traditional networking | RDMA, kernel bypass, collective communication (NCCL) |
| Monolithic servers | Kubernetes, device plugins, autoscaling, serverless AI |

---

## Weekly Time Budget

- **10 hours/week total**
- Phase 0: 10h on xv6
- Phase 1: 7h Linux + 2h notes/blog + 1h reading papers
- Phase 2: 5h Linux + 3h gvisor + 2h notes/blog

---

## Key Resources

- Linux kernel source: `D:/start/OS/linux/linux-master` (7.1.0)
- xv6-riscv source: `D:/start/OS/xv6-riscv`
- gvisor source: `D:/start/OS/gvisor`
- MIT 6.S081 course (xv6)
- LWN.net (Linux weekly news)
- OSDI/SOSP/NSDI papers on systems for ML (vLLM, Ray, etc.)

https://01.me/2013/06/msra-interview/

https://01.me/2013/08/why-you-should-learn-linux/

today i read the blog of why you should learn linux, and the blog of msra interview.i found boj has so many practices of linux, i should learn more from it.if i want to become an AI Infra Scientist, i should learn more from linux.today i decide to learn the source code of linux.i know linux is a large project, and the important things are the design,data flow and the architecture of linux.

what's more,in the future i will use English as my first working language.

boj wrote these blogs in 2013,and the technology and the tools are different from now.thirteen years later,the core thesis still holds-Linux and Systems knowledge remain the beadrock of AI Infrastructure-but the landscape has evolved.I ask kimi-k2.6 for advise, here's what to prioritize now:

1.Deep Linux Internals Are Non-Negotiable
Modern AI infrastructure (Kubernetes, container runtimes, GPU drivers, network stacks) runs on Linux. The "practices" you found in your Linux source study translate directly to:      
- CPU/GNU scheduling: Understanding cgroup v2, CPU affinity, and the Completely Fair Scheduler helps you optimize inference latency under load.
- Memory management: Huge pages, NUMA topology, and memory tiers (CXL, HBM) are critical for LLM serving.
- eBPF: It has become the standard for kernel-level observability and even networking/service mesh offloading. Learn bpftrace and writing eBPF programs.

2.GPU/Accelerator Systems Programming
Unlike 2013, AI Infra is GPU-centric. Learn:
- CUDA/ROCm programming models, but more importantly, how they interface with the Linux kernel (NVIDIA kernel driver, DRM subsystem, DMA-BUF).
- GPU scheduling and virtualization: MIG, time-slicing, vGPU, and the emerging standards around AI accelerators.
- NCCL and collective communication: These sit at the intersection of networking, kernel bypass (RDMA), and topology awareness.

3.Rust Is Entering Systems Infrastructure
While C still dominates the kernel, Rust is now used in major infrastructure projects (e.g., parts of the Linux kernel, Cloudflare stacks, vectorized databases). For AI Infra, many new high-performance components (data loaders, inference engines, GPU memory managers) are hybrid Rust/C++/Python.

4. Modern Toolchain and Observability
The 2013 post praised UNIX toolchains. In 2026, your toolchain for AI Infra includes:
- Container orchestration: Deep Kubernetes understanding (scheduler predicates, device plugins, autoscaling).
- Observability: Prometheus/Grafana for metrics, OpenTelemetry for tracing, and eBPF-based continuous profiling (e.g., Parca, Pyroscope).
- Infrastructure as Code: Terraform/Pulumi are basics; learn GitOps (ArgoCD/Flux) for managing training clusters.

5. The "English-first, Official-docs-first" Rule Still Applies
This is even more critical in AI. The best resources are:
- Linux Kernel documentation and LWN.net.
- NVIDIA/AMD/Intel technical blogs and developer guides.
- Papers, not just blogs: Read OSDI/SOSP/NSDI papers on systems for ML (e.g., vLLM, Ray, XGBoost, Spark internals).
- Avoid deprecated or rehashed forum posts. AI moves fast; stale information is everywhere.

6. From "Printf Debugging" to Systems Debugging
Replace printf with:
- gdb/LLDB for C/C++.
- bpftrace/perf for kernel and performance debugging.
- NVIDIA Nsight Systems/Compute for GPU kernel analysis.
- Distributed tracing for debugging multi-node training/inference pipelines.

7. Learn the New "OS for AI" Layer
The role of an "AI Infra Scientist" often means you are building the operating environment for AI workloads:
- Model serving engines: Deeply understand how vLLM, TensorRT-LLM, or TGI manage KV-cache, batching (continuous batching), and memory pools.
- Serverless/FAAS for AI: Cold-start optimization, checkpointing, and fast model loading.
- Power and sustainability: Data center power capping, carbon-aware scheduling, and dynamic frequency scaling for GPUs/TPUs.

hope for the best!


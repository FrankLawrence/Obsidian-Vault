---
Tags: 
Created: 2026-06-14 18:43:28
---
(Links:: [[Advanced Computer Architecture]])
# Variable Latency

> [!definition] 
> Variable latencies in computations, memory accesses, or loop execution time such as floating-point units, cache hit/miss, variable loop bounds, and early-exit conditions prevent good pipelining using standard HLS techniques

In static HLS, you have to assume *worst-case latency* (only cache misses), and to then reserver additional pipeline stages for the variable-latency operations. This gives us high throughput (given no dependencies) but a *larger latency & area* as a side-effect.

Sparse-matrix dense-vector multiplication (SpMV):
```cpp
for (i = 0; i < num_rows, i++) {
	tmp = 0;
	s = row[i]; e = row[i+1];
	for (c = s; c < e; c++) {
		cid = col[c];
		tmp += val[c] * vec[cid];
	}
	out[i] = tmp;
}
```

**Pipeline Stalling**: 
- Static HLS: stall *entire pipeline* in case of a variable-latency event
	- Schedule each operation based on it's *minimum latency*
	- If an operation does not complete within min. latency, block operation and *stall pipeline*
- Dynamic HLS: naturally handles variable latencies
	- Handshaking mechanism stalls the successors of long-latency operation
	- Other computations can advance during stall
	- Yet, computations in the same unit happen *strictly in order*
	- In general, schedule adapts well during loop-carried variable-latency dependencies, but not so much due to in-order computation execution

Superscalar processors perform fully *out-of-order pipelines*, so operations are executed out-of-order, only respecting dependencies. In HLS, associativity analysis *could even give a better result*, yet nobody went there in HLS in a general way, not even respecting dependence ordering.
# Memory and Caches
Memory and caches are a key source for variable latencies. Separate variable-latency memory accesses and computation: Data is loaded from memory, stored in FIFOs, and sent to execution datapath as soon as ready. This requires nontrivial *code restructuring* (by user or compiler) [^1]. 

![[Pasted image 20260614194011.png|700]]
![[Execute Decoupling.svg|900]]

This gives an execution similar to that of a *dynamic schedule* but composing *static building blocks*. But in-order execution still prevents full datapath utilization.
We can extend this with out-of-order execution by allowing out-of-order dequeuing which requires even less trivial code restructuring. Latency of some iterations can be *completely hidden* which depends on distribution of short/long iterations. The *effective iteration interval can be 1* even in the presence of cache misses unlike previous solutions where a cache miss does not necessarily lower the II (other iterations can proceed out-of-order).

## Locally Out-of-Order Pipelines
In some cases, operations cannot be done simply out-of-order, and the end result will stay the same (e.g. with non-commutative operations). This is called "mulitthreaded" pipeline synthesis: specific operations are suspended (i.e. context saved) and can execute out-of-order. The suspended operations release resources and subsequence ones can continue without stalling. No general tagging and no reordering thanks to associativity and commutativity!

![[Pasted image 20260614200844.png|700]]

# Nested Loops
## Variable Loop Bounds
In addition to operations, the number of loop iterations can also be variable, such as with loop bounds computed at runtime or early exit conditions! Take as example the outer-loop:

```cpp
for (i = 0; i < num_rows, i++) {
	tmp = 0;
	s = row[i]; e = row[i+1];
	for (c = s; c < e; c++) {
		cid = col[c];
		tmp += val[c] * vec[cid];
	}
	out[i] = tmp;
}
```

![[Variable Loop Bounds.svg|800]]

The inner-loop pipeline has to empty before another inner loop can start: pipeline empty for inner loop iteration latency + loop bound computation latency.
Access/Execute decoupling works here, but the transformation is even more complex:
- Compute loop bounds and enqueue into FIFO
- Dequeue bounds and execute inner loop

With dynamic HLS we naturally start a new loop *as soon as pipeline is ready*. This requires no special mechanism or transformation.
## Loop-Carried Dependencies
With dynamic HLS, we have in-order pipelines: Different operations execute *out-of-order* with respect to each other, but each operation processes its own data *in order*. We have limited throughput in case of *long-latency loop-carried dependencies*.

The long-latency addition limits the II of each inner loop to II=3, but a new inner loop can start at best one cycle after the last iteration of the previous loop.
Fully *out-of-order pipelines* allow all loop iterations to be started speculatively. All operations execute *completely out-of-order*, only respecting dependencies.

# Multiple Pipelines
So far we had instruction-level parallelism within a *single datapath*. However, we can *replicate datapath/kernel* to increase parallelism. 
Traditional unrolling (with HLS pragma) involves replicating computations within a loop, achieving spatial parallelism in regular loops. This is not suitable for irregular code (e.g. unknown loop bounds or memory/data dependencies).
With **task parallelism** (loop replication), we execute *multiple loop instances* in parallel.

A C-based HLS tool requires a *structural description* of the pipeline connectivity. *Fork-join* schemes express the same in *software implementations*.

**Simple multithreading**:
- Replicate loops and execute *multiple loop instances* in parallel
- Increases parallelism, but datapaths *not fully used if inner loop latencies differ*
# Advanced. Fork-Join Schemes
In cases where tasks have *strongly dynamic behaviour* and run over a fixed number of threads, we can use *simple automatic partitioning* to distribute workloads. This is similar ot A/E decoupling in *connecting static FSM through buffers*. Load balancing is achieved to maximize parallelism and kernel utilization (dispatch computation to processing units based on availability).

![[Pasted image 20260614215456.png|700]]

**Multithreaded runtime systems in hardware**:
- Very few efforts to "imitate" Cilk and TBB in HLS: *ParallelXP*
	- Not quite Cilk/TBB nor really HLS
	- Exploits the continuation passing idea to create a *customizable runtime system* where users can plug in their won processing elements
- Very few efforts to "imitate" Cilk and TBB in HLS: *TAPAS*
	- More of an HLS/compiler effort, based on a compiler front end supporting fork-join
	- Only *direct connection between task units* based on parent-child relations (multiple units for the same task? load balancing among them?)
# Conclusions
- High-Level Synthesis is a *fairly mature field* with a number of options available
- Of commercial interest *virtually only for FPGAs*; ASIC designers seem to stick almost exclusively to RTL because they want to get everything they can from technology
- Still, it is *not really meant to* (nor in fact does) *give access to [[FPGA]]s for software programmers*; most of the available options speed-up development for hardware engineers who more or less know what they want
- Only *very slow movement towards* software support for *irregular parallelism* (the one which might interest software programmers attracted by FPGAs)

> [!bug]+ Caveat 1
> The accelerator design must compensate for the FPGA reconfigurability:
> - Processors exploit amazingly well transistors in a given technology
> - GPUs do that too and also extract a huge lot of parallelism-when the application fits the architecture
> - ASICs are for the lucky few, but if an application is sufficiently important (e.g. Google's TPUs), they are unbeatable, almost by definition
> - On FPGAs, one must fight the overhead of reconfigurability (one order of magnitude slower and bigger?) before one can gain!
> - Maybe FPGAs are not the right reconfigurable platform for accelerators...

> [!bug]+ Caveat 2
> It's the memory, stupid!
> - HLS is getting better at designing the computational part of accelerators
> - Most of the performance depends on moving data efficiently
> - Even the simplest aspects of this are hard or hopeless for HLS compilers (memory disambiguation)
> - Few tools to help designing application-specific memory systems
> - Expect to plan data movement by hand-and to code it in RTL...

> [!bug]+ Caveat 3
> HLS may give you great kernels but does not give you full accelerators
> - HLS tools are ok with fine grain parallelism (akin to ILP) but not more
> - HLS tools have embraced some languages for specific computational patterns (CUDA, OpenCL,…)—but are GPUs not better when you can use these languages proficiently?
> - Very limited efforts and progress in adopting programming models where FPGA acceleration might truly excel
> - Manual design in RTL (with ad-hoc use of HLS, perhaps) seems the only way of achieving truly competitive accelerators today

---
References:
- [[2. Challenges of HLS.pdf]]

[^1]: T. Ham, J. L. Aragón, and M. Martonosi, ACM TACO, June 2017

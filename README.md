# Advanced-Computer-Architecture

## First Lab Questions:

Results collected by gem5 running on VirtualBox VM Ubuntu 19.10.
 


### Question 1:
```python

parser.add_argument("--cpu", type=str, choices=list(cpu_types.keys()),
                        default="atomic",
                        help="CPU model to use")
parser.add_argument("--cpu-freq", type=str, default="4GHz")
parser.add_argument("--num-cores", type=int, default=1,
                        help="Number of CPU cores")
parser.add_argument("--mem-type", default="DDR3_1600_8x8",
                        choices=ObjectList.mem_list.get_names(),
                        help = "type of memory to use")
parser.add_argument("--mem-channels", type=int, default=2,
                        help = "number of memory channels")
parser.add_argument("--mem-ranks", type=int, default=None,
                        help = "number of memory ranks per channel")
parser.add_argument("--mem-size", action="store", type=str,
                        default="2GB",
                        help="Specify the physical memory size")


cpu_types = {
    "atomic" : ( AtomicSimpleCPU, None, None, None, None),
    "minor" : (MinorCPU,
               devices.L1I, devices.L1D,
               devices.WalkCache,
               devices.L2),
    "hpi" : ( HPI.HPI,
              HPI.HPI_ICache, HPI.HPI_DCache,
              HPI.HPI_WalkCache,
              HPI.HPI_L2)
}
```



### Question 2:

#### a.
From ```config.ini```:
<br/>
<br/>
Verification of 2GB default memory size:
```ini
mem_ranges=0:2147483648
```
Verification of 4GHz cpu-freq:
```ini
clock=250
```
Verification of 1 thread running (num-cores = 1):
```ini
numThreads=1
```

#### b.
From ```stats.txt``` :
```ini
system.cpu_cluster.cpus.committedInsts           5028                       # Number of instructions committed
system.cpu_cluster.cpus.cpi                 19.348449                       # CPI: cycles per instruction
system.cpu_cluster.cpus.numCycles               97284                       # number of cpu cycles simulated
```
We notice that the reported number of commited instructions is very close to the expected number of instructions by numCycles / cpi ~= 5028.
#### c.
From ```stats.txt``` :
```ini
system.cpu_cluster.l2.overall_accesses::total          479                       # number of overall (read+write) accesses
```
If gem5 didn't provide us with this value, we would calculate it by adding up the following values:
```ini
system.cpu_cluster.l2.overall_accesses::.cpu_cluster.cpus.inst          332                       # number of overall (read+write) accesses
system.cpu_cluster.l2.overall_accesses::.cpu_cluster.cpus.data          147                       # number of overall (read+write) accesses
```



### Question 3:

As seen on https://github.com/arm-university/arm-gem5-rsk/blob/master/gem5_rsk.pdf:


#### Implementation of the gem5 In-order CPU Models


##### AtomicSimpleCPU
The AtomicSimpleCPU uses Atomic memory accesses. In gem5, the AtomicSimpleCPU performs all operations for an instruction on every CPU ```tick()``` and it can get a rough estimation of overall cache access time using the latency estimates from the atomic accesses. Naturally, AtomicSimpleCPU provides the fastest functional simulation, and is used for fast-forwarding to get to a Region Of Interest (ROI) in gem5.

##### TimingSimpleCPU
The TimingSimpleCPU adopted Timing memory access instead of the simple Atomic one. This means that it waits until memory access returns before proceeding, therefore it provides some level of timing. TimingSimpleCPU is also a fast-to-run model, since it simplifies some aspects including pipelining, which means that only a single instruction is being processed at any time. Each arithmetic instruction is executed by TimingSimpleCPU in a single cycle, while memory accesses require multiple cycles. For instance, the TimingSimpleCPU calls ```sendTiming()``` and will only complete fetch after getting a successful return from ```recvTiming()```.

##### MinorCPU
We need a more comprehensive and detailed CPU model in order to emulate realistic systems, therefore we should utilize the detailed in-order CPU models available in gem5. In older versions of gem5, a model named InOrder CPU was capable of doing the job for us, but now there is a new model called MinorCPU.  The MinorCPU is a flexible in-order processor model which was originally developed to support the Arm ISA, and is applicable to other ISAs as well. MinorCPU has a fixed four-stage in-order execution pipeline, while having configurable data structures and execute behavior; therefore it can be configured at the micro-architecture level to model a specific processor.  The four-stage pipeline implemented by MinorCPU includes fetching lines, decomposition into macro-ops, decomposition of macro-ops into micro-ops and execute. These stages are named Fetch1, Fetch2, Decode and Execute, respectively. The pipeline class controls the cyclic tick event and the idling (cycle skipping).

#### a.
Execution time for TimingSimpleCPU and MinorCPU:
```js
TimingSimpleCPU:

```
```csv
MinorCPU:
sim_seconds                                  0.000033                       # Number of seconds simulated
```

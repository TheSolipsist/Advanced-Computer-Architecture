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
From config.ini:
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
From stats.txt:
```ini
system.cpu_cluster.cpus.committedInsts           5028                       # Number of instructions committed
system.cpu_cluster.cpus.cpi                 19.348449                       # CPI: cycles per instruction
system.cpu_cluster.cpus.numCycles               97284                       # number of cpu cycles simulated
```
We notice that the reported number of commited instructions is very close to the expected number of instructions by numCycles / cpi ~= 5028.
#### c.
From stats.txt:
```ini
system.cpu_cluster.l2.overall_accesses::total          479                       # number of overall (read+write) accesses
```

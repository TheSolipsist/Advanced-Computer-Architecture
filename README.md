# Advanced-Computer-Architecture

## First Lab Questions:

 
 
### 1:
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

### 2:

Verification of 2GB default memory size:
```ini
mem_ranges=0:2147483648
```
Verification of 4GHz cpu-freq:
```ini
```

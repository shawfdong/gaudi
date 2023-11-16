# Gaudi

## Access

```
ssh -l asml 172.27.25.67
```

## Hardware

### CPUs

```
asml@gaudi2poc1:~$ lscpu
Architecture:                       x86_64
CPU op-mode(s):                     32-bit, 64-bit
Byte Order:                         Little Endian
Address sizes:                      46 bits physical, 57 bits virtual
CPU(s):                             160
On-line CPU(s) list:                0-159
Thread(s) per core:                 2
Core(s) per socket:                 40
Socket(s):                          2
NUMA node(s):                       2
Vendor ID:                          GenuineIntel
CPU family:                         6
Model:                              106
Model name:                         Intel(R) Xeon(R) Platinum 8380 CPU @ 2.30GHz
Stepping:                           6
Frequency boost:                    enabled
CPU MHz:                            889.092
CPU max MHz:                        2301.0000
CPU min MHz:                        800.0000
BogoMIPS:                           4600.00
Virtualization:                     VT-x
L1d cache:                          3.8 MiB
L1i cache:                          2.5 MiB
L2 cache:                           100 MiB
L3 cache:                           120 MiB
NUMA node0 CPU(s):                  0-39,80-119
NUMA node1 CPU(s):                  40-79,120-159
Vulnerability Gather data sampling: Mitigation; Microcode
Vulnerability Itlb multihit:        Not affected
Vulnerability L1tf:                 Not affected
Vulnerability Mds:                  Not affected
Vulnerability Meltdown:             Not affected
Vulnerability Mmio stale data:      Mitigation; Clear CPU buffers; SMT vulnerable
Vulnerability Retbleed:             Not affected
Vulnerability Spec store bypass:    Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:           Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:           Mitigation; Enhanced IBRS, IBPB conditional, RSB filling, PBRSB-eIBRS SW sequence
Vulnerability Srbds:                Not affected
Vulnerability Tsx async abort:      Not affected
Flags:                              fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts
                                    acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art ar
                                    ch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulq
                                    dq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4
                                    _1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm a
                                    bm 3dnowprefetch cpuid_fault epb cat_l3 invpcid_single ssbd mba ibrs ibpb stibp ibrs
                                    _enhanced tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2
                                     smep bmi2 erms invpcid cqm rdt_a avx512f avx512dq rdseed adx smap avx512ifma clflus
                                    hopt clwb intel_pt avx512cd sha_ni avx512bw avx512vl xsaveopt xsavec xgetbv1 xsaves
                                    cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local wbnoinvd dtherm ida arat pln pts a
                                    vx512vbmi umip pku ospke avx512_vbmi2 gfni vaes vpclmulqdq avx512_vnni avx512_bitalg
                                     tme avx512_vpopcntdq rdpid md_clear pconfig flush_l1d arch_capabilities
```

### Memory

```
asml@gaudi2poc1:~$ lsmem
RANGE                                  SIZE  STATE REMOVABLE BLOCK
0x0000000000000000-0x000000007fffffff    2G online       yes     0
0x0000000100000000-0x000001007fffffff 1022G online       yes 2-512

Memory block size:         2G
Total online memory:       1T
Total offline memory:      0B
```

According to `sudo dmidecode -t memory`, there are 32x 64GB DDR4 DIMMs.

### Disks

```
asml@gaudi2poc1:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
nvme1n1                   259:0    0    7T  0 disk /home
nvme0n1                   259:1    0  1.8T  0 disk
├─nvme0n1p1               259:2    0  512M  0 part /boot/efi
├─nvme0n1p2               259:3    0    1G  0 part /boot
└─nvme0n1p3               259:4    0  1.8T  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0  200G  0 lvm  /
```

### Gaudi2

Eight Gaudi2 processors:

```
asml@gaudi2poc1:~$ lspci -d :1020: -nn
19:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
1a:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
43:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
44:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
b3:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
b4:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
cc:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
cd:00.0 Processing accelerators [1200]: Habana Labs Ltd. Device [1da3:1020] (rev 01)
```

### InfiniBand

```
asml@gaudi2poc1:~$ lspci | grep Mellanox
4b:00.0 Infiniband controller: Mellanox Technologies MT28908 Family [ConnectX-6]
```

## Software

### OS

```
asml@gaudi2poc1:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:        20.04
Codename:       focal
```

### SynapseAI SW Stack

```
asml@gaudi2poc1:~$ apt list --installed | grep habana

habanalabs-container-runtime/focal,now 1.12.1-10 amd64 [installed]
habanalabs-dkms/focal,now 1.12.1-10 all [installed]
habanalabs-firmware-odm/now 1.12.1-10 amd64 [installed,local]
habanalabs-firmware-tools/focal,now 1.12.1-10 amd64 [installed]
habanalabs-firmware/focal,now 1.12.1-10 amd64 [installed]
habanalabs-graph/focal,now 1.12.1-10 amd64 [installed]
habanalabs-qual/focal,now 1.12.1-10 amd64 [installed]
habanalabs-rdma-core/focal,now 1.12.1-10 all [installed]
habanalabs-thunk/focal,now 1.12.1-10 all [installed]
```
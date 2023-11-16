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

asml@gaudi2poc1:~$ uname -r
5.4.0-166-generic
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

### hl-smi

```
asml@gaudi2poc1:~$ hl-smi
+-----------------------------------------------------------------------------+
| HL-SMI Version:                              hl-1.12.1-fw-46.0.5.0          |
| Driver Version:                                     1.12.1-cb7a7bc          |
|-------------------------------+----------------------+----------------------+
| AIP  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | AIP-Util  Compute M. |
|===============================+======================+======================|
|   0  HL-225              N/A  | 0000:43:00.0     N/A |                   0  |
| N/A   30C   N/A    84W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
|   1  HL-225              N/A  | 0000:44:00.0     N/A |                   0  |
| N/A   28C   N/A    87W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
|   2  HL-225              N/A  | 0000:1a:00.0     N/A |                   0  |
| N/A   30C   N/A    93W / 600W |    768MiB / 98304MiB |     1%           N/A |
|-------------------------------+----------------------+----------------------+
|   3  HL-225              N/A  | 0000:19:00.0     N/A |                   0  |
| N/A   27C   N/A    93W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
|   4  HL-225              N/A  | 0000:b3:00.0     N/A |                   0  |
| N/A   26C   N/A    93W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
|   5  HL-225              N/A  | 0000:b4:00.0     N/A |                   0  |
| N/A   27C   N/A    91W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
|   6  HL-225              N/A  | 0000:cc:00.0     N/A |                   0  |
| N/A   27C   N/A    98W / 600W |    768MiB / 98304MiB |     2%           N/A |
|-------------------------------+----------------------+----------------------+
|   7  HL-225              N/A  | 0000:cd:00.0     N/A |                   0  |
| N/A   27C   N/A    85W / 600W |    768MiB / 98304MiB |     0%           N/A |
|-------------------------------+----------------------+----------------------+
| Compute Processes:                                               AIP Memory |
|  AIP       PID   Type   Process name                             Usage      |
|=============================================================================|
|   0        N/A   N/A    N/A                                      N/A        |
|   1        N/A   N/A    N/A                                      N/A        |
|   2        N/A   N/A    N/A                                      N/A        |
|   3        N/A   N/A    N/A                                      N/A        |
|   4        N/A   N/A    N/A                                      N/A        |
|   5        N/A   N/A    N/A                                      N/A        |
|   6        N/A   N/A    N/A                                      N/A        |
|   7        N/A   N/A    N/A                                      N/A        |
+=============================================================================+
```

Details about the first Gaudi2 processor:

```
asml@gaudi2poc1:~$ sudo hl-smi -L -i 0000:43:00.0

================ HL-SMI LOG ================

Timestamp                               : Thu Nov 16 04:54:24 UTC 2023
Driver Version                          : 1.12.1-cb7a7bc
HL-SMI Version                          : hl-1.12.1-fw-46.0.5.0 (Oct 19 2023 - 01:19:50)

Attached AIPs                           : 8

[0] AIP (accel0) 0000:43:00.0
        Product Name                    : HL-225
        Model Number                    : F08GL0AIG032A
        Serial Number                   : AM50016138
        Module status                   : Operational
        Module ID                       : 0
        PCB Assembly Version            : V2A
        PCB Version                     : R0F
        HL Revision                     : 1
        AIP UUID                        : 01P0-HL2080A0-15-TF8A81-09-07-06
        AIP Status                      : Production Level
        Firmware [FIT] Version          : Linux gaudi2 5.10.18-hl-gaudi2-1.12.1-fw-46.0.5-sec-6 #1 SMP PREEMPT Thu Oct 12 22:23:07 IDT 2023 aarch64 GNU/Linux
        Firmware [SPI] Version          : Preboot version hl-gaudi2-1.12.1-fw-46.0.5-sec-6 (Oct 12 2023 - 22:14:34)
        Firmware [UBOOT] Version        : U-Boot 2021.04-hl-gaudi2-1.12.1-fw-46.0.5-sec-6 (Oct 12 2023 - 22:22:29 +0300) build#: 10814
        Firmware [OS] Version           : Zephyr 2.7.2-hl-gaudi2-1.12.1-fw-46.0.5-sec-6 (Oct 12 2023 - 22:14:47)
        CPLD Version                    : 0x0000000d
        PCI
                Bus                     : 0x43
                Device                  : 0x00
                Domain                  : 0x0000
                Rev                     : 01
                Device Id               : 0x1da31020
                Bus Id                  : 0000:43:00.0
                Sub System Id           : 0x1da31020
                AIP Link Info
                        Link Speed
                                Max     : 16GT/s
                                Current : 16GT/s
                        Link Width
                                Max     : x16
                                Current : x16
        Fan Speed
                                        : N/A
        Clocks Throttle Reasons
                HW Slowdown             : Not Active
                        Thermal Slowdown
                                        : Not Active
                        Power Slowdown
                                        : Not Active
        Memory Type                     : HBM
        Memory Usage
                Total                   : 98304 MB
                Used                    : 768 MB
                Free                    : 97536 MB
        Temperature
                [ 1] On Chip 0          :  29 C
                [ 2] On Chip 1          :  29 C
                [ 3] On Chip 2          :  30 C
                [ 4] On Chip 3          :  30 C
                [ 5] HBM 0 Temp         :  29 C
                [ 6] HBM 1 Temp         :  32 C
                [ 7] HBM 2 Temp         :  30 C
                [ 8] HBM 3 Temp         :  29 C
                [ 9] HBM 4 Temp         :  29 C
                [10] HBM 5 Temp         :  29 C
                [11] On Chip TD 0       :  29 C
                [12] On Chip TD 1       :  30 C
                [13] On Chip TD 2       :  29 C
                [14] On Chip TD 3       :  29 C
                [15] On Board 2 Top     :  30 C
                [16] On Board 01 Top    :  30 C
                [17] On Board 01 Bot    :  31 C
                [18] On Board 23 Top    :  30 C
                [19] On Board 23 Bot    :  31 C
                [20] CPLD Temp          :  33 C
                [21] VRM1 Temp          :  34 C
                [22] VRM2 Temp          :  39 C
        Power Readings (54V PSU)
                Power Management        : auto
                Power Draw              : 50 W
                Power Max               : 550 W
                Power Limit             : 550 W
        Power Readings (12V PSU)
                Power Management        : N/A
                Power Draw              : 31 W
                Power Max               : 50 W
                Power Limit             : 50 W
        Clocks
                [1] soc                 : 1650 MHz
        Clocks Max
                [1] soc                 : 1650 MHz
        Clocks Limit
                [1] soc                 : 1650 MHz
                [2] tpc                 : 1800 MHz
        Network Information
                [ 1] MAC                : b0:fd:0b:d7:c4:8d
                [ 2] MAC                : b0:fd:0b:d7:c4:8e
                [ 3] MAC                : b0:fd:0b:d7:c4:8f
                [ 4] MAC                : b0:fd:0b:d7:c4:90
                [ 5] MAC                : b0:fd:0b:d7:c4:91
                [ 6] MAC                : b0:fd:0b:d7:c4:92
                [ 7] MAC                : b0:fd:0b:d7:c4:93
                [ 8] MAC                : b0:fd:0b:d7:c4:94
                [ 9] MAC                : b0:fd:0b:d7:c4:95
                [10] MAC                : b0:fd:0b:d7:c4:96
                [11] MAC                : b0:fd:0b:d7:c4:97
                [12] MAC                : b0:fd:0b:d7:c4:98
                [13] MAC                : b0:fd:0b:d7:c4:99
                [14] MAC                : b0:fd:0b:d7:c4:9a
                [15] MAC                : b0:fd:0b:d7:c4:9b
                [16] MAC                : b0:fd:0b:d7:c4:9c
                [17] MAC                : b0:fd:0b:d7:c4:9d
                [18] MAC                : b0:fd:0b:d7:c4:9e
                [19] MAC                : b0:fd:0b:d7:c4:9f
                [20] MAC                : b0:fd:0b:d7:c4:a0
                [21] MAC                : b0:fd:0b:d7:c4:a1
                [22] MAC                : b0:fd:0b:d7:c4:a2
                [23] MAC                : b0:fd:0b:d7:c4:a3
                [24] MAC                : b0:fd:0b:d7:c4:a4
                [25] MAC                : b0:fd:0b:d7:c4:a5
                [26] MAC                : b0:fd:0b:d7:c4:a6
                [27] MAC                : b0:fd:0b:d7:c4:a7
                [28] MAC                : b0:fd:0b:d7:c4:a8
                [29] MAC                : b0:fd:0b:d7:c4:a9
                [30] MAC                : b0:fd:0b:d7:c4:aa
                [31] MAC                : b0:fd:0b:d7:c4:ab
                [32] MAC                : b0:fd:0b:d7:c4:ac
                [33] MAC                : b0:fd:0b:d7:c4:ad
                [34] MAC                : b0:fd:0b:d7:c4:ae
                [35] MAC                : b0:fd:0b:d7:c4:af
                [36] MAC                : b0:fd:0b:d7:c4:b0
                [37] MAC                : b0:fd:0b:d7:c4:b1
                [38] MAC                : b0:fd:0b:d7:c4:b2
                [39] MAC                : b0:fd:0b:d7:c4:b3
                [40] MAC                : b0:fd:0b:d7:c4:b4
                [41] MAC                : b0:fd:0b:d7:c4:b5
                [42] MAC                : b0:fd:0b:d7:c4:b6
                [43] MAC                : b0:fd:0b:d7:c4:b7
                [44] MAC                : b0:fd:0b:d7:c4:b8
                [45] MAC                : b0:fd:0b:d7:c4:b9
                [46] MAC                : b0:fd:0b:d7:c4:ba
                [47] MAC                : b0:fd:0b:d7:c4:bb
                [48] MAC                : b0:fd:0b:d7:c4:bc
        Replaced Rows
                Single Bit ECC          : 0
                Double Bit ECC          : 0
                Pending                 : No
```

## PyTorch

Install the dependencies:

```
asml@gaudi2poc1:/opt/habanalabs$ cd
asml@gaudi2poc1:~$ ls
habanalabs-firmware-odm-1.12.1-10.amd64.deb  habanalabs-installer.sh
asml@gaudi2poc1:~$ ./habanalabs-installer.sh install -t dependencies
[sudo] password for asml:
================================================================================
Script arguments
================================================================================
args:
- ${args[--type]} = dependencies
================================================================================
Environment
================================================================================
Device                               gaudi2
OS                                   ubuntu
OS version                           20.04
Log file                             /var/log/habana_logs/install-2023-11-16-05-09-27.log
Release version                      1.12.1-10
Habanalabs server                    vault.habana.ai
Rewrite installer config             no
Install type                         dependencies
Python repo URL                      https://vault.habana.ai/artifactory/api/pypi/gaudi-python/simple
Habanalabs software                  [OK]
habanalabs-container-runtime=1.12.1-10
habanalabs-dkms=1.12.1-10
habanalabs-firmware=1.12.1-10
habanalabs-firmware-odm=1.12.1-10
habanalabs-firmware-tools=1.12.1-10
habanalabs-graph=1.12.1-10
habanalabs-qual=1.12.1-10
habanalabs-rdma-core=1.12.1-10
habanalabs-thunk=1.12.1-10
The installed version is: 4.1.5
python3.8-dev                        [OK]
python3-pip                          [OK]
python3.8-venv                       [OK]
cmake                                [OK]
curl                                 [OK]
gcc                                  [OK]
google-perftools                     [OK]
iproute2                             [OK]
libatlas-base-dev                    [OK]
libcairo2-dev                        [OK]
libcurl4                             [OK]
libgl1                               [OK]
libglib2.0-dev                       [OK]
libjemalloc2                         [OK]
libjpeg-dev                          [OK]
liblapack-dev                        [OK]
libnuma-dev                          [OK]
libopenblas-dev                      [OK]
libpcre2-dev                         [OK]
libselinux1-dev                      [OK]
lsof                                 [OK]
moreutils                            [OK]
numactl                              [OK]
protobuf-compiler                    [OK]
unzip                                [OK]
wget                                 [OK]
Do you wish to install [y/n]: y
================================================================================
Install the framework dependencies packages
================================================================================
================================================================================
Full install log: /var/log/habana_logs/install-2023-11-16-05-09-27.log
================================================================================
```

Install PyTorch

```
asml@gaudi2poc1:~$ ./habanalabs-installer.sh install --type pytorch --venv
================================================================================
Script arguments
================================================================================
args:
- ${args[--type]} = pytorch
- ${args[--venv]} = 1
================================================================================
Environment
================================================================================
Device                               gaudi2
OS                                   ubuntu
OS version                           20.04
Log file                             /home/asml/.habana_logs/install-2023-11-16-05-10-58.log
Release version                      1.12.1-10
Habanalabs server                    vault.habana.ai
Rewrite installer config             no
Install type                         pytorch
Virtual environment path             /home/asml/habanalabs-venv
Python repo URL                      https://vault.habana.ai/artifactory/api/pypi/gaudi-python/simple
Habanalabs software                  [OK]
habanalabs-container-runtime=1.12.1-10
habanalabs-dkms=1.12.1-10
habanalabs-firmware=1.12.1-10
habanalabs-firmware-odm=1.12.1-10
habanalabs-firmware-tools=1.12.1-10
habanalabs-graph=1.12.1-10
habanalabs-qual=1.12.1-10
habanalabs-rdma-core=1.12.1-10
habanalabs-thunk=1.12.1-10
================================================================================
Check PyTorch packages and dependencies
================================================================================
The installed version is: 4.1.5
Python 3.8                           [OK]
~/habanalabs-venv ~
~
PyTorch package was not detected.
gcc                                  [OK]
cmake                                [OK]
lsof                                 [OK]
curl                                 [OK]
wget                                 [OK]
unzip                                [OK]
libcurl4                             [OK]
moreutils                            [OK]
iproute2                             [OK]
libcairo2-dev                        [OK]
libglib2.0-dev                       [OK]
libselinux1-dev                      [OK]
libnuma-dev                          [OK]
libpcre2-dev                         [OK]
libatlas-base-dev                    [OK]
libjpeg-dev                          [OK]
liblapack-dev                        [OK]
libnuma-dev                          [OK]
google-perftools                     [OK]
numactl                              [OK]
libopenblas-dev                      [OK]
python3.8-dev                        [OK]
python3-pip                          [OK]
python3.8-venv                       [OK]
Detected Habanalabs packages
Do you wish to install [y/n]: y
================================================================================
Install Habanalabs PyTorch python modules
================================================================================
WARNING: Skipping pillow-simd as it is not installed.
================================================================================
Set up Habanalabs PyTorch environment
================================================================================
================================================================================
Validate Habanalabs PyTorch installation
================================================================================
============================= HABANA PT BRIDGE CONFIGURATION ===========================
 PT_HPU_LAZY_MODE = 1
 PT_RECIPE_CACHE_PATH =
 PT_CACHE_FOLDER_DELETE = 0
 PT_HPU_RECIPE_CACHE_CONFIG =
 PT_HPU_MAX_COMPOUND_OP_SIZE = 9223372036854775807
 PT_HPU_LAZY_ACC_PAR_MODE = 1
 PT_HPU_ENABLE_REFINE_DYNAMIC_SHAPES = 0
---------------------------: System Configuration :---------------------------
Num CPU Cores : 160
CPU RAM       : 1056459720 KB
------------------------------------------------------------------------------
Loading Habana modules from /home/asml/habanalabs-venv/lib/python3.8/site-packages/habana_frameworks/torch/lib
Habanalabs PyTorch test was completed successfully
================================================================================
Attention!
================================================================================
All Python modules were installed in Python virtual environment.
To activate a virtual environment, please perform the following:
1. Change the current working directory: cd /home/asml/habanalabs-venv
2. Run: source ./bin/activate
================================================================================
Full install log: /home/asml/.habana_logs/install-2023-11-16-05-10-58.log
================================================================================
```

Activate the venv:

```
asml@gaudi2poc1:~$ source ~/habanalabs-venv/bin/activate
(habanalabs-venv) asml@gaudi2poc1:~$ which python
/home/asml/habanalabs-venv/bin/python
```
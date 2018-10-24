---
title: "Introduction 2"
teaching: 0
exercises: 0
questions:
- "Key question"
objectives:
- "First objective."
keypoints:
- "First key point."
---

### What are some of reasons to access a remote computer system?

-   Your computer does not have enough resources to run the desired
    analysis (memory, processors, disk space, network bandwidth).
-   You want to produce results faster than your computer can.
-   You cannot install software in your computer (application does not
    have support for your operating system, conflicts with other
    existing applications)

### Main Computational Resources

![Computer
Node](https://raw.githubusercontent.com/datacarpentry/cloud-genomics/gh-pages/lessons/images/Computer.png)

-   Find how much disk space is available in your system. Check that you
    have enough space to get and extract the input file of :

<!-- -->

    ssh account@datacarpentry.host
    df -h

    wget http://www.hpa-bioinformatics.org.uk/lgp/resource/Sample280.fastq.gz
    ls -lah Sample280.fastq.gz

    gunzip Sample280.fastq.gz
    ls -lah Sample280.fastq

The output on iPlant Atmosphere:

    --2015-03-25 09:40:33--  http://www.hpa-bioinformatics.org.uk/lgp/resource/Sample280.fastq.gz
    Resolving www.hpa-bioinformatics.org.uk... 194.74.226.185
    Connecting to www.hpa-bioinformatics.org.uk|194.74.226.185|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 215949113 (206M) [application/x-gzip]
    Saving to: \u201cSample280.fastq.gz\u201d

    100%[==========================================================================================>] 215,949,113  213K/s   in 16m 48s 

    2015-03-25 09:57:21 (209 KB/s) - \u201cSample280.fastq.gz\u201d saved [215949113/215949113]

    -rw-r--r-- 1 dnasubway01 iplant-everyone 206M Jun 29  2011 Sample280.fastq.gz

    -rw-r--r-- 1 dnasubway01 iplant-everyone 627M Jun 29  2011 Sample280.fastq

The output on Amazon EC2:

    --2015-03-25 15:30:23--  http://www.hpa-bioinformatics.org.uk/lgp/resource/Sample280.fastq.gz
    Resolving www.hpa-bioinformatics.org.uk (www.hpa-bioinformatics.org.uk)... 194.74.226.185
    Connecting to www.hpa-bioinformatics.org.uk (www.hpa-bioinformatics.org.uk)|194.74.226.185|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 215949113 (206M) [application/x-gzip]
    Saving to: \u2018Sample280.fastq.gz\u2019

    Sample280.fastq.gz                100%[===============================================================>] 205.94M   376KB/s   in 9m 23s 

    2015-03-25 15:39:46 (375 KB/s) - \u2018Sample280.fastq.gz\u2019 saved [215949113/215949113]

    -rw-rw-r-- 1 ec2-user ec2-user 206M Jun 29  2011 Sample280.fastq.gz

    -rw-rw-r-- 1 ec2-user ec2-user 627M Jun 29  2011 Sample280.fastq

The output on Azure:

    --2015-03-25 14:37:18--  http://www.hpa-bioinformatics.org.uk/lgp/resource/Sample280.fastq.gz
    Resolving www.hpa-bioinformatics.org.uk... 194.74.226.185
    Connecting to www.hpa-bioinformatics.org.uk|194.74.226.185|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 215949113 (206M) [application/x-gzip]
    Saving to: \u201cSample280.fastq.gz\u201d

    100%[===========================================================================================================================================>] 215,949,113  216K/s   in 16m 26s 

    2015-03-25 14:53:44 (214 KB/s) - \u201cSample280.fastq.gz\u201d saved [215949113/215949113]

    -rw-r--r--. 1 root root 206M Jun 29  2011 Sample280.fastq.gz

    -rw-r--r--. 1 root root 627M Mar 25 14:57 Sample280.fastq

-   Find how many cores and processors are available in your system:

<!-- -->

    cat /proc/cpuinfo

    processor  : 0
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 44
    model name  : Westmere E56xx/L56xx/X56xx (Nehalem-C)
    stepping    : 1
    cpu MHz     : 2400.084
    cache size  : 4096 KB
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 11
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good unfair_spinlock pni pclmulqdq vmx ssse3 cx16 pcid sse4_1 sse4_2 popcnt aes hypervisor lahf_lm
    bogomips    : 4800.16
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 40 bits physical, 48 bits virtual
    power management:

    processor   : 1
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 44
    model name  : Westmere E56xx/L56xx/X56xx (Nehalem-C)
    stepping    : 1
    cpu MHz     : 2400.084
    cache size  : 4096 KB
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 11
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good unfair_spinlock pni pclmulqdq vmx ssse3 cx16 pcid sse4_1 sse4_2 popcnt aes hypervisor lahf_lm
    bogomips    : 4800.16
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 40 bits physical, 48 bits virtual
    power management:

On an Amazon EC2 instance, the following output shows availability of 2
Intel Xeon processors:

    cat /proc/cpuinfo 
    processor  : 0
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 62
    model name  : Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
    stepping    : 4
    microcode   : 0x415
    cpu MHz     : 2494.060
    cache size  : 25600 KB
    physical id : 0
    siblings    : 2
    core id     : 0
    cpu cores   : 2
    apicid      : 0
    initial apicid  : 0
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
    bogomips    : 4988.12
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 46 bits physical, 48 bits virtual
    power management:

    processor   : 1
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 62
    model name  : Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
    stepping    : 4
    microcode   : 0x415
    cpu MHz     : 2494.060
    cache size  : 25600 KB
    physical id : 0
    siblings    : 2
    core id     : 1
    cpu cores   : 2
    apicid      : 2
    initial apicid  : 2
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
    bogomips    : 4988.12
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 46 bits physical, 48 bits virtual
    power management:

On an Microsoft Azure instance, the following output shows availability
of 2 Intel Xeon processors:

    processor  : 0
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 63
    model name  : Intel(R) Xeon(R) CPU E5-2698B v3 @ 2.00GHz
    stepping    : 2
    cpu MHz     : 1995.365
    cache size  : 40960 KB
    physical id : 0
    siblings    : 4
    core id     : 0
    cpu cores   : 4
    apicid      : 0
    initial apicid  : 0
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc rep_good unfair_spinlock pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 erms
    bogomips    : 3990.73
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 42 bits physical, 48 bits virtual
    power management:

    processor   : 1
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 63
    model name  : Intel(R) Xeon(R) CPU E5-2698B v3 @ 2.00GHz
    stepping    : 2
    cpu MHz     : 1995.365
    cache size  : 40960 KB
    physical id : 0
    siblings    : 4
    core id     : 1
    cpu cores   : 4
    apicid      : 1
    initial apicid  : 1
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc rep_good unfair_spinlock pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 erms
    bogomips    : 3990.73
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 42 bits physical, 48 bits virtual
    power management:

    processor   : 2
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 63
    model name  : Intel(R) Xeon(R) CPU E5-2698B v3 @ 2.00GHz
    stepping    : 2
    cpu MHz     : 1995.365
    cache size  : 40960 KB
    physical id : 0
    siblings    : 4
    core id     : 2
    cpu cores   : 4
    apicid      : 2
    initial apicid  : 2
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc rep_good unfair_spinlock pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 erms
    bogomips    : 3990.73
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 42 bits physical, 48 bits virtual
    power management:

    processor   : 3
    vendor_id   : GenuineIntel
    cpu family  : 6
    model       : 63
    model name  : Intel(R) Xeon(R) CPU E5-2698B v3 @ 2.00GHz
    stepping    : 2
    cpu MHz     : 1995.365
    cache size  : 40960 KB
    physical id : 0
    siblings    : 4
    core id     : 3
    cpu cores   : 4
    apicid      : 3
    initial apicid  : 3
    fpu     : yes
    fpu_exception   : yes
    cpuid level : 13
    wp      : yes
    flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc rep_good unfair_spinlock pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 erms
    bogomips    : 3990.73
    clflush size    : 64
    cache_alignment : 64
    address sizes   : 42 bits physical, 48 bits virtual
    power management:

-   Find how many processes are running in your system, and how much
    resources is each consuming:

<!-- -->

    top

-   Find how long it takes to run an application:

<!-- -->

    time <your_application>

-   For example, let's measure the time it takes to separate the R1
    sequences into a separate file:

<!-- -->

    time awk "/\/1$/{print; getline; print}" Sample280.fastq > Sample280_1.fastq

The output on iPlant Atmosphere, Amazon EC2, and Microsoft Azure are
shown respectively below, indicating that EC2 took half of the time of
Azure.

    real  0m20.893s
    user    0m19.573s
    sys 0m1.309s

    real  0m14.856s
    user    0m14.260s
    sys 0m0.592s

    real  0m36.294s
    user    0m12.762s
    sys 0m0.432s

### Distributed System Definitions and stacks:

(Note that many definitions exist for these terms)

-   Distributed application: an application that can be executed on a
    distributed system platform (e.g., mpiBLAST)
-   Distributed system platform: software layers that facilitates
    coordination and management of a distributed system (e.g.,
    queue-based system, and MapReduce)
-   Distributed system:
-   High Performance Computing (HPC): large assemble of physical
    machines and a homogeneous operating system (e.g., your
    institutions' HPC, XSEDE's HPC)
-   Cloud Computing: virtual machines, distributed platforms and/or
    applications offered as a service (e.g., Amazon Web Services,
    Microsoft Azure, Google Cloud Computing)

-   Virtual machine (VM): software computer that like a physical
    computer can run an operating system and applications
-   Operating system (OS): the basic software layer that allows
    execution and management of applications
-   Physical machine: the hardware (processors, memory, disk
    and network)

### HPC vs. Cloud:

<table style="width:19%;">
<colgroup>
<col width="8%" />
<col width="11%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HPC</th>
<th align="left">Cloud</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">User account on the system</td>
<td align="left">root account on the system</td>
</tr>
<tr class="even">
<td align="left">Limited control of the system</td>
<td align="left">Full control of the system</td>
</tr>
<tr class="odd">
<td align="left">Central shared file system</td>
<td align="left">Local file system</td>
</tr>
<tr class="even">
<td align="left">Jobs submitted into a queue</td>
<td align="left">Jobs executed on each resource</td>
</tr>
<tr class="odd">
<td align="left">Account-based isolation</td>
<td align="left">OS-based isolation</td>
</tr>
<tr class="even">
<td align="left">Batch-oriented execution of applications</td>
<td align="left">support for batch or interactive applications</td>
</tr>
<tr class="odd">
<td align="left">Request for resource and time allocation</td>
<td align="left">Pay-per-use</td>
</tr>
<tr class="even">
<td align="left">etc.</td>
<td align="left">etc.</td>
</tr>
</tbody>
</table>

![HPC vs.
Cloud](https://raw.githubusercontent.com/datacarpentry/cloud-genomics/gh-pages/lessons/images/HpcVsCloud.png)

### Resources:

-   Cloud computing offerings:
-   Amazon EC2: <http://aws.amazon.com/ec2/>
-   Microsoft Azure: <https://azure.microsoft.com/en-us/>
-   Google Cloud Platform: <https://cloud.google.com/>
-   CyVerse (was iPlant) Atmosphere:
    <http://www.cyverse.org/atmosphere>
-   CyVerse Atmosphere help page:
    <https://pods.iplantcollaborative.org/wiki/display/atmman/Atmosphere+Manual+Table+of+Contents>
-   HPC offerings:
-   XSEDE: <https://www.xsede.org/high-performance-computing>

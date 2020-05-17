# Maîtrise de poste - Day 1

## Self-footprinting

### Host OS

🌞 Déterminer les principales informations de votre machine

 - nom de la machine

```
→ hostname
adrienit
```

<span style="color:green">

→ **Nom de la machine** = adrienit

</span>

 - OS et version

```
→ cat /etc/os-release
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
ID=kali
VERSION="2020.1"
VERSION_ID="2020.1"
VERSION_CODENAME="kali-rolling"
ID_LIKE=debian
ANSI_COLOR="1;31"
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
```

<span style="color:green">

→ **PRETTY_NAME**="Kali GNU/Linux Rolling"
→ **VERSION**="2020.1"

</span>

 - architecture processeur (32-bit, 64-bit, ARM, etc)

```
→ uname -a
Linux adrienit 5.4.0-kali4-amd64 #1 SMP Debian 5.4.19-1kali1 (2020-02-17) x86_64 GNU/Linux
```

<span style="color:green">

→ **Architecture processeur** = x86_64 GNU/Linux

</span>

 - modèle du processeur

```
lscpu | grep "Model name"
Model name: Intel(R) Core(TM) i7-8565U CPU @ 1.80GHz
```

<span style="color:green">

→ **Modele du processeur** = Intel(R) Core(TM) i7-8565U CPU @ 1.80GHz

</span>

 - quantité RAM et modèle de la RAM

On a plusieurs méthodes : 

Avec htop, c'est la mémoire en live

Avec screen fetch, qui donne beaucoup d'info : 

Ou avec la commande dmidecode : 

```
dmidecode --type memory
# dmidecode 3.2
Getting SMBIOS data from sysfs.
SMBIOS 3.2.1 present.
# SMBIOS implementations newer than version 3.2.0 are not
# fully supported by this version of dmidecode.

Handle 0x003A, DMI type 16, 23 bytes
Physical Memory Array
        Location: System Board Or Motherboard
        Use: System Memory
        Error Correction Type: None
        Maximum Capacity: 8 GB
        Error Information Handle: Not Provided
        Number Of Devices: 2

Handle 0x003B, DMI type 17, 40 bytes
Memory Device
        Array Handle: 0x003A
        Error Information Handle: Not Provided
        Total Width: 64 bits
        Data Width: 64 bits
        Size: 4096 MB
        Form Factor: SODIMM
        Set: None
        Locator: ChannelA-DIMM0
        Bank Locator: BANK 0
        Type: DDR4
        Type Detail: Synchronous
        Speed: 2667 MT/s
        Manufacturer: SK Hynix
        Serial Number: 00000000
        Asset Tag: 9876543210
        Part Number: HMA851S6CJR6N-VK
        Rank: 1
        Configured Memory Speed: 2400 MT/s
        Minimum Voltage: 1.2 V
        Maximum Voltage: 1.2 V
        Configured Voltage: 1.2 V

Handle 0x003C, DMI type 17, 40 bytes
Memory Device
        Array Handle: 0x003A
        Error Information Handle: Not Provided
        Total Width: 64 bits
        Data Width: 64 bits
        Size: 4096 MB
        Form Factor: SODIMM
        Set: None
        Locator: ChannelB-DIMM0
        Bank Locator: BANK 2
        Type: DDR4
        Type Detail: Synchronous
        Speed: 2667 MT/s
        Manufacturer: SK Hynix
        Serial Number: 00000000
        Asset Tag: 9876543210
        Part Number: HMA851S6CJR6N-VK
        Rank: 1
        Configured Memory Speed: 2400 MT/s
        Minimum Voltage: 1.2 V
        Maximum Voltage: 1.2 V
        Configured Voltage: 1.2 V
```

<span style="color:green">

→ **Quantité Ram** = Maximum Capacity: 8 GB
→ **Modèle de la RAM** = Manufacturer: SK Hynix

</span>

### Device 

🌞 Trouver

 - la marque et le modèle de votre processeur

```
→ lscpu
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   39 bits physical, 48 bits virtual
CPU(s):                          8
On-line CPU(s) list:             0-7
Thread(s) per core:              2
Core(s) per socket:              4
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       GenuineIntel
CPU family:                      6
Model:                           142
Model name:                      Intel(R) Core(TM) i7-8565U CPU @ 1.80GHz
Stepping:                        11
CPU MHz:                         1770.004
CPU max MHz:                     4600.0000
CPU min MHz:                     400.0000
BogoMIPS:                        3999.93
Virtualization:                  VT-x
L1d cache:                       128 KiB
L1i cache:                       128 KiB
L2 cache:                        1 MiB
L3 cache:                        8 MiB
NUMA node0 CPU(s):               0-7
Vulnerability Itlb multihit:     KVM: Mitigation: Split huge pages
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Mitigation; Clear CPU buffers; SMT vulnerable
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Full generic retpoline, IBPB conditional, IBRS_FW, STIBP conditional, RSB filling
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl 
                                 xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand l
                                 ahf_lm abm 3dnowprefetch cpuid_fault epb invpcid_single ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx smap clflush
                                 opt intel_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp md_clear flush_l1d arch_capabilities
```

<span style="color:green">

→ **Marque du CPU** = Intel
→ **Modele du CPU** = i7-8565U

</span>

 - - identifier le nombre de processeurs, le nombre de coeur

<span style="color:green">

→ J'ai **2 Threads** avec **4 coeurs**, ce qui me donne **8 Coeurs et 2 Processeurs**

</span>

 - - si c'est un proc Intel, expliquer le nom du processeur (oui le nom veut dire quelque chose)

<span style="color:green">

→ **Intel**
→ **Nom du processeur** = Core (Grand public)
→ **i3/5/7/9** = La puissance de la gamme
→ **8565** = Plus c'est grand plus c'est puissant
→ **U** = Utilisation destinée au ordi portable.

</span>

 - la marque et le modèle :

 - - de votre touchpad/trackpad

```
→ cat /proc/bus/input/devices| grep Touchpad
N: Name="SYNA7DAB:00 06CB:CD40 Touchpad"
```

<span style="color:green">

→ **SYNA7DAB:00 06CB:CD40**

</span>

 - - de vos enceintes intégrées

```
→ cat /proc/asound/cards
0 [PCH            ]: HDA-Intel - HDA Intel PCH
                     HDA Intel PCH at 0xa1218000 irq 147
```
<span style="color:green">

→ **HDA Intel PCH**

</span>

 - - de votre disque dur principal

```
→ fdisk -l
Disk /dev/nvme0n1: 238.49 GiB, 256060514304 bytes, 500118192 sectors
Disk model: Skhynix BC501 NVMe 256GB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: B3F616DE-1BCC-4108-9754-E14C4F7DAD2A

Device             Start       End   Sectors  Size Type
/dev/nvme0n1p1      2048    206847    204800  100M EFI System
/dev/nvme0n1p2    206848    239615     32768   16M Microsoft reserved
/dev/nvme0n1p3    239616 354641919 354402304  169G Microsoft basic data
/dev/nvme0n1p4 498003968 500101119   2097152    1G Windows recovery environment
/dev/nvme0n1p5 354641920 481538047 126896128 60.5G Linux filesystem
/dev/nvme0n1p6 481538048 498003967  16465920  7.9G Linux swap
```

<span style="color:green">

→ **Disque du principale** = Linux filesystem

</span>

🌞 Disque dur

On refait un fdisk et on sort les infos : 

 - identifier les différentes partitions de votre/vos disque(s) dur(s)

→ EFI System
→ Microsoft reserved
→ Microsoft basic data
→ Windows recovery environment
→ Linux filesystem
→ Linux swap

 - déterminer le système de fichier de chaque partition

Pour windows : Microsoft reserved
Pour linux : Linux filesystem

 - expliquer la fonction de chaque partition

→ EFI System
Destiné pour les fichers système ou les fichiers qui ont l'extension ".efi"
Ou encore les applications utilisées par le microprogramme au démarrage. 

→ Microsoft reserved

X

→ Microsoft basic data
Les fichiers de l'utilisateur

→ Windows recovery environment
Cette partition permet "de revenir dans le temps", en gros ça permet à l'ordinateur de revenir dans un état antérieur s'il a subit des evenements négatifs 

→ Linux filesystem
Les fichiers de l'utilisateur

→ Linux swap
C'est la partition qui qui sert à décharger la RAM de l'ordi lorsqu'elle arrive à saturation.

### Network

🌞 Afficher la liste des cartes réseau de votre machine

```
→ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 04:ea:56:21:65:4c brd ff:ff:ff:ff:ff:ff
    inet 192.168.20.116/24 brd 192.168.20.255 scope global dynamic noprefixroute wlan0
       valid_lft 76264sec preferred_lft 76264sec
    inet6 fe80::2a56:c619:2a93:e1bb/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

 - expliquer la fonction de chacune d'entre elles

1: lo = L'interface loopback, c'est toujours la meme ip : 127.0.0.1
"**lo** pour **lo**opback"
2: wlan0 = L'interface wifi, ip local qui change
"**wl**an0 pour **W**ire**L**ess"

🌞 Lister tous les ports TCP et UDP en utilisation

*Il faut savoir que cette liste peut changer en fonction de c'qui tourne sur le PC.*

```
→ ss -tu (t = TCP et u = UDP)
Netid                 State                  Recv-Q                  Send-Q                                          Local Address:Port                                        Peer Address:Port                   Process
udp                   ESTAB                  0                       0                                        192.168.20.116%wlan0:bootpc                                      192.168.20.1:bootps
tcp                   ESTAB                  0                       0                                              192.168.20.116:54942                                    162.159.136.234:https
```

 - déterminer quel programme tourne derrière chacun des ports

```
ss -tup (p pour process)
Netid              State              Recv-Q              Send-Q                                  Local Address:Port                                Peer Address:Port               Process
udp                ESTAB              0                   0                                192.168.20.116%wlan0:bootpc                              192.168.20.1:bootps              users:(("NetworkManager",pid=659,fd=23))
tcp                ESTAB              0                   0                                      192.168.20.116:54942                            162.159.136.234:https               users:(("firefox-esr",pid=1355,fd=184))
```
On remarque que c'est NetworkManager et Firefox qui tourne derriere les 2 ports

 - expliquer la fonction de chacun de ces programmes
NetworkManager = C'est l'outil de gestion des connexions réseau
Firefox-esr = mon naviguateur

### Users

🌞 Déterminer la liste des utilisateurs de la machine

 - la liste complète des utilisateurs de la machine (je vous vois les Windowsiens...)

```
awk -F: '{ print $1}' /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy
www-data
backup
list
irc
gnats
nobody
_apt
systemd-timesync
systemd-network
systemd-resolve
mysql
ntp
messagebus
uuidd
redsocks
rwhod
iodine
tcpdump
miredo
dnsmasq
usbmux
rtkit
_rpc
Debian-snmp
statd
postgres
stunnel4
sshd
sslh
pulse
avahi
saned
inetsim
colord
geoclue
lightdm
king-phisher
systemd-coredump
tss
strongswan
nm-openvpn
nm-openconnect
```

*Stp me juge pas je suis en root, faut que je créé mon user*

 - déterminer le nom de l'utilisateur qui est full admin sur la machine

C'est pour cela que je n'ai pas de user appartenant au group sudo ou root : 

```
→ getent group sudo
→ getent group root
```

### Processus

🌞 Déterminer la liste des processus de la machine

```
ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.1 166460 10808 ?        Ss   12:08   0:02 /sbin/init splash
root           2  0.0  0.0      0     0 ?        S    12:08   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   12:08   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   12:08   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/0:0H-events_highpri]
root           9  0.0  0.0      0     0 ?        I<   12:08   0:00 [mm_percpu_wq]
root          10  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/0]
root          11  0.0  0.0      0     0 ?        I    12:08   0:02 [rcu_sched]
root          12  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/0]
root          13  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/0]
root          14  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/1]
root          15  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/1]
root          16  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/1]
root          18  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/1:0H-events_highpri]
root          19  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/2]
root          20  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/2]
root          21  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/2]
root          23  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/2:0H-kblockd]
root          24  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/3]
root          25  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/3]
root          26  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/3]
root          28  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/3:0H-events_highpri]
root          29  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/4]
root          30  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/4]
root          31  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/4]
root          33  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/4:0H-events_highpri]
root          34  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/5]
root          35  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/5]
root          36  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/5]
root          38  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/5:0H-kblockd]
root          39  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/6]
root          40  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/6]
root          41  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/6]
root          43  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/6:0H-kblockd]
root          44  0.0  0.0      0     0 ?        S    12:08   0:00 [cpuhp/7]
root          45  0.0  0.0      0     0 ?        S    12:08   0:00 [migration/7]
root          46  0.0  0.0      0     0 ?        S    12:08   0:00 [ksoftirqd/7]
root          48  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/7:0H-kblockd]
root          49  0.0  0.0      0     0 ?        S    12:08   0:00 [kdevtmpfs]
root          50  0.0  0.0      0     0 ?        I<   12:08   0:00 [netns]
root          51  0.0  0.0      0     0 ?        S    12:08   0:00 [kauditd]
root          53  0.0  0.0      0     0 ?        S    12:08   0:00 [khungtaskd]
root          54  0.0  0.0      0     0 ?        S    12:08   0:00 [oom_reaper]
root          55  0.0  0.0      0     0 ?        I<   12:08   0:00 [writeback]
root          56  0.0  0.0      0     0 ?        S    12:08   0:00 [kcompactd0]
root          57  0.0  0.0      0     0 ?        SN   12:08   0:00 [ksmd]
root          58  0.0  0.0      0     0 ?        SN   12:08   0:00 [khugepaged]
root          64  0.0  0.0      0     0 ?        I    12:08   0:00 [kworker/6:1-events]
root         106  0.0  0.0      0     0 ?        I<   12:08   0:00 [kintegrityd]
root         107  0.0  0.0      0     0 ?        I<   12:08   0:00 [kblockd]
root         108  0.0  0.0      0     0 ?        I<   12:08   0:00 [blkcg_punt_bio]
root         110  0.0  0.0      0     0 ?        I    12:08   0:00 [kworker/4:1-events]
root         111  0.0  0.0      0     0 ?        I<   12:08   0:00 [edac-poller]
root         112  0.0  0.0      0     0 ?        I<   12:08   0:00 [devfreq_wq]
root         113  0.0  0.0      0     0 ?        S    12:08   0:00 [kswapd0]
root         114  0.0  0.0      0     0 ?        I<   12:08   0:00 [kthrotld]
root         115  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/122-pciehp]
root         116  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/123-aerdrv]
root         117  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/123-pcie-dp]
root         118  0.0  0.0      0     0 ?        I<   12:08   0:00 [acpi_thermal_pm]
root         123  0.0  0.0      0     0 ?        I<   12:08   0:00 [ipv6_addrconf]
root         134  0.0  0.0      0     0 ?        I<   12:08   0:00 [kstrp]
root         138  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/u17:0-rb_allocator]
root         193  0.0  0.0      0     0 ?        I<   12:08   0:00 [nvme-wq]
root         194  0.0  0.0      0     0 ?        I<   12:08   0:00 [ata_sff]
root         195  0.0  0.0      0     0 ?        I<   12:08   0:00 [nvme-reset-wq]
root         196  0.0  0.0      0     0 ?        I<   12:08   0:00 [nvme-delete-wq]
root         199  0.0  0.0      0     0 ?        I<   12:08   0:00 [cryptd]
root         216  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_0]
root         217  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_0]
root         218  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_1]
root         219  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_1]
root         220  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_2]
root         221  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_2]
root         222  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_3]
root         223  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_3]
root         224  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_4]
root         225  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_4]
root         227  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_5]
root         228  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_5]
root         229  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_6]
root         230  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_6]
root         231  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_7]
root         232  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_7]
root         233  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_8]
root         234  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_8]
root         235  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_9]
root         236  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_9]
root         237  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_10]
root         238  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_10]
root         239  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_11]
root         240  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_11]
root         241  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_12]
root         242  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_12]
root         243  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_13]
root         244  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_13]
root         245  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_14]
root         246  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_14]
root         247  0.0  0.0      0     0 ?        S    12:08   0:00 [scsi_eh_15]
root         248  0.0  0.0      0     0 ?        I<   12:08   0:00 [scsi_tmf_15]
root         257  0.0  0.0      0     0 ?        I    12:08   0:01 [kworker/u16:13-events_unbound]
root         325  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/2:1H]
root         327  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/3:1H-events_highpri]
root         329  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/109-SYNA7DA]
root         335  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/50-MELF0411]
root         342  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/0:1H-kblockd]
root         374  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/7:1H-events_highpri]
root         379  0.0  0.0      0     0 ?        S    12:08   0:00 [jbd2/nvme0n1p5-]
root         380  0.0  0.0      0     0 ?        I<   12:08   0:00 [ext4-rsv-conver]
root         428  0.0  0.3  49968 28256 ?        Ss   12:08   0:01 /lib/systemd/systemd-journald
root         435  0.0  0.0      0     0 ?        I<   12:08   0:00 [rpciod]
root         436  0.0  0.0      0     0 ?        I<   12:08   0:00 [xprtiod]
root         440  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/6:1H-events_highpri]
root         442  0.0  0.0  22576  6896 ?        Ss   12:08   0:00 /lib/systemd/systemd-udevd
root         481  0.0  0.0      0     0 ?        I<   12:08   0:00 [tpm_dev_wq]
root         493  0.0  0.0      0     0 ?        I<   12:08   0:00 [cfg80211]
root         494  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/136-mei_me]
root         495  0.0  0.0      0     0 ?        S    12:08   0:00 [watchdogd]
root         526  0.0  0.0      0     0 ?        S    12:08   0:02 [irq/137-iwlwifi]
root         528  0.0  0.0      0     0 ?        S    12:08   0:01 [irq/138-iwlwifi]
root         529  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/139-iwlwifi]
root         530  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/140-iwlwifi]
root         531  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/141-iwlwifi]
root         532  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/142-iwlwifi]
root         535  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/143-iwlwifi]
root         536  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/144-iwlwifi]
root         537  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/145-iwlwifi]
root         538  0.0  0.0      0     0 ?        S    12:08   0:00 [irq/146-iwlwifi]
root         592  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/5:1H-events_highpri]
root         593  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/u17:1-rb_allocator]
root         631  0.0  0.0   8088  4896 ?        Ss   12:08   0:02 /usr/sbin/haveged --Foreground --verbose=1 -w 1024
root         652  0.0  0.1 313056 10548 ?        Ssl  12:08   0:00 /usr/sbin/ModemManager --filter-policy=strict
root         654  0.0  0.1  11288 10660 ?        S<Ls 12:08   0:01 /usr/bin/atop -R -w /var/log/atop/atop_20200429 600
root         656  0.0  0.0   6508  2836 ?        Ss   12:08   0:00 /usr/sbin/cron -f
message+     658  0.0  0.0   8344  5204 ?        Ss   12:08   0:01 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
root         659  0.0  0.2 326176 16736 ?        Ssl  12:08   0:02 /usr/sbin/NetworkManager --no-daemon
root         662  0.0  0.0 221776  4256 ?        Ssl  12:08   0:00 /usr/sbin/rsyslogd -n -iNONE
root         664  0.0  0.0  10000  5108 ?        Ss   12:08   0:00 /usr/sbin/smartd -n
root         698  0.0  0.1 237632  9604 ?        Ssl  12:08   0:00 /usr/lib/policykit-1/polkitd --no-debug
root         713  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/4:1H-events_highpri]
root         714  0.0  0.0      0     0 ?        I<   12:08   0:00 [kworker/1:1H-kblockd]
root         769  0.0  0.0 236280  7312 ?        SLsl 12:08   0:00 /usr/sbin/lightdm
root         774  0.0  0.0  15988  7792 ?        Ss   12:08   0:00 /lib/systemd/systemd-logind
root         787  0.0  0.1  13720  8836 ?        Ss   12:08   0:00 /sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
root         794  0.7  1.7 906808 139044 tty7    Rsl+ 12:08   1:27 /usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
root         795  0.0  0.0   2640  1632 tty1     Ss+  12:08   0:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
rtkit        859  0.0  0.0 152684  2860 ?        SNsl 12:08   0:00 /usr/libexec/rtkit-daemon
root         875  0.0  0.0 163528  7936 ?        Sl   12:08   0:00 lightdm --session-child 14 23
root         898  0.0  0.1  18060  9564 ?        Ss   12:08   0:00 /lib/systemd/systemd --user
root         899  0.0  0.0 167380  2760 ?        S    12:08   0:00 (sd-pam)
root         909  0.0  0.3 632676 24332 ?        S<sl 12:08   0:00 /usr/bin/pulseaudio --daemonize=no
root         915  0.0  0.4 292940 35024 ?        Ssl  12:08   0:02 xfce4-session
root         923  0.0  0.0   7576  4856 ?        Ss   12:08   0:04 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
root         948  0.0  0.0   5864   472 ?        Ss   12:08   0:00 /usr/bin/ssh-agent x-session-manager
root         958  0.0  0.0 309084  6408 ?        Ssl  12:08   0:00 /usr/lib/at-spi2-core/at-spi-bus-launcher
root         963  0.0  0.0   7176  4428 ?        S    12:08   0:00 /usr/bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-address 3
root         967  0.0  0.0 229912  6004 ?        Sl   12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/xfconf/xfconfd
root         973  0.0  0.0 170800  6956 ?        Sl   12:08   0:00 /usr/lib/at-spi2-core/at-spi2-registryd --use-gnome-session
root         979  0.0  0.0  80928  3400 ?        SLs  12:08   0:00 /usr/bin/gpg-agent --supervised
root         981  0.2  0.8 606188 68704 ?        Sl   12:08   0:27 xfwm4 --display :0.0 --sm-client-id 2302f204e-cb01-480e-bf23-76b44a65e3df
root         984  0.0  0.0 236700  7376 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfsd
root        1005  0.0  0.2 239416 19840 ?        Ssl  12:08   0:00 xfsettingsd --display :0.0 --sm-client-id 2abb5e03f-70e1-4fe4-b925-6a7d7854a37e
root        1029  0.0  0.1 249356  9696 ?        Ssl  12:08   0:02 /usr/lib/upower/upowerd
root        1255  0.2  0.6 334380 49064 ?        Sl   12:08   0:28 xfce4-panel --display :0.0 --sm-client-id 29d24cd20-4c4b-49a8-9d6b-b9aa8aa9a24a
root        1266  0.0  0.5 416800 47100 ?        Sl   12:08   0:01 Thunar --sm-client-id 2e7e10d1e-8096-4b63-bd21-7e7baa968989 --daemon
root        1273  0.0  1.3 516384 106172 ?       Sl   12:08   0:03 xfdesktop --display :0.0 --sm-client-id 2d46805b3-8211-40fe-a0a7-56dfa2133484
root        1274  0.0  0.1 346012 11452 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfs-udisks2-volume-monitor
root        1280  0.0  0.1 392180 11780 ?        Ssl  12:08   0:00 /usr/lib/udisks2/udisksd
root        1286  0.0  0.4 320992 37296 ?        Sl   12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libwhiskermenu.so 1 12582919 whiskermenu Whisker Menu Show a menu to eas
root        1295  0.0  0.2 205620 21764 ?        Sl   12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libsystray.so 15 12582921 systray Notification Area Area where notificat
root        1296  0.1  0.4 513392 35472 ?        Sl   12:08   0:12 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libpulseaudio-plugin.so 16 12582922 pulseaudio PulseAudio Plugin Adjust 
root        1298  0.0  0.4 245980 33588 ?        Sl   12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libnotification-plugin.so 17 12582923 notification-plugin Notification P
root        1300  0.0  0.4 246208 34080 ?        Sl   12:08   0:02 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libxfce4powermanager.so 18 12582924 power-manager-plugin Power Manager P
root        1301  0.0  0.1 313788  8528 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfs-afc-volume-monitor
root        1302  0.0  0.4 246020 33856 ?        Sl   12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libactions.so 20 12582925 actions Action Buttons Log out, lock or other 
root        1308  0.0  0.0 232736  5612 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfs-mtp-volume-monitor
root        1318  0.0  0.0 232940  5764 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfs-goa-volume-monitor
root        1330  0.0  0.1 235108  8144 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfs-gphoto2-volume-monitor
root        1343  0.0  0.0 159216  5696 ?        Ssl  12:08   0:00 /usr/lib/gvfs/gvfsd-metadata
root        1344  0.0  0.4 318872 32680 ?        Ssl  12:08   0:00 /usr/lib/x86_64-linux-gnu/xfce4/notifyd/xfce4-notifyd
root        1347  0.0  0.0 310800  7492 ?        Sl   12:08   0:00 /usr/lib/gvfs/gvfsd-trash --spawner :1.11 /org/gtk/gvfs/exec_spaw/0
root        1355  0.7  4.1 2907916 331344 ?      Sl   12:08   1:24 /usr/lib/firefox-esr/firefox-esr --sm-client-id 234b33838-d340-4372-aedb-96791cfa81da
root        1360  0.0  0.2 206020 19000 ?        Ssl  12:08   0:00 xfce4-power-manager --restart --sm-client-id 2ccb84cfc-7d69-4d90-b915-77a14f8cf294
root        1368  0.0  0.5 453764 46956 ?        Sl   12:08   0:00 /usr/bin/python3 /usr/bin/blueman-applet
root        1374  0.0  0.3 276364 24888 ?        Sl   12:08   0:00 light-locker
root        1375  0.0  0.2 197684 18480 ?        Sl   12:08   0:00 /usr/lib/policykit-1-gnome/polkit-gnome-authentication-agent-1
root        1377  0.0  0.0 233920  4712 ?        Sl   12:08   0:00 /usr/libexec/geoclue-2.0/demos/agent
root        1385  0.0  0.5  74448 41340 ?        S    12:08   0:00 /usr/bin/python3 /usr/share/system-config-printer/applet.py
root        1388  0.0  0.0 155932  5212 ?        Sl   12:08   0:00 /usr/lib/dconf/dconf-service
root        1398  0.0  0.5 400696 41340 ?        Sl   12:08   0:01 nm-applet
root        1404  0.0  0.1 859120  8584 ?        Sl   12:08   0:00 xiccd
root        1426  0.0  0.0  22656   328 ?        Ssl  12:08   0:00 xcape -e Super_L Control_L Escape
colord      1430  0.0  0.1 241340 12676 ?        Ssl  12:08   0:00 /usr/lib/colord/colord
root        1496  0.0  0.6 333200 50140 ?        Sl   12:08   0:00 /usr/bin/python3 /usr/bin/blueman-tray
root        1513  0.0  0.0  44468  6812 ?        Ss   12:08   0:00 /usr/lib/bluetooth/obexd
root        1524  0.0  1.8 2466516 148568 ?      Sl   12:08   0:03 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 1 -isForBrowser -prefsLen 1 -prefMapSize 182490 -parentBuildID 20200206211857 -greomni /usr/lib/firefox-esr/omni.
root        1567  0.0  1.2 2404488 102404 ?      Sl   12:08   0:04 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 2 -isForBrowser -prefsLen 5805 -prefMapSize 182490 -parentBuildID 20200206211857 -greomni /usr/lib/firefox-esr/om
root        1628  0.1  1.6 2482108 135912 ?      Sl   12:08   0:11 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 3 -isForBrowser -prefsLen 6537 -prefMapSize 182490 -parentBuildID 20200206211857 -greomni /usr/lib/firefox-esr/om
root        1688  0.0  0.0      0     0 ?        I<   12:08   0:00 [iprt-VBoxWQueue]
root        1695  0.0  0.0      0     0 ?        S    12:08   0:00 [iprt-VBoxTscThr]
root        1775  0.1  0.8 610548 69724 ?        Rl   12:08   0:20 /usr/bin/qterminal
root        1778  0.0  0.1  15580  9164 pts/0    Ss   12:08   0:02 /usr/bin/zsh
root        1787  0.0  0.0   7944  3356 pts/0    S    12:08   0:00 /usr/bin/zsh -dfxc          exec >&4         echo $$         /root/.oh-my-zsh/custom/themes/powerlevel10k/gitstatus/bin/gitstatusd-linux-x86_64 --lock-fd=3 --parent-pid
root        1788  0.0  0.0 135832  2680 pts/0    Sl   12:08   0:00 /root/.oh-my-zsh/custom/themes/powerlevel10k/gitstatus/bin/gitstatusd-linux-x86_64 --lock-fd=3 --parent-pid=1778 --num-threads=16 --max-num-staged=-1 --max-num-unstaged
root        2135  0.0  0.0      0     0 ?        I    12:23   0:00 [kworker/2:0-mm_percpu_wq]
root        2206  0.0  0.0      0     0 ?        I    12:39   0:00 [kworker/3:2-mm_percpu_wq]
root        2284  0.0  0.0      0     0 ?        I    13:09   0:00 [kworker/1:1-events]
root        2528  5.2  4.0 2919136 328112 ?      Sl   13:27   5:31 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 4 -isForBrowser -prefsLen 8584 -prefMapSize 182490 -parentBuildID 20200206211857 -greomni /usr/lib/firefox-esr/om
root        2578  0.0  0.8 2380856 69536 ?       Sl   13:28   0:00 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 5 -isForBrowser -prefsLen 8584 -prefMapSize 182490 -parentBuildID 20200206211857 -greomni /usr/lib/firefox-esr/om
root        2953  0.0  0.0      0     0 ?        D    13:51   0:01 [kworker/u16:1+events_unbound]
root        3142  0.0  0.0      0     0 ?        I    14:31   0:00 [kworker/0:0-events]
root       21942  0.0  0.0      0     0 ?        I    14:36   0:00 [kworker/1:2-events]
root       30544  0.0  0.0      0     0 ?        I    14:39   0:00 [kworker/7:1-cgroup_destroy]
root       30628  0.0  0.0      0     0 ?        I    14:47   0:00 [kworker/3:0-mm_percpu_wq]
root       30645  0.0  0.0      0     0 ?        I    14:57   0:00 [kworker/u16:0-events_unbound]
root       30646  0.0  0.0      0     0 ?        I    14:57   0:00 [kworker/6:0]
root       30649  0.0  0.0      0     0 ?        I    14:57   0:00 [kworker/5:2-rcu_gp]
root       30652  0.0  0.0      0     0 ?        I    14:57   0:00 [kworker/7:2-mm_percpu_wq]
root       30653  0.0  0.0      0     0 ?        I    14:57   0:00 [kworker/4:2-events]
root       30666  0.0  0.0      0     0 ?        I    15:00   0:00 [kworker/2:2]
root       30675  0.0  0.0      0     0 ?        I    15:02   0:00 [kworker/0:2-events]
root       30680  0.0  0.0      0     0 ?        I    15:04   0:00 [kworker/5:1-rcu_gp]
root       30686  0.0  0.0      0     0 ?        I    15:07   0:00 [kworker/0:1-events]
root       30736  0.0  0.0      0     0 ?        I    15:09   0:00 [kworker/4:0-events]
root       30740  0.0  0.0      0     0 ?        I    15:10   0:00 [kworker/5:0-events]
root       30747  0.0  0.0   8572  3032 pts/0    R+   15:12   0:00 ps aux
```

 - choisissez 5 services système et expliquer leur utilité

systemd = C'est le programme qui fait tourner l'OS
NetworkManager = C'est l'outil de gestion des connexions réseau
rsyslogd = C'est un programme de journalisation (logs)
bash = c'est un shell
xfce4-panel = C'est l'environnement de bureau, en gros ça permet d'avoir du graphique



- déterminer les processus lancés par l'utilisateur qui est full admin sur la machine

```
→ ps aux | grep root
Je vais pas recopier la liste, elle est au dessus puisque je suis root.
```

### Scripting

#### Footprinting.sh

```=bash
echo "-------------Network-------------"
echo "Getting average download speed . . .\nGetting average upload speed . . .\n"
echo "Ip principale : "$(hostname -I | awk '{print $1}')
echo $(ping -c 4 8.8.8.8 | awk -F '/' 'END {print "Average time to 8.8.8.8 : "$5"ms"}')
echo $(speedtest | grep -e Download -e Upload)
echo "---------------------------------\n"
echo "-------------Hardware-------------"
echo "Nom de la machine : "$(hostname)
# echo "Ip public : " $(curl -s ifconfig.me)
echo "Os : "$(cat /etc/*-release | head -n 1 | sed 's/...........//')
echo "Version de l'os : "$(uname -r)
echo "Date et heure d'allumage : "$(uptime --since)
echo "Mise à jour : "$(sudo /usr/lib/update-notifier/apt-check --human-readable | awk 'NR==1 { T=$1" mises à jour disponibles" } NR==2 {T=T", dont "$1" de sécurité" } END { print T }')
echo $(free -m | awk 'NR==2{printf "Ram utilisé: %sMB/%sMB (%.2f%%)\n", $3,$2,$3*100/$2 }')
echo $(df -h | awk '$NF=="/"{printf "Espace Disk utilisé: %d/%dGB (%s)\n", $3,$2,$5}')
echo "----------------------------------\n"
echo "Listes des Users :\n"
echo -n $(awk -F: '{ print $1}' /etc/passwd)"\n"
```

#### Input.sh

```=bash
#!/bin/bash

echo  "Tapez 1 pour lock l'écran, 2 pour éteindre le PC : "
read choix$choix
if [ $choix == 1 ]; then
    xdotool key Super_L+l
elif [ $choix == 2 ]; then
    echo  "Entrez en minutes dans combien de temps éteindre le PC : "
    read time$time
    shutdown -h $time
fi
```

### Gestion de softs

🌞 Expliquer l'intérêt de l'utilisation d'un gestionnaire de paquets

 - par rapport au téléchargement en direct sur internet

→ Tout simplement, parce que tout les packets (du moins beaucoup), se trouve au meme endroit, une seule commande nous suffit à télécharger ce que l'on souhaite.

 - penser à l'identité des gens impliqués dans un téléchargement (vous, l'éditeur logiciel, etc.)

→ X

 - penser à la sécurité globale impliquée lors d'un téléchargement

→ Les packets disponible dans un gestionnaire de paquets sont vérifiés par ce dernier, lors d'un téléchargement, il y a donc peu de risque. Mais il faut quand meme ce méfier.

🌞 Utiliser un gestionnaire de paquet propres à votre OS pour

 - lister tous les paquets déjà installés

```
→ apt list --installed
Trop de packages lol
```

 - déterminer la provenance des paquets (= quel serveur nous délivre les paquets lorsqu'on installe quelque chose)

```
→ apt-cache policy <package>
```

### Partage de fichiers

🌞 Monter et accéder à un partage de fichiers

→ X

### Chiffrement et notion de confiance

🌞 Expliquer en détail l'utilisation de certificats

→ Un certificat SSL ou TLS permet d'établir avec certitude le lien entre 2 personnes (chiffrements de mails) ou encore entre un client et un site web.

 - quelle est l'information principale d'un certificat ?

→ L'information principe d'un certificat est bien evidement sa clé publique.

 - quelles sont d'autres informations importantes pour la sécurité d'un certificat ?

→ La période de validité
→ Les algorithmes de signature et hachage derrière le chiffrement 

### Chiffrement de mails

🌞 En utilisant votre client mail préféré, mettez en place du chiffrement de mail

→ J'ai pas trouvé de certificat mais sinon c'est assez simple 

Il faut juste importer le certificat dans thunderbird, on setup une passphrase.

Une fois cela fait. c'est fini, suffit juste d'aller dans l'onglet sécurité > "Chiffrer ce mail"

Et c'est plié 


### TLS

🌞 Expliquer

 - que garantit HTTPS par rapport à HTTP ?

→ HTTPS : connexions chiffrées
("s" pour sécure) : Fonctionne grace au protocole TLS

→ Meilleur score SEO, site mieux référencé

 - qu'est-ce que signifie précisément et techniquement le cadenas vert que nous présente nos navigateurs lorsque l'on visite un site web "sécurisé"

→ Le cadena vert signifie que l'identité du site est validé.
→ Il repose aussi sur le protocole SSL qui lui est chargé de chiffrer les données envoyé par l'utilisateur

🌞 Accéder à un serveur web sécurisé (client)

- accéder à un site web en HTTPS

![](https://i.imgur.com/mnVPJVv.png)

- visualiser le certificat

![](https://i.imgur.com/rPoVWAU.gif)

- - détailler l'ensemble des informations et leur utilité

→ Trop long (80 lignes)

 - - déterminer ce qui permet de faire confiance au HTTPS de ce site web

→ L'utilisation du chiffrement TLS + La date d'expiration du certificat

### SSH

#### Serveur

→ Je vais le secure plus tard

Monter un server ssh : 

→ On doit installer `openssh-server` mais il y est deja.

→ On lui dit de le lander au demerage : `systemctl enable --now ssh.service`

→ On va venir parametrer notre config ssh : 
 
 - On supprime le login en root en editant la ligne ici : 

 - - `/etc/ssh/sshd_config`
 - - PermitRootLogin no

 - On change le port : 22 to 2222

 - - Toujours dans le sshd_config on modifie la ligne `Port 22` en `Port 2222`

 - - On oublie pas le firwall avec ces commandes : 


```
firewall-cmd --add-port=2222/tcp --permanent
firewall-cmd --reload
```

 - - On vient ensuite modifier le SELinux en mode permissif
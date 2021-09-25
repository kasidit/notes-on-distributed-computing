<h1>Notes on Distributed and Parallel Processing</h1>

<h2>การสร้าง github repository สำหรับส่ง Assignments</h2>
<p><p>
ก่อนอื่นผู้อ่านต้องสร้างบัญชีใน github ซึ่งขอให้ศึกษาเองจาก github.com เว็บไซต์ ในโน๊ตนี้เราสมมุติว่าผู้อ่านมีบัญชีใน github แล้ว 
และจะอธิบายการสร้างรีโพสิทอรี่ (repository) ใหม่และ upload ไฟล์เข้าสู่รีโพสิทอรี่นั้น ด้วย ssh โดยผู้เขียนจะบรรยายขั้นตอนดังนี้
<p><p>
<h3>1. สร้างรีโพสิทอรี่ใหม่บนเว็บไซต์ github.com </h3><br>
<p>
    ผู้อ่านต้องกดปุ่มเครื่องหมายบวกข้างไอคอนของบัญชีของผู้อ่านและเลือก New repository และเว็บไซด์จะแสดงหน้าเว็บดังภาพ github-1
<p>
    <kbd><img src="newrepo.png" width="800" ></kbd>
<p>
<p>
ภาพ github-1 การสร้างรีโพสิทอรี่ใหม่ <br> 
<p><p>
    หลังจากนั้น บนหน้าเว็บเพจในภาพ github-2 กำหนดค่าชื่อเป็น CS337 และให้เป็น private (โดยที่ผู้อ่านจะสามารถระบุชื่อบัญชีผู้ใช้ระบบ github คนอื่นให้เข้ามาดูหรือร่วมใช้รีโพสิทอรี่นี้ได้ในภายหลัง)
    และกำหนดให้สร้างไฟล์ readme ตั้งแต่แรก สุดท้ายให้กด Create Repository ก็จะได้เว็บเพจดังภาพ github-3
<p>
    <kbd><img src="repodetails.png" width="800" ></kbd>
<p>
ภาพ github-2 กรอกรายละเอียดรีโพสิทอรี่ใหม่ <br>
<p><p>
<p>
    <kbd><img src="repocreated.png" width="800" ></kbd>
<p>
ภาพ github-3 รีโพสิทอรี่ใหม่ <br> 
<p><p>
    หลังจากนั้น ให้ผู้อ่านกดปุ่ม code และเลือกแทบ ssh (ขีดเส้นใต้สีส้ม) ดังภาพ github-4 และจำค่า
<pre>
git@github.com:kasidit/CS337.git
</pre>
เพื่อนำไปใช้อ้างอิงในอนาคต (สามารถเซฟไว้ในคลิบบอร์ดได้)
<p><p>
<p>
    <kbd><img src="repossh.png" width="800"></kbd>
<p>
ภาพ github-4 ชื่อรีโพสิทอรี่แบบ ssh <br> 
<p><p>
<h3>2. สร้างไดเรกทอรี่ที่ต้องการจะ upload ข้อมูลและสร้างคีย์ </h3><br>
<p>
    
<h2>ติดตั้ง MPICH บน ubuntu 16.04</h2>
<p><p>
ผมสร้างเครื่อง Virtual Machine (VM) จำนวน 2 เครื่องโดยใช้ virtual box โดยที่กำหนดให้แต่ละเครื่องมี vcpu 2 cores และ 2 GB RAM และ network interfaces ดังนี้ 
<ul>
<li>Interface 1: เป็น NAT 
<li>Interface 2: เป็น Host-Only Network และอยู่ใน subnet 192.168.56.0/24
<li>Interface 3: เป็น Internal Network ชื่อ "intnet2"
</ul>
หลังจากติดตั้ง ubuntu 16.04 บน VM ทั้งสองแล้ว ผมเข้าไปกำหนดค่าใน /etc/sudoers เพื่อให้ login id ของผม (ในตัวอย่างนี้ผมใช้ชื่อ account ว่า "openstack") ให้สามารถใช้ sudo ได้โดยไม่ใส่พาสเวิด และผมกำหนด Network interfaces ของ VM เครื่องที่ 1 (VM1) ให้มีค่าดังนี้ ()
<pre>
$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:85:09:7e brd ff:ff:ff:ff:ff:ff
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:94:55:a4 brd ff:ff:ff:ff:ff:ff
4: enp0s9: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 08:00:27:e8:55:24 brd ff:ff:ff:ff:ff:ff
$ 
$ sudo nano /etc/network/interfaces
$ cat /etc/network/interfaces 
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
#
source /etc/network/interfaces.d/*
#
# The loopback network interface
auto lo
iface lo inet loopback
#
# The primary network interface
auto enp0s3
iface enp0s3 inet dhcp
#
auto enp0s8
iface enp0s8 inet dhcp
#
auto enp0s9
iface enp0s9 inet static
address 192.168.1.11
netmask 255.255.255.0
network 192.168.1.0
#
$ sudo reboot
</pre>
Reboot เครื่องเพื่อให้ network interface enp0s9 ของ VM1 เป็น 192.168.1.11
<p><p>
สำหรับ VM เครื่องที่ 2 (VM2) ผมกำหนดค่า network configuration ดังนี้ 
<pre>
$ sudo nano /etc/network/interfaces
$ cat /etc/network/interfaces 
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
#
source /etc/network/interfaces.d/*
#
# The loopback network interface
auto lo
iface lo inet loopback
#
# The primary network interface
auto enp0s3
iface enp0s3 inet dhcp
#
auto enp0s8
iface enp0s8 inet dhcp
#
auto enp0s9
iface enp0s9 inet static
address 192.168.1.12
netmask 255.255.255.0
network 192.168.1.0
#
$ sudo reboot
</pre>
กำหนดค่า IP บน enp0s9 ของ VM2 ให้เป็น 192.168.1.12
<p><p>
login เข้าไปใน VM ทั้งสองใหม่ ติดตั้ง mpich
<pre>
$  sudo apt install mpich
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  g++ g++-5 gfortran gfortran-5 hwloc-nox libcr-dev libcr0 libgfortran-5-dev
  libgfortran3 libhwloc-plugins libhwloc5 libltdl7 libmpich-dev libmpich12
  libpciaccess0 libstdc++-5-dev ocl-icd-libopencl1
Suggested packages:
  g++-multilib g++-5-multilib gcc-5-doc libstdc++6-5-dbg gfortran-multilib
  gfortran-doc gfortran-5-multilib gfortran-5-doc libgfortran3-dbg blcr-dkms
  libhwloc-contrib-plugins libstdc++-5-doc blcr-util mpich-doc opencl-icd
The following NEW packages will be installed:
  g++ g++-5 gfortran gfortran-5 hwloc-nox libcr-dev libcr0 libgfortran-5-dev
  libgfortran3 libhwloc-plugins libhwloc5 libltdl7 libmpich-dev libmpich12
  libpciaccess0 libstdc++-5-dev mpich ocl-icd-libopencl1
0 upgraded, 18 newly installed, 0 to remove and 132 not upgraded.
Need to get 21.0 MB of archives.
After this operation, 79.4 MB of additional disk space will be used.
Do you want to continue? [Y/n]y
....
....
$ 
</pre>
ตดตั้ง NFS server บน VM1 สร้าง mount directory และกำหนดค่า /etc/exports
<pre>
$ sudo apt install nfs-kernel-server
$ 
$ sudo mkdir /nfs
$ sudo vi /etc/exports
$
openstack@cs715host2:~$ cat /etc/exports
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/nfs   192.168.1.0/24(rw,sync,no_root_squash,no_subtree_check)
$
$ sudo systemctl restart nfs-kernel-server
$
</pre>
ติดตั้ง NFS client บน VM2 และ Remote Mount NFS server และทดสอบโดยการสร้าง directory "mydir"
<pre>
$ sudo apt install nfs-common
$ sudo mkdir -p /nfs
$ sudo mount 192.168.1.11:/nfs /nfs
$
$ cd /nfs
$ sudo mkdir mydir
$ $ ls -l
total 4
drwxr-xr-x 2 root root 4096 May  5 23:51 mydir
$
</pre>
ทดสอบว่า บน VM1 ใน NFS directory มี "mydir" ปรากฏอยู่หรือไม่
<pre>
$ cd /nfs
$ ls -l
total 4
drwxr-xr-x 2 root root 4096 May  5 23:51 mydir
$
</pre>
กำหนดค่าในไฟล์ /etc/fstab บน VM2 ให้ Remote mount ไปยัง NFS directory ตั้งแต่ตอน boot เครื่อง และ reboot เครื่อง
<pre>
$ sudo vi /etcfstab
$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=85ddcd59-efed-4a26-936d-d8ca4449a8e8 /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
UUID=47a679a2-11db-4d89-a58b-0b450e79e1a1 none            swap    sw              0       0
192.168.1.11:/nfs    /nfs   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
$
$ reboot
</pre>
<p><p>
สร้าง public และ private keys บน VM1
<pre>

</pre>
<h2>รันโปรแกรม MPI จาก NAS Parallel Benchmark 3.3 ด้วย MPICH </h2>
<p><p>
ผมเขียนอันนี้นานแล้ว ไม่เกี่ยวกับข้างบน        
ในหัวข้อนี้ผมจะติดตั้ง MPICH บน cluster ที่ประกอบด้วย 4 nodes ซึ่งในที่นี้กำหนดให้เป็น VMs ชื่อ shibuya-1, shibuya-2, shibuya-6, shibuya-7 ดังที่ระบุใน /etc/hosts ไฟล์ข้างล่าง
<pre>
$ cat /etc/hosts
127.0.0.1       localhost
192.168.90.21   shibuya-1
192.168.90.22   shibuya-2
192.168.90.26   shibuya-6
192.168.90.27   shibuya-7
#
10.90.0.21   hs-1
10.90.0.22   hs-2
10.90.0.26   hs-6
10.90.0.27   hs-7
</pre>
จะเห็นว่ามี IP addresses สอง subnets คือ 10.90.0.0/24 และ 192.168.90.0/24  
(under construction)
<p><p>
<h2>รันโปรแกรม terasort hadoop benchmark บน cloudera 5.13 quickstart VM </h2>
<p><p>
การรัน terasort benchmark ใช้คำสั่งต่อไปนี้ ขั้นแรก หาไฟล์ hadoop-mapreduce-examples.jar
<pre>
$ find /usr/ -name hadoop-*examples*.jar
find: `/usr/lib64/audit': Permission denied
/usr/lib/hue/apps/oozie/examples/lib/hadoop-examples.jar
/usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples-2.6.0-cdh5.13.0.jar
/usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar
/usr/lib/hadoop-0.20-mapreduce/hadoop-examples-mr1.jar
/usr/lib/hadoop-0.20-mapreduce/hadoop-examples.jar
/usr/lib/hadoop-0.20-mapreduce/hadoop-examples-2.6.0-mr1-cdh5.13.0.jar
/usr/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples.jar
/usr/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.13.0.jar
$
</pre>
ขั้นสอง รัน teragen เพื่อสร้างไฟล์ 5MB 
<pre>
<b>$ hadoop jar hadoop-mapreduce-examples.jar teragen 50000 terasort-input</b>
Not a valid JAR: /home/cloudera/kasidit/hadoop-mapreduce-examples.jar
[cloudera@quickstart kasidit]$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar  teragen 50000 ./terasort-input
18/09/08 11:05:18 INFO client.RMProxy: Connecting to ResourceManager at quickstart.cloudera/10.90.0.21:8032
18/09/08 11:05:19 INFO terasort.TeraGen: Generating 50000 using 2
18/09/08 11:05:19 INFO mapreduce.JobSubmitter: number of splits:2
18/09/08 11:05:20 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1536426895125_0003
18/09/08 11:05:20 INFO impl.YarnClientImpl: Submitted application application_1536426895125_0003
18/09/08 11:05:20 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1536426895125_0003/
18/09/08 11:05:20 INFO mapreduce.Job: Running job: job_1536426895125_0003
18/09/08 11:05:28 INFO mapreduce.Job: Job job_1536426895125_0003 running in uber mode : false
18/09/08 11:05:28 INFO mapreduce.Job:  map 0% reduce 0%
18/09/08 11:05:33 INFO mapreduce.Job:  map 100% reduce 0%
18/09/08 11:05:34 INFO mapreduce.Job: Job job_1536426895125_0003 completed successfully
18/09/08 11:05:34 INFO mapreduce.Job: Counters: 31
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=294162
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=164
                HDFS: Number of bytes written=5000000
                HDFS: Number of read operations=8
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=4
        Job Counters
                Launched map tasks=2
                Other local map tasks=2
                Total time spent by all maps in occupied slots (ms)=3629056
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=7088
                Total vcore-milliseconds taken by all map tasks=7088
                Total megabyte-milliseconds taken by all map tasks=3629056
        Map-Reduce Framework
                Map input records=50000
                Map output records=50000
                Input split bytes=164
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=84
                CPU time spent (ms)=2960
                Physical memory (bytes) snapshot=283983872
                Virtual memory (bytes) snapshot=1503252480
                Total committed heap usage (bytes)=98566144
        org.apache.hadoop.examples.terasort.TeraGen$Counters
                CHECKSUM=107145675668275
        File Input Format Counters
                Bytes Read=0
        File Output Format Counters
                Bytes Written=5000000
$
</pre>
ถัดไป sort ด้วย terasort 
<pre>
$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar  terasort ./terasort-input ./terasort-output
18/09/08 11:07:45 INFO terasort.TeraSort: starting
18/09/08 11:07:47 INFO input.FileInputFormat: Total input paths to process : 2
Spent 129ms computing base-splits.
Spent 3ms computing TeraScheduler splits.
Computing input splits took 133ms
Sampling 2 splits of 2
Making 1 from 50000 sampled records
Computing parititions took 558ms
Spent 694ms computing partitions.
18/09/08 11:07:47 INFO client.RMProxy: Connecting to ResourceManager at quickstart.cloudera/10.90.0.21:8032
18/09/08 11:07:48 INFO mapreduce.JobSubmitter: number of splits:2
18/09/08 11:07:49 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1536426895125_0004
18/09/08 11:07:49 INFO impl.YarnClientImpl: Submitted application application_1536426895125_0004
18/09/08 11:07:49 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1536426895125_0004/
18/09/08 11:07:49 INFO mapreduce.Job: Running job: job_1536426895125_0004
18/09/08 11:07:56 INFO mapreduce.Job: Job job_1536426895125_0004 running in uber mode : false
18/09/08 11:07:56 INFO mapreduce.Job:  map 0% reduce 0%
18/09/08 11:08:02 INFO mapreduce.Job:  map 100% reduce 0%
18/09/08 11:08:08 INFO mapreduce.Job:  map 100% reduce 100%
18/09/08 11:08:09 INFO mapreduce.Job: Job job_1536426895125_0004 completed successfully
18/09/08 11:08:09 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=2164979
                FILE: Number of bytes written=4777781
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=5000276
                HDFS: Number of bytes written=5000000
                HDFS: Number of read operations=9
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters
                Launched map tasks=2
                Launched reduce tasks=1
                Data-local map tasks=2
                Total time spent by all maps in occupied slots (ms)=3776000
                Total time spent by all reduces in occupied slots (ms)=2116608
                Total time spent by all map tasks (ms)=7375
                Total time spent by all reduce tasks (ms)=4134
                Total vcore-milliseconds taken by all map tasks=7375
                Total vcore-milliseconds taken by all reduce tasks=4134
                Total megabyte-milliseconds taken by all map tasks=3776000
                Total megabyte-milliseconds taken by all reduce tasks=2116608
        Map-Reduce Framework
                Map input records=50000
                Map output records=50000
                Map output bytes=5100000
                Map output materialized bytes=2166940
                Input split bytes=276
                Combine input records=0
                Combine output records=0
                Reduce input groups=50000
                Reduce shuffle bytes=2166940
                Reduce input records=50000
                Reduce output records=50000
                Spilled Records=100000
                Shuffled Maps =2
                Failed Shuffles=0
                Merged Map outputs=2
                GC time elapsed (ms)=122
                CPU time spent (ms)=5470
                Physical memory (bytes) snapshot=439963648
                Virtual memory (bytes) snapshot=2262458368
                Total committed heap usage (bytes)=146276352
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=5000000
        File Output Format Counters
                Bytes Written=5000000
18/09/08 11:08:09 INFO terasort.TeraSort: done
$
</pre>
ต่อไปก็ตรวจความถูกต้องด้วย teravalidate
<pre>
$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar  teravalidate ./terasort-output ./terasort-validate
18/09/08 11:10:02 INFO client.RMProxy: Connecting to ResourceManager at quickstart.cloudera/10.90.0.21:8032
18/09/08 11:10:03 INFO input.FileInputFormat: Total input paths to process : 1
Spent 19ms computing base-splits.
Spent 2ms computing TeraScheduler splits.
18/09/08 11:10:03 INFO mapreduce.JobSubmitter: number of splits:1
18/09/08 11:10:04 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1536426895125_0005
18/09/08 11:10:04 INFO impl.YarnClientImpl: Submitted application application_1536426895125_0005
18/09/08 11:10:04 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1536426895125_0005/
18/09/08 11:10:04 INFO mapreduce.Job: Running job: job_1536426895125_0005
18/09/08 11:10:11 INFO mapreduce.Job: Job job_1536426895125_0005 running in uber mode : false
18/09/08 11:10:11 INFO mapreduce.Job:  map 0% reduce 0%
18/09/08 11:10:16 INFO mapreduce.Job:  map 100% reduce 0%
18/09/08 11:10:22 INFO mapreduce.Job:  map 100% reduce 100%
18/09/08 11:10:23 INFO mapreduce.Job: Job job_1536426895125_0005 completed successfully
18/09/08 11:10:23 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=97
                FILE: Number of bytes written=295199
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=5000139
                HDFS: Number of bytes written=22
                HDFS: Number of read operations=6
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters
                Launched map tasks=1
                Launched reduce tasks=1
                Data-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=1764352
                Total time spent by all reduces in occupied slots (ms)=1749504
                Total time spent by all map tasks (ms)=3446
                Total time spent by all reduce tasks (ms)=3417
                Total vcore-milliseconds taken by all map tasks=3446
                Total vcore-milliseconds taken by all reduce tasks=3417
                Total megabyte-milliseconds taken by all map tasks=1764352
                Total megabyte-milliseconds taken by all reduce tasks=1749504
        Map-Reduce Framework
                Map input records=50000
                Map output records=3
                Map output bytes=80
                Map output materialized bytes=93
                Input split bytes=139
                Combine input records=0
                Combine output records=0
                Reduce input groups=3
                Reduce shuffle bytes=93
                Reduce input records=3
                Reduce output records=1
                Spilled Records=6
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=77
                CPU time spent (ms)=2280
                Physical memory (bytes) snapshot=305377280
                Virtual memory (bytes) snapshot=1522327552
                Total committed heap usage (bytes)=97517568
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=5000000
        File Output Format Counters
                Bytes Written=22
$
</pre>



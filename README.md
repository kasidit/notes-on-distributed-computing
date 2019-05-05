<h1>จิปาถะเรื่อง distributed programming</h1>

<h2>ติดตั้ง MPICH บย ubuntu 16/04</h2>
<p><p>
ผมสร้างเครื่อง Virtual Machine (VM) จำนวน 2 เครื่องโดยใช้ virtual box โดยที่กำหนดให้แต่ละเครื่องมี vcpu 2 cores และ 2 GB RAM และ network interfaces ดังนี้ 
<ul>
<li>Interface 1: เป็น NAT 
<li>Interface 2: เป็น Host-Only Network และอยู่ใน subnet 192.168.56.0/24
<li>Interface 3: เป็น Internal Network ชื่อ "intnet"
</ul>
หลังจากติดตั้ง ubuntu 16.04 บน VM ทั้งสองแล้วให้กำหนด Network interfaces ของ VM เครื่องที่ 1 (VM1) ดังนี้ (เพื่อความสะดวกให้ตั้งข้อกำหนดให้ login id ของคุณสามารถใช้ sudo โดยไม่ใส่พาสเวิด)
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

source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#The primary network interface
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet dhcp

auto enp0s9
iface enp0s9 inet static
address 192.168.1.11
netmask 255.255.255.0
network 192.168.1.0

$ sudo reboot
</pre>
Reboot เครื่องเพื่อให้ network interface enp0s9 เป็น 192.168.1.11
<p><p>
สำหรับ VM เครื่องที่ 2 (VM2) ให้กำหนด network configuration ดังนี้ 
<pre>
$ sudo nano /etc/network/interfaces
$ cat /etc/network/interfaces 
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#The primary network interface
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet dhcp

auto enp0s9
iface enp0s9 inet static
address 192.168.1.12
netmask 255.255.255.0
network 192.168.1.0

$ sudo reboot
</pre>
ทำเหมือน VM1 เพื่อกำหนดค่า enp0s9 เป็น 192...12

<p><p>
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
<p><p>
<h2>2. รันโปรแกรม terasort hadoop benchmark บน cloudera 5.13 quickstart VM </h2>
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



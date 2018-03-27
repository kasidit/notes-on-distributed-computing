<h1>จิปาถะเรื่อง programming</h1>

<h2>1. รันโปรแกรม MPI จาก NAS Parallel Benchmark 3.3 ด้วย MPICH </h2>
<p><p>
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

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
    <i>ภาพ github-1 การสร้างรีโพสิทอรี่ใหม่ </i><br> 
<p><p>
    หลังจากนั้น บนหน้าเว็บเพจในภาพ github-2 กำหนดค่าชื่อเป็น CS337 และให้เป็น private (โดยที่ผู้อ่านจะสามารถระบุชื่อบัญชีผู้ใช้ระบบ github คนอื่นให้เข้ามาดูหรือร่วมใช้รีโพสิทอรี่นี้ได้ในภายหลัง)
    และกำหนดให้สร้างไฟล์ readme ตั้งแต่แรก สุดท้ายให้กด Create Repository ก็จะได้เว็บเพจดังภาพ github-3
<p>
    <kbd><img src="repodetails.png" width="800" ></kbd>
<p>
    <i>ภาพ github-2 กรอกรายละเอียดรีโพสิทอรี่ใหม่ </i><br>
<p><p>
<p>
    <kbd><img src="repocreated.png" width="800" ></kbd>
<p>
    <i>ภาพ github-3 รีโพสิทอรี่ใหม่ </i><br> 
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
    <i>ภาพ github-4 ชื่อรีโพสิทอรี่แบบ ssh </i><br> 
<p><p>
<h3>2. สร้างคู่คีย์ด้วย ssh-keygen </h3>
<p>
    ผู้อ่านต้องล็อกอินเข้าสู่เครื่องที่มีข้อมูลที่ต้องการจะอัพโหลดขึ้นสู่ github แล้วออกคำสั่งต่อไปนี้เพื่อสร้างคีย์ ซึ่งผู้เขียนจะสร้างคีย์แบบ ed25519 เนื่องจากมีขนาดเล็กและมีความปลอดภัยสูง โดยที่เราจะกำหนดให้คำสั่ง ssh-keygen จะสร้างไพรเวทคีย์ (private key) และสร้างพับบลิกคีย์ (public key) ไว้ในดีฟอลต์ (default) ไดเรกทอรี่สำหรับยรรจุไฟลืคีย์คือที่ $HOME/.ssh 
<pre>
$ cd $HOME
$ 
$ ssh-keygen -t ed25519 
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/openstack/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/openstack/.ssh/id_ed25519
Your public key has been saved in /home/openstack/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:1KocjsWSet/fXWV8Ws8qHbP1dsvBWOjumZnqqpq8U8M openstack@vm4
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|         .       |
|        . .      |
|     o . .    .. |
|    o = S    . .*|
|   . * E    . *==|
|  . o = .    +.B=|
|   ..o..   .o.Oo=|
|     ==.oo+o+Oo+o|
+----[SHA256]-----+
$ 
</pre>
คำสั่งข้างต้นจะเข้ารหัสไพรเวทคีย์ด้วยพาสเฟส (passphrase) ที่ผู้อ่านป้อนให้ ในกรณีที่ผู้อ่านไม่ใส่พาสเฟส คำสั่ง ssh-keygen ก็จะไม่เข้ารหัสไพรเวทคีย์ (ซึ่งถ้าไฟล์ถูกก็อปปี้โดยผู้ที่ไม่ได้รับอนุญาตไฟล์คีย์ก็จะถูกนำไปใช้ได้เลย) หลังจากนั้น ssh-keygen จะเก็บไพรเวทคีย์ไว้ที่ไฟล์ $HOME/.ssh/id_ed25519 และเก็บพับบลิกคีย์ที่ $HOME/.ssh/id_ed25519.pub
<p>
    ผู้อ่านอาจพิมพ์ข้อความพับบลิกคีย์บนหน้าจอเพื่อนำไปใช้ในขั้นตอนต่อไป
<pre>
$ cat $HOME/.ssh/id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINOwEGLCh7fsdYsIRxOINEBf3NtnnSOzlSFbg7qdNC77 openstack@vm4
</pre>
<p><p>
<h3>3. นำเข้าพับบลิกคีย์เข้าสู่เว็บไซต์ github </h3>
<p>
    ผู้อ่านสามารถนำเข้าพับบลิกคีย์สู่บัญชีผู้ใช้ github โดยกดปุ่มขยายเมนูที่ไอคอนของบัญชีของผู้อ่านดังภาพที่ github-5 และเลือก Settings
<p>
    <kbd><img src="keymenu1.png" width="200"></kbd>
<p>
    <i>ภาพ github-5 เลือก Settings จากเมนู</i><br> 
<p>
    หลังจากนั้น github จะแสดงเว็บเพจ Settings ดังภาพ github-6 ซึ่งผู้อ่านจะเลือกกด SSH and GPG keys แทบทางด้านซ้ายของเพจ
<p>
    <kbd><img src="keymenu2.png" width="800"></kbd>
<p>
  <i>ภาพ github-6 เว็บเพจ Settings</i><br>
<p> github จะแสดงหน้าจอถัดไปคือแสดงรายการ Keys ที่ผู้อ่านมี ในกรณีที่ผู้อ่านไม่เคยนำเข้า SSH public key ใดมาก่อนก็จะไม่มีรายการคีย์ปรากฏ แต่ถ้าผู้อ่านเคยนำเข้าคีย์ไม่ว่าจะเป็น SSH Keys หรือ GPG Keys มาแล้วก็จะปรากฏรายการของพับบลิกคีย์เหล่านั้นดังตัวอย่างในภาพ github-7 
<p>
    <kbd><img src="keymenu3.png" width="800"></kbd>
<p>
    <i>ภาพ github-7 รายการของพับบลิกคีย์ </i><br> 
<p>
    การที่ผู้อ่านสามารถนำเข้าพับบลิกคีย์ได้หลายไฟล์ หมายความว่าผู้อ่านสามารถดาวน์โหลดและอัพโหลดข้อมูลของรีโพสิทอรี่ของผู้อ่านในบัญชีผู้ใช้ github ของผู้อ่านได้จากคอมพิวเตอร์หลายเครื่อง ในอันดับถัดไป ผู้อ่านจะกดปุ่ม New Ssh Key และหน้าจอจะเปลี่ยนเป็นดังภาพ github-8
<p>
    <kbd><img src="keymenu4.png" width="800"></kbd>
<p>
    <i>ภาพ github-8 ป้อนค่าพับบลิกคีย์ใหม่ </i><br> 
<p>
    ในภาพ github-8 ผู้อ่านต้องก็อปปี้ค่าของพับบลิกคีย์ที่พิมพ์ไว้บนหน้าจอในขั้นตอนก่อนหน้า และนำมาใส่ในช่อง key และในช่องไทเทิ้ล (title) ผู้อ่านจะต้องระบุชื่อคีย์นี้ ซึ่งเป้นชื่อใดก้ได้ แต่ในตัวอย่างผู้เขียนตั้งชื่อให้สะท้อนถึงเครื่องที่จะสามารถใช้งานรีโพสิทอรี่ในบัญชีผู้ใช้ของ github นี้ได้ ซึ่งสมมุติว่าให้เครื่องชื่อ vm4 หลังจากนั้นให้กดปุ่ม Add SSH Key และเว็บไซต์ github จะแสดงหน้าจอดังภาพ github-9 ให้ผู้อ่านป้อนรหัสลับของบัญชีผู้ใช้ github ของผู้อ่าน
<p>
    <kbd><img src="keymenu5.png" width="250"></kbd>
<p>
    <i>ภาพ github-9 คอนเฟิร์มด้วยพาสเวิร์ดของบัญชีผู้ใช้ github </i><br> 
<p>
    เมื่อป้อนรหัสพาสเวิร์ดและผ่านการพิสูจน์ตัวตนและสิทธิ์แล้ว เว็บไซต์ github จะแสดงหน้าจอดังภาพ github-10 ซึ่งเป้นการแจ้งว่าการนำเข้าพับบลิกคีย์ของผู้อ่านเสร็จเรียบร้อย
<p>
    <kbd><img src="keymenu6.png" width="800"></kbd>
<p>
    <i>ภาพ github-10 แสดงรายการคีย์หลังนำเข้าสำเร็จ</i><br> 
<p><p>
<h3>4. สร้างไดเรกทอรี่ที่ต้องการจะ upload ข้อมูล และดาวน์โหลดข้อมูลจากรีโพสิทอรี่</h3>
<p>
กลับมาที่เครื่องคอมพิวเตอร์ที่ผู้อ่านต้องการอัพโหลดข้อมูลเข้าสู่ github ซึ่งในที่นี้เราสมมุติว่าผู้อ่านต้องการอัพโหลดข้อมูลจากเครื่อง ubuntu ที่ผู้อ่านสร้างคู่คีย์ SSH ก่อนหน้า 
ซึ่งผู้อ่านจะเริ่มต้นจากไดเรกทอรี่ $HOME และสร้างไดเรกทอรี่ย่อยชื่อ CS337 สำหรับเก็บข้อมูลที่ผู้อ่านต้องการจะอัพโหลดเข้าสู่ github รีโพสิทอรี่ CS337 ในบัญชี github 
ของผู้อ่าน และดาวน์โหลดไฟล์และไดเรกทอรี่จากรีโพสิทอรี่นั้นมายังพื้นที่ไดเรกทอรี่ที่สร้างขึ้นบนเครื่อง ubuntu นี้
    <p>
<pre>
$ cd $HOME 
$ mkdir CS337
$ cd CS337
</pre>
<p>
เมื่อผู้อ่าน cd เข้าสู่ไดเรกทอรี่ CS337 ผู้อ่านจะต้องออกคำสั่ง git init เพื่อตั้งค่าเริ่มต้นให้ git ซึ่งคำสั่งจะสร้างไดเรกทอรี่ย่อยชื่อ .git ใน CS337 เพื่อเก็บค่า
สถานะการปฏิบัติงานของ github และในอันดับถัดไป ผู้อ่านต้องดาวน์โหลดข้อมูลในรีโพสิทอรี่ในบัญชี github ที่ผู้อ่านต้องการจะอัพโหลดและดาวน์โหลดข้อมูล ซึ่งเนื่องจากผู้อ่านได้สร้าง
รีโพสิทอรี่ CS337 นี้บนเว็บไซต์ github ในขั้นตอนก่อนหน้า ดังนั้นก่อนที่ผู้อ่านจะสามารถอัพโหลดข้อมูลใหม่เข้าสู่รีโพรี่ CS377 จากไดเรกทอรี่ $HOME/CS337 นี้ได้ ผู้อ่านจะต้องทำ
ให้ข้อมูลที่อยู่บน github และบนเครื่องคอมพิวเตอร์ของผู้อ่านเหมือนกันก่อน ด้วยคำสั่งต่อไปนี้
    <p>
<pre>
$ git init
Initialized empty Git repository in /home/openstack/CS337/.git/
$ 
$ git pull git@github.com:kasidit/CS337.git
The authenticity of host 'github.com (20.205.243.166)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,20.205.243.166' (RSA) to the list of known hosts.
Enter passphrase for key '/home/openstack/.ssh/id_ed25519':
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 584 bytes | 584.00 KiB/s, done.
From github.com:kasidit/CS337
 * branch            HEAD       -> FETCH_HEAD
$
</pre>
<p>
    จากคำสั่งที่ใช้ ผู้อ่านจะใช้คำสั่ง git pull เพื่อดาวน์โหลดข้อมูลจากรีโพสิทอรี่ CS337 บน github มายังไดเรกทอรี่ CS337 
ขอให้สังเกตุว่าผู้อ่านต้องใส่ค่า git@github.com:kasidit/CS337.git แทนที่จะเป็น URL ของรีโพสิทอรี่คือ 
https://github.com/kasidit/CS337.git ทั้งนี้เพราะผู้อ่านจะใช้ SSH และคู่คีย์ SSH ในการดาวน์โหลดและอัพโหลด
ข้อมูลระหว่างรีโพสิทอรี่กับไดเรกทอรี่บนเครื่องคอมพิวเตอร์อ่าน ในทางกลับกันถ้าผุ้อ่านต้องการดาวน์โหลดและอัพโหลดด้วยโปรโตคอล https 
ผู้อ่านจะต้่องใช้ URL แต่ผู้เขียนไม่ได้เขียนอธิบายไว้ในที่นี้เนื่องจากการใช้ SSH นั้นปลอดภัยกว่า และมีข่าวว่า github อาจ
ไม่อนุญาตให้ใช้ https อัพโหลดข้อมูลในอนาคต
<p><p>
    ในการดาวน์โหลด github กำหนดให้เราจะใช้คำสั่ง git pull ในขณะที่เราจะต้องใช้คำสั่ง git push ในการอัพโหลด 
ในการดาวน์โหลด เนื่องจากเป็นการติดต่อกับเซิร์ฟเวอร์ของ github ครั้งแรก ssh จึงถามผุ้ใช้ให้คอนเฟิร์มความถูกต้อง แล้วหลังจากนั้น
ก็จะถามหาพาสเฟส เพื่อจะได้นำไปถอดรหัสไพรเวทคีย์เพื่อนำไปใช้สร้างช่องทางเชื่อมต่อที่ถูกเข้ารหัสแบบ ssh ซึ่งคำสั่ง git pull จะดาวน์โหลด
ไฟล์ README.md ซึ่งมีเนื้อหาคือ \# CS337 ในนั้นมาไว้ในไดเรกทอรี่ CS337 
<p>
    หลังจากนั้น ผู้อ่านจะสร้างไฟล์ ชื่อ myfirstfile.txt และสร้างไดเรกทอรี่ย่อยชื่อ Assign1 และสร้างไฟล์ file1.txt ในนั้น
    <p>
<pre>
$ ls
README.md
$ cat README.md
# CS337 $ 
$ cat > myfirstfile.txt
Hello World!
for CS337
$ 
$ mkdir Assign1
$ echo "my first assignment" > Assign1/file1.txt
</pre>
<p><p>
<h3>5. upload ข้อมูลสู่ github </h3>
<p>
<p>
ในอันดับถัดไป ผู้อ่านต้องใช้คำสั่ง git add เพื่อ add ไดเรกทอรี่และไฟล์ทั้งหมดที่มีการเปลี่บนแปลงใน CS337 นี้เข้าสู่รายการของ
ไฟล์และไดเรกทอรี่ ที่จะได้รับการอัพโหลดเข้าสู่รีโพสิทอรี่ CS337 บน github แล้วหลังจากนั้นต้องใช้คำสั่ง git commit เพื่อคอมมิตรายการ
ไฟล์และไดเรกทอรี่เหล่านั้น 
    <p>
<pre>
$ git add .
$ git commit -m "first update"

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'openstack@vm4.(none)')
$ git config --global user.email "kasiditchanchio@gmail.com"
$ git config --global user.name "kasidit"
$ git commit -m "first update"
[master d384b53] first update
 2 files changed, 3 insertions(+)
 create mode 100644 Assign1/file1.txt
 create mode 100644 myfirstfile.txt
$
</pre>
<p>
จากคำสั่งข้างต้น ขอให้สังเกตุว่า git add . คือการเพิ่มรายการไฟล์และไดเรกทอรี่ทุกรายการที่อยู่ภายในไดเรกทอรี่ปัจจุบัน 
สำหรับ git commit ออปชั่น -m ตามด้วยข้อความที่ระบุความหมายของการอัพเดทรีโพสิทอรี่แต่ละครั้ง หลังจากใช้คำสั่ง 
git commit เป็นครั้งแรก คำสั่ง git จะแจ้งให้ผู้อ่านระบุข้อมูลชื่อและอีเมล์ของผู้อ่าน และต้องออกคำสั่ง git commit 
อีกครั้งหนึ่ง 
<p><p>
    ในการอัพโหลดข้อมูล ผู้อ่านจะต้องใช้คำสั่ง git remote add เพื่อกำหนดให้คำสั่ง git รู้ว่าจะต้องอัพโหลดข้อมูลไป
ที่ ssh เซิร์ฟเวอร์และรีโพสิทอรี่ใดของ github และให้ชื่อว่าเป็น origin แล้วหลังจากนั้นก็จะออกคำสั่ง git push origin master 
เพื่อกำหนดชื่อ เวอร์ชั่นหลักหรือสาขา (branch) ของรีโพสิทอรี่ CS337 ที่จะอัพโหลดข้อมูลจากไดเรกทอรี่นี้ขึ้นไปว่าชื่อ main 
ซึ่งเป็นสาขาดีฟอลต์
โดยใช้คำสั่งต่อไปนี้ 
    <p>
<pre>
$ git branch -M main
$ git remote add origin git@github.com:kasidit/CS337.git
$ git push origin main
Enter passphrase for key '/home/openstack/.ssh/id_ed25519': 
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (5/5), 415 bytes | 415.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To github.com:kasidit/CS337.git
   d6017a0..b82769b  main -> main
$
</pre>
<p>
ขอให้สังเกตุว่า เมื่อใช้คำสั่ง git push ผู้อ่านจะต้องป้อนพาสเฟสเพื่อถอดรหัสของไพรเวทคีย์
หลังจากนั้น ผู้อ่านสามารถเช็คเว็บไซต์ github ที่รีโพสิทอรี่ CS337 ก็จะเห็นว่าไฟล์จากไดเรกทอรี่ CS337 ถูก
อัพโหลดไปไว้ในนั้นแล้ว ดังภาพ github-11
<p>
    <kbd><img src="uploaded.png" width="800"></kbd>
<p>
    <i>ภาพ github-11 รีโพสิทอรี่หลังจากคำสั่ง git push</i><br> 
<p><p>
<h3>6. การ invite ผู้ร่วมงาน collaborator ให้ใช้ไพรเวทรีโพสิทอรี่ได้ </h3>
<p>
สมมุติว่าผู้ใช้บัญชี github ชื่อ kasidit ต้องการให้ผู้ใช้บัญชี github ชื่อ ckasidit สามารถเข้ามาใช้งาน 
รีโพสิทอรี่ CS337 ได้ เว็บไซต์ github มีขั้นตอนดังนี้
<p>
    <kbd><img src="allow1.png" width="700"></kbd>
<p>
    <i>ภาพ github-12 กำหนด setting ให้อนุญาตผู้ใช้อื่นเข้าร่วมใช้รีโพสิทอรี่</i><br> 
<p>
    ในอันดับแรก ในภาพ github-12 ผู้อ่านต้องกดแทบ Settings ในหน้าเพจของรีโพสิทอรี่ CS337 ที่ต้องการจะแชร์กับผู้อื่น 
    และจะได้หน้าจอใหม่ในภาพ github-13
<p>
<p>
    <kbd><img src="allow2.png" width="700"></kbd>
<p>
    <i>ภาพ github-13 ภายใน Settings </i><br> 
<p>
เมื่อเข้าสู่หน้า Setting ผู้อ่านจะเห็นเมนูคำสั่งทางด้านซ้ายให้กดเมนู Manage Access จะเข้าสู่หน้าเว็บเพจ github-14
<p>
<p>
    <kbd><img src="allow3.png" width="700"></kbd>
<p>
    <i>ภาพ github-14 หลังจากกด Manage Access</i><br> 
<p>
ในภาพ ให้ผู้อ่านกด Invite a collaborator ระบบจะแสดงหน้าเพจดังภาพ github-15 
และผู้อ่านจะระบชื่อบัญชี github ที่จะเชิญซึ่งในตัวอย่างนี้กำหนดให้เป็นบัญชีชื่อ ckasidit
<p> 
<p>
    <kbd><img src="allow4.png" width="700"></kbd>
<p>
    <i>ภาพ github-15 ป้อนชื่อบัญชีผู้ใช้ที่จะเชิญให้แชร์ CS337 รีโพสิทอรี่</i><br> 
<p>
เลือกแล้วกด Add ckasidit to this repository ดังภาพ github-16
<p> 
<p>
    <kbd><img src="allow5.png" width="700"></kbd>
<p>
    <i>ภาพ github-16 กดเชิญ</i><br> 
<p>
หลังจากนั้น ในช่อง Manage Access บนหน้าเพจของ github จะแสดงรายการ
บัญชีที่เพิ่งเชิญ และระบุว่า pending invite ดังภาพ github-17 เนื่องจากผู้ถูกเชิญยัง
ไม่ได้ตอบรับหรือปฏิเสธ
<p> 
<p>
    <kbd><img src="allow6.png" width="700"></kbd>
<p>
    <i>ภาพ github-17 รอการตอบรับ หรือปฏิเสธ</i><br> 
<p>
บนฝั่งของผู้ถูกเชิญ ระบบ github จะส่งอีเมล์ไปให้กับผู้ถูกเชิญ โดยที่
อีเมล์แอดเดรสที่ github ส่งคำเชิญไปจะเป็น อีเมล์ที่ผู้ใช้บัญชี 
ckasidit ใช้สมัครและสร้างบัญชีผู้ใช้บน github
<p> 
<p>
    <kbd><img src="allow7.png" width="700"></kbd>
<p>
    <i>ภาพ github-18 อีเมล์ที่ส่งมาให้ผู้ถูกเชิญ</i><br> 
<p>
ในภาพ github-18 ผู้ถูกเชิญต้องกดลิงค์ accept or decline ซึ่งจะนำ
    ผู้ใช้ที่ถูกเชิญ ซึ่งต้องล็อกอินใช้งาน github ภายใต้บัญชีของตนก่อน และระบบ
จะแสดงหน้าเพจในภาพ github-19 ที่จะให้ผู้ถูกเชิญกดเลือก
<p> 
<p>
    <kbd><img src="allow8.png" width="700"></kbd>
<p>
    <i>ภาพ github-19 ผู้ถุกเชิญเลือกว่าจะรับหรือปฏิเสธคำเชิญ</i><br> 
<p>
ในกรณีที่ตอบรับ github จะแสดงหน้าเพจของรีโพสิทอรี่ที่ผู้ถูกเชิญได้รับอนุญาต
ให้เข้าถึง ภายใต้ล็อกอินของผู้ถูกเชิญ (บัญชีผู้ใช้ ckasidit ในตัวอย่างนี้) ดังภาพ github-20
<p> 
<p>
    <kbd><img src="allow9.png" width="700"></kbd>
<p>
    <i>ภาพ github-20 แสดงหน้าเพจให้ผู้ถุกเชิญเข้าใช้</i><br> 
<p>
สำหรับเจ้าของรีโพสิทอรี่ เมื่อ refresh หน้าเพจ Manage Access อีกครั้งจะเป็นว่า 
Pending Invitation หายไป กลายเป็นผู้ร่วมใช้รีโพสิทอรี่เต็มตัว ดังภาพ github-21
<p> 
<p>
    <kbd><img src="allow10.png" width="700"></kbd>
<p>
    <i>ภาพ github-21 รายการผู้ร่วมใช้รีโพสิทอรี่</i><br> 
<p><p>
ในกรณีที่เข้าของรีโพสิทอรี่ต้องการยกเลิกการเข้าถึงก็สามารถทำได้ทันที
    โดยกดไอคอน ถังขยะ ทางด้านขวาของชื่อผู้ร่วมใช้รีโพสิทอรี่






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
...
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
...
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
...
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



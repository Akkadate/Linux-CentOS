# Linux-CentOS
คำสั่งที่มีประโยชน์

ตรวจสอบ Process ที่ใช้ Port:
netstat -anp | grep <your_server_ip>

ตรวจสอบ Process ID (PID) และหยุด:
kill -9 <PID>

ตรวจสอบ Log ของ Server
tail -f /var/log/nginx/access.log

ตรวจสอบการใช้งานพอร์ต 8443 บนระบบ
netstat -tuln | grep 8443

ss -tuln | grep 8443

ตรวจสอบ Process ที่ใช้พอร์ต:
หลังจากทราบ PID (Process ID) แล้ว คุณสามารถใช้คำสั่ง:
ps -aux | grep <PID>

ในการตรวจสอบว่าเครื่องของคุณใช้ CentOS รุ่นอะไร (เช่น CentOS 6, 7 หรือ 8) คุณสามารถใช้คำสั่งดังต่อไปนี้:
1. ตรวจสอบด้วยไฟล์ /etc/centos-release
      cat /etc/centos-release

2. ใช้คำสั่ง lsb_release (ถ้ามีติดตั้ง)
lsb_release -a

3. ตรวจสอบด้วยคำสั่ง hostnamectl (เฉพาะ CentOS 7 และใหม่กว่า)
hostnamectl
cat /etc/os-release


วิธีตรวจสอบว่าใช้อะไรอยู่
ตรวจสอบสถานะของ Firewalld:  
systemctl status firewalld

## IPTABLEs
ตรวจสอบสถานะ iptables:
systemctl status iptables

1. แสดงกฎ iptables ทั้งหมด
   iptables -L -n -v
   -L: แสดงรายการกฎที่กำหนดไว้
   -n: แสดงที่อยู่ IP และพอร์ตในรูปแบบตัวเลข (ไม่แปลงเป็นชื่อ)
   -v: แสดงข้อมูลเพิ่มเติม เช่น จำนวนแพ็กเก็ตและขนาดข้อมูล

   2. แสดงกฎเฉพาะ Chain
      iptables -L INPUT -n -v
   
3. ดูกฎในรูปแบบของคำสั่ง iptables
iptables-save

IPTABLEs
iptables -L -n -v      # แสดงกฎทั้งหมด
iptables -A INPUT -p tcp --dport 80 -j ACCEPT  # อนุญาต HTTP

บันทึกการเปลี่ยนแปลง
service iptables save  # บันทึกการตั้งค่า

3. ป้องกัน DDoS (หากระบบมีการใช้งานเว็บ)
iptables -A INPUT -p tcp --dport 80 -m limit --limit 25/minute --limit-burst 100 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -m limit --limit 25/minute --limit-burst 100 -j ACCEPT

service iptables save

 ## ลบกฎโดยระบุรายละเอียด
 ระบุหมายเลขกฎ (Rule Number) ที่ต้องการลบ
 ดูว่ากฏอยู่บรรทัดไหน
iptables -L INPUT -n -v --line-numbers

ลบกฎโดยใช้หมายเลข
iptables -D INPUT 1   (ลบบรรทัดที่ 1)

บันทึกการเปลี่ยนแปลง
service iptables save  # บันทึกการตั้งค่า


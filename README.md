# 🔐 SIEM Implementation with Elastic Stack – Brute Force Detection

## 📌 Tổng quan
Xây dựng hệ thống SIEM với Elastic Stack để phát hiện tấn công brute-force SSH.

**Công nghệ sử dụng:** Elastic Stack (ELK), Kali Linux (Hydra), pfSense, Ubuntu Server, Windows Server.

---

## 🏗 Kiến trúc

(Sơ đồ kiến trúc)
<img width="880" height="560" alt="sodo" src="https://github.com/user-attachments/assets/7e96cc0f-696f-4d4b-a942-4393bcd74db4" />

---


## ⚙️ Cài đặt

### 1. Fleet Server
(Fleet Server)
<img width="1893" height="467" alt="fleetserver" src="https://github.com/user-attachments/assets/d6af8a6c-2cad-4910-a7ac-d660547aacaf" />

### 2. Đăng ký Agent
(Đăng ký agent Windows + Ubuntu)
<img width="1919" height="868" alt="dangkyagent" src="https://github.com/user-attachments/assets/50be3376-a557-4570-a5c1-847542d40a9b" />

### 3. Cấu hình pfSense Port Forward
(pfSense port forward 2222 → 22)
<img width="1919" height="672" alt="pfsense-portforward" src="https://github.com/user-attachments/assets/c1a8375f-5fd6-4f65-9d1f-f11f8ac833a1" />

---

## 💥 Mô phỏng tấn công

### Máy kali dùng hydra để bruteforce

hydra -l manh -P password.txt ssh://192.168.28.130 -s 2222
<img width="1680" height="481" alt="hydra-attack" src="https://github.com/user-attachments/assets/bcdc516d-62f0-40cd-a605-505bfaf42259" />
<img width="1664" height="873" alt="attack -ubuntu" src="https://github.com/user-attachments/assets/1181b2f6-1853-4a19-aa57-de651b1926bb" />

---

### 🔍 Truy vấn log trên Kibana
event.outcome:"failure" and process.name:"sshd"
event.dataset:"system.auth" AND process.name:"sshd" AND event.outcome:"failure"
<img width="1919" height="891" alt="query-log-fail" src="https://github.com/user-attachments/assets/5d7ec65c-51bd-46b1-bb38-a9d2a6e9e667" />

## 📊 Dashboard

<img width="1910" height="909" alt="dashboard1" src="https://github.com/user-attachments/assets/288f5c83-48bd-47d8-95ea-1c09971d49ff" />

<img width="1919" height="971" alt="dashboard2" src="https://github.com/user-attachments/assets/d04c6ff5-693b-4bf9-aa03-7ef63d77d255" />

<img width="1919" height="948" alt="dashboard3" src="https://github.com/user-attachments/assets/443ad2e6-c942-48d5-b65d-8d88deb21da2" />

## ⚠️ Tạo rule cảnh báo

### Rule 1: Brute Force từ cùng 1 IP - ≥50 lần fail/5 phút
Custom query: event.dataset:"system.auth" AND process.name:"sshd" AND event.outcome:"failure"
<img width="1072" height="386" alt="customquery" src="https://github.com/user-attachments/assets/d3e7ed49-76b2-4d2d-9fc4-d0a56cb3e976" />
<img width="1267" height="633" alt="threshold" src="https://github.com/user-attachments/assets/29a7b9f2-ce8c-4efb-a9bf-af1941399806" />
<img width="1200" height="554" alt="schedule" src="https://github.com/user-attachments/assets/0576f96b-8390-4dc1-a088-201d3e3fdb80" />
<img width="1919" height="957" alt="ketquaalert1" src="https://github.com/user-attachments/assets/63b4708d-21db-4ef3-bb46-3c1077fb1664" />

### Rule 2: 
Custom query: event.dataset:"system.auth" AND process.name:"sshd" AND event.outcome:"failure"
<img width="1062" height="707" alt="severity" src="https://github.com/user-attachments/assets/e7b5964e-8069-429c-bca5-580b3c03591b" />
<img width="1918" height="942" alt="alertkq2" src="https://github.com/user-attachments/assets/9ae9dd22-969a-45ae-8899-982f195dbea0" />

## ✅ Kết quả
Thu thập log từ Windows và Ubuntu

Cấu hình pfSense port forward

Phát hiện brute-force SSH

Dashboard hiển thị 3 biểu đồ

Rule cảnh báo hoạt động

# 🔐 SIEM Lab – Brute Force SSH Detection with ELK

## 📌 Overview
Dự án mô phỏng hệ thống SIEM sử dụng Elastic Stack để thu thập và phân tích log, từ đó phát hiện tấn công brute-force SSH trong mạng nội bộ.

---

## 🎯 Objective
- Mô phỏng quy trình SOC: Thu thập log → Phát hiện → Cảnh báo → Phân tích
- Phát hiện tấn công brute-force thông qua log SSH
- Thực hành sử dụng ELK và viết detection rule
- Nâng cao kỹ năng phân tích log và giám sát bảo mật

---

## 🧱 Architecture
![Architecture](images/architecture.png)

**Thành phần hệ thống:**
- Kali Linux (Attacker)
- pfSense Firewall (Port Forward)
- Ubuntu Server (SSH Target)
- Windows Server
- Elastic Stack (Elasticsearch, Kibana, Fleet, Agent)

---

## ⚙️ Log Collection
- Sử dụng Elastic Agent để thu thập log từ Windows và Ubuntu
- Fleet Server quản lý các agent
- Log được gửi về ELK để phân tích và trực quan hóa

---

## 🔍 Detection Logic

```kql
event.dataset:"system.auth" AND process.name:"sshd" AND event.outcome:"failure"

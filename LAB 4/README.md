# README - LAB 4 NMAP

## 1. Thông tin sinh viên

- Họ và tên: Ngô Thảo Vy
- MSSV: 1150080123
- Tên Lab: LAB 4 - Khảo sát và đánh giá bề mặt mạng bằng Nmap
- Môn học: An toàn Bảo mật Hệ thống Thông tin

---

## 2. Phiên bản môi trường

### Máy thật
- Hệ điều hành: Windows 11
- Phần mềm ảo hóa: VMware Fusion

### Máy ảo sử dụng
- Kali Linux: Máy quét (Scanner)
- Metasploitable 2: Máy mục tiêu (Target)
- Windows VM: Máy mục tiêu đối chiếu

### Mạng thực hành
- Kiểu mạng: Host-Only
- Subnet: 255.255.255.0 (/24)

### Địa chỉ IP

| Thiết bị | IP | Subnet | Vai trò |
|---|---|---|---|
| Kali Linux | 192.168.157.128 | 255.255.255.0 (/24) | Máy quét |
| Metasploitable 2 | 192.168.157.129 | 255.255.255.0 (/24) | Máy mục tiêu |
| Windows VM | 192.168.157.130 | 255.255.255.0 (/24) | Máy mục tiêu phụ |

### Công cụ
- Nmap
- Npcap
- Zenmap
- VMware Fusion
- Kali Linux Terminal

---

## 3. Cách dựng môi trường

### Bước 1: Cài Nmap trên Windows 11

Cài Nmap trên Windows 11 và kiểm tra phiên bản sau khi cài đặt.

Nmap được sử dụng để thực hiện các hoạt động khảo sát host, cổng, dịch vụ và hệ điều hành trong mạng lab.

### Bước 2: Cài và kiểm tra Nmap trên Kali Linux

Kiểm tra Nmap trên Kali Linux.

Nếu Nmap chưa được cài đặt thì sử dụng apt để cài đặt.

Sau khi cài đặt, kiểm tra lại phiên bản Nmap.

### Bước 3: Thiết lập mạng Host-Only

Thiết lập các máy ảo sử dụng cùng một mạng Host-Only.

Các máy được sử dụng:

- Kali Linux
- Metasploitable 2
- Windows VM

Không sử dụng Bridged cho máy Metasploitable 2 để tránh đưa máy mục tiêu vào mạng thật.

### Bước 4: Kiểm tra địa chỉ IP

Kiểm tra IP của từng máy:

- Kali Linux: 192.168.157.128
- Metasploitable 2: 192.168.157.129
- Windows VM: 192.168.157.130

### Bước 5: Kiểm tra kết nối

Từ Kali Linux thực hiện ping đến Metasploitable 2 để kiểm tra kết nối trước khi bắt đầu quét.

Nếu kết nối thành công thì tiến hành các bài thực hành Nmap.

---

## 4. Các tình huống đã thực hiện

### 4.1. Phát hiện các host đang hoạt động

Thực hiện host discovery trên mạng Host-Only.

Kết quả phát hiện:

| IP | Vai trò | Trạng thái |
|---|---|---|
| 192.168.157.128 | Kali - Scanner | Host is up |
| 192.168.157.129 | Metasploitable 2 - Target | Host is up |
| 192.168.157.130 | Windows VM - Target phụ | Host is up |

Ngoài ra còn phát hiện:
- 192.168.157.1
- 192.168.157.254

Hai địa chỉ này không thuộc nhóm máy ảo mục tiêu khảo sát.

**Kết quả: PASS**

---

### 4.2. TCP Connect Scan (-sT)

Thực hiện TCP Connect Scan trên Metasploitable 2.

Kết quả:

- Open: 23 cổng
- Closed: 977 cổng
- Filtered: 0 cổng
- Tổng số cổng mặc định được quét: 1000

Một số dịch vụ phát hiện:

- FTP - 21/tcp
- SSH - 22/tcp
- Telnet - 23/tcp
- SMTP - 25/tcp
- HTTP - 80/tcp
- SMB - 445/tcp
- MySQL - 3306/tcp
- PostgreSQL - 5432/tcp
- VNC - 5900/tcp

Thời gian quét: khoảng 4.65 giây.

**Kết quả: PASS**

---

### 4.3. SYN Scan (-sS)

Thực hiện SYN Scan trên cùng máy mục tiêu.

Kết quả:

- Open: 23 cổng
- Closed: 977 cổng
- Filtered: 0 cổng
- Thời gian quét: khoảng 4.72 giây

So với -sT, kết quả số lượng cổng open/closed/filtered giống nhau trong lần thực hiện này.

SYN Scan thường yêu cầu quyền cao hơn vì cần khả năng gửi và nhận gói TCP ở mức thấp hơn.

**Kết quả: PASS**

---

### 4.4. So sánh -sT và -sS

| Tiêu chí | -sT | -sS |
|---|---:|---:|
| Open | 23 | 23 |
| Closed | 977 | 977 |
| Filtered | 0 | 0 |
| Thời gian | 4.65 giây | 4.72 giây |
| Quyền | Không cần raw-packet cao | Thường cần sudo/root |

Nhận xét:

Hai phương pháp cho kết quả tương đương trong lần quét này. -sT sử dụng kết nối TCP của hệ điều hành, trong khi -sS sử dụng cơ chế SYN scan và thường yêu cầu quyền cao hơn.

**Kết quả: PASS**

---

### 4.5. FIN / Xmas / NULL Scan

Thực hiện ba kỹ thuật:

- FIN Scan (-sF)
- Xmas Scan (-sX)
- NULL Scan (-sN)

Kết quả:

| Kỹ thuật | Open | Closed | Open\|Filtered |
|---|---|---:|---:|
| FIN | Không xác định | 977 | 23 |
| Xmas | Không xác định | 977 | 23 |
| NULL | Không xác định | 977 | 23 |

Nhận xét:

Không được kết luận `open|filtered` đồng nghĩa với `open`. Trạng thái này cho biết Nmap không thể phân biệt chính xác giữa cổng mở và cổng bị bộ lọc làm im lặng.

**Kết quả: PASS**

---

### 4.6. ACK Scan (-sA)

Thực hiện ACK Scan trên Metasploitable 2.

Kết quả:

- 1000 cổng ở trạng thái `unfiltered`

ACK Scan không dùng để khẳng định cổng đang mở mà chủ yếu dùng để kiểm tra khả năng gói ACK đi qua bộ lọc.

So với SYN Scan:

- SYN Scan: 23 open, 977 closed
- ACK Scan: 1000 unfiltered

Hai kết quả không mâu thuẫn vì mục đích của hai kỹ thuật khác nhau.

**Kết quả: PASS**

---

### 4.7. UDP Scan

Thực hiện UDP Scan có kiểm soát trên một số cổng phổ biến.

Kết quả:

| Cổng | Trạng thái | Dịch vụ |
|---|---|---|
| 53/udp | open | domain |
| 67/udp | open\|filtered | dhcps |
| 137/udp | open | netbios-ns |

Nhận xét:

UDP Scan thường chậm hơn TCP Scan và dễ xuất hiện trạng thái `open|filtered` vì UDP không có cơ chế bắt tay như TCP.

**Kết quả: PASS**

---

### 4.8. Version Detection (-sV)

Thực hiện nhận diện phiên bản dịch vụ.

Một số kết quả:

| Port | Service | Version |
|---|---|---|
| 21/tcp | FTP | vsftpd 2.3.4 |
| 22/tcp | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 80/tcp | HTTP | Apache httpd 2.2.8 |
| 445/tcp | SMB | Samba 3.X–4.X |
| 3306/tcp | MySQL | MySQL 5.0.51a-3ubuntu5 |

Nhận xét:

Các dịch vụ được phát hiện có nhiều phiên bản cũ. Đây là thông tin quan trọng để đánh giá bề mặt tấn công và xác định những dịch vụ cần được cập nhật hoặc hạn chế truy cập.

**Kết quả: PASS**

---

### 4.9. OS Detection (-O)

Thực hiện OS fingerprinting trên Metasploitable 2.

Nmap nhận diện mục tiêu là:

- Linux 2.6.x
- Chi tiết khoảng Linux 2.6.9 - 2.6.33

Kết quả OS Detection chỉ mang tính nhận diện dựa trên fingerprint nên không nên xem là tuyệt đối.

**Kết quả: PASS**

---

### 4.10. Aggressive Scan (-A)

Thực hiện Aggressive Scan để tổng hợp nhiều thông tin.

Kết quả quan sát được:

- Version detection
- OS detection
- Traceroute
- Default NSE scripts

Traceroute cho thấy đường đi chỉ có 1 hop đến địa chỉ:

`192.168.157.129`

Các NSE script cũng cung cấp thêm thông tin như FTP anonymous login và SMB information.

**Kết quả: PASS**

---

### 4.11. NSE - SMB và MS17-010

Thực hiện các NSE script liên quan đến SMB trên Metasploitable 2.

Kết quả:

- Cổng 445/tcp: open
- `smb-os-discovery`: phát hiện Unix/Samba 3.0.20-Debian
- Hostname: metasploitable
- Domain: localdomain
- `smb-vuln-ms17-010`: không báo `VULNERABLE`

Kết luận:

Chưa có đủ bằng chứng để kết luận Metasploitable 2 bị ảnh hưởng bởi MS17-010.

Biện pháp phòng thủ:

- Cập nhật Samba.
- Đóng cổng 445 nếu không cần thiết.
- Giới hạn truy cập SMB bằng firewall.

**Kết quả: PASS**

---

### 4.12. Xuất kết quả Nmap

Đã thực hiện các hình thức xuất kết quả:

- Normal text
- XML
- Grepable

Các file kết quả được sử dụng để lưu bằng chứng và phục vụ việc phân tích sau khi quét.

**Kết quả: PASS**

---

## 5. Kết quả tổng hợp

| STT | Tình huống | Kết quả |
|---:|---|---|
| 1 | Host Discovery | PASS |
| 2 | TCP Connect Scan (-sT) | PASS |
| 3 | SYN Scan (-sS) | PASS |
| 4 | So sánh -sT và -sS | PASS |
| 5 | FIN Scan (-sF) | PASS |
| 6 | Xmas Scan (-sX) | PASS |
| 7 | NULL Scan (-sN) | PASS |
| 8 | ACK Scan (-sA) | PASS |
| 9 | UDP Scan | PASS |
| 10 | Version Detection (-sV) | PASS |
| 11 | OS Detection (-O) | PASS |
| 12 | Aggressive Scan (-A) | PASS |
| 13 | NSE SMB Discovery | PASS |
| 14 | NSE MS17-010 | PASS |
| 15 | Xuất kết quả Nmap | PASS |

---

## 6. Lỗi gặp phải và cách khắc phục

### Lỗi 1: Không kết nối được giữa các máy ảo

**Nguyên nhân:**
Các máy ảo chưa sử dụng cùng mạng Host-Only hoặc cấu hình IP/subnet chưa đúng.

**Cách khắc phục:**
- Kiểm tra Adapter của từng máy ảo.
- Đảm bảo Kali và Metasploitable 2 cùng Host-Only Network.
- Kiểm tra subnet mask.
- Kiểm tra địa chỉ IP.
- Kiểm tra máy mục tiêu đã khởi động.
- Thực hiện ping lại trước khi quét.

---

### Lỗi 2: Nmap chưa được nhận diện trên Windows

**Nguyên nhân:**
Terminal chưa nhận PATH mới sau khi cài Nmap.

**Cách khắc phục:**
- Đóng cửa sổ Terminal/Command Prompt cũ.
- Mở lại Terminal.
- Kiểm tra lại bằng lệnh kiểm tra phiên bản Nmap.
- Nếu vẫn không nhận diện, kiểm tra lại tùy chọn đăng ký PATH khi cài đặt.

---

### Lỗi 3: SYN Scan yêu cầu quyền cao

**Nguyên nhân:**
SYN Scan cần quyền thực hiện thao tác với raw packet.

**Cách khắc phục:**
Thực hiện lệnh bằng quyền phù hợp, thường sử dụng `sudo` trên Kali Linux.

---

### Lỗi 4: UDP Scan xuất hiện open|filtered

**Nguyên nhân:**
UDP không có cơ chế bắt tay như TCP nên Nmap có thể không đủ thông tin để phân biệt cổng mở và cổng bị bộ lọc làm im lặng.

**Cách khắc phục:**
Không kết luận `open|filtered` là `open`. Có thể thực hiện thêm các kiểm tra phù hợp để xác minh dịch vụ.

---

### Lỗi 5: NSE MS17-010 không báo VULNERABLE

**Nguyên nhân:**
Script không phát hiện dấu hiệu dễ bị ảnh hưởng hoặc không có đủ thông tin để kết luận.

**Cách khắc phục:**
Không tự suy diễn rằng hệ thống đã được vá. Chỉ kết luận có dấu hiệu dễ bị ảnh hưởng khi script thực sự trả về `VULNERABLE`.

---

## 7. Kết luận

Lab 4 đã được thực hiện trên môi trường mạng Host-Only với Kali Linux đóng vai trò máy quét và Metasploitable 2, Windows VM đóng vai trò máy mục tiêu.

Các nội dung chính đã thực hiện gồm host discovery, TCP Connect Scan, SYN Scan, FIN/Xmas/NULL Scan, ACK Scan, UDP Scan, nhận diện phiên bản dịch vụ, nhận diện hệ điều hành, Aggressive Scan, NSE và xuất kết quả Nmap.

Qua quá trình thực hành, em đã hiểu được cách sử dụng Nmap để khảo sát các host, cổng và dịch vụ trong một mạng nội bộ được cô lập. Đồng thời, em cũng hiểu được sự khác nhau giữa các kỹ thuật quét và cách đọc các trạng thái `open`, `closed`, `filtered` và `open|filtered`.

Toàn bộ quá trình được thực hiện trong môi trường máy ảo Host-Only phục vụ mục đích học tập và thực hành an toàn hệ thống thông tin.

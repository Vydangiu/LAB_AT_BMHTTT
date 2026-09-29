# LAB 3 – Nhận diện và ứng phó các mối đe dọa đến an toàn thông tin

## Thông tin sinh viên
- Họ tên: `<Ngô Thảo Vy>`
- MSSV: `<1150080123>`
- Lớp: `<11CNPM2>`

## Tên bài lab
Lab 3 – Nhận diện và ứng phó các mối đe dọa đến an toàn thông tin (Identifying and Responding to Information Security Threats)

## Phiên bản môi trường thực hành
| Thành phần | Phiên bản |
|---|---|
| Ảo hóa | VMware Workstation Pro 26H1 |
| Máy ảo | Windows 11 25H2 x64, OS build 26200.9445 (KB5124008) |
| Endpoint protection | Microsoft Defender Antivirus (Windows 11) |
| Shell | Windows PowerShell 5.1 |
| Sysmon | 15.22 |
| Autoruns | 14.3 |
| Process Explorer | 17.14 |
| Wireshark | 4.6.8 Stable + Npcap |
| Python | 3.14.7 |
| Gói dữ liệu | LAB3_Threats_Assets.zip — SHA-256: `96236f95ce59d0cc37f9b7ab8fbb04522f21870cd0b53aad6e52da5a23655439` |

## Cách dựng môi trường
1. Tạo VM Windows 11 25H2 x64 trong VMware Workstation Pro 26H1 (tối thiểu 2 vCPU, 6 GB RAM, 64 GB đĩa), Network Adapter = **Host-only**.
2. Cài Windows, cập nhật KB5124008, tạo snapshot `LAB3_CLEAN_20260914`.
3. Tạo cấu trúc thư mục làm việc `C:\LAB3` (`Evidence`, `Tools`, `Downloads`, `Assets`) bằng PowerShell (Administrator).
4. Tải và giải nén gói `LAB3_Threats_Assets.zip` sau khi kiểm tra SHA-256 khớp manifest.
5. Cài Python 3.14.7 và Wireshark 4.6.8 (giữ Npcap) qua `winget`.
6. Tải bộ Sysinternals (Sysmon, Autoruns, Process Explorer) trực tiếp từ `download.sysinternals.com`.
7. Chạy baseline hệ thống (OS, Defender, Firewall, Network, Processes) trước khi thực hiện bất kỳ tình huống nào.

## Các tình huống đã thực hiện
| Tình huống | Nội dung | Kết quả |
|---|---|---|
| TH0 | Baseline hệ thống trước thực hành | `PASS / FAIL` |
| TH1 | Vulnerability – Threat – Risk – Attack, risk register, phân loại 5 nguồn đe dọa | `PASS / FAIL` |
| TH2 | Malware – EICAR test, Defender detection/quarantine | `PASS / FAIL` |
| TH3 | Tấn công mật khẩu – Event 4624/4625/4648, credential rotation | `PASS / FAIL` |
| TH4 | Backdoor – persistence (Run key + Scheduled Task), listener 127.0.0.1:8080, Sysmon/Autoruns/Process Explorer | `PASS / FAIL` |
| TH5 | Sniffing/MITM/Spoofing – Wireshark HTTP loopback vs TLS/443 | `PASS / FAIL` |
| TH6 | DoS/DDoS/Mail bombing – local load test, dataset TEST-NET, mail log offline | `PASS / FAIL` |
| TH7 | Social Engineering/Phishing – phân tích mẫu offline | `PASS / FAIL` |
| Cleanup | Gỡ artefact, verify, hash evidence, revert snapshot | `PASS / FAIL` |

## Lỗi gặp phải và cách khắc phục
| # | Lỗi | Nguyên nhân | Cách khắc phục |
|---|---|---|---|
| 1 | `<Điền mô tả lỗi>` | `<Điền nguyên nhân>` | `<Điền cách khắc phục>` |
| 2 | | | |

## Cấu trúc thư mục trong repo
```
LAB3/
├── README.md
├── Evidence/
│   ├── baseline_*.txt
│   ├── defender_eicar.txt
│   ├── auth_events_before_rotation.txt
│   ├── sysmon_persistence.txt
│   ├── autoruns_before.csv
│   ├── autoruns_after.csv
│   ├── autoruns_diff.txt
│   ├── local_load_test.txt
│   ├── ddos_sources.txt
│   ├── mail_sender_counts.txt
│   ├── mail_volume.txt
│   └── evidence_sha256.csv
└── Screenshots/
    ├── H1_VM_WindowsVersion.png
    ├── H2_ToolVersions.png
    ├── H3_Baseline_Defender_Firewall.png
    ├── H4_ProtectionHistory_EICAR.png
    ├── H5_Event4625.png
    ├── H6_Sysmon_Event1.png
    ├── H7_Autoruns_LAB3_Run_Demo.png
    ├── H8_ProcessExplorer_Python.png
    ├── H9_HTTP_Plaintext.png
    ├── H10_TLS_443.png
    ├── H10_Load_and_Log_Analysis.png
    ├── H10_Phishing_Offline.png
    └── H11_Recovery_Verification.png
```

## Lưu ý
- Không có mật khẩu, token/API key, cookie/session, dữ liệu cá nhân, email thật hoặc log chưa làm sạch trong repo.
- Không có installer/executable (Sysinternals/Wireshark/Python) hoặc file bị Defender quarantine trong repo.
- Toàn bộ ảnh chụp là ảnh chụp trực tiếp từ VM/PC của sinh viên, timestamp khớp với log/output tương ứng.

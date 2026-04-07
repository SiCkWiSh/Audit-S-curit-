# Windows Security Audit: Full System Compromise & Credential Exfiltration

![Security Audit](https://img.shields.io/badge/Audit-Physical%20Access-red)
![Privilege Escalation](https://img.shields.io/badge/PrivEsc-NT%20AUTHORITY%2FSYSTEM-brightgreen)
![Tools](https://img.shields.io/badge/Tools-Metasploit%20%7C%20John%20%7C%20NirSoft-blue)

## 👤 Author
- **Auditor:** Idriss Qarqabi
- **Date:** April 2026

---

## ⚠️ Disclaimer
*This project is for educational and professional audit purposes only. Unauthorized access to computer systems is illegal. The methods described here were performed in a controlled laboratory environment to demonstrate critical security vulnerabilities.*

---

## 📝 Overview
This repository documents a complete security audit chain, demonstrating how physical access to an unencrypted Windows workstation can lead to a total system compromise and the theft of all stored credentials.

The audit highlights the critical importance of **Full Disk Encryption (FDE)** and secure BIOS configurations.

---

## 🛠️ Attack Lifecycle

The audit followed a 5-phase methodology:

### 1. Physical Access & Disk Mounting
- **Vecteur:** Booting via a Live USB (Ubuntu 24.04).
- **Action:** Mounting the NTFS partition bypassing Windows hibernation locks.
- **Vulnerability:** Lack of BitLocker encryption.

### 2. Privilege Escalation (Utilman Exploitation)
- **Action:** Replacing `Utilman.exe` with `cmd.exe` in `System32`.
- **Result:** Gained a SYSTEM shell from the Windows login screen.

### 3. Defensive Neutralization
- **Action:** Disabling Windows Defender and EDR policies via Registry manipulation.
- **Command:** `Set-MpPreference -DisableRealtimeMonitoring $true`

### 4. Data Exfiltration (The Loot)
- **Tools:** Metasploit (Meterpreter), Kiwi (Mimikatz), NirSoft Suite.
- **Extracted Data:**
    - Browser Passwords (Chrome/Edge).
    - WPA2 WiFi Profiles.
    - Full SAM Database (NTLM Hashes).

### 5. Offline Cryptanalysis
- **Tool:** John the Ripper / Hashcat.
- **Target:** NTLM Hashes recovery.
- **Result:** 100% success on weak passwords (e.g., `test:test`).

---

## 📊 Key Findings

| User Account | RID | NTLM Hash | Status |
| :--- | :--- | :--- | :--- |
| **adminj23** | 1003 | `09d2db5bc9d04485ebdcdc9a5832210e` | 🛡️ High Complexity |
| **enseignant** | 1004 | `95a0fd602cb9a34ca7be6cbd9c84c987` | 🛡️ Medium Complexity |
| **test** | 1006 | `0cb6948805f797bf2a82807973b89537` | ✅ **Cracked (test)** |



---

## 🛡️ Mitigation Recommendations

To prevent the attacks demonstrated in this audit, the following measures are mandatory:
1. **Enable BitLocker:** Encrypt all system and data partitions.
2. **UEFI Hardening:** Set an admin password and disable booting from external USB devices.
3. **LAPS Implementation:** Use unique, rotating passwords for local administrator accounts.
4. **Credential Guard:** Enable Windows Defender Credential Guard to protect the LSASS process.

---

## 📂 Repository Structure
- `/scripts`: Custom scripts for defensive bypass.
- `/report`: Full LaTeX Audit Report (PDF).
- `/diagrams`: High-resolution attack schema.

---
*© 2026 Idriss Qarqabi - Cybersecurity Portfolio*

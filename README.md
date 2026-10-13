# 🚀 Cross-Platform Metasploit Guide

<p align="center">
  <strong>Cybersecurity Lab Notes</strong><br>
  Staged & Stageless Payloads · Metasploit Framework · Meterpreter
</p>

> A practical reference for learning network addressing, payload architectures, Metasploit configuration, and session troubleshooting in authorized lab environments.

---

## 📑 Table of Contents

1. [Finding Your Local IP Address](#1-finding-your-local-ip-address)
2. [Payload Generation and Inspection](#2-payload-generation-and-inspection)
3. [Configuring the Metasploit Handler](#3-configuring-the-metasploit-handler)
4. [Stageless Payloads](#4-stageless-payloads)
5. [Staged vs. Stageless](#5-staged-vs-stageless)
6. [Meterpreter Command Reference](#6-meterpreter-command-reference)
7. [Troubleshooting](#7-troubleshooting)
8. [Safety and Authorization](#8-safety-and-authorization)

---

## 1. Finding Your Local IP Address

Before configuring a lab, identify the network interface used by the machine running Metasploit.

### 🐧 Linux

```bash
ip a
```

Show the addresses assigned to the machine:

```bash
hostname -I
```

### 🍎 macOS

```bash
ipconfig getifaddr en0
```

### 🪟 Windows

Run in Command Prompt or PowerShell:

```powershell
ipconfig
```

### 💡 Important Notes

- Replace `<YOUR_IP>` in configuration examples with the correct address for your authorized lab.
- `127.0.0.1` is the loopback address and normally cannot be used to reach your machine from a different computer.
- For virtual machines, verify whether the network uses NAT, bridged networking, or a host-only adapter.

---

## 2. Payload Generation and Inspection

Metasploit supports payloads and output formats for different operating systems and architectures. Always verify compatibility with the target and the installed framework version.

### 🔎 List Available Payloads

```bash
msfvenom --list payloads
```

Filter the list for common desktop platforms on Linux or macOS:

```bash
msfvenom --list payloads | grep -E 'windows|linux|osx'
```

### 📦 List Supported Output Formats

```bash
msfvenom --list formats
```

### 🪟 Windows

**Common executable format:** PE

Check whether the selected payload supports the target's Windows architecture before choosing an output format.

### 🐧 Linux

**Common executable format:** ELF

Inspect an existing lab artifact:

```bash
ls -lh payload.elf
```

Check its file type:

```bash
file payload.elf
```

### 🍎 macOS

**Common executable format:** Mach-O

Check the file type and architecture of an existing lab artifact:

```bash
file payload.macho
```

### ✅ Artifact Verification

| Check | Purpose |
|---|---|
| `ls -lh` | Check that the file exists and inspect its size. |
| `file` | Identify the file format and, where available, its architecture. |
| Architecture compatibility | Ensure the artifact matches the intended target. |
| Framework compatibility | Confirm that the installed Metasploit version supports the selected payload. |

> **Note:** A file's existence and size do not prove that it is valid or compatible.

---

## 3. Configuring the Metasploit Handler

Start the Metasploit console:

```bash
msfconsole
```

Select the multi-handler module:

```text
use exploit/multi/handler
```

Inspect the module's available options:

```text
show options
```

For a controlled lab, configure the handler with the exact payload specified by your authorized exercise:

```text
set PAYLOAD <EXACT_LAB_PAYLOAD>
set LHOST <YOUR_IP>
set LPORT 4444
show options
```

### 🔍 Verify Before Starting

Confirm the following:

- The payload name matches the lab artifact.
- The listener address corresponds to the intended network interface.
- The port matches the lab configuration.
- The target architecture and operating system are supported.
- Network and firewall settings permit the intended lab traffic.

> **Important:** Staged and stageless payloads use different payload identifiers in Metasploit. Select the exact type required by your lab rather than substituting one for the other.

---

## 4. Stageless Payloads

A stageless payload packages its intended session functionality into a single payload instead of retrieving a separate second stage.

### 🧠 How It Works

1. A standalone payload contains the required functionality.
2. The payload attempts to establish the configured network connection.
3. A compatible handler must be configured for the same payload type.
4. Session establishment still depends on network reachability, system compatibility, and applicable security controls.

### 🐧 Linux — ELF

Typical artifact extension:

```text
.elf
```

Inspect a lab artifact:

```bash
ls -lh payload.elf
file payload.elf
```

### 🪟 Windows

Common executable format:

```text
.exe
```

Inspect a Windows artifact from a suitable analysis environment. Confirm its executable format and architecture before testing.

### 🍎 macOS — Mach-O

Typical artifact extension:

```text
.macho
```

Inspect a lab artifact:

```bash
ls -lh payload.macho
file payload.macho
```

### ⚙️ Stageless Configuration Checklist

- [ ] Identify the exact stageless payload supported by the framework.
- [ ] Confirm the target operating system and processor architecture.
- [ ] Ensure the handler uses the matching payload identifier.
- [ ] Verify the listener address and port.
- [ ] Test exclusively in an isolated, authorized lab.
- [ ] Record any network or compatibility errors for troubleshooting.

> **Remember:** Stageless does not mean network-independent. A reverse-connection payload still depends on successful network communication with its listener.

---

## 5. Staged vs. Stageless

| Feature | Staged Payload | Stageless Payload |
|---|---|---|
| Delivery model | Initial component followed by another stage | Functionality packaged into one payload |
| Initial size | Often smaller | Often larger |
| Additional stage transfer | Required | Not required |
| Handler compatibility | Must match the staged payload | Must match the stageless payload |
| Network requirements | Depends on the delivery and session design | Depends on the connection and session design |
| Main consideration | Stage delivery must succeed | Standalone artifact must be compatible |

### Naming Convention

In many Meterpreter payload names:

- `.../meterpreter/reverse_tcp` represents a staged payload.
- `.../meterpreter_reverse_tcp` represents a stageless payload.

The actual payload names available depend on the platform and Metasploit version. Verify them rather than assuming every operating system supports the same variants.

---

## 6. Meterpreter Command Reference

The following commands are useful when learning the Meterpreter interface in an authorized lab.

| Command | Description |
|---|---|
| `help` | List the available commands. |
| `sysinfo` | Display system information. |
| `getuid` | Display the identity associated with the session. |
| `pwd` | Print the current working directory. |
| `ls` | List files and directories. |
| `background` | Return to the Metasploit prompt while retaining the session. |
| `shell` | Open a system shell where supported. |
| `download` | Transfer an authorized file from the remote session. |
| `upload` | Transfer an authorized file to the remote session. |

Explore command-specific usage through the built-in help documentation.

---

## 7. Troubleshooting

### ❌ No Incoming Connection

Check:

- The listener uses the correct interface.
- The lab target can reach the listener.
- The selected port is configured consistently.
- Firewall and virtual-machine networking settings are correct.

### ❌ Payload and Handler Do Not Match

Check:

- Staged versus stageless payload selection.
- Operating system and processor architecture.
- Payload availability in the installed Metasploit version.
- Handler configuration options.

### ❌ The Artifact Does Not Run

Check:

- File format and architecture.
- Operating-system compatibility.
- File integrity and permissions.
- Whether the lab platform supports the selected artifact.

### 🔧 Useful Diagnostic Commands

```bash
ip a
```

```bash
ss -lnt
```

```bash
msfvenom --list payloads
```

```bash
msfvenom --list formats
```

---

## 8. Safety and Authorization

This guide is intended for cybersecurity education, authorized penetration testing, and isolated lab environments.

- Obtain explicit permission before testing a system.
- Prefer intentionally vulnerable virtual machines and controlled lab targets.
- Do not execute payloads on systems you do not own or have authorization to test.
- Keep lab environments isolated where possible.
- Remove temporary artifacts and restore lab snapshots after testing.

---

<p align="center">
  <strong>Learn deeply. Test responsibly. Build securely. 🔐</strong>
</p>

<p align="center">
  <sub>Metasploit Framework · Cybersecurity Learning Notes · Authorized Lab Use Only</sub>
</p>

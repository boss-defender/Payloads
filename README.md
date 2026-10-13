# 🚀 Cross-Platform Metasploit Reference

> **Cybersecurity Lab Notes** · Staged vs. Stageless Payloads · Authorized Testing Only

---

## 📑 Table of Contents

1. [Finding Your Local IP Address](#1-finding-your-local-ip-address)
2. [Understanding Payload Types](#2-understanding-payload-types)
3. [Staged Payload Reference](#3-staged-payload-reference)
4. [Stageless Payload Reference](#4-stageless-payload-reference)
5. [Handler Configuration Concepts](#5-handler-configuration-concepts)
6. [Meterpreter Command Reference](#6-meterpreter-command-reference)
7. [Safety and Authorization](#7-safety-and-authorization)

---

## 1. Finding Your Local IP Address

Identify the address of the network interface used by your authorized lab.

| Operating System | Command |
|---|---|
| 🐧 Linux | `ip a` |
| 🍎 macOS | `ipconfig getifaddr en0` |
| 🪟 Windows | `ipconfig` |

> **Tip:** Use the correct interface address for your lab network. The loopback address `127.0.0.1` refers to the local machine.

---

## 2. Understanding Payload Types

Metasploit supports different payload delivery architectures. Understanding their differences helps you choose and troubleshoot lab configurations.

### ⚡ Staged Payloads

A staged payload delivers an initial component first, then retrieves additional payload components as the session is established.

- **Initial component:** Establishes the connection and receives the next stage.
- **Stage:** Contains the remaining functionality.
- **Typical naming convention:** `reverse_tcp` in some payload paths.

### 🔒 Stageless Payloads

A stageless payload packages the intended functionality into a single payload rather than retrieving a separate second stage.

- **Standalone delivery:** The payload contains its required session functionality.
- **Typical naming convention:** `meterpreter_reverse_tcp` in supported payload paths.
- **Compatibility:** The selected payload, target architecture, operating system, and handler must match.

| Feature | Staged | Stageless |
|---|---|---|
| Delivery | Initial component followed by a stage | Single payload |
| Network behavior | Retrieves an additional stage | Does not retrieve a separate second stage |
| Initial payload size | Generally smaller | Generally larger |
| Configuration | Must match the staged payload | Must match the stageless payload |

> **Important:** Staged and stageless payload names are not interchangeable. Always consult the payload documentation for the installed Metasploit version.

---

## 3. Staged Payload Reference

Staged payloads use an initial connection component and subsequently receive the main payload stage.

### 🐧 Linux

Architecture: x86-64

### 🪟 Windows

Architecture: Select the payload architecture appropriate to the target.

### 🍎 macOS

Architecture: Select the appropriate Intel or Apple Silicon payload supported by the installed framework.

**Configuration checklist**

- [ ] Confirm the target operating system and architecture.
- [ ] Verify the payload is supported by the installed Metasploit version.
- [ ] Ensure the handler uses the exact matching payload.
- [ ] Confirm that lab routing and firewall rules permit the intended connection.

---

## 4. Stageless Payload Reference

Stageless payloads package the intended session functionality into one payload. This section applies the same conceptual checks across all three operating systems.

### 🐧 Linux — ELF

- **Format:** ELF executable
- **Architecture:** x86-64, where supported
- **Validation:** Confirm the output file exists and inspect its file type.

### 🪟 Windows

- **Format:** Choose a format supported by the selected payload and target.
- **Architecture:** Match the target system.
- **Validation:** Confirm the output format and architecture before testing.

### 🍎 macOS — Mach-O

- **Format:** Mach-O executable
- **Architecture:** Choose a supported Intel or Apple Silicon variant.
- **Validation:** Check the binary format and architecture on the target platform.

### 🔎 Output Verification

For an ELF file in your authorized lab:

```bash
ls -lh payload.elf
```

Inspect its file type:

```bash
file payload.elf
```

> A nonzero file size alone does not prove that a payload is valid, compatible, or capable of establishing a session.

---

## 5. Handler Configuration Concepts

The Metasploit multi-handler is used to handle incoming connections from compatible payloads.

**Core configuration fields**

| Setting | Purpose |
|---|---|
| `PAYLOAD` | Selects the exact payload type. |
| `LHOST` | Specifies the local listener address. |
| `LPORT` | Specifies the listening port. |

### Troubleshooting Checklist

- [ ] The handler payload matches the payload architecture and type.
- [ ] The listener is bound to the correct interface.
- [ ] The target can reach the listener over the lab network.
- [ ] Host firewalls and virtual network settings are configured appropriately.
- [ ] The payload is compatible with the target operating system.

---

## 6. Meterpreter Command Reference

For an established session in an authorized lab:

| Command | Description |
|---|---|
| `help` | List available commands. |
| `sysinfo` | Display system information. |
| `getuid` | Display the session's user identity. |
| `pwd` | Show the current working directory. |
| `ls` | List files and directories. |
| `background` | Return to the Metasploit prompt while retaining the session. |
| `shell` | Open a system shell where supported. |

---

## 7. Safety and Authorization

This reference is intended for cybersecurity education and authorized lab work.

- Obtain explicit permission before testing a system.
- Prefer isolated virtual machines and intentionally vulnerable targets.
- Avoid running payloads on systems you do not own or administer.
- Remove temporary artifacts and restore lab snapshots when testing is complete.

<p align="center">
  <strong>Learn deeply. Test responsibly. Build securely. 🔐</strong>
</p>

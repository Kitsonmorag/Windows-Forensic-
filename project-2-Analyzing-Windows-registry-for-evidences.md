# 🧭 Lab: Analyzing Windows Registry for Evidence of Malicious Activity

The Windows Registry is a critical hierarchical database that stores low-level settings for the operating system and installed applications. Analyzing the Windows Registry can reveal traces of malicious activity and provide valuable evidence during forensic investigations. This project will guide you through the process of using the Volatility tool to analyze Windows Registry hives and identify potential security incidents.

A hands‑on lab using **Volatility** to extract and analyze Windows Registry hives from a memory image to find persistence, user activity, and other forensic artifacts.

---

## 📋 Overview
- **Goal:** Identify registry-based artifacts of malicious activity from a memory image
- **Skills:** Memory forensics, Windows Registry analysis, command-line Volatility usage
- **Estimated time:** 1–2 hours

---

## 🔧 Lab Setup & Prerequisites
To complete this project, you will need access to a Windows operating system and a memory image for analysis. You can use a physical machine or set up a virtual machine using software like VirtualBox or VMware.

- ✅ **Environment:** Windows analysis machine (physical or VM)
- ✅ **Permissions:** Administrative privileges on the analysis machine
- ✅ **Knowledge:** Basic Windows/Registry concepts and CLI familiarity

Tools:
For this project, we will use the following tool:
1. **Volatility**: An advanced memory forensics framework.

- **Volatility** (Volatility 3 recommended for Python 3; Volatility 2 requires Python 2.7)
- Optional: Registry Explorer, Rekall, Redline

> ⚠️ Tip: Prefer Volatility 3 for new investigations unless you depend on legacy Volatility 2 plugins.

---

## 🛠️ Installing Volatility
1. Download from the official source: https://www.volatilityfoundation.org/releases 🔗
2. Extract to your chosen directory
3. Ensure Python compatibility (Volatility 2 → Python 2.7; Volatility 3 → Python 3.x)
4. Verify installation:

```sh
volatility --help
```

---

## 🔍 Exercises
**Objective**: Learn how to extract Windows Registry hives from a memory image using Volatility.
### 1️⃣ Extracting Windows Registry Hives
**Objective:** Dump registry hives from a memory image.

Steps:
1. Open a command prompt and navigate to the Volatility installation directory.
2. Run the following command to list the available profiles in the memory image:
```
  ```sh
   volatility -f <memory_image> imageinfo
   ```
3. Dump a hive at a virtual address:
```sh
```
volatility -f <memory_image> --profile=<profile> dumpregistry -o <virtual_address> -D <output_directory>

```
4. Note the virtual addresses of the hives and dump them using the dumpregistry plugin:
```sh
volatility -f <memory_image> --profile=<profile> dumpregistry -o <virtual_address> -D <output_directory>

```
Expected Output: You should be able to extract the Windows Registry hives from the memory image and save them to your specified directory.

---

### 2️⃣ Analyze the SAM Hive For user information
**Objective:** Extract user account info and password hashes.
Objective: Use Volatility to analyze the SAM hive and extract user account information.

Steps:

1. Ensure you have extracted the SAM hive from the memory image in Exercise 1.
2. Run the following command to parse the SAM hive and extract user account information:
```sh
volatility -f <memory_image> --profile=<profile> hashdump -y <system_hive> -s <sam_hive>
```
**Expected Output**: You should be able to view user account information, including username and hashed passwords.
---

### 3️⃣ Investigate Autorun Entries (SOFTWARE Hive)
**Objective:** Detect persistence via autorun keys.

Steps:

1. Ensure you have extracted the Software hive from the memory image in Exercise 1.
2. Run the following command to analyze autorun entries:
```sh
volatility -f <memory_image> --profile=<profile> printkey -o <software_hive_virtual_address> -K "Microsoft\Windows\CurrentVersion\Run"
```

**Expected Output**: You should be able to identify any autorun entries in the Software hive, which could indicate potential malware persistence mechanisms.
```

**Look for:** Unexpected entries, strange command lines, or suspicious paths.

---

### 4️⃣ Examine UserAssist (NTUSER.DAT)
Objective: Analyze User Assist keys to determine recently accessed applications by a specific user.

**Steps**:

1. Ensure you have extracted the NTUSER.DAT hive from the memory image in Exercise 1.
2. Run the following command to list User Assist keys:
```sh
volatility -f <memory_image> --profile=<profile> userassist -i
```

**Expected Output**: You should be able to view a list of recently accessed applications and their execution counts, which can help identify suspicious activity.
---

### 5️⃣ Correlate Registry Artifacts
**Objective:** Correlate information from different registry hives to gain a comprehensive understanding of potential malicious activity.

**Steps**:

1. Review the extracted data from previous exercises, including user account information, autorun entries, and User Assist keys.
2. Identify any patterns or correlations that could indicate a security incident.
3. Document your findings and provide recommendations for further investigation or remediation.

Expected Output: You should be able to correlate registry artifacts and provide a detailed analysis of potential malicious activity, supporting your findings with evidence from the Windows Registry.

With these exercises, you will gain practical experience in analyzing the Windows Registry for evidence of malicious activity using the Volatility tool. This will enhance your skills in memory forensics and help you effectively investigate security incidents.

---

## 📌 Reporting & Remediation Notes
- Include: hive filenames, virtual addresses, command output (or screenshots), timestamps, and hashes
- Recommend: isolate affected hosts, rotate credentials for compromised accounts, and run AV/EDR scans using observed indicators

---

## 💡 Further Reading & Tools
- Volatility docs: https://www.volatilityfoundation.org/releases 🔗
- Related tools: Rekall, Registry Explorer, Redline

---

## ✅ Quick Checklist
- [ ] Verify image integrity (hash)
- [ ] Run `imageinfo` → identify profile
- [ ] Dump SYSTEM, SAM, SOFTWARE, NTUSER.DAT as needed
- [ ] Run `hashdump`, `printkey`, `userassist` and other plugins
- [ ] Correlate artifacts and produce a report

---

**Author:** Mr Shakti Prasad Mahapatro — Cybersecurity Expert | AI | Automation's

*Additions or feedback welcome. If you'd like, I can open a PR with this file and add it under `training/labs/`.*

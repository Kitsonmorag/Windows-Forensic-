# 🧭 Lab: Analyzing Windows Registry for Evidence of Malicious Activity

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

### 1️⃣ Extracting Windows Registry Hives
**Objective:** Dump registry hives from a memory image.

Steps:
1. Discover the profile:
```sh
volatility -f <memory_image> imageinfo
```
2. List hives:
```sh
volatility -f <memory_image> --profile=<profile> hivelist
```
3. Dump a hive at a virtual address:
```sh
volatility -f <memory_image> --profile=<profile> dumpregistry -o <virtual_address> -D <output_directory>
```

**Expected:** Hive files saved in `<output_directory>` (e.g., `SYSTEM`, `SAM`, `SOFTWARE`, `NTUSER.DAT`).

---

### 2️⃣ Analyze the SAM Hive (User Accounts)
**Objective:** Extract user account info and password hashes.

Steps:
1. Ensure SAM hive is extracted
2. Run hashdump:
```sh
volatility -f <memory_image> --profile=<profile> hashdump -y <system_hive> -s <sam_hive>
```

**Expected:** Usernames and NTLM password hash data for offline analysis.

---

### 3️⃣ Investigate Autorun Entries (SOFTWARE Hive)
**Objective:** Detect persistence via autorun keys.

Steps:
1. Query the Run key in SOFTWARE hive:
```sh
volatility -f <memory_image> --profile=<profile> printkey -o <software_hive_virtual_address> -K "Microsoft\\Windows\\CurrentVersion\\Run"
```

**Look for:** Unexpected entries, strange command lines, or suspicious paths.

---

### 4️⃣ Examine UserAssist (NTUSER.DAT)
**Objective:** Find recently executed programs for a user.

Steps:
1. Ensure `NTUSER.DAT` is dumped
2. List UserAssist entries:
```sh
volatility -f <memory_image> --profile=<profile> userassist -i
```

**Expected:** App names, run counts, last-run times—useful to link suspicious actions to interactive sessions.

---

### 5️⃣ Correlate Registry Artifacts
**Objective:** Build a timeline and hypothesis from multiple registry sources.

Steps:
1. Review SAM, SOFTWARE, NTUSER.DAT, SYSTEM and other hives
2. Look for correlations (e.g., autorun entry + UserAssist execution by same user)
3. Document findings with timestamps, hashes, and evidence paths

**Tip:** Keep timestamps in UTC and preserve original hive files and command outputs for evidence.

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

**Author:** Kitsonmorag — Blue Team Lab

*Additions or feedback welcome. If you'd like, I can open a PR with this file and add it under `training/labs/`.*

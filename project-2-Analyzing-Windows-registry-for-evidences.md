cat > README.md << 'EOF'
# 🛡️ Analyzing Windows Registry for Evidence of Malicious Activity

## 📌 Project Overview
The Windows Registry is a critical hierarchical database that stores configuration settings and options for the Windows operating system and installed applications. During forensic investigations, analyzing registry artifacts extracted from memory can reveal evidence of malicious activity such as persistence mechanisms, unauthorized user accounts, and suspicious application execution.

This project demonstrates how to use Volatility, a memory forensics framework, to extract and analyze Windows Registry hives from a memory image.

---

## 🧪 Lab Setup
To complete this project, you will need:
- A Windows operating system
- A memory image (.raw, .mem, .dd)
- A physical machine or virtual machine (VirtualBox / VMware)

---

## 📚 Pre-requisites
- Basic understanding of Windows OS internals
- Familiarity with the Windows Registry
- Command-line interface experience
- Administrative privileges
- Python 2.7 installed

---

## 🛠️ Tools Used
### Volatility Framework
Volatility is an advanced open-source memory forensics framework used to extract digital artifacts from volatile memory.

---

cat > README.md << 'EOF'
git clone https://github.com/volatilityfoundation/volatility.git
cd volatility
python vol.py -h
Volatility 2.x requires Python 2.7

Exercises

Exercise 1: Extracting Windows Registry Hives

Objective:
Extract Windows Registry hives from a memory image using Volatility.

Steps:
volatility -f memory_image.raw imageinfo
volatility -f memory_image.raw --profile=Win7SP1x64 hivelist
volatility -f memory_image.raw --profile=Win7SP1x64 dumpregistry -o <VIRTUAL_ADDRESS> -D output/

Expected Output:
Registry hives:
SAM
SYSTEM
SOFTWARE
NTUSER.DAT

Exercise 2: Analyzing the SAM Hive

Objective:
Extract user account information and password hashes.

Steps:
volatility -f memory_image.raw --profile=Win7SP1x64 hashdump -y SYSTEM -s SAM

Expected Output:
Usernames
RIDs
NTLM password hashes

Exercise 3: Investigating Autorun Entries

Objective:
Identify persistence mechanisms via registry autorun keys.

Steps:
volatility -f memory_image.raw --profile=Win7SP1x64 printkey -o <SOFTWARE_HIVE_ADDRESS> -K "Microsoft\\Windows\\CurrentVersion\\Run"

Expected Output:
Programs configured to run at startup.

Exercise 4: Examining User Assist Keys

Objective:
Identify recently executed applications.

Steps:
volatility -f memory_image.raw --profile=Win7SP1x64 userassist -i

Expected Output:
Application paths
Execution counts
Timestamps

Exercise 5: Correlating Registry Artifacts

Objective:
Correlate multiple registry artifacts.

Steps:
Review SAM hive (user accounts)
Review SOFTWARE hive (autorun entries)
Review NTUSER.DAT (User Assist)

Identify:
Suspicious startup programs
Unauthorized user accounts
Unexpected application executions

Expected Output:
A detailed forensic assessment supported by registry evidence.

Key Learning Outcomes:
Registry hive extraction from memory
User account and credential analysis
Malware persistence detection
Behavioral analysis via User Assist
Correlation of forensic artifacts
EOF

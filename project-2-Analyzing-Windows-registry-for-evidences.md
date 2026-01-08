# 🛡️ Analyzing Windows Registry for Evidence of Malicious Activity

## 📌 Project Overview
The Windows Registry is a critical hierarchical database that stores configuration settings and options for the Windows operating system and installed applications. During forensic investigations, analyzing registry artifacts extracted from memory can reveal evidence of malicious activity such as persistence mechanisms, unauthorized user accounts, and suspicious application execution.

This project demonstrates how to use **Volatility**, a memory forensics framework, to extract and analyze Windows Registry hives from a memory image.

---

## 🧪 Lab Setup

To complete this project, you will need:

- A Windows operating system
- A memory image (`.raw`, `.mem`, `.dd`, etc.)
- A physical machine or virtual machine (VirtualBox / VMware recommended)

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

## 🔧 Installing Volatility

```bash
# Clone Volatility (example)
git clone https://github.com/volatilityfoundation/volatility.git

# Navigate to directory
cd volatility

# Verify installation
python vol.py -h

# metadata_

# Digital Forensics: Metadata & File Integrity Analysis

## 📌 Project Overview
This repository contains a forensic investigation of various digital artifacts. The objective was to identify hidden data (flags), detect file masquerading (spoofing), and analyze corrupted system files using standard industry tools.

## 🛠️ Forensic Toolkit
[cite_start]The following tools were utilized during the investigation[cite: 7]:

| Tool | Primary Command | Description |
| :--- | :--- | :--- |
| **file** | `file <filename>` | [cite_start]Identifies the true file type regardless of extension[cite: 7]. |
| **exiftool** | `exiftool <filename>` | [cite_start]Reads metadata such as GPS, author, and comments[cite: 7]. |
| **strings** | `strings <filename>` | [cite_start]Extracts readable character sequences from binary data[cite: 7]. |
| **binwalk** | `binwalk -e <filename>` | [cite_start]Scans for and extracts "nested" or hidden files[cite: 7]. |
| **Hexeditor** | `hexeditor <filename>` | [cite_start]Inspects raw bytes to check for corrupted headers[cite: 7]. |

---

## 🔍 Investigation Findings

### 1. Metadata Flag Discovery (`ocean.jpg`)
* [cite_start]**Observation:** The image details were analyzed using `exiftool`[cite: 1].
* [cite_start]**Discovery:** A hidden flag was located within the **Comment** section of the metadata[cite: 1].
* [cite_start]**Result:** `Comment: THIS IS THE HIDDEN FLAG`[cite: 1].
><img width="940" height="773" alt="image" src="https://github.com/user-attachments/assets/195ab16f-7546-4237-80e6-348cf5cb8935" />


### 2. Embedded File Extraction (`dog.jpg`)
* [cite_start]**Observation:** The file was scanned with `binwalk` to check for hidden data[cite: 2].
* [cite_start]**Discovery:** A hidden **Zip archive** was detected inside the JPEG[cite: 2].
* [cite_start]**Action:** The archive was extracted, revealing a file named `hidden_text.txt`[cite: 2].
* [cite_start]**Result:** `cat hidden_text.txt` -> `THIS IS A HIDDEN FLAG`[cite: 2].
><img width="940" height="149" alt="image" src="https://github.com/user-attachments/assets/66759a57-ed06-4eac-b1f4-1ea17a9e2605" />
><img width="940" height="371" alt="image" src="https://github.com/user-attachments/assets/b71d49a3-c57c-4044-8276-6dd05af61cf8" />



### 3. Extension Spoofing Analysis (`solitaire.exe`)
* [cite_start]**Status:** **CRITICAL - MALICIOUS DISGUISE**[cite: 3].
* [cite_start]**Observation:** The file used a `.exe` extension, but the underlying bytes proved it was actually a **PNG Image**[cite: 3].
* [cite_start]**Technical Evidence:** The Magic Bytes were identified as `89 50 4E 47 0D 0A 1A 0A`[cite: 4].
* [cite_start]**Resolution:** The extension must be corrected to `.png` to view the file properly[cite: 4].
><img width="940" height="280" alt="image" src="https://github.com/user-attachments/assets/308487d6-42d7-4917-8a05-5ba5a1893432" />


### 4. Header Corruption (`.DS_Store`)
* [cite_start]**Status:** **CORRUPTED / WIPED**[cite: 5].
* [cite_start]**Observation:** Inspected using hex analysis, revealing that the "Fundamental DNA" of the file was altered[cite: 5].
* [cite_start]**Technical Evidence:** The first 8 bytes evaluated to `00 00 00 01 42 75 64 31`, which does not match any standard file signature[cite: 5].
* [cite_start]**Conclusion:** The header was likely purposely wiped by an attacker or severely corrupted[cite: 6].
><img width="940" height="232" alt="image" src="https://github.com/user-attachments/assets/b2b4334a-83fe-4d0b-aec7-fdf5f2e866e4" />


---

## ⚙️ Usage
To replicate this analysis, ensure you have a Linux environment (e.g., Kali Linux) and run:

```bash
# Install Dependencies
sudo apt update && sudo apt install exiftool binwalk -y

# Run Automated Analysis
python3 analyzer.py

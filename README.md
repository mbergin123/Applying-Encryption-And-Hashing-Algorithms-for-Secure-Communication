# Applying-Encryption-And-Hashing-Algorithms-for-Secure-Communication
This lab demonstrates how hashing and GPG encryption protect data integrity and confidentiality. I generated MD5/SHA-1 hashes, detected file modifications, created GPG key pairs, exchanged public keys, and securely encrypted and decrypted messages between users.
---

## **Learning Objectives**
By completing this lab, I was able to:

- Apply common cryptographic and hashing techniques to ensure message confidentiality and integrity.  
- Verify the integrity of files using MD5 and SHA-1 hashing.  
- Demonstrate how file changes affect hash values.  
- Explain why hashing a file before execution is essential for security.  
- Generate GPG key pairs for two users and exchange public keys.  
- Encrypt and decrypt messages using GnuPG to ensure secure communication.  

---

## **Network Topology**
**Filename:** `Top_Encrypt.png`

This lab was performed on:

- **vWorkstation (Windows Server 2016)**  
- **TargetLinux01 (Debian Linux)**  
- **TargetLinux02 (Debian Linux)**  

The Student and Instructor accounts existed on **TargetLinux01**.

---

## **Tools Used**
- **GNU Privacy Guard (GPG / GnuPG)** – Asymmetric encryption & key management  
- **MD5 / SHA1 Hashing Utilities** – Integrity verification  
- **WinSCP** – Secure file transfer  
- **vi / gedit Text Editors** – File creation and modification  

---

# **Activity Log**

Below is the complete step-by-step documentation of everything I performed during this lab, including the filenames for each screenshot.

---

## **1. File Creation & Initial Hashing**

### **Created the initial file**
Typed text in gedit and saved as `Example.txt`.  
**Filename:** `getit_saved.png`

### **Viewed MD5 documentation**
```
md5sum --help
```
**Filename:** `md5sum_help.png`

### **Navigated to Documents & verified file**
Used `ls -l` and `cat Example.txt`.  
**Filename:** `cd_example.png`

### **Generated MD5 hash**
```
md5sum Example.txt
```
**Filename:** `md5sum_example.png`

### **Stored the MD5 hash**
```
md5sum Example.txt > Example.txt.md5
```
**Filename:** `example_stored.png`

### **Viewed stored hash**
```
cat Example.txt.md5
```
**Filename:** `cat_md5sum.png`

### **Verified MD5 integrity**
```
md5sum -c Example.txt.md5
```
**Filename:** `example_ok.png`

---

## **2. SHA-1 Hashing**

### **Viewed SHA-1 documentation**
```
sha1sum --help
```
**Filename:** `sha1_help.png`

### **Generated SHA-1 hash**
```
sha1sum Example.txt
```
**Filename:** `sha1_example.png`

### **Stored SHA-1 hash**
```
sha1sum Example.txt > Example.txt.sha1
```
**Filename:** `sha1_stored.png`

### **Viewed stored SHA-1**
```
cat Example.txt.sha1
```

### **Verified SHA-1 integrity**
```
sha1sum -c Example.txt.sha1
```
**Filename:** `cat_sha1.png`

---

## **3. File Modification & Integrity Change Detection**

### **Appended text to modify the file**
```
echo marilyn >> Example.txt
```
**Filename:** `echo_example.png`

### **Generated NEW MD5 hash**
New hash differed from original.  
**Filename:** `mod_md5sum.png`

### **Generated NEW SHA-1 hash**
New SHA-1 also differed.  
**Filename:** `mod_sha1.png`

This demonstrated that **any change to a file produces a completely different hash**, proving hashing's usefulness in integrity verification.

---

## **4. GPG Key Generation (Student Account)**

### **Generated Student GPG key**
Used:
```
gpg --gen-key
```
Chose RSA 1024-bit, no expiration.  
**Filename:** `gen_key.png`

### **Ran entropy loop**
Opened a second terminal:
```
./entropy_loop.sh
```
**Filename:** `keep_machine_busy.png`

### **Exported Student public key**
```
gpg --export -a > student.pub
```
**Filenames:** `pgp_export.png`, `studentPub`

---

## **5. GPG Key Generation (Instructor Account)**

### **Switched users & generated Instructor key**
```
su instructor
gpg --gen-key
```
**Filename:** `Instructor_key.png`

### **Exported Instructor public key**
```
gpg --export -a > instructor.pub
```
**Filenames:** `instructor_stored.png`, `insturctorPub.png`

---

## **6. Public Key Exchange**

### **Copied Instructor key to Student account**
```
sudo cp /home/Instructor/instructor.pub /home/student/Documents/
```
**Filename:** `instructor_copy.png`

### **Listed Student keys**
Confirmed only Student’s key was present.  
**Filename:** `list_publickeys.png`

### **Imported Instructor public key**
```
gpg --import instructor.pub
```
**Filename:** `instructor_import.png`

### **Verified both keys now exist**
**Filename:** `both_keys.png`

---

## **7. Encrypting the Cleartext Message**

### **Created cleartext message**
```
echo "this is a clear-text message from marilyn" > cleartext.txt
```
**Filename:** `cleartextmessage.png`

### **Encrypted using Instructor key**
```
gpg -e cleartext.txt
```
Chose recipient: **Instructor**  
**Filename:** `encrypt_cleartext.png`

### **Verified encrypted file exists**
`cleartext.txt.gpg` is now present.  
**Filename:** `cleargpg_listed.png`

### **Viewed encrypted contents (ciphertext)**
```
cat cleartext.txt.gpg
```
Unreadable output confirmed encryption.  
**Filename:** `encrypted_proof.png`

---

## **8. Message Delivery & Decryption**

### **Copied encrypted file to Instructor**
```
sudo cp cleartext.txt.gpg /home/Instructor/
sudo chown instructor:instructor cleartext.txt.gpg
```
**Filename:** `sudo_instructor.png`

### **Decrypted message using Instructor’s private key**
```
su Instructor
gpg -d cleartext.txt.gpg
```
The decrypted output revealed:  
**“this is a clear-text message from marilyn”**  
**Filename:** `decrypt_cleartext.png`

---

# **Skills Demonstrated**

✔ File integrity verification using MD5 & SHA-1  
✔ Detecting tampering through hash comparison  
✔ Linux command-line proficiency  
✔ GPG key generation, import, export, and trust workflow  
✔ Asymmetric encryption & decryption  
✔ Secure message transfer between users  
✔ Understanding of confidentiality vs. integrity concepts  

---

# **Why This Lab Is Important**

This lab mirrors real-world cybersecurity workflows used in:

- Secure communications  
- Email encryption  
- Software integrity verification  
- Digital signatures  
- Secure file distribution  
- Insider threat prevention  

Hashing ensures files are unmodified, while GPG encryption ensures only intended recipients can read sensitive information. These are critical skills for SOC analysts, penetration testers, security engineers, and systems administrators.

---

# **Final Reflection**

This lab reinforced the importance of both hashing and encryption in secure communication. Hashing provided a reliable method of detecting any unauthorized file changes, while GPG enabled true end-to-end encryption between two users. Completing the full key lifecycle (creation → export → import → encryption → decryption) gave me practical experience with asymmetric cryptography — a foundational skill in cybersecurity.

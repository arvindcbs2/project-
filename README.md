# Enterprise Active Directory User Provisioning Script

## 📌 Overview

This PowerShell script provides an **enterprise-grade solution** for **bulk Active Directory user provisioning** using a CSV file. It is designed for **production environments** with a focus on **security, validation, logging, and scalability**.

The script:

* Creates AD users in bulk
* Validates mandatory CSV fields
* Highlights and skips incomplete records
* Assigns users to one or more AD groups
* Enforces password and security best practices
* Generates audit-ready logs

---

## ✨ Features

✔ Bulk user creation from CSV
✔ Mandatory field validation (no silent failures)
✔ Highlights empty or missing CSV columns
✔ Safe re-runs (idempotent – no duplicate users)
✔ Assigns multiple AD groups per user
✔ Forces password change at first logon
✔ Full execution logging (audit & compliance ready)
✔ Error handling without script interruption

---

## 🧰 Prerequisites

* Windows Server or workstation with:

  * **Active Directory module for PowerShell**
* Script must be run by an account with:

  * Permission to create AD users
  * Permission to modify group membership

```powershell
Import-Module ActiveDirectory
```

---

## 📂 CSV File Structure

The script expects a CSV file with the following headers:

```csv
firstname,lastname,username,password,OU,City,Company,jobtitle,department,office,State,Email,Description,Groups
```

### 🔹 Example CSV Entry

```csv
Arvind,Sharma,asharma,Temp@123,"OU=Users,DC=jmbaxigrp,DC=com",Mumbai,JMBAXI,IT Executive,IT,HO,MH,arvind@jmbaxigrp.com,New Joiner,"HR-Users;IT-Users"
```

### 🔹 Notes

* **Groups**: Multiple groups must be separated by a semicolon (`;`)
* **OU**: Must be a valid Distinguished Name
* **Password**: Must comply with domain password policy

---

## 🔐 Mandatory Fields Validation

The following fields are **mandatory** and cannot be empty:

* firstname
* lastname
* username
* password
* OU
* Email

If any of these fields are missing, the user is skipped and highlighted:

```text
WARNING: Skipping user [asharma] - Missing mandatory fields: OU, Email
```

✔ Script continues processing other users
✔ No crash or partial execution

---

## 👥 Group Assignment Logic

* The `Groups` column supports **multiple AD groups**
* Groups are split using `;`
* If a group does not exist or fails, the script logs a warning but continues

Example:

```csv
Groups
HR-Users;VPN-Users;IT-Admins
```

---

## 📝 Logging & Auditing

* Execution is logged using `Start-Transcript`
* Log file example:

```text
C:\Scripts\AD_User_Creation.log
```

Logs include:

* User creation success
* Skipped users with missing data
* Group assignment status
* Errors and warnings

---

## ▶ How to Run

1. Update CSV path and log path in the script
2. Ensure CSV headers match expected format
3. Run PowerShell **as Administrator**

```powershell
.\Create-ADUsers.ps1
```

---

## 🛡 Security Best Practices Implemented

* Passwords are converted to secure strings
* Users are forced to change password at first login
* No plaintext passwords stored outside CSV
* Script avoids hard failures

---

## 📈 Enterprise Use Cases

* HR onboarding automation
* Bulk user creation during mergers
* Campus hiring / fresher onboarding
* Migration from legacy systems

---


## 🚀 Future Enhancements (Optional)

* Automatic secure password generator
* HTML execution report
* Email notification to HR / Manager
* Rollback logic on partial failure
* Azure AD / Entra ID integration

---

## 📜 License

Internal enterprise automation script – customize as per organizational security policies.

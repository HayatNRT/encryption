# DATA SECURITY & PROTECTION OVERVIEW

**Product:** [Benefit]
**Provider:** NRT Consultancy Pvt. Ltd.
**Prepared by:** Tarique Hayat
**Version:** 1.0
**Date:** [18-11-2025]

---

## 1. Purpose of This Document

This document describes the technical and organisational measures implemented to ensure the secure processing, storage, and protection of personal data in compliance with GDPR and related data protection regulations. It is intended for clients, legal bodies, compliance teams, auditors, and security staff.

---

## 2. Security Principles

The system is designed around the following principles:

* Security by design and by default
* Encryption at rest and in transit
* Zero plaintext storage
* Least-privilege access controls
* Separated key management
* GDPR-ready data lifecycle
* Support for privacy rights (erasure, access, retention control)

---

## 3. Encryption of Personal Data

All sensitive data stored in the system is encrypted using strong AES-based encryption.

Key points:

* Field-level encryption
* No raw personal data stored in the database
* Encrypted data remains encrypted in backups
* Decryption only happens inside the secured application layer

This protects personal data even if a database or backup exposure occurs.

---

## 4. Key Management & Rotation

* Encryption keys are automatically generated and securely stored
* Keys are never exposed in configuration files
* The system supports full **key rotation**
* During rotation, all encrypted records are securely re-encrypted
* Write operations are safely locked during rotation to prevent corruption

---

## 5. Data Storage & Infrastructure

* Data is stored in controlled and secure regions
* Hosting region: [Insert region – e.g., AWS Mumbai / EU Frankfurt / etc.]
* No personal data exists in plaintext on infrastructure-level systems
* DB and backup access is restricted and monitored

---

## 6. Access Control

* Decrypted data is only accessible via authorized backend services
* Direct DB access does not reveal readable personal data
* Role-based access control can be enforced
* Administrative access is strictly limited and logged

---

## 7. GDPR Anonymisation & User Deletion

To comply with GDPR Article 17 (Right to Erasure), the system supports:

* Removal of personal identifiers
* Conversion of personal values to anonymised placeholders
* Preservation of database integrity
* No remaining personal data after deletion

Example of anonymisation:

* Name → "DELETED USER"
* Email → "deleted_user_[id]@privacy.local"
* Phone → null

---

## 8. Logging & Audit

* System logs never contain decrypted personal data
* Only technical and diagnostic information is logged
* Administrative events (e.g. key rotation, anonymisation) can be audited
* Sensitive values are not stored or transmitted in logs

---

## 9. Backup & Disaster Recovery

* Backups include only encrypted values
* Backup storage inherits encryption
* Access to backups is restricted
* Retention policies are applied in accordance with compliance requirements

---

## 10. AI & GPS Data Handling

**GPS:**
The system does not currently store precise geolocation or GPS metadata. If location data is introduced in the future, it will follow the same encryption and minimisation framework.

**AI:**
No personal data is currently used for AI model training or automated decision-making. Future AI usage would require pseudonymisation and compliance safeguards.

---

## 11. Regulatory Compliance Summary

The architecture and security model support:

* GDPR Article 5 – Integrity & Confidentiality
* GDPR Article 17 – Right to Erasure
* GDPR Article 25 – Privacy by Design
* GDPR Article 32 – Security of Processing
* Data minimisation & pseudonymisation requirements

---

## 12. Responsible Contact

**Security & Data Protection Contact**
Name: Tarique Hayat
Email: [hayatnrt7@gmail.com](mailto:hayatnrt7@gmail.com)
Company: NRT Consultancy Pvt. Ltd.

---


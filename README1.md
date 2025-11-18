📄 DATA SECURITY & PROTECTION OVERVIEW

Product: Benefit
Provider: NRT Consultancy Pvt. Ltd.
Prepared by: Sachin Prasad
Version: 1.0
Date: 18-11-2025

1. Purpose

This document outlines the technical and organizational measures implemented to ensure secure processing, storage, and protection of personal data within the Benefit platform. It demonstrates compliance with GDPR and other data protection regulations. The intended audience includes clients, legal authorities, auditors, security teams, and compliance officers.

2. Core Security Principles

Benefit is built on modern data protection and privacy-by-design principles, including:

Security by design and by default

Encryption at rest and in transit

Zero plaintext data storage

Least-privilege / access-based controls

Independent key management

GDPR-ready data lifecycle

Support for privacy rights (erasure, access, export, retention)

3. Personal Data Encryption

All sensitive data stored in the system is encrypted using strong, industry-standard AES encryption.

Key Highlights

Field-level encryption (sensitive fields are encrypted individually)

No raw personal data in the database

All backups contain encrypted data only

Data is decrypted only inside the secure application layer

This prevents exposure even in the event of unauthorized database access.

4. Encryption Key Management & Rotation

The platform includes built-in cryptographic lifecycle management:

Keys are auto-generated and stored securely

Keys are never included in application configuration

Full key rotation is supported

All existing records are re-encrypted during rotation

A safe maintenance lock prevents conflicting write operations

5. Data Storage & Hosting

Data is stored only in secure and controlled hosting regions

Hosting Region: [Insert region – e.g., AWS Mumbai / EU Frankfurt]

No plaintext personal data exists in infrastructure or storage layers

Database and backup locations are protected and access-controlled

6. Access Control

Decryption only happens within the backend application services

Direct DB access cannot expose plaintext personal data

Role-based access control (RBAC) can be enforced

Administrative access is restricted and logged

7. GDPR-Compliant Anonymisation & Deletion

The platform supports GDPR Article 17 (Right to Erasure) through anonymisation-safe deletion:

Method:

Personal identifiers are permanently removed or replaced

Database integrity (foreign keys & history) remains intact

No personal data remains after anonymisation

Example:

Name  → DELETED USER
Email → deleted_user_[id]@privacy.local
Phone → null

8. Logging & Audit Controls

Logs do not contain decrypted personal data

Only non-sensitive operational information is logged

Sensitive values are never stored in logs

Audit logs can include key rotations and deletion events

9. Backup & Disaster Recovery

All backups contain encrypted data only

Backup access is controlled and monitored

Retention policies follow compliance requirements

Backup restores preserve encryption and privacy

10. AI & GPS Data Handling
GPS Data

The current version of Benefit does not store or process precise geolocation (GPS).
If introduced later, location data will follow the same encryption and minimisation policies.

AI / Machine Learning

The system does not use personal data for automated decision-making or AI model training.
Future implementation, if any, will require pseudonymisation, consent, and regulatory safeguards.

11. Regulatory Compliance Alignment

The Benefit platform aligns with:

GDPR Article	Compliance
Art. 5	Integrity & Confidentiality
Art. 17	Right to Erasure
Art. 25	Privacy by Design & Default
Art. 32	Security of Processing
-	Data minimisation & pseudonymisation
12. Contact Information

Security & Data Protection Contact
Name: Sachin Prasad
Email: psachin.nrt@gmail.com

Company: NRT Consultancy Pvt. Ltd.

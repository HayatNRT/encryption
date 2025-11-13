
# 🔐 Spring Boot Starter – Data Encryption

**Artifact:** `spring-boot-starter-data-encryption`
**Group ID:** `org.spring.hayat`
**Version:** `2.0.4`

---

## 📘 Overview

`spring-boot-starter-data-encryption` is a **plug-and-play Spring Boot starter** that enables **automatic field-level encryption and decryption** for JPA entities.
It helps secure sensitive data at rest using **AES encryption**, with full **auto-configuration** support — no manual setup required.

This starter is designed for real-world enterprise environments where **data security, encryption key lifecycle management, and maintenance safety** are top priorities.

---

## ⚙️ Dependency Setup

Add the following dependency to your Maven `pom.xml`:

```xml
<dependency>
    <groupId>org.spring.hayat</groupId>
    <artifactId>spring-boot-starter-data-encryption</artifactId>
    <version>2.0.4</version>
</dependency>
```

---

## 🔧 Configuration Properties

| Property                             | Type      | Description                                                                                | Required                 |
| ------------------------------------ | --------- | ------------------------------------------------------------------------------------------ | ------------------------ |
| `spring.nrt-encryption.enabled`      | `boolean` | Enables the encryption starter’s auto-configuration.                                       | ✅ Yes                    |
| `spring.nrt-encryption.package-scan` | `string`  | Base package path to scan for encryptable entities. Required when performing key rotation. | ⚙️ Only for key rotation |

### Example:

```properties
spring.nrt-encryption.enabled=true
spring.nrt-encryption.package-scan=org.spring.nrt.entity (Your Entity Package)
```

---

## 🧱 Core Components

### 1. **`@Convert(converter = AesEncryption.class)`**

Mark any entity field you want to encrypt with this annotation.

```java
@Entity
public class Customer extends EncryptableEntity {

    @Id
    private Long id;

    @Convert(converter = AesEncryption.class)
    private String aadhaarNumber;

    @Convert(converter = AesEncryption.class)
    private String panNumber;
}
```

🔒 These annotated fields are automatically **encrypted when persisted** and **decrypted when retrieved**.

---

### 2. **`EncryptableEntity` (Base Class)**

Every entity containing encrypted fields **must extend** this base class.
It provides the internal hooks for encryption and decryption processes.

```java
public class Customer extends EncryptableEntity {
    ...
}
```

---

### 3. **`EncryptionRepository<T, ID>`**

A custom repository that replaces `JpaRepository` and ensures that all CRUD operations are encryption-aware.

```java
public interface CustomerRepository extends EncryptionRepository<Customer, Long> {
}
```

This repository layer manages encryption/decryption transparently during persistence operations.

---

### 4. **`@MaintenanceLock`**

`@MaintenanceLock` is a critical annotation designed to **prevent accidental writes to the database during maintenance mode**, such as **key rotation or bulk re-encryption**.

It ensures data remains **consistent, stable, and uncorrupted** while encryption keys are being updated.

```java
@Service
public class CustomerService {

    @MaintenanceLock
    public void updateSensitiveData() {
        // Any write or update to DB
    }
}
```

#### 💡 How It Works

* When the system enters **maintenance mode** (e.g., during `reEncryptAll()`),
  `@MaintenanceLock` ensures **no database writes** occur.
* This prevents inconsistent or partially encrypted records.
* Once the re-encryption completes, normal DB operations automatically resume.

✅ Apply it to:

* Service methods performing DB writes
* Scheduler or batch operations
* WebSocket handlers writing data to DB

---

## 🔑 Encryption Key Lifecycle

### 1. **Key Initialization**

When `spring.nrt-encryption.enabled=true`:

* The system checks if an encryption key already exists in the database.
* If absent, a new key is securely generated and stored.
* This key is then automatically loaded into the encryption context for all operations.

---

## 🔄 Key Rotation (Re-Encryption)

To ensure long-term data security, this starter supports **key rotation** — securely replacing the old key with a new one and **re-encrypting all existing data**.

### 🧩 Prerequisite

Define the base entity package in your configuration:

```properties
spring.nrt-encryption.package-scan=org.spring.nrt.entity
```

This enables the system to **dynamically discover all encryptable entities** for re-encryption.

### 🔹 Usage

```java
@Autowired
private EncryptionKeyService encryptionKeyService;

public void rotateKeys() {
    encryptionKeyService.reEncryptAll();
}
```

### 🔹 Internal Workflow

1. The system scans all entities within the configured package.
2. Decrypts existing encrypted fields using the **old key**.
3. Generates a new AES key and persists it in the database.
4. Re-encrypts all data using the new key.
5. Locks all write operations during this process via `@MaintenanceLock` to maintain **data consistency**.

⚠️ **Important:**
Run `reEncryptAll()` only during a **maintenance window** or when user write operations are minimal.

---

## 🪄 Auto-Configuration Flow

When `spring.nrt-encryption.enabled=true`:

1. Auto-configuration triggers the encryption context setup.
2. Beans for AES encryption, key management, and repository support are registered.
3. The encryption key is verified or generated in the database.
4. `@MaintenanceLock` interceptor is initialized to enforce safe DB operations during maintenance.
5. Repositories extending `EncryptionRepository` become fully encryption-aware.

---

## ✅ Example Usage

```java
@Service
public class CustomerService {

    private final CustomerRepository repository;
    private final EncryptionKeyService encryptionKeyService;

    public CustomerService(CustomerRepository repository, EncryptionKeyService encryptionKeyService) {
        this.repository = repository;
        this.encryptionKeyService = encryptionKeyService;
    }

    @MaintenanceLock
    public void saveCustomer(Customer customer) {
        repository.save(customer);
    }

    public List<Customer> getAllCustomers() {
        return repository.findAll();
    }

    @MaintenanceLock
    public void rotateEncryptionKey() {
        // Perform key rotation safely
        encryptionKeyService.reEncryptAll();
    }
}
```

---

## 🧩 Advantages

* 🔒 **AES encryption** for sensitive entity fields
* ⚙️ **Zero configuration setup** — just one property
* 🧠 **Automatic key generation and storage**
* 🔄 **Seamless key rotation** with re-encryption support
* 📦 **Dynamic entity scanning** for re-encryption
* 🚫 **`@MaintenanceLock` prevents accidental DB writes during maintenance**
* 🧱 Compatible with all Spring Boot JPA-based projects

---

## ⚙️ Best Practices

* Always configure `spring.nrt-encryption.package-scan` before key rotation.
* Schedule `reEncryptAll()` during **low-traffic maintenance windows**.
* Avoid logging decrypted values.
* Back up your encryption key table regularly.
* Never expose encryption keys through APIs or configuration files.

---

## 🚀 Future Enhancements

* Integration with **AWS KMS / Azure Key Vault / GCP Secret Manager**
* **Automated key rotation scheduler**
* Encryption audit logging
* CLI tool for manual re-encryption

---

## 🧾 License & Credits

Developed and maintained by **Tarique Hayat**
Under **NRT Consultancy Pvt. Ltd.**

**Namespace:** `org.spring.hayat`
**Artifact:** `spring-boot-starter-data-encryption`
**Version:** `2.0.4`

© 2025 – **NRT Consultancy Pvt. Ltd. | Secure Data Layer Initiative**

---

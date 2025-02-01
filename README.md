# **CIS Dynamic UI & API Framework 🚀**
> A metadata-driven **UI framework** with **business rules**, **soft delete**, and **audit logging**, using **FastAPI, PostgreSQL, and MongoDB**.

---

## **🔹 Overview**
This project provides a system that allows dynamic **form generation, role-based access, business rules execution, and auditing**.

### **🛠️ Tech Stack**
- **FastAPI** → API framework
- **PostgreSQL** → Stores UI metadata, business rules, permissions
- **MongoDB** → Stores actual business data (Claims, Referrals, etc.)
- **JWT Authentication** → Secure user authentication
- **Redis (Optional)** → Caching for faster metadata queries

---

## **📌 Database Schema**
This project uses **PostgreSQL** for structured metadata storage and **MongoDB** for business data.

### **📑 Core Tables**
#### **1️⃣ Entities & Fields (PostgreSQL)**
- `ENTITIES` → Stores metadata for business entities (e.g., Claim, Referral).
- `ENTITY_FIELDS` → Stores fields for each entity dynamically.

#### **2️⃣ UI Panels & Layout (PostgreSQL)**
- `PANELS` → Defines UI sections like **Header, Footer, Grid**.

#### **3️⃣ Business Rules (PostgreSQL)**
- `BUSINESS_RULES` → Stores validation rules (onSave, onBlur).
- `RULE_ASSIGNMENTS` → Maps rules to fields.

#### **4️⃣ Role-Based Security (PostgreSQL)**
- `USERS`, `ROLES`, `USER_ROLES`
- `ROLE_CAPABILITIES` → Maps permissions (CRUD on entities).

#### **5️⃣ Extensions (PostgreSQL)**
- `EXTENSION_TABLES` → Allows adding new fields post-deployment.
- `EXTENSION_COLUMNS` → Stores additional columns for each entity.

#### **6️⃣ Audits & Soft Delete (PostgreSQL)**
- `AUDIT_LOGS` → Tracks all changes (CREATE, UPDATE, DELETE, RESTORE).
- `IsDeleted` flag in all tables enables **soft deletion**.

---

## **🔹 API Design**
APIs are modular and follow **separation of concerns**.

### **1️⃣ Metadata APIs (PostgreSQL)**
| Method | Endpoint | Description |
|--------|---------|-------------|
| `GET` | `/metadata/entities/` | Get list of entities |
| `GET` | `/metadata/entities/{entity_id}/fields` | Get fields of an entity |
| `GET` | `/metadata/entities/{entity_id}/panels` | Get UI panels |

### **2️⃣ Business Data APIs (MongoDB)**
| Method | Endpoint | Description |
|--------|---------|-------------|
| `POST` | `/data/{entity_name}` | Insert new record |
| `GET` | `/data/{entity_name}` | Retrieve all records |
| `GET` | `/data/{entity_name}/{record_id}` | Retrieve specific record |

### **3️⃣ Business Rules APIs**
| Method | Endpoint | Description |
|--------|---------|-------------|
| `POST` | `/rules/execute` | Execute a business rule on input |

### **4️⃣ Security & Authentication APIs**
| Method | Endpoint | Description |
|--------|---------|-------------|
| `POST` | `/security/users/login` | Authenticate user |
| `GET` | `/security/permissions/{role_id}` | Get user role permissions |

### **5️⃣ Logging & Soft Delete APIs**
| Method | Endpoint | Description |
|--------|---------|-------------|
| `GET` | `/logs/entities/{entity_id}` | Get entity change logs |
| `POST` | `/data/{entity_name}/{record_id}/restore` | Restore soft-deleted record |

---

## **🔹 Workflow & Joins**
1. **User logs in** via `/security/users/login`
2. **UI dynamically loads metadata** (`/metadata/entities/{entity_id}`)
3. **User fills out form & submits data** (`/data/{entity_name}`)
4. **Business rules execute** (`/rules/execute`)
5. **Role-based security checks** (`/security/permissions/{role_id}`)
6. **Changes are logged** (`/logs/entities/{entity_id}`)
7. **Soft-delete records are archived** (`IsDeleted = true`)

---

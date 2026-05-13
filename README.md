# BLAST 🔗
### Blockchain Ledger & Audit System for Transactions

> A tamper-proof, immutable banking transaction ledger built on a private blockchain with a Spring Boot backend, JWT-secured REST API and role-based access control.

---

## 🔗 Project Link

>  🚀 **Deployed Project:** [Click here to open BLAST](https://pbl-omega-brown.vercel.app/)  

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [File Structure](#-file-structure)
- [Getting Started](#-getting-started)
- [API Reference](#-api-reference)
- [Roles & Permissions](#-roles--permissions)
- [Blockchain Design](#-blockchain-design)
- [Team](#-team)

---

## 🧭 Overview

Traditional banking databases allow privileged users to silently modify or delete records — creating serious audit risk and compliance vulnerabilities. **BLAST** addresses this by recording every financial transaction on a private blockchain, making tampering immediately detectable.

The system combines:
- A **MySQL-backed Spring Boot API** for transaction management
- A **private Ethereum blockchain (Ganache)** for on-chain immutability via Web3j
- **JWT authentication** with three roles: Admin, Auditor, and User
- **SHA-256 block hashing** to ensure chain integrity

---

## ✨ Features

- Create and store financial transactions in MySQL
- Sync transactions to a private Ethereum blockchain (Ganache)
- On-chain immutability — any data change breaks the hash chain
- Tamper detection via blockchain integrity verification
- JWT-based stateless authentication
- Three-tier role-based access control (Admin / Auditor / User)
- Duplicate transaction prevention at the service layer
- Centralized, consistent error responses via GlobalExceptionHandler
- Bean Validation on all API inputs

---

## 🛠 Tech Stack

|    Layer   |              Technology             |
|------------|-------------------------------------|
| Language   | Java 17+                            |
| Framework  | Spring Boot 3.x                     |
| Security   | Spring Security + JWT (JJWT 0.11.5) |
| ORM        | Spring Data JPA / Hibernate         |
| Database   | MySQL 8                             |
| Blockchain | Ganache (local Ethereum) + Web3j    |
| Build Tool | Maven                               | 
| Frontend   | Vanilla HTML / CSS / JavaScript     |

---

## 🏗 Architecture

```
Client (Browser)
      │
      ▼
 Frontend (HTML / CSS / JS)
      │  REST API calls
      ▼
 Spring Boot Backend (port 8081)
      │
      ├──► AuthController        ──► UserService       ──► MySQL (users table)
      │
      ├──► TransactionController ──► TransactionService ──► MySQL (transactions table)
      │
      └──► BlockchainController  ──► BlockchainService  ──► Ganache (port 7545)
                                                               via Web3j
```

**Request flow for protected endpoints:**

```
Request ──► JwtAuthFilter ──► SecurityConfig (role check) ──► Controller ──► Service ──► Repository
              (validates JWT,       (permits or 403)
           sets SecurityContext)
```

---

## 📁 File Structure

```
PBL/
├── backend/
│   ├── .mvn/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/sable/
│   │   │   │   │
│   │   │   │   ├── blockchain/
│   │   │   │   │   ├── BlockchainController.java   # GET /blockchain/send/{id}, /blockchain/sync
│   │   │   │   │   └── BlockchainService.java      # Web3j integration, sends ETH via Ganache
│   │   │   │   │
│   │   │   │   ├── controller/
│   │   │   │   │   ├── AuthController.java         # POST /api/auth/register, /api/auth/login
│   │   │   │   │   └── TransactionController.java  # CRUD endpoints for transactions
│   │   │   │   │
│   │   │   │   ├── dto/
│   │   │   │   │   ├── AuthResponse.java           # Returned after login/register (token + user info)
│   │   │   │   │   ├── LoginRequest.java           # { usernameOrEmail, password }
│   │   │   │   │   ├── RegisterRequest.java        # { username, email, password, role }
│   │   │   │   │   └── TransactionRequest.java     # { transactionId, sender, receiver, amount }
│   │   │   │   │
│   │   │   │   ├── exception/
│   │   │   │   │   ├── DuplicateTransactionException.java  # 409 Conflict
│   │   │   │   │   ├── GlobalExceptionHandler.java         # Centralized error → HTTP mapping
│   │   │   │   │   └── ResourceNotFoundException.java      # 404 Not Found
│   │   │   │   │
│   │   │   │   ├── model/
│   │   │   │   │   ├── Role.java                   # Enum: ADMIN, AUDITOR, USER
│   │   │   │   │   ├── Transaction.java            # JPA entity → transactions table
│   │   │   │   │   └── User.java                   # JPA entity → users table
│   │   │   │   │
│   │   │   │   ├── repository/
│   │   │   │   │   ├── TransactionRepository.java  # JPA queries for transactions
│   │   │   │   │   └── UserRepository.java         # JPA queries for users
│   │   │   │   │
│   │   │   │   ├── security/
│   │   │   │   │   ├── JwtAuthFilter.java          # Per-request JWT validation filter
│   │   │   │   │   ├── JwtUtil.java                # Token generation, parsing, validation
│   │   │   │   │   └── SecurityConfig.java         # Route permissions, CORS, session policy
│   │   │   │   │
│   │   │   │   ├── service/
│   │   │   │   │   ├── TransactionService.java     # Business logic for transactions
│   │   │   │   │   └── UserService.java            # Registration, login, BCrypt hashing
│   │   │   │   │
│   │   │   │   └── SableApplication.java           # Spring Boot entry point
│   │   │   │
│   │   │   └── resources/
│   │   │       └── application.properties          # DB, JPA, JWT, and server config
│   │   │
│   │   └── test/
│   │       ├── java/
│   │       └── resources/
│   │
│   ├── target/
│   ├── mvnw
│   ├── mvnw.cmd
│   └── pom.xml                                     # Maven dependencies
│
├── frontend/
│   ├── src/
│   ├── index.html                                  # Main UI entry point
│   ├── script.js                                   # API calls and DOM logic
│   └── style.css                                   # Styling
│
├── .gitattributes
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

|   Tool   |         Version        |
|----------|------------------------|
| Java JDK | 17 or higher           |
| Maven    | 3.8+                   |
| MySQL    | 8.0+                   |
| Ganache  | Latest (Truffle Suite) |

---

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/BLAST.git
cd BLAST
```

### 2. Set Up MySQL

```sql
CREATE DATABASE sable_db;
```

> Hibernate auto-creates the tables on first run via `ddl-auto=update`.

### 3. Configure `application.properties`

Open `backend/src/main/resources/application.properties` and update:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/blast_db
spring.datasource.username=YOUR_MYSQL_USERNAME
spring.datasource.password=YOUR_MYSQL_PASSWORD

jwt.secret=YOUR_SECRET_KEY_MINIMUM_32_CHARACTERS
jwt.expiration-ms=86400000
```

### 4. Start Ganache

- Open the Ganache desktop app
- Start a new workspace on **port 7545**
- Copy a private key from one of the test accounts
- Paste it into `BlockchainService.java` as `SENDER_PRIVATE_KEY`

> ⚠️ For production, move the private key to an environment variable — never commit it to version control.

### 5. Run the Backend

```bash
cd backend
./mvnw spring-boot:run
```

The backend starts at: `http://localhost:8081`

### 6. Open the Frontend

Open `frontend/index.html` directly in your browser, or serve it locally:

```bash
cd frontend
npx serve .
```

---

## 📡 API Reference

### Authentication — Public (no token required)

| Method |       Endpoint       |       Description       |
|--------|----------------------|-------------------------|
| `POST` | `/api/auth/register` | Register a new user     |
| `POST` | `/api/auth/login`    | Login and receive a JWT |

**Register — Request Body:**
```json
{
  "username": "alice",
  "email": "alice@bank.com",
  "password": "secret123",
  "role": "AUDITOR"
}
```

**Login — Request Body:**
```json
{
  "usernameOrEmail": "alice",
  "password": "secret123"
}
```

**Auth Response (both endpoints):**
```json
{
  "token": "eyJhbGci...",
  "username": "alice",
  "email": "alice@bank.com",
  "role": "AUDITOR",
  "message": "Login successful"
}
```

---

### Transactions — Protected

> Include `Authorization: Bearer <token>` in the request header.

| Method | Endpoint | Role Required | Description |
|---|---|---|---|
| `GET` | `/api/transactions/test` | Public | Health check |
| `POST` | `/api/transactions` | ADMIN | Create a new transaction |
| `GET` | `/api/transactions` | ADMIN, AUDITOR | Get all transactions |
| `GET` | `/api/transactions/{transactionId}` | ADMIN, AUDITOR | Get by transaction ID |

**Create Transaction — Request Body:**
```json
{
  "transactionId": "TXN-001",
  "sender": "0xSenderAddress",
  "receiver": "0xReceiverAddress",
  "amount": 1.5
}
```

---

### Blockchain — Protected

| Method |              Endpoint              | Role Required |               Description               |
|------- |------------------------------------|---------------|-----------------------------------------|
| `GET`  | `/blockchain/send/{transactionId}` | ADMIN         | Send a single transaction on-chain      |
| `GET`  | `/blockchain/sync`                 | ADMIN         | Sync all unsynced transactions to chain |

---

### Using the Token

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

---

## 🔐 Roles & Permissions

|           Action          | ADMIN | AUDITOR |
|---------------------------|:-----:|:-------:|
| Register / Login          |  ✅  |    ✅   |
| View transactions         |  ✅  |    ✅   |
| Create transaction        |  ✅  |    ❌   |
| Sync to blockchain        |  ✅  |    ❌   |
| Send transaction on-chain |  ✅  |    ❌   |
| View blockchain data      |  ✅  |    ✅   |

---

## ⛓ Blockchain Design

### Block Structure

```
Block {
  index
  timestamp
  transactions[]
  previous_hash
  current_hash  =  SHA-256(index + timestamp + transactions + previous_hash)
}
```

### Why It Works

- Every block's hash is derived from its own data **plus** the previous block's hash
- Any modification to a past transaction changes its block hash
- That mismatch cascades — every subsequent block becomes invalid
- Chain verification starts from the genesis block and re-checks every hash in sequence

### On-Chain Sync Flow

```
Transaction created in MySQL  (onChain = false)
             │
             ▼
  ADMIN triggers /blockchain/send/{id}
  or /blockchain/sync (all unsynced)
             │
             ▼
  Web3j sends ETH transfer to Ganache
             │
             ▼
  onChain = true  (saved back to MySQL)
             │
             ▼
  Transaction hash returned to caller
```

---

## 👥 Team

|       Name       |           Role          |
|------------------|-------------------------|
| *Nudaa Deshmukh* |  *Backend Development*  |
| *Koyal Kembhavi* |    *Blockchain Core*    |
| *Riya Kumbhoje*  | *Frontend Development*  |

---

> Built with ☕ Java · 🔐 Spring Security · ⛓ Blockchain · 🗄 MySQL

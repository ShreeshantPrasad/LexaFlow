# 🚀 LexaFlow — AI-Powered Legal & Compliance Automation Platform

<div align="center">

![Java](https://img.shields.io/badge/Java-17-orange?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-green?style=for-the-badge&logo=springboot)
![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Firebase](https://img.shields.io/badge/Database-Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Microservices](https://img.shields.io/badge/Architecture-Microservices-blue?style=for-the-badge)
![Kafka](https://img.shields.io/badge/Event_Driven-Kafka-black?style=for-the-badge&logo=apachekafka)
![Docker](https://img.shields.io/badge/Deployment-Docker-2496ED?style=for-the-badge&logo=docker)
![Python](https://img.shields.io/badge/AI_Module-Python-yellow?style=for-the-badge&logo=python)

**Enterprise-grade Legal Automation for SMEs, Startups & Freelancers**

[Features](#-key-features) • [Architecture](#-architecture--design) • [Tech Stack](#%EF%B8%8F-tech-stack) • [Setup](#-installation--setup) • [Roadmap](#-roadmap)

</div>

---

## 📖 Table of Contents

1. [About the Project](#-about-the-project)
2. [Problem Statement & Solution](#-problem-statement--solution)
3. [Architecture & Design](#-architecture--design)
4. [Firebase Architecture](#-firebase-architecture)
5. [System Workflow](#-system-workflow)
6. [Key Features](#-key-features)
7. [Microservices Breakdown](#-microservices-breakdown)
8. [Tech Stack](#%EF%B8%8F-tech-stack)
9. [Installation & Setup](#-installation--setup)
10. [API Endpoints](#-api-endpoints)
11. [Roadmap](#-roadmap)
12. [Motivation & Story](#-motivation--story)
13. [Author](#-author)

---

## 💡 About the Project

**LexaFlow** is a **full-stack**, enterprise-grade platform that automates **legal compliance, contract lifecycle management, and GST invoicing** — all in one unified system.

Built for **SMEs, Startups, and Freelancers** who cannot afford dedicated legal teams, LexaFlow brings AI into the legal workflow. It uses **Natural Language Processing (NLP)** to detect risky contract clauses, **digital signatures** for document authentication, and **automated GST invoicing** to ensure end-to-end compliance without manual intervention.

The **frontend** is built with **React 18 + TypeScript**, providing a fully type-safe, component-driven UI with real-time feedback on contract risk analysis and invoice status. The **backend** is powered by **Spring Boot Microservices** with an **Event-Driven Architecture (EDA)** via **Apache Kafka**. All data is stored and synced in real-time using **Firebase (Firestore + Firebase Storage + Firebase Auth)**, enabling seamless cloud-native persistence without managing traditional database servers.

> 💼 *This project demonstrates full-stack engineering: React/TypeScript frontend, distributed microservices backend, Firebase real-time cloud database, event-driven communication, AI integration, and secure authentication.*

---

## 🎯 Problem Statement & Solution

### 🚩 The Problem

| Pain Point | Impact |
|---|---|
| **High Legal Costs** | SMEs can't afford lawyers for every contract review |
| **Manual Errors** | Human drafting misses critical clauses or compliance risks |
| **Compliance Gaps** | Tracking GST, GDPR/DPDP, and audit trails manually is chaotic |
| **Slow Turnaround** | Draft → Review → Sign → Invoice cycle takes days or weeks |
| **Rights Unawareness** | ~90% of Indian citizens are unaware of their fundamental rights |

### ✅ The LexaFlow Solution

| Feature | How It Helps |
|---|---|
| **AI Risk Analysis** | Instantly scans contracts for risky/missing clauses using NLP |
| **End-to-End Automation** | Contract creation → Signing → Invoice → Archiving, all automated |
| **Firebase Real-time Sync** | All contract changes and status updates reflect instantly across all users |
| **Immutable Security** | SHA-256 hashing + Firestore Audit Logs ensure tamper-proof records |
| **Microservices Design** | Services run independently — one failure doesn't bring down the system |
| **Rights Chatbot** | Multilingual chatbot explains constitutional rights in plain language |

---

## 🏗 Architecture & Design

LexaFlow follows a **Distributed Microservices Architecture**. All services are fully decoupled — they communicate **asynchronously via Kafka** for events and **synchronously via REST APIs** through a central **API Gateway**. **Firebase** acts as the cloud-native persistence layer for all microservices.

```
┌─────────────────────────────────────────────────────────────────────────┐
│               FRONTEND  —  React 18 + TypeScript  (Port: 3000)          │
│        Vite · React Router · Axios · Tailwind CSS · React Query         │
│                 Firebase SDK (Auth + Firestore real-time)               │
└───────────────────────────────┬─────────────────────────────────────────┘
                                │ HTTPS / REST
                                ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     API GATEWAY  (Port: 8080)                           │
│              Routing · Rate Limiting · JWT Validation                   │
└───────┬───────────────┬───────────────┬───────────────┬─────────────────┘
        │               │               │               │
        ▼               ▼               ▼               ▼
  ┌──────────┐   ┌──────────────┐  ┌─────────┐  ┌────────────┐
  │   Auth   │   │   Contract   │  │ Invoice │  │ Compliance │
  │ Service  │   │   Service    │  │ Service │  │  Service   │
  │ :8081    │   │   :8082      │  │  :8083  │  │   :8084    │
  └────┬─────┘   └──────┬───────┘  └────┬────┘  └─────┬──────┘
       │                │  REST          │             │
       │                ▼               │             │
       │         ┌─────────────┐        │             │
       │         │  AI Service │        │             │
       │         │ (Python)    │        │             │
       │         │   :5000     │        │             │
       │         └─────────────┘        │             │
       │                                │             │
       └────────────────┬───────────────┴─────────────┘
                        │  READ / WRITE (Firebase Admin SDK)
                        ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        FIREBASE  (Cloud Database Layer)                 │
│    Firestore (NoSQL) · Firebase Storage · Firebase Auth · Realtime DB   │
└─────────────────────────────────────────────────────────────────────────┘

              ┌──────────── EVENT PUBLISHING ────────────┐
              ▼                                          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                       APACHE KAFKA  (Event Bus)                         │
│         Topics: contract.signed · invoice.generated · audit.log         │
└────────────────┬────────────────────────────┬───────────────────────────┘
                 │                            │
                 ▼                            ▼
         ┌──────────────┐            ┌──────────────┐
         │ Notification │            │    Audit     │
         │   Service    │            │   Service    │
         │   :8086      │            │   :8085      │
         └──────────────┘            └──────────────┘
```

**Service Discovery** is handled by **Eureka Server (Port: 8761)**, which maintains a live registry of all running microservices.

---

## 🔥 Firebase Architecture

LexaFlow uses **Firebase** as its primary cloud-native database layer — replacing traditional SQL databases entirely. This means **no local database servers**, no migration scripts, and real-time sync out of the box.

### Firebase Services Used

| Firebase Service | Used By | Purpose |
|---|---|---|
| **Firestore (NoSQL)** | All Services | Stores users, contracts, invoices, audit logs as documents/collections |
| **Firebase Storage** | Contract + Invoice Service | Stores uploaded contract PDFs, signed documents, and invoice PDFs |
| **Firebase Auth** | Auth Service + Frontend | Handles user registration, login, and Google OAuth Sign-In |
| **Realtime Database** | Frontend | Powers live contract status updates and notification counters |

### Firestore Collection Structure

```
firestore/
│
├── users/
│   └── {userId}/
│         ├── name, email, role (ADMIN / VENDOR / CLIENT)
│         └── createdAt, lastLoginAt
│
├── contracts/
│   └── {contractId}/
│         ├── title, status (DRAFT / SIGNED / EXPIRED)
│         ├── sha256Hash, riskScore, riskDetails[]
│         ├── storageUrl (Firebase Storage link)
│         ├── parties[], signedAt, expiresAt
│         └── versions/   ← sub-collection
│               └── {versionId}/ → Draft_v1, Draft_v2, Final
│
├── invoices/
│   └── {invoiceId}/
│         ├── contractId, amount, cgst, sgst, igst
│         ├── pdfStorageUrl (Firebase Storage link)
│         └── generatedAt, status
│
├── auditLogs/
│   └── {logId}/
│         ├── userId, action, ipAddress
│         ├── timestamp, checksum
│         └── serviceOrigin (e.g., "contract-service")
│
└── chatHistory/
    └── {sessionId}/
          ├── userId, language
          └── messages[] → { role, content, timestamp }
```

### Why Firebase over Traditional SQL?

| | Traditional SQL (PostgreSQL) | Firebase (Firestore) |
|---|---|---|
| **Setup** | Requires local/cloud DB server | Zero setup — fully managed |
| **Real-time** | Needs WebSockets/polling | Built-in real-time listeners |
| **Scaling** | Manual horizontal scaling | Auto-scales with Google infra |
| **Auth** | Custom implementation | Firebase Auth out of the box |
| **File Storage** | Separate S3/MinIO needed | Firebase Storage built-in |

---

## 🔄 System Workflow

Here is the step-by-step lifecycle of a contract inside LexaFlow:

```
Step 1 ─── User Login
           └─► Firebase Auth verifies credentials / Google OAuth
           └─► Auth Service issues a JWT Token (Role: Admin / Vendor / Client)
           └─► User document synced in Firestore users/ collection

Step 2 ─── Contract Upload
           └─► Contract PDF uploaded to Firebase Storage
           └─► Download URL + metadata saved in Firestore contracts/ collection
           └─► SHA-256 hash generated and stored for tamper detection

Step 3 ─── AI Risk Analysis
           └─► Contract text extracted and sent to AI Service (Python/spaCy)
           └─► NLP engine scans for risky clauses
           └─► Risk Score (0–100) written back to Firestore contract document
           └─► Frontend updates in real-time via Firestore listener (no refresh needed)

Step 4 ─── Review & Approval
           └─► User reviews flagged clauses on the React dashboard
           └─► Contract version locked (Draft → Final) updated in Firestore

Step 5 ─── Digital Signature
           └─► All parties sign digitally
           └─► Signature metadata + timestamp saved to Firestore

Step 6 ─── Event Published to Kafka
           └─► Topic: `contract.signed`
           └─► Payload: contractId, parties, timestamp

Step 7 ─── Invoice Auto-Generated
           └─► Invoice Service consumes the Kafka event
           └─► GST (CGST/SGST/IGST) auto-calculated by client location
           └─► PDF invoice generated, uploaded to Firebase Storage
           └─► Invoice document created in Firestore invoices/ collection

Step 8 ─── Audit Logging
           └─► Audit Service writes immutable log to Firestore auditLogs/ collection
           └─► Every action recorded: User ID, IP, Timestamp, Checksum
```

---

## 🌟 Key Features

### 🔐 1. Secure Authentication & RBAC
- **Firebase Auth** handles registration, login, and Google OAuth Sign-In out of the box
- JWT tokens issued by Auth Service for secure inter-service communication
- Role-Based Access Control: `ADMIN`, `VENDOR`, `CLIENT` stored in Firestore user documents
- Password encryption using **BCrypt** server-side; Firebase handles OAuth token flows

### 📄 2. Smart Contract Management
- Contract PDFs stored securely in **Firebase Storage**; metadata lives in **Firestore**
- Full version control (Draft v1, v2, Final) stored as Firestore sub-collections
- **Tamper-proof** via SHA-256 cryptographic hashing stored in the Firestore document
- Real-time contract status updates on the frontend via **Firestore snapshot listeners**
- Contract expiry tracking managed via Firestore document fields + scheduled Kafka events

### 🤖 3. AI-Powered Risk Analysis
- **Clause Detection:** Identifies missing standard clauses (Confidentiality, Indemnity, Termination)
- **Risk Scoring:** Assigns a score from 0–100 based on clause severity
- **Keyword Highlighting:** Flags high-risk phrases (e.g., *"Termination without notice"*, *"Unlimited liability"*)
- **Summary Generation:** Produces a concise summary of lengthy legal documents
- Risk score is written back to **Firestore** and reflects on the UI in real-time — no polling needed

### 💰 4. Automated GST Invoicing
- Auto-calculates **CGST, SGST, IGST** based on supplier and client location
- Generates professional **PDF invoices** uploaded directly to **Firebase Storage**
- Invoice records stored in **Firestore** for permanent accounting and audit history
- Shareable download links generated via Firebase Storage signed URLs

### 🛡 5. Audit & Compliance
- **Immutable Audit Logs** written to a security-rules-locked **Firestore** collection (no client delete allowed)
- **Data Privacy Compliance:** GDPR / India's DPDP Act — user data deletable from Firestore on Right to be Forgotten requests
- All logs include: User ID, IP Address, Action, Timestamp, SHA-256 Checksum

### 🖥 6. React + TypeScript Frontend
- **Type-safe UI** built with React 18 and TypeScript — zero runtime type surprises
- **React Router v6** for client-side navigation with protected routes (role-based)
- **React Query** for server state management — auto caching and background refetching
- **Firebase SDK** integrated directly for real-time Firestore listeners in the UI
- **Real-time Risk Dashboard** — contract status and risk score update live without page refresh
- **Tailwind CSS** for responsive, mobile-first design

### 🌐 7. Multilingual Fundamental Rights Chatbot

> 💡 *In India, approximately 90% of citizens are unaware of their fundamental rights — making them vulnerable to exploitation and injustice. This feature directly addresses that gap.*

- **Multilingual Support:** Responds in Hindi, English, Tamil, Bengali, Marathi, and more — language is never a barrier for justice
- **Rights Q&A Engine:** Powered by an NLP/LLM model trained on the Indian Constitution, legal provisions, and landmark case summaries
- **Plain Language Answers:** Converts dense legal jargon into simple, clear explanations anyone can understand
- **Voice Recognition Input:** Speech-to-text support so even low-literacy users can ask questions verbally
- **Chat History** stored per session in Firestore `chatHistory/` collection for continuity
- **Example queries a user can ask:**
  - *"Mujhe bina wajah arrest kiya gaya — kya yeh galat hai?"*
  - *"What is my right to education?"*
  - *"Can police enter my house without a warrant?"*
  - *"What is Article 21?"*

---

## 🔧 Microservices Breakdown

| Service | Port | Responsibility | Key Technologies |
|---|---|---|---|
| **Eureka Discovery** | `8761` | Service Registry — tracks all running instances | Spring Cloud Netflix Eureka |
| **API Gateway** | `8080` | Single entry point, routing, rate limiting, JWT validation | Spring Cloud Gateway |
| **Auth Service** | `8081` | Registration, login, JWT generation, Firebase Auth integration, RBAC | Spring Security, Firebase Admin SDK |
| **Contract Service** | `8082` | Contract CRUD, versioning, SHA-256 hashing, Firebase Storage upload | Spring Boot, Firebase Admin SDK |
| **Invoice Service** | `8083` | Invoice generation, GST calculation, PDF export to Firebase Storage | Spring Boot, Apache PDFBox, Firebase Admin SDK |
| **Compliance Service** | `8084` | GDPR/DPDP policy management, Firestore data deletion requests | Spring Boot, Firebase Admin SDK |
| **AI Analysis Service** | `5000` | NLP risk scanning, clause detection, risk scoring | Python, Flask, spaCy/NLTK |
| **Rights Chatbot Service** | `5001` | Multilingual fundamental rights Q&A, voice recognition | Python, Flask, LLM, SpeechRecognition |
| **Notification Service** | `8086` | Email/SMS alerts (Kafka consumer) | Spring Boot, JavaMail |
| **Audit Service** | `8085` | Immutable event logging to Firestore (Kafka consumer) | Spring Boot, Kafka, Firebase Admin SDK |

---

## ⚙️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, TypeScript 5.x, Vite, Tailwind CSS, React Router v6, React Query, Axios, Firebase SDK |
| **Language** | Java 17, Python 3.10 |
| **Framework** | Spring Boot 3.x, Flask |
| **Architecture** | Microservices, Event-Driven Architecture (EDA) |
| **Service Discovery** | Spring Cloud Netflix Eureka |
| **API Gateway** | Spring Cloud Gateway |
| **Messaging** | Apache Kafka, Apache Zookeeper |
| **Authentication** | Firebase Auth (Email/Password + Google OAuth), Spring Security, JWT |
| **Database** | Firebase Firestore (NoSQL, primary data store) |
| **File Storage** | Firebase Storage (contract PDFs, invoice PDFs, signed documents) |
| **Realtime Sync** | Firebase Realtime Database (live UI updates) |
| **AI / NLP** | Python, spaCy, NLTK |
| **PDF Generation** | Apache PDFBox |
| **Containerization** | Docker, Docker Compose |
| **Build Tool** | Maven (Backend), Vite (Frontend) |

---

## 🚀 Installation & Setup

### Prerequisites

Make sure the following are installed on your machine:

- **Java 17+** — [Download](https://adoptium.net/)
- **Node.js 18+** — [Download](https://nodejs.org/) (for React frontend)
- **Docker Desktop** — [Download](https://www.docker.com/products/docker-desktop/)
- **Maven 3.8+** — [Download](https://maven.apache.org/)
- **Python 3.10+** — [Download](https://www.python.org/)
- **Firebase Project** — [Create one free at Firebase Console](https://console.firebase.google.com/)

---

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/lexaflow.git
cd lexaflow
```

### Step 2: Configure Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/) → **Create Project** → name it `LexaFlow`
2. Enable the following in your project:
   - **Firestore Database** (start in production mode)
   - **Firebase Storage**
   - **Firebase Authentication** → Enable **Email/Password** and **Google** providers
3. Go to **Project Settings → Service Accounts** → Generate a new private key → download `serviceAccountKey.json`
4. Place the key in each backend service's resources folder:

```bash
cp serviceAccountKey.json auth-service/src/main/resources/
cp serviceAccountKey.json contract-service/src/main/resources/
cp serviceAccountKey.json invoice-service/src/main/resources/
cp serviceAccountKey.json audit-service/src/main/resources/
```

5. Go to **Project Settings → General → Your Apps** → copy your Firebase web config and set up the frontend `.env`:

```bash
cd frontend
cp .env.example .env
```

Fill in `frontend/.env`:

```env
VITE_API_BASE_URL=http://localhost:8080
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

### Step 3: Build All Java Services

```bash
mvn clean install -DskipTests
```

### Step 4: Set Up the AI & Chatbot Services

```bash
cd ai-service && pip install -r requirements.txt && cd ..
cd chatbot-service && pip install -r requirements.txt && cd ..
```

### Step 5: Install Frontend Dependencies

```bash
cd frontend && npm install
```

### Step 6: Start Everything with Docker Compose

This command starts all microservices, Kafka, and Zookeeper. Firebase is **fully cloud-hosted — no local database container required!**

```bash
docker-compose up --build -d
```

### Step 7: Verify Services Are Running

Open Eureka Dashboard to confirm all services are registered:

```
http://localhost:8761
```

---

### 📌 Access Points

| Service | URL |
|---|---|
| **Frontend (React App)** | `http://localhost:3000` |
| **API Gateway** | `http://localhost:8080` |
| **Eureka Dashboard** | `http://localhost:8761` |
| **Swagger / API Docs** | `http://localhost:8080/swagger-ui.html` |
| **AI Service** | `http://localhost:5000` |
| **Rights Chatbot** | `http://localhost:5001` |
| **Firebase Console** | `https://console.firebase.google.com/` |

---

## 📡 API Endpoints

### Auth Service
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register a new user (saved to Firestore) |
| `POST` | `/api/auth/login` | Login and get JWT token via Firebase Auth |
| `POST` | `/api/auth/refresh` | Refresh expired JWT |

### Contract Service
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/contracts` | Create a new contract (Firestore + Firebase Storage) |
| `GET` | `/api/contracts/{id}` | Get contract by ID from Firestore |
| `PUT` | `/api/contracts/{id}` | Update contract metadata in Firestore |
| `POST` | `/api/contracts/{id}/sign` | Digitally sign a contract |
| `GET` | `/api/contracts/{id}/risk` | Get AI risk analysis report |

### Invoice Service
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/invoices/{id}` | Get invoice details from Firestore |
| `GET` | `/api/invoices/{id}/pdf` | Download PDF via Firebase Storage signed URL |

### Rights Chatbot Service
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/chat/ask` | Ask a question about fundamental rights |
| `GET` | `/api/chat/history/{sessionId}` | Get chat session history from Firestore |

---

## 🗺 Roadmap

- [x] Core microservices with Eureka & API Gateway
- [x] Firebase Auth integration with JWT generation
- [x] Contract Management with Firestore + Firebase Storage
- [x] SHA-256 tamper-proof hashing stored in Firestore
- [x] AI Risk Analysis via Python/NLP, results written to Firestore
- [x] Kafka event-driven Invoice generation, invoices saved to Firestore
- [x] Immutable Audit Logging in Firestore (security-rule-locked)
- [x] React + TypeScript Frontend with real-time Firestore listeners
- [ ] **Multilingual Fundamental Rights Chatbot** — Help the 90% of Indian citizens unaware of their rights
- [ ] **Speech Recognition Model** — Regional language voice-to-text for low-literacy users
- [ ] **Firebase Cloud Functions** — Serverless triggers for automated contract expiry alerts
- [ ] **Blockchain Verification** — Store contract hashes on Ethereum/Hyperledger
- [ ] **Aadhaar eSign Integration** — Government ID verification for Indian users
- [ ] **PWA / Mobile Support** — Progressive Web App for mobile users

---

## 💪 Motivation & Story

> *"Exam mein fail hua — but failure is just the 1st step towards success. I learnt to be consistent. I learnt perseverance."*

This project wasn't built in one go. It came from failure, from restarting, and from refusing to quit. The idea for the **Fundamental Rights Chatbot** came from a simple but powerful observation — in a country of 1.4 billion people, the majority don't know the rights the Constitution guarantees them. They face injustice not out of helplessness, but out of unawareness.

LexaFlow is an attempt to make **legal knowledge accessible to everyone** — not just corporations or people who can afford lawyers.

---

## 👨‍💻 Author

**Shreeshant Prasad**
*Final Year B.Tech — Computer Science*
*Full-Stack Developer | Java, Spring Boot, Firebase & React + TypeScript*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/yourprofile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/yourusername)

---

## 📜 License

This project is developed for **academic and demonstration purposes**.

---

<div align="center">
  <sub>Built with ☕ Java · ⚛️ React + TypeScript · 🔥 Firebase · 🐍 Python & ❤️ by Shreeshant Prasad</sub>
</div>

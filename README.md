# 🚀 LexaFlow — AI-Powered Legal & Compliance Automation Platform

<div align="center">

![Java](https://img.shields.io/badge/Java-17-orange?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-green?style=for-the-badge&logo=springboot)
![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Microservices](https://img.shields.io/badge/Architecture-Microservices-blue?style=for-the-badge)
![Kafka](https://img.shields.io/badge/Event_Driven-Kafka-black?style=for-the-badge&logo=apachekafka)
![Docker](https://img.shields.io/badge/Deployment-Docker-2496ED?style=for-the-badge&logo=docker)
![Python](https://img.shields.io/badge/AI_Module-Python-yellow?style=for-the-badge&logo=python)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-316192?style=for-the-badge&logo=postgresql)

**Enterprise-grade Legal Automation for SMEs, Startups & Freelancers**

[Features](#-key-features) • [Architecture](#-architecture--design) • [Tech Stack](#%EF%B8%8F-tech-stack) • [Setup](#-installation--setup) • [Roadmap](#-roadmap)

</div>

---

## 📖 Table of Contents

1. [About the Project](#-about-the-project)
2. [Problem Statement & Solution](#-problem-statement--solution)
3. [Architecture & Design](#-architecture--design)
4. [System Workflow](#-system-workflow)
5. [Key Features](#-key-features)
6. [Microservices Breakdown](#-microservices-breakdown)
7. [Tech Stack](#%EF%B8%8F-tech-stack)
8. [Installation & Setup](#-installation--setup)
9. [API Endpoints](#-api-endpoints)
10. [Roadmap](#-roadmap)
11. [Author](#-author)

---

## 💡 About the Project

**LexaFlow** is a **full-stack**, enterprise-grade platform that automates **legal compliance, contract lifecycle management, and GST invoicing** — all in one unified system.

Built for **SMEs, Startups, and Freelancers** who cannot afford dedicated legal teams, LexaFlow brings AI into the legal workflow. It uses **Natural Language Processing (NLP)** to detect risky contract clauses, **digital signatures** for document authentication, and **automated GST invoicing** to ensure end-to-end compliance without manual intervention.

The **frontend** is built with **React 18 + TypeScript**, providing a fully type-safe, component-driven UI with real-time feedback on contract risk analysis and invoice status. The **backend** is powered by **Spring Boot Microservices** with an **Event-Driven Architecture (EDA)** via **Apache Kafka**, ensuring high scalability, fault tolerance, and real-time processing.

> 💼 *This project demonstrates full-stack engineering: React/TypeScript frontend, distributed microservices backend, event-driven communication, AI integration, and secure authentication.*

---

## 🎯 Problem Statement & Solution

### 🚩 The Problem

| Pain Point | Impact |
|---|---|
| **High Legal Costs** | SMEs can't afford lawyers for every contract review |
| **Manual Errors** | Human drafting misses critical clauses or compliance risks |
| **Compliance Gaps** | Tracking GST, GDPR/DPDP, and audit trails manually is chaotic |
| **Slow Turnaround** | Draft → Review → Sign → Invoice cycle takes days or weeks |

### ✅ The LexaFlow Solution

| Feature | How It Helps |
|---|---|
| **AI Risk Analysis** | Instantly scans contracts for risky/missing clauses using NLP |
| **End-to-End Automation** | Contract creation → Signing → Invoice → Archiving, all automated |
| **Immutable Security** | SHA-256 hashing + Audit Logs ensure tamper-proof records |
| **Microservices Design** | Services run independently — one failure doesn't bring down the system |

---

## 🏗 Architecture & Design

LexaFlow follows a **Distributed Microservices Architecture**. All services are fully decoupled — they communicate **asynchronously via Kafka** for events and **synchronously via REST APIs** through a central **API Gateway**.

```
┌─────────────────────────────────────────────────────────────────────────┐
│               FRONTEND  —  React 18 + TypeScript  (Port: 3000)          │
│        Vite · React Router · Axios · Tailwind CSS · React Query         │
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
  └──────────┘   └──────┬───────┘  └────┬────┘  └────────────┘
                         │  REST         │
                         ▼              │
                  ┌─────────────┐       │
                  │  AI Service │       │
                  │ (Python)    │       │
                  │   :5000     │       │
                  └─────────────┘       │
                                        │
              ┌─────────────────────────┘
              │    EVENT PUBLISHING
              ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                       APACHE KAFKA  (Event Bus)                         │
│         Topics: contract.signed · invoice.generated · audit.log         │
└────────────────┬────────────────────────────┬───────────────────────────┘
                 │                            │
                 ▼                            ▼
         ┌──────────────┐            ┌──────────────┐
         │ Notification │            │    Audit     │
         │   Service    │            │   Service    │
         └──────────────┘            └──────────────┘
```

**Service Discovery** is handled by **Eureka Server (Port: 8761)**, which maintains a live registry of all running microservices.

---

## 🔄 System Workflow

Here is the step-by-step lifecycle of a contract inside LexaFlow:

```
Step 1 ─── User Login
           └─► Auth Service issues a JWT Token (Role: Admin / Vendor / Client)

Step 2 ─── Contract Upload
           └─► Contract Service stores the document
           └─► SHA-256 hash is generated for tamper detection

Step 3 ─── AI Risk Analysis
           └─► Contract text is sent to the AI Service (Python/spaCy)
           └─► NLP engine scans for risky clauses
           └─► Risk Score (0–100) is returned with highlighted clauses

Step 4 ─── Review & Approval
           └─► User reviews flagged clauses
           └─► Contract version is locked (Draft → Final)

Step 5 ─── Digital Signature
           └─► All parties sign digitally
           └─► Signature metadata is stored with timestamp

Step 6 ─── Event Published to Kafka
           └─► Topic: `contract.signed`
           └─► Payload: contract ID, parties, timestamp

Step 7 ─── Invoice Auto-Generated
           └─► Invoice Service consumes the Kafka event
           └─► GST (CGST/SGST/IGST) auto-calculated by client location
           └─► PDF invoice generated and stored

Step 8 ─── Audit Logging
           └─► Audit Service logs every action: User ID, IP, Timestamp, Checksum
           └─► Immutable log stored for legal proof
```

---

## 🌟 Key Features

### 🔐 1. Secure Authentication & RBAC
- JWT-based stateless authentication
- Role-Based Access Control: `ADMIN`, `VENDOR`, `CLIENT`
- Password encryption using **BCrypt**
- Token expiry and refresh mechanism

### 📄 2. Smart Contract Management
- Full **CRUD** operations with version control (Draft v1, v2, Final)
- **Tamper-proof storage** via SHA-256 cryptographic hashing
- Contract expiry alerts and renewal tracking
- Supports multiple contract types (Service, NDA, Vendor agreements)

### 🤖 3. AI-Powered Risk Analysis
- **Clause Detection:** Identifies missing standard clauses (Confidentiality, Indemnity, Termination)
- **Risk Scoring:** Assigns a score from 0–100 based on clause severity
- **Keyword Highlighting:** Flags high-risk phrases (e.g., *"Termination without notice"*, *"Unlimited liability"*)
- **Summary Generation:** Produces a concise summary of lengthy legal documents

### 💰 4. Automated GST Invoicing
- Auto-calculates **CGST, SGST, IGST** based on supplier and client location
- Generates professional **PDF invoices** instantly on contract signing
- Stores invoice history for accounting and audit purposes

### 🖥 6. React + TypeScript Frontend
- **Type-safe UI** built with React 18 and TypeScript — zero runtime type surprises
- **React Router v6** for client-side navigation with protected routes (role-based)
- **React Query** for server state management — auto caching, background refetching
- **Axios interceptors** for attaching JWT tokens to every API request automatically
- **Real-time Risk Dashboard** — visualizes AI risk score and flagged clauses on upload
- **Tailwind CSS** for responsive, mobile-first design
- **Immutable Audit Logs:** Every API action is logged with a checksum to prevent tampering
- **Data Privacy Compliance:** Modules for GDPR / India's DPDP Act (Right to be Forgotten, Data Export)
- All logs include: User ID, IP Address, Action, Timestamp

---

## 🔧 Microservices Breakdown

| Service | Port | Responsibility | Key Technologies |
|---|---|---|---|
| **Eureka Discovery** | `8761` | Service Registry — tracks all running instances | Spring Cloud Netflix Eureka |
| **API Gateway** | `8080` | Single entry point, routing, rate limiting, JWT validation | Spring Cloud Gateway |
| **Auth Service** | `8081` | Registration, login, JWT generation, RBAC | Spring Security, JJWT, BCrypt |
| **Contract Service** | `8082` | Contract CRUD, versioning, SHA-256 hashing, workflow | Spring Boot, PostgreSQL |
| **Invoice Service** | `8083` | Invoice generation, GST calculation, PDF export | Spring Boot, Apache PDFBox |
| **Compliance Service** | `8084` | GDPR/DPDP policy management, data export requests | Spring Boot, PostgreSQL |
| **AI Analysis Service** | `5000` | NLP risk scanning, clause detection, risk scoring | Python, Flask, spaCy/NLTK |
| **Notification Service** | `8086` | Email/SMS alerts (Kafka consumer) | Spring Boot, JavaMail |
| **Audit Service** | `8085` | Immutable event logging (Kafka consumer) | Spring Boot, Kafka, PostgreSQL |

---

## ⚙️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, TypeScript 5.x, Vite, Tailwind CSS, React Router v6, React Query, Axios |
| **Language** | Java 17, Python 3.10 |
| **Framework** | Spring Boot 3.x, Flask |
| **Architecture** | Microservices, Event-Driven Architecture (EDA) |
| **Service Discovery** | Spring Cloud Netflix Eureka |
| **API Gateway** | Spring Cloud Gateway |
| **Messaging** | Apache Kafka, Apache Zookeeper |
| **Security** | Spring Security, JWT, BCrypt |
| **Database** | PostgreSQL (primary), Redis (caching) |
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
- **Python 3.10+** (for AI service) — [Download](https://www.python.org/)

---

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/lexaflow.git
cd lexaflow
```

### Step 2: Build All Java Services

```bash
mvn clean install -DskipTests
```

### Step 3: Set Up the AI Service

```bash
cd ai-service
pip install -r requirements.txt
```

### Step 4: Set Up the Frontend

```bash
cd frontend
npm install
```

Create a `.env` file inside the `frontend/` folder:

```env
VITE_API_BASE_URL=http://localhost:8080
```

### Step 5: Start Everything with Docker Compose

This single command starts all microservices, PostgreSQL, Kafka, Zookeeper, and Redis in the correct order.

```bash
docker-compose up --build -d
```

### Step 6: Verify Services Are Running

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

---

## 📡 API Endpoints

### Auth Service
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register a new user |
| `POST` | `/api/auth/login` | Login and get JWT token |
| `POST` | `/api/auth/refresh` | Refresh expired JWT |

### Contract Service
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/contracts` | Create a new contract |
| `GET` | `/api/contracts/{id}` | Get contract by ID |
| `PUT` | `/api/contracts/{id}` | Update contract |
| `POST` | `/api/contracts/{id}/sign` | Digitally sign a contract |
| `GET` | `/api/contracts/{id}/risk` | Get AI risk analysis report |

### Invoice Service
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/invoices/{id}` | Get invoice details |
| `GET` | `/api/invoices/{id}/pdf` | Download PDF invoice |

---

## 🗺 Roadmap

- [x] Core microservices with Eureka & API Gateway
- [x] JWT Authentication with RBAC
- [x] Contract Management with SHA-256 hashing
- [x] AI Risk Analysis via Python/NLP
- [x] Kafka event-driven Invoice generation
- [x] Immutable Audit Logging
- [x] React + TypeScript Frontend with protected routes & Risk Dashboard
- [ ] **Blockchain Verification** — Store contract hashes on Ethereum/Hyperledger
- [ ] **Aadhaar eSign Integration** — Government ID verification for Indian users
- [ ] **LLM Chatbot** — Query contracts in plain English via an AI assistant
- [ ] **PWA / Mobile Support** — Progressive Web App for mobile users

---

## 👨‍💻 Author

**Shreeshant Prasad**
*Final Year B.Tech — Computer Science*
*Full-Stack Developer | Java, Spring Boot & React + TypeScript Enthusiast*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/yourprofile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/yourusername)

---

## 📜 License

This project is developed for **academic and demonstration purposes**.

---

<div align="center">
  <sub>Built with ☕ Java · ⚛️ React + TypeScript · 🐍 Python & ❤️ by Shreeshant Prasad</sub>
</div>

# VerifyAI – AI-Powered Application Validation, Document Authentication & Regulatory Compliance Verification

> **Smart India Hackathon (SIH 2026) Prototype Submission**  
> *Original Product Identity – Automated Institutional Compliance & Document Forensic Analysis Platform.*

---

## 1. Executive Summary & Objective

**VerifyAI** is an advanced, government-grade SaaS platform engineered to streamline and secure institutional approval processing (e.g. AICTE-style approvals, Extension of Approvals (EOA), Intake Increases, and New Institution applications).

The platform replaces manual scrutiny with a robust, hybrid intelligence pipeline:
1. **Automated Document Validation**: Ingests multi-format institutional documents (PDF, PNG, JPG), verifying MIME types, file sizes, and magic byte headers.
2. **Cryptographic Checksum & Integrity Inspection**: Generates immutable SHA-256 hashes, detects duplicate submissions across the repository, validates cryptographic PKCS#7 digital signature dictionaries, and audits PDF metadata for suspicious consumer editors (e.g. Photoshop, Canva, iLovePDF).
3. **AI/OCR Entity Extraction**: Extracts text, tables, and structured entities (Institution Name, Registration ID, Land Area, Built-up Area, Approved Intake, Issue Date) using PyMuPDF and pattern recognition.
4. **Side-by-Side Application Cross-Verification**: Computes string similarity and numeric bounds, highlighting exact field mismatches.
5. **Dynamic Regulatory Compliance Engine**: Evaluates normative benchmarks (e.g., Land Area &ge; 2.5 Acres, Built-up Area &ge; 4.0 sq.m/seat, Student-to-Faculty Ratio &le; 1:20). Administrators can modify or toggle rules in real-time without modifying source code.
6. **Explainable AI Risk Prioritization**: Computes an explainable Review Priority Score (Low, Medium, High) with human-readable rationale to triage cases for authorized scrutiny officers without making definitive legal determinations of fraud.
7. **Human-Controlled Decision Making**: Appointed Verification Officers hold final authority to approve, request specific document corrections, or reject applications with a full digital audit trail.

---

## 2. Technology Stack

- **Frontend**: HTML5, Vanilla CSS3 (Custom Glassmorphism & Cyber-Scanline Design System), JavaScript (ES6+), Tailwind CSS (CDN), Lucide Icons, Chart.js.
- **Backend**: Python 3.11+, FastAPI (REST APIs with CORS, rate limiting, and Pydantic v2 schemas), Uvicorn.
- **Database & ORM**: SQLAlchemy 2.0 with full MySQL 8.0 support (`pymysql`) and seamless zero-configuration local fallback to SQLite (`verifyai.db`).
- **AI & Document Processing**: PyMuPDF (`fitz`), Pillow, Regex Entity Recognition, Cryptography (`hashlib`, `cryptography`, `python-jose`).
- **Security**: Cryptographic PBKDF2-HMAC-SHA256 password hashing, JWT Bearer authentication, server-side Role-Based Access Control (RBAC), IP-based rate limiting, input validation.

---

## 3. Project Folder Structure

```
sih project/
│
├── frontend/                     # Modern Government-Grade UI
│   ├── index.html                # Hero, animated scanner, 6 features, process timeline
│   ├── login.html                # Auth portal with 1-click SIH Demo Persona Switcher
│   ├── institution-dashboard.html# College portal: metrics, status chart, applications
│   ├── application.html          # 4-step wizard: Institution, Details, Uploads, Review
│   ├── officer-dashboard.html    # Officer queue with priority filters & scrutiny actions
│   ├── admin-dashboard.html      # Super Admin console: rule editor, analytics, audit logs
│   ├── verification.html         # VerifyAI Intelligence Center (Explainable AI View)
│   ├── css/
│   │   └── styles.css            # Dark navy theme, glassmorphic cards, scanner laser
│   └── js/
│       ├── api.js                # Unified API client with automatic offline fallback
│       ├── auth.js               # Session management and role-based route protection
│       ├── demo.js               # SIH 2026 Interactive Demo Controller
│       ├── institution.js        # Institution dashboard charts & application tables
│       ├── application.js        # Multi-step wizard, SHA-256 preview, sample injector
│       ├── officer.js            # Verification officer priority triage queue
│       ├── admin.js              # Compliance rule CRUD, analytics charts, audit trail
│       └── verification.js       # Side-by-side comparison & forensic inspector
│
├── backend/                      # Production-Grade FastAPI Backend
│   ├── main.py                   # App entrypoint, CORS, static mounts, startup seeder
│   ├── api/                      # Modular REST endpoints
│   │   ├── auth.py               # Register, login, logout, me
│   │   ├── applications.py       # Application CRUD and submission
│   │   ├── documents.py          # Secure upload, SHA-256 hashing, forensic verification
│   │   ├── compliance.py         # Compliance evaluation and rule configuration
│   │   ├── officer.py            # Officer scrutiny review and correction requests
│   │   ├── admin.py              # Central analytics, audit trail, user directory
│   │   └── notifications.py      # Database-backed notifications
│   ├── models/                   # SQLAlchemy ORM Models
│   │   └── models.py             # User, Institution, Application, Document, etc.
│   ├── schemas/                  # Pydantic v2 Request/Response validation
│   │   └── schemas.py
│   ├── security/                 # Cryptography & Access Control
│   │   ├── auth.py               # PBKDF2 hashing, JWT generation & verification
│   │   ├── permissions.py        # Role guards (Institution, Officer, Super Admin)
│   │   └── rate_limiter.py       # IP-based rate limiting
│   ├── services/                 # Business Logic
│   │   ├── auth_service.py
│   │   ├── application_service.py
│   │   ├── document_service.py
│   │   ├── compliance_service.py
│   │   ├── notification_service.py
│   │   └── audit_service.py
│   ├── database/                 # Connection & Persistence
│   │   ├── connection.py         # Dual MySQL / SQLite engine manager
│   │   └── seed_data.py          # Auto-seeds 3 SIH Scenarios and test personas
│   └── migrations/
│       └── init.sql              # Complete MySQL DDL script
│
├── ai_engine/                    # Core AI & Compliance Microservices
│   ├── ocr_service.py            # PyMuPDF text & structured entity extraction
│   ├── integrity_checker.py      # SHA-256 hash, duplicate detection, metadata forensics
│   ├── document_validator.py     # Magic bytes check & side-by-side entity matcher
│   ├── compliance_engine.py      # Configurable rule evaluation (SFR, Land, Built-up)
│   └── risk_analyzer.py          # Explainable Review Priority score (Low, Medium, High)
│
├── sample_data/                  # Synthetic Demonstration Assets
│   ├── generate_sample_pdfs.py   # Script to generate synthetic test PDFs
│   └── sample_documents/         # Valid & tampered sample test certificates
│
├── tests/                        # Automated Pytest Suite
│   ├── test_auth.py              # PBKDF2 hashing and JWT token tests
│   ├── test_compliance.py        # Normative rules & Student-Faculty Ratio tests
│   └── test_verification.py      # Entity matching and mismatch detection tests
│
├── run.bat                       # 1-Click Windows Batch Server Launcher
├── run.ps1                       # 1-Click PowerShell Server Launcher
├── requirements.txt              # Python dependencies
├── .env.example                  # Environment configuration template
└── README.md                     # Comprehensive documentation
```

---

## 4. User Roles & Pre-Seeded Credentials

VerifyAI includes three role tiers with synthetic evaluation credentials pre-seeded into the database:

| Role | Name | Email | Password | Primary Functions |
|---|---|---|---|---|
| **Super Admin** | Dr. Rajesh Sharma | `admin@verifyai.org` | `Admin@2026` | Dynamic compliance rule editor, national analytics, audit logs, user management |
| **Verification Officer** | Priya Nair | `officer@verifyai.org` | `Officer@2026` | Scrutiny review queue, document forensics inspection, correction requests, final sign-off |
| **Institution Admin (Scenario 1)** | Prof. Arvind Joshi (DITS) | `principal@dits.edu.in` | `Principal@2026` | Fully compliant college; all 5 documents verified, EOA approved |
| **Institution Admin (Scenario 2)** | Fr. Thomas D'Souza (Xavier) | `admin@xavier.edu.in` | `Principal@2026` | Incomplete application; missing documents and built-up area shortfall |
| **Institution Admin (Scenario 3)** | Dr. Suresh Rao (Aryabhatta) | `director@aryabhatta.edu.in` | `Principal@2026` | Entity mismatch (college name mismatch) & Photoshop editing metadata anomaly |

> **SIH 2026 1-Click Demo Bar**: On any page, click the top-right **"SIH 2026 Demo Mode"** pill to switch personas instantly without retyping passwords.

---

## 5. SIH Demonstration Scenarios

### Scenario 1 – Successful Validation (Routine / Fast-Track)
- **Institution**: Delhi Institute of Technology & Science (`1-1092837461`, New Delhi).
- **Application Number**: `VAI-2026-10492` (Extension of Approval).
- **Result**: All 5 mandatory document categories uploaded; cryptographic SHA-256 unique; extracted entities match 100%; Student-Faculty Ratio 1:15 &le; 1:20; Land 6.5 Acres &ge; 2.5 Acres.
- **Review Priority**: `LOW (0.0 pts)` &rarr; Officer marks **Verified & Approved**.

### Scenario 2 – Incomplete Application (Deficit & Missing Documents)
- **Institution**: St. Xavier College of Engineering & Research (`1-2948571023`, Pune).
- **Application Number**: `VAI-2026-20831` (Intake Increase).
- **Result**: Missing Faculty Qualification Records and Land Title Deed; SFR ratio is 1:26.6 (exceeds 1:20 norm); built-up area is deficient.
- **Review Priority**: `HIGH (56.0 pts)` &rarr; Officer clicks **Request Correction** with itemized notices.

### Scenario 3 – Critical Discrepancy & Forensic Tamper Alert
- **Institution**: Aryabhatta Polytechnic & Technical Campus (`1-3847592019`, Bengaluru).
- **Application Number**: `VAI-2026-30914` (New Institution Approval).
- **Result**:
  - **Entity Mismatch**: Certificate displays *"Aryabhatta Institute of Science & Vocational Studies"* vs Application *"Aryabhatta Polytechnic & Technical Campus"* (54.0% similarity).
  - **Forensic Tamper Alert**: PDF metadata inspector identifies document was saved using consumer graphics editing software (*Adobe Photoshop 2023, iLovePDF*), and modification timestamp differs from creation date.
- **Review Priority**: `HIGH (98.5 pts)` &rarr; Case expedited for officer investigation and formal correction request.

---

## 6. Installation & Execution Guide

### Prerequisites
- Python 3.11+
- Modern Web Browser (Chrome, Edge, Firefox)

### Option A: 1-Click Launch (Recommended for Evaluation)
Simply double-click:
```cmd
run.bat
```
or run in PowerShell:
```powershell
.\run.ps1
```
The script will automatically initialize the database, seed all demonstration records, and start the FastAPI dev server at `http://127.0.0.1:8000`.

### Option B: Manual Command-Line Execution
1. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
2. **Seed Initial Database**:
   ```bash
   python -c "from backend.database.seed_data import seed_database; seed_database()"
   ```
3. **Start Server**:
   ```bash
   uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
   ```
4. **Access Applications**:
   - **Web Application Portal**: [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
   - **Interactive Swagger API Docs**: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
   - **VerifyAI Intelligence Center**: [http://127.0.0.1:8000/verification.html?id=3](http://127.0.0.1:8000/verification.html?id=3)

### Running Automated Test Suite
Run the test suite to verify authentication, compliance logic, and entity comparison:
```bash
pytest tests/ -v
```

---

## 7. MySQL Configuration Guide

By default, the platform uses a high-performance local SQLite database (`verifyai.db`) for immediate zero-configuration operation without requiring a local MySQL service.

To connect to a production MySQL 8.0 server:
1. Create database schema:
   ```sql
   mysql -u root -p < backend/migrations/init.sql
   ```
2. Update `.env`:
   ```ini
   DATABASE_URL=mysql+pymysql://root:your_password@localhost:3306/verifyai
   ```
3. Restart the server. SQLAlchemy will automatically connect to MySQL with connection pooling enabled.

---

## 8. Functioning vs. Pending Features

| Feature | Implementation Status | Implementation Details |
|---|---|---|
| **PyMuPDF OCR Extraction** | **Fully Functional** | Live text, table, and metadata extraction from PDF files. |
| **Regex Entity Recognition** | **Fully Functional** | Identifies names, AICTE codes, land areas, built-up areas, and student intakes. |
| **Side-by-Side Field Comparison** | **Fully Functional** | Levenshtein string similarity & bounds checking with color-coded mismatch alerts. |
| **SHA-256 File Hashing** | **Fully Functional** | Computes cryptographic digest and flags duplicate files across the repository. |
| **Forensic PDF Inspection** | **Fully Functional** | Detects consumer editing tools (Photoshop, Canva, iLovePDF) and date mismatches. |
| **Regulatory Compliance Engine** | **Fully Functional** | Dynamic evaluation of AICTE-style rules with admin toggle/add capabilities. |
| **Explainable Risk Scoring** | **Fully Functional** | 0-100 triage scoring categorized into Low, Medium, High review priority. |
| **Role-Based Portals** | **Fully Functional** | Separate portals for Institution Admin, Verification Officer, and Super Admin. |
| **Interactive Chart.js Analytics** | **Fully Functional** | Real-time status distribution and monthly intake trend charts. |
| **Government PKI Signature Verifier** | *Simulated / Prototype* | Inspects `/ByteRange` and `/Sig` PDF dictionaries; full HSM/eMudhra OCSP CRL chain validation identified as production milestone. |
| **Tesseract Image OCR Engine** | *Hybrid / Baseline* | Primary PDF extraction handled via PyMuPDF; Tesseract OCR hook configured for scanned raster bitmaps. |

---

## 9. Cybersecurity & Integrity Standards

- **Password Security**: Passwords hashed using PBKDF2-HMAC-SHA256 with 100,000 iterations and unique 16-byte random salts.
- **Access Control**: Role-based access control (RBAC) enforced on the backend via FastAPI dependency injection; tokens signed with HS256 JWTs.
- **File Upload Protection**: Validates MIME types, extensions, and file magic bytes (`%PDF`, `\x89PNG`, `\xFF\xD8\xFF`); files stored with UUID filenames outside web root.
- **Rate Limiting**: In-memory IP rate limiter protects authentication and upload endpoints against brute-force attacks.
- **SQL Injection Prevention**: 100% parameterized queries through SQLAlchemy ORM.
- **Digital Audit Trail**: Every user login, document upload, status alteration, and officer comment is immutably logged with timestamp, user ID, and IP address.

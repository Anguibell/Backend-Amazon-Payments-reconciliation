# 🏦 Backend API – Amazon Payments Reconciliation for SAP HANA

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-000000?style=flat&logo=express&logoColor=white)
![SAP HANA](https://img.shields.io/badge/SAP_HANA-0FAAFF?style=flat&logo=sap&logoColor=white)

## 📌 Overview
Enterprise RESTful API built with **Node.js** and **Express** that interfaces directly with **SAP HANA** via the official `@sap/hana-client` library. 

Developed as part of an end-to-end automated financial reconciliation solution between **Amazon** and **SAP Business One** (hosted on IDA Cloud), this backend acts as a dynamic dispatcher that maps incoming frontend requests to specialized **SAP HANA Stored Procedures**, returning structured JSON payloads for real-time financial monitoring, exception tracking, and accounting summaries.

🔗 **Companion Frontend Repository:** [Amazon-Payments-reconciliation (React Portal)](https://github.com/Anguibell/Amazon-Payments-reconciliation)

---

## 🚀 Key Technical Features
- **Native SAP HANA Connectivity:** Direct, high-performance database connection and query execution using `@sap/hana-client`.
- **Dynamic Stored Procedure Dispatcher:** Exposes a unified REST endpoint (`POST /api/Dynamic/process`) that dynamically routes execution based on numerical procedure IDs.
- **Financial & Accounting Coverage:** Orchestrates 11 specialized stored procedures covering order reconciliation, sales/payment fees, general ledger (GL) summaries, and Pareto exception analytics.
- **Frontend-Ready Serialization:** Parses and delivers stringified JSON responses directly from SAP HANA's first result column to match the existing frontend contract.

---

## 📂 Project Structure
```text
.
├── server.js                 # Main Express server entry point
├── src/
│   ├── db/
│   │   └── hana.js           # SAP HANA connection pool & Stored Procedure execution
│   ├── routes/
│   │   └── dynamic.js        # Dynamic request processing & validation
│   └── mappings/
│       └── sp-mappings.js    # Numerical ID -> SAP HANA Stored Procedure mapping
├── .env                      # Environment variables (excluded from version control)
└── package.json
```

---

## 🔌 API Reference

### `POST /api/Dynamic/process`
Main endpoint that processes reconciliation queries and reporting requests from the React frontend.

**Headers:**
- `Id`: Numerical ID mapping to the target SAP HANA Stored Procedure *(Required)*
- `Token`: Authorization token header for session validation
- `Content-Type`: `application/json`

**Request Body Example:**
```json
{
  "engine": "Worker",
  "fecha_desde": "01-01-2025",
  "fecha_hasta": "12-31-2025",
  "status": "ALL",
  "limit": 50
}
```
*(Note: Expected date format is `MM-dd-yyyy`, e.g., `01-15-2025`).*

---

## 🗄️ SAP HANA Stored Procedures Mapping

| Header `Id` | SAP HANA Stored Procedure | Business Module / Description |
| :---: | :--- | :--- |
| **257** | `CTS_ORDERS_RECONCILIATIONS` | Orders Reconciliation |
| **261** | `CTS_AMAZON_SALES_RECONCILIATIONS` | Amazon Sales Reconciliation |
| **262** | `CTS_AMAZON_INPAYMENTS_RECONCILIATIONS` | Incoming Payments Reconciliation |
| **263** | `CTS_AMAZON_SALES_FEES_RECONCILIATIONS` | Sales Fees Validation & Reconciliation |
| **264** | `CTS_AMAZON_INPAYMENTS_FEES_RECONCILIATIONS` | Payment Fees Validation & Reconciliation |
| **265** | `CTS_RECON_DASHBOARD_HEADER` | Executive KPI Dashboard Header |
| **266** | `CTS_RECON_SETTLEMENT_DETAIL` | Granular Settlement Transaction Detail |
| **267** | `CTS_EXCEPTIONS_DASHBOARD` | Discrepancies & Exceptions Dashboard |
| **269** | `CTS_ACCOUNTING_GL_SUMMARY` | General Ledger (GL) Accounting Summary |
| **271** | `CTS_REPORT_MONTHLY_RECON_SUMMARY` | Monthly Reconciliation Summary Report |
| **279** | `CTS_EXCEPTIONS_PARETO_GLOBAL` | Global Pareto Analysis of Financial Exceptions |

---

## ⚙️ Installation & Local Setup

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Configure environment variables:**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your SAP HANA instance credentials:
   ```env
   HANA_SERVER=your-server:30015
   HANA_USER=your-username
   HANA_PASSWORD=your-password
   HANA_DATABASE=your-database
   HANA_SCHEMA=your-schema
   ```

3. **Start the server:**
   - Production mode:
     ```bash
     npm start
     ```
   - Development mode (with auto-reload):
     ```bash
     npm run dev
     ```

---

## 🔒 Enterprise Confidentiality Notice
> This backend was engineered for a private corporate environment at **Celeritech** interfacing with proprietary **SAP HANA / SAP Business One** infrastructure. All database credentials, internal IP addresses, and proprietary SQLScript definitions are omitted in compliance with data security policies.

---
**Developer:** [Anguibell Romero](https://www.linkedin.com/in/anguibell-romero) | [GitHub Profile](https://github.com/Anguibell)

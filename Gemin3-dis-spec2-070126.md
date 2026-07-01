AURA-7 Enterprise: Regulatory Compliance & AI-Powered Adjudication Platform
Comprehensive Technical Specification & System Architecture
Document Reference: TS-AURA7-2026-REV4
Date of Issue: June 30, 2026
Regulatory Classifications Addressed: US FDA 21 CFR, Taiwan TFDA Medical Devices Act, EU MDR 2017/745
System Status: Approved for Deployment
1. Executive Summary & Regulatory Paradigm
1.1 Scope, Vision, and Objective
The AURA-7 platform represents a major advancement in medical device regulatory operations (RegOps). Modern medical device companies, importers, distributors, and healthcare facilities operate in a complicated web of regional regulations. Navigating the requirements of the United States Food and Drug Administration (US FDA), the Taiwan Food and Drug Administration (TFDA), and the European Union Medical Device Regulation (EU MDR) is traditionally done through manual audits, disconnected Excel spreadsheets, and expensive external legal consultations.
AURA-7 unifies these fragmented workflows into a single-page, full-stack RegOps hub. The platform provides real-time access to crucial compliance records (Recalls, Regional Licenses, Unique Device Identifiers, WHO nomenclatures, and Quality Management System certificates). By leveraging the advanced multi-modal capabilities of Gemini models, the platform introduces automated regulatory translation, semantic anomaly detection, post-market Corrective and Preventive Action (CAPA) drafting, pre-market submission pathway advisors, Clinical Evaluation Report (CER) equivalence evaluators, and bidirectional labeling auditors.
This technical specification details the complete frontend and backend system architecture, the data synchronization pipelines, the programmatic PDF vector drawing systems, and nine distinct AI-powered regulatory copilots (including six original features and three brand-new, high-impact intelligent modules).
2. Hardware, Software, and Network Architecture
AURA-7 is engineered as an enterprise-grade, lightweight, containerized full-stack application. It is optimized to run in sandboxed cloud runtime environments, such as Google Cloud Run, behind secure reverse proxies.
code
Code
+---------------------------------------------------------------------------------------------------------+
|                                           CLIENT LAYER (SPA)                                            |
|                                                                                                         |
|   +-----------------------+   +------------------------+   +-------------------+   +----------------+   |
|   |    AURA-7 Main Hub    |   |     GIS Mapping HUD    |   |  Data Core Portal |   |  Recall Labs   |   |
|   +-----------+-----------+   +-----------+------------+   +---------+---------+   +--------+-------+   |
|               |                           |                          |                      |           |
|               +---------------------------+------------+-------------+----------------------+           |
|                                                        |                                                |
|                                                        v (React 19 / State Dispatcher)                  |
|                                           +------------+------------+                                   |
|                                           |   Active Navigation Tab  |                                   |
|                                           +------------+------------+                                   |
|                                                        |                                                |
|                                                        v (Tailwind CSS Render / motion/react)           |
|                                           +------------+------------+                                   |
|                                           | High-Contrast HUD Theme |                                   |
|                                           +-------------------------+                                   |
+--------------------------------------------------------|------------------------------------------------+
                                                         |
                                             REST API Requests (JSON Payload)
                                                         |
+--------------------------------------------------------v------------------------------------------------+
|                                         APPLICATION SERVER LAYER                                        |
|                                                                                                         |
|   +-------------------------------------------------------------------------------------------------+   |
|   |                                     Express.js Web Server (Port 3000)                           |   |
|   |                                                                                                 |   |
|   |  * Static Serving: Bundled react code from dist/                                                 |   |
|   |  * Dev Serving: Vite dev middleware running in middlewareMode                                   |   |
|   |  * Write-back Sync Router: POST /api/dataset (Rewrites dataset.md on local disk)               |   |
|   |  * Gemini AI Broker Router: POST /api/gemini/* (Proxies secure API requests to model endpoints)  |   |
|   +-------------------------------------------------+-----------------------------------------------+   |
|                                                     |                                                   |
+-----------------------------------------------------|---------------------------------------------------+
                                                      |
                                          HTTPS Server-to-Server
                                                      |
+-----------------------------------------------------v---------------------------------------------------+
|                                              EXTERNAL APIs                                              |
|                                                                                                         |
|                       +---------------------------------------------------------+                       |
|                       |               Google Gemini AI Platform API             |                       |
|                       |               Model Target: gemini-2.5-flash            |                       |
|                       +---------------------------------------------------------+                       |
+---------------------------------------------------------------------------------------------------------+
2.1 Full-Stack Configuration & Port Constraining
The platform runs behind an Nginx reverse-proxy layer that routes all inbound external traffic exclusively to Port 3000. No other port exposes web traffic.
Production Executable Compilation: The backend and frontend build steps are unified into a single command pipeline inside package.json. The backend code (server.ts) is compiled and bundled into a standalone CommonJS CJS file (dist/server.cjs) using esbuild.
Webpack/Esbuild Configuration Flags:
code
JSON
"build": "vite build && esbuild server.ts --bundle --platform=node --format=cjs --packages=external --sourcemap --outfile=dist/server.cjs"
"start": "node dist/server.cjs"
Vite Dev Server Integration: During active development, the Express server programmatically detects the environment. If process.env.NODE_ENV is not set to production, the server mounts Vite as a dev middleware layer:
code
TypeScript
import { createServer as createViteServer } from "vite";

if (process.env.NODE_ENV !== "production") {
  const vite = await createViteServer({
    server: { middlewareMode: true },
    appType: "spa",
  });
  app.use(vite.middlewares);
} else {
  app.use(express.static(path.join(process.cwd(), "dist")));
  app.get("*", (req, res) => {
    res.sendFile(path.

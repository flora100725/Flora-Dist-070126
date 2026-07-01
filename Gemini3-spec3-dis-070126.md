AURA-7 Enterprise Regulatory Safety & Forensic Compliance Platform
Comprehensive Technical Specification & Architectural Blueprint
Section 1: Executive Architectural Overview & Design Concept
1.1 Platform Vision & Scope
The AURA-7 Enterprise Regulatory Safety & Forensic Compliance Platform is designed as a secure, full-stack, corporate cockpit for regulatory affairs (RA), quality assurance (QA), and clinical safety engineers within the medical device sector. Operating within highly structured jurisdictions—principally governed by the Taiwan Food and Drug Administration (TFDA) under the Medical Device Management Act (醫療器材管理法), the US Food and Drug Administration (FDA) 21 CFR regulations, and the European Union Medical Device Regulation (EU MDR 2017/745)—the platform establishes an unified control center. It bridges unstructured, real-world clinical safety telemetry with rigid, statutory compliance schemas.
The application scope centers on the ingestion, standardization, and intelligence audit of five canonical data registries:
Clinical Safety Recalls (醫療器材召回事件)
Medical Device Licenses (醫療器材許可證字號)
Unique Device Identification (UDI) Registry (TUDID 數據庫)
WHO Global Nomenclature Standards (EMDN/GMDN Classifications)
Quality Management Systems (QMS/QSD) Facility Certificates (製造廠品質管理系統)
AURA-7 delivers interactive analytical tools, including a Python execution sandbox powered by deep-learning models to automate data visualization, a Taiwan-specific Legal Auditor, and six multi-axis cognitive copilots. These copilots simulate market entry pathways, evaluate Clinical Evaluation Reports (CER), audit instruction labels, and predict supply-chain disruption indexes.
1.2 Design Philosophy: The "Cosmic Slate / Calm Twilight" Visual Theme
The visual architecture of AURA-7 is structured around an industrial, high-contrast, eye-safe cockpit theme, specifically optimized for high-density, multi-hour regulatory monitoring sessions.
code
Code
┌────────────────────────────────────────────────────────────────────────┐
│  AURA-7 FORENSIC COCKPIT                                 [UTC 20:27]  │
├────────────────────────────────────────────────────────────────────────┤
│  [01] Dashboard   [02] Recalls   [03] Licenses   [04] UDI Registry     │
│  [05] WHO Nomen.  [06] Copilots  [07] Analytics  [08] Legal Auditor    │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  ┌─────────────────────────────────┐  ┌─────────────────────────────┐  │
│  │  // LIVE COMPLIANCE SIGNAL      │  │  // COGNITIVE COMPLIANCE    │  │
│  │  Active Recalls: 24             │  │  Pathways Plausible: 85%    │  │
│  │  Approved QSD: 35               │  │  Risk Level: MODERATE       │  │
│  └─────────────────────────────────┘  └─────────────────────────────┘  │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  // INTERACTIVE PYTHON ANALYTICS & VIEWPORT                      │  │
│  │  [ matplotlib, pandas, seaborn compilation ]                     │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
The Canvas Palette: Formulated on deep slate, off-blacks, and rich charcoal tones (#121212 base, #1C1C1E card structures, with a background noise overlay of rgba(0,0,0,0.4)). This visual baseline reduces screen glare and minimizes pupillary fatigue.
High-Contrast Signaling Elements: Crucial notifications, regulatory alerts, and structural boundaries utilize a razor-sharp red accent (#FF2800), balanced by industrial amber warning indicators (#F5C518), and clean compliance greens (#10B981) for approved states.
Aesthetic Typography Pairing:
Primary UI Display: Inter (sans-serif) is utilized for dense, multi-column data grids, tabular layouts, and control-bar selectors. This ensures text legibility at sub-10px font scaling.
Display Metadata / Headers: Space Grotesk or Outfit is deployed on section titles and operational telemetry indexes, delivering a clinical, technical aesthetic.
Terminal and Code Environments: JetBrains Mono is embedded for raw database payloads, log files, JSON responses, and Python code blocks.
Architectural Honesty (Anti-AI-Slop Mandate): No decorative, mock telemetry, fake container port statuses, or imaginary latency values are permitted. Visual components are strictly tied to real state variables. Outer margins are kept clean, utilizing generous negative space to focus attention on active regulatory compliance tasks.
1.3 Full-Stack Decoupled Framework Architecture
The platform operates as a containerized, full-stack application leveraging a decoupled architecture composed of a client-side Single Page Application (SPA) and an asynchronous Node.js server.
code
Code
┌─────────────────────────────────────────────────────────┐
       │                 CLIENT PANEL (Vite SPA)                 │
       └────────────────────────────┬────────────────────────────┘
                                    │
                         HTTP REST JSON Payload
                                    │
       ┌────────────────────────────▼────────────────────────────┐
       │                EXPRESS SERVER (Node.js)                 │
       └────────────────────────────┬────────────────────────────┘
                                    │
                   Lazy SDK Initialization & Routing
                                    │
       ┌────────────────────────────▼────────────────────────────┐
       │                GOOGLE GEMINI AI ENGINE                  │
       │                 (gemini-3.5-flash)                      │
       └─────────────────────────────────────────────────────────┘
Constructed in TypeScript using React functional components. State management is bound directly to local viewports with transition layers governed by motion/react (utilizing subtle slide-over entries, fade-in sequences, and layout animations to preserve user orientation when switching tabs).
All critical operations involving API keys, credential parameters, and regulatory processing logic run exclusively server-side. The Express application acts as an API gateway, proxying requests to the Google Gemini AI engine. This structure ensures that confidential regulatory tokens and the master Gemini API key (process.env.GEMINI_API_KEY) are protected and never transmitted to the client browser.
Authentication, session limits, and API throttling are managed at the reverse-proxy layer. Server-side endpoints strictly validate inbound JSON payloads against TypeScript contracts to prevent query injection and buffer-overflow exploits.
Section 2: Data Schemas & Structural Baseline
2.1 Canonical Dataset Schemas
The core database of AURA-7 coordinates five medical device domains. Each domain is defined by a rigid schema representing mandatory fields required by national regulatory bodies during registration, licensing, and hazard mitigation operations.
code
Code
┌─────────────────────────────┐
                  │    AURA-7 CORE REGISTRIES   │
                  └──────────────┬──────────────┘
         ┌───────────────┬───────┼───────┬───────────────┐
         ▼               ▼       ▼       ▼               ▼
    [Recalls]       [Licenses] [UDIs]  [WHO Nom.]      [QMS]
id (String): Unique, system-generated tracking key.
udi (String): Unique Device Identification code (GS1/HIBCC standard).
title (String): Brief name of the recall event in Traditional Chinese and English.
manufacturer (String): Certified manufacturing legal entity.
recall_class (Enum: "1" | "2"): Priority class. Class 1 denotes life-threatening safety violations; Class 2 denotes moderate clinical risks.
reason_for_recall (String): Comprehensive root-cause description (e.g., sterilization failures, software loop anomalies).
action_date (String): Official date of recall publication (Format: MM/DD/YYYY).
status (Enum: "Pending" | "Active" | "Closed"): Present phase of recall execution.
impact_radius (Number): Estimated volume of distributed devices impacted in the territory.
許可證字號 (String): Official TFDA Registration Number (e.g., 衛部醫器輸字第034567號).
中文品名 (String): Traditional Chinese device nomenclature.
英文品名 (String): Certified English device name.
醫療器材級數 (Enum: "Class I" | "Class II" | "Class III"): Risk classification ranking.
製造廠名稱 (String): Registered manufacturer facility.
製造廠國別 (String): Origin country of the manufacturing facility.
有效日期 (String): Regulatory expiration threshold (Format: MM/DD/YYYY).
註銷狀態 (Enum: "正常" | "註銷"): Legal status flag.
udi_di (String): Device Identifier (the fixed GS1-GTIN or HIBCC package code).
發行機構 (Enum: "GS1" | "HIBCC" | "ICCBBA"): Globally recognized issuing agency.
品牌名稱 (String): Commercial trade name.
型號 (String): Specific model designation.
規格 (String): Dimensions, mechanical parameters, and design options.
包裝層級 (Enum: "單一產品" | "內包裝" | "外箱"): Distribution packaging hierarchy level.
啟用日期 (String): Active registry timestamp in the TUDID database.
code (String): Canonical alphanumeric taxonomic code (e.g., EMDN code starting with W).
preferred_term_en (String): Standardized English therapeutic term.
preferred_term_zh (String): Standardized Traditional Chinese medical translation.
definition (String): Legal, clinical boundary definition of the device category.
hierarchical_level (Number): Taxonomic depth index (from Level 1 Category down to Level 5 Attribute).
gmdn_equivalence (String): Corresponding Global Medical Device Nomenclature mapping key.
qsd_no (String): Official TFDA Quality System Documentation audit number (e.g., QSD12345).
製造廠名稱 (String): Legal entity name of manufacturing facility.
廠址 (String): Physical manufacturing facility location.
認可登錄品項 (String): Scope of certified device groups approved for production.
有效日期 (String): Re-audit deadline timestamp.
案件狀況 (Enum: "核准" | "補件" | "廢止"): Current application status of QSD clearance.
2.2 Markdown Document Database Engine (dataset.md)
AURA-7 employs a file-based Markdown database system targeting /dataset.md as its primary persistence ledger. The system uses structured markdown syntax, partitioning data domains into separate subheadings containing highly structured, standard Markdown tabular blocks.
The server parses dataset.md into structured JSON arrays through an Abstract Syntax Tree (AST) transformer. The parsing pipeline runs as follows:
code
Code
┌──────────────────────────────┐
                  │          dataset.md          │
                  └──────────────┬───────────────┘
                                 │
                        Line-by-Line Stream
                                 │
                  ┌──────────────▼───────────────┐
                  │      REGULAR EXPRESSIONS     │
                  │   Detects Headers (e.g. ##)  │
                  └──────────────┬───────────────┘
                                 │
                         Table Row Splitter
                                 │
                  ┌──────────────▼───────────────┐
                  │    TRIMMING & SANITIZATION   │
                  │   Strips Leading/Trailing |  │
                  └──────────────┬───────────────┘
                                 │
                       Field Mapped JSON Node
                                 │
                  ┌──────────────▼───────────────┐
                  │      MEMOIZED REACT STATE    │
                  └──────────────────────────────┘
Header Segmentation: The parser processes the file line-by-line, locating second-level headers (## Recalls, ## Licenses, ## UDI Registry, ## WHO Nomenclature, ## QMS Certificates).
Table Extraction: Once a target header is detected, the parser compiles all subsequent lines until the next heading or EOF.
Boundary Cleanup & Splitting:
Lines beginning with | are trimmed.
The secondary boundary separator (|---|---|...) is discarded.
Columns are split by the pipe delimiter (|), with leading and trailing whitespaces stripped.
Schema Alignment: The header row maps columns to the exact database keys defined in Section 2.1.
Serialization Execution: On write actions, the system reverses the pipeline. It constructs standard markdown tables from JSON arrays, prepends headers, and writes the output back to dataset.md. This architecture preserves human readability and maintains full system synchronization.
Section 3: Server-Side Endpoints & AI Integration Pipelines
3.1 AI Engine Setup & Lazy Initialization
To guarantee backend stability under cold-start containerization constraints, the platform avoids module-load SDK initialization. It implements Lazy Initialization patterns, instantiating the Gemini API client only when an API call is received.
code
TypeScript
// server/gemini.ts
import { GoogleGenAI } from "@google/genai";

let aiInstance: GoogleGenAI | null = null;

export function getGeminiClient(): GoogleGenAI {
  if (!aiInstance) {
    const apiKey = process.env.GEMINI_API_KEY;
    if (!apiKey) {
      throw new Error("CRITICAL CONFIGURATION ERROR: GEMINI_API_KEY environment variable is undefined.");
    }
    aiInstance = new GoogleGenAI({ apiKey });
  }
  return aiInstance;
}
3.2 Endpoint Operational Routing & Schema Declarations
Operational Context: Processes raw, unstructured clinical safety data (e.g., email alerts, clinical safety reports) and normalizes it into canonical JSON structures aligned with target schemas.
Request Contract:
code
JSON
{
  "rawData": "Recall ALERT: Abiomed Impella heart pumps (Class I). Date: June 18 2026. UDI: 00844747012354. Critical valve failures have caused 3 adverse events in Taipei.",
  "model": "gemini-3.5-flash",
  "customPrompt": "Clean up typo fields, map categories strictly, and normalize date formats to MM/DD/YYYY. Selected Dataset Type Target: Recall"
}
System Instructions & Prompt Engineering:
"You are a forensic medical device regulatory auditor. You must parse the unstructured data into a structured JSON payload that matches the specified dataset schema. Your response must contain only valid JSON, matching the contract below, without markdown blocks."
Structured JSON Output Contract:
code
JSON
{
  "detectedType": "Recall",
  "records": [
    {
      "id": "REC-2026-0084",
      "udi": "00844747012354",
      "title": "Abiomed Impella Valve Structural Failure",
      "manufacturer": "Abiomed, Inc.",
      "recall_class": "1",
      "reason_for_recall": "Critical valve failures resulting in structural collapse and blood flow leakage.",
      "action_date": "06/18/2026",
      "status": "Active",
      "impact_radius": 3
    }
  ]
}
Operational Context: Analyzes a device description, technologies, and intended use, and plots pre-market clearance tracks for multiple markets.
Request Contract:
code
JSON
{
  "deviceName": "OcuScan AI Fundus Camera",
  "intendedUse": "Non-invasive detection of diabetic retinopathy using deep learning algorithms in clinic environments.",
  "techType": "Software (SaMD)",
  "targetMarkets": { "tfda": true, "fda": true, "mdr": false },
  "model": "gemini-3.5-flash"
}
Prompt Architecture:
"Identify the classification (Class I, II, or III) under TFDA, FDA, and EU MDR guidelines. Detail the recommended regulatory pathways, testing requirements, estimated approval timelines, and budget projections."
Structured JSON Output Contract:
code
JSON
{
  "classifications": {
    "tfda": "Class II (Active diagnostic device with diagnostic software)",
    "fda": "Class II (Regulation 21 CFR 886.1120)",
    "mdr": "Class IIa (Rule 11 SaMD)"
  },
  "recommendedPathway": "Compile the TFDA Pre-market approval registration dossier using predicate device equivalence data. For the US FDA, submit a 510(k) notification utilizing equivalence data.",
  "estimatedTimeline": "12 to 18 Months",
  "estimatedCost": "$125,000 USD",
  "standardsRequired": [
    "IEC 62304:2006 (Medical device software lifecycle processes)",
    "ISO 14971:2019 (Application of risk management to medical devices)",
    "IEC 62366-1:2015 (Application of usability engineering)"
  ]
}
Operational Context: Conducts Clinical Evaluation Report (CER) comparison audits between a target medical device and predicate clinical profiles.
Request Contract:
code
JSON
{
  "targetDeviceSpecs": "316L Stainless Steel vascular stent, 2.5mm diameter, electropolished surface finish, polymer-free drug coating of Sirolimus.",
  "literatureText": "Predicate Device Alpha utilizes cobalt-chromium alloy, 2.75mm diameter, with a bio-erodible PLGA polymer matrix carrier for Everolimus delivery.",
  "model": "gemini-3.5-flash"
}
Prompt Architecture:
"Perform a quantitative gap analysis evaluating equivalence along three coordinates: Technical, Biological, and Clinical. Calculate individual gap scores (0-100% gap) and an overall Equivalence Congruence Score."
Structured JSON Output Contract:
code
JSON
{
  "equivalenceScore": 72,
  "congruent": false,
  "technicalGapScore": 35,
  "biologicalGapScore": 45,
  "clinicalGapScore": 20,
  "technicalDifferences": [
    "Material disparity: 316L Stainless Steel vs Cobalt-Chromium alloy modifies radial strength parameters.",
    "Drug vehicle disparity: Polymer-free coating vs Bio-erodible PLGA polymer matrix carrier.",
    "Active pharmaceutical agent: Sirolimus vs Everolimus."
  ],
  "requiredTestingToReconcile": "Conduct non-clinical bench testing evaluating radial fatigue limits, drug-elution kinetics, and animal biocompatibility (ISO 10993-4 hemocompatibility) to bridge technical and biological gaps."
}
Operational Context: Evaluates labeling drafts, patient manuals, or Instructions for Use (IFU) against national safety regulations to flag unauthorized clinical claims.
Request Contract:
code
JSON
{
  "labelText": "The CardioPulse watch is a complete diagnostic tool that absolutely cures hypertension, cures heart failure, and provides continuous, diagnostic-grade ECG data without medical supervision.",
  "sourceLang": "en",
  "model": "gemini-3.5-flash"
}
Prompt Architecture:
"Evaluate this labeling copy for clinical risk claims. Highlight high-risk assertions and medical jargon. Translate the analyzed text into Traditional Chinese, aligning it with TFDA regulatory standards."
Structured JSON Output Contract:
code
JSON
{
  "violationsCount": 3,
  "translatedText": "CardioPulse 手錶是一款全面的診斷工具，可在無醫師監督下提供連續、診斷級別的心電圖（ECG）數據。",
  "violations": [
    {
      "claim": "absolutely cures hypertension",
      "severity": "Critical",
      "issue": "Prohibited curative clinical claim. Over-the-counter wellness wearables cannot claim to cure chronic diseases.",
      "remedy": "Replace with: 'Assists in monitoring trend data for blood pressure reference.'"
    },
    {
      "claim": "cures heart failure",
      "severity": "Critical",
      "issue": "Prohibited medical efficacy claims. This statement violates advertising guidelines.",
      "remedy": "Remove all references to curing disease."
    },
    {
      "claim": "diagnostic-grade ECG data without medical supervision",
      "severity": "High",
      "issue": "Overstates diagnostic device classification. Clinical diagnostic decisions must involve certified physicians.",
      "remedy": "Revise to: 'Provides reference ECG waveforms; not intended to replace standard clinical diagnostics.'"
    }
  ]
}
Operational Context: Identifies clinical regulatory risks based on a strategic launch plan.
Request Contract:
code
JSON
{
  "scenario": "Developing a Class III bio-resorbable vascular scaffold with AI-driven real-time arterial sizing. Launching first in Taiwan, followed by a US 510(k) and EU MDR Article 120 transition.",
  "model": "gemini-3.5-flash"
}
Structured JSON Output Contract:
code
JSON
{
  "approvalProbability": 45,
  "regulatoryHurdles": [
    "TFDA demands local clinical validation for innovative Class III bio-resorbable materials.",
    "US FDA will reject standard 510(k) routing; a De Novo or Pre-Market Approval (PMA) is required.",
    "EU MDR demands rigorous clinical investigation data, excluding reliance on historical predicates."
  ],
  "riskMatrix": [
    {
      "risk": "Material Biocompatibility Issues",
      "severity": "Critical",
      "mitigation": "Perform comprehensive ISO 10993 testing over 24 months."
    },
    {
      "risk": "De Novo Route Rejection by US FDA",
      "severity": "High",
      "mitigation": "Initiate early Q-Sub meetings with CDRH review branches."
    }
  ],
  "executiveSummary": "The regulatory plan faces a low probability of first-pass approval due to the integration of Class III materials with AI diagnostic components. This shifts the US pathway from 510(k) to De Novo or PMA, and the TFDA route to a full clinical trial review."
}
Operational Context: Analyzes a recall, predicting hospital impacts, quarantine scopes, and supply-chain logistics.
Request Contract:
code
JSON
{
  "recallEvent": {
    "title": "Abiomed Impella Valve Failure",
    "manufacturer": "Abiomed, Inc.",
    "reason_for_recall": "Critical valve structural collapse causing leakage.",
    "recall_class": "1",
    "udi": "00844747012354"
  },
  "model": "gemini-3.5-flash"
}
Structured JSON Output Contract:
code
JSON
{
  "estimatedCost": "$12,450,000 USD",
  "patientsAffected": 1450,
  "liabilityRisk": "Severe",
  "clinicsImpactedCount": 84,
  "logisticsOutageMap": [
    {
      "port": "Keelung Customs District",
      "disruptionLevel": "Severe",
      "alternativeRoute": "Reroute incoming sterile components through Kaohsiung port under pre-clearance status."
    },
    {
      "port": "Taipei Hospital Distribution Hub",
      "disruptionLevel": "Moderate",
      "alternativeRoute": "Isolate lot numbers matching the recall prefix; route reserve inventory from the southern warehouse."
    }
  ],
  "recommendations": [
    "Quarantine all un-implanted devices with serial prefix ABI-2026.",
    "Notify active clinical centers within 24 hours.",
    "Deploy a clinical field representative team to assist with device retrieval."
  ]
}
Operational Context: Performs a gap analysis between draft manufacturing policies and ISO 13485:2016 requirements.
Request Contract:
code
JSON
{
  "qmsPolicy": "Quality records of sterile barrier validations are logged once every 30 days. Bioburden spikes trigger a manual quarantine, unless cleared by the supervisor.",
  "model": "gemini-3.5-flash"
}
Structured JSON Output Contract:
code
JSON
{
  "gapScore": 55,
  "missingSops": [
    "SOP for Automated Quarantine Locking in Manufacturing Execution Systems",
    "SOP for Bioburden Alert Thresholds and CAPA Investigation triggers",
    "SOP for Validation Frequencies of Sterile Barrier Systems matching ISO 11607"
  ],
  "isoClauseGaps": [
    {
      "clause": "Clause 7.5.6 (Validation of processes for production and service provision)",
      "gapDescription": "A 30-day logging cycle for sterile barrier validation records is insufficient to verify sterile integrity.",
      "correctiveAction": "Implement real-time sensor logging and validate sterile packaging runs daily."
    },
    {
      "clause": "Clause 8.3 (Control of nonconforming product)",
      "gapDescription": "Allowing a supervisor to manually clear a quarantine after a bioburden spike without a formal investigation is a critical compliance gap.",
      "correctiveAction": "Enforce automated CAPA locks. Quarantine releases must require certified Quality Assurance sign-off."
    }
  ],
  "roadmapToAuditReady": "Audit readiness is projected to require 4 weeks. Key milestones: implement automated quarantine controls, revise sterilization validation schedules, and conduct QA training sessions."
}
Operational Context: Solves legal inquiries regarding Taiwan's Medical Device Management Act.
Request Contract:
code
JSON
{
  "action": "qa",
  "query": "請詳細解釋醫療器材管理法第13條關於許可證查驗登記與登錄的規定、法規適用範圍及違規處罰。",
  "model": "gemini-3.5-flash"
}
Structured JSON Output Contract:
code
JSON
{
  "referencedArticles": ["Article 13", "Article 62"],
  "content": "### 醫療器材管理法第13條法律評析\n\n根據中華民國《醫療器材管理法》第13條第1項規定，製造、輸入醫療器材，應向中央主管機關（衛生福利部食品藥物管理署）申請查驗登記並取得醫療器材許可證後，始得為之。\n\n#### 一、 法規適用與要件\n1. **國產製造**：需具備符合QMS（品質管理系統）之製造廠執照，並檢附技術文件申請。\n2. **輸入代理**：必須由合格醫療器材商代理，並檢附產地國之製售證明（CFS）與技術審查文件（STED）。\n\n#### 二、 違規罰則（結合第62條）\n未經核准擅自製造或輸入醫療器材者，構成刑事犯罪：\n* 處 **三年以下有期徒刑、拘役**，或科或併科新臺幣 **一千萬元以下罰金**。\n* 其產品依第57條規定，沒入銷毀並責令限期回收。"
}
Operational Context: Translates a charting prompt into executable Python code utilizing pandas, matplotlib, and seaborn. It outputs both the Python code block and a JSON representation used by the React client to render corresponding SVG graphics.
Request Contract:
code
JSON
{
  "datasetType": "Recall",
  "model": "gemini-3.5-flash",
  "prompt": "Plot recall class distributions over time, flag top 5 manufacturers, and map seasonal incident surges.",
  "recordsCount": 20
}
Structured JSON Output Contract:
code
JSON
{
  "pythonCode": "import pandas as pd\nimport matplotlib.pyplot as plt\nimport seaborn as sns\n\ndata = pd.DataFrame([...])\nplt.style.use('dark_background')\n# Matplotlib structural plotting codes\n",
  "charts": [
    {
      "id": 1,
      "title": "Recall Intensity Class Distribution",
      "type": "bar",
      "labels": ["Class I (Critical)", "Class II (High)"],
      "datasets": [{ "label": "Alert Volume", "data": [8, 14] }],
      "description": "Field safety alerts are heavily weighted toward Class II categories."
    },
    {
      "id": 2,
      "title": "Incident Chronology Trends",
      "type": "line",
      "labels": ["Jan", "Feb", "Mar", "Apr", "May", "Jun"],
      "datasets": [{ "label": "Alerts", "data": [12, 19, 15, 8, 24, 18] }],
      "description": "Alert frequency peaked in May 2026 due to recall actions."
    },
    {
      "id": 3,
      "title": "Manufacturer Recalls Breakdown",
      "type": "pie",
      "labels": ["Abiomed", "Boston Sci", "Medline", "Others"],
      "datasets": [{ "label": "Incidents", "data": [5, 3, 4, 12] }],
      "description": "Abiomed and Medline account for 38% of active recall incidents."
    },
    {
      "id": 4,
      "title": "Risk Profile Area Map",
      "type": "area",
      "labels": ["SOP Fail", "Leakage", "Adverse Event", "Design Deviation"],
      "datasets": [{ "label": "Probability Score", "data": [40, 65, 80, 50] }],
      "description": "Adverse clinical events represent the largest failure pathways."
    },
    {
      "id": 5,
      "title": "Status of Regulatory Operations",
      "type": "bar",
      "labels": ["Pending", "Recalled", "Resolved", "Monitoring"],
      "datasets": [{ "label": "Dossiers", "data": [15, 24, 30, 10] }],
      "description": "Over 40% of cases are currently in a resolved or closed state."
    },
    {
      "id": 6,
      "title": "Regional Distribution Index",
      "type": "radar",
      "labels": ["Taipei", "New Taipei", "Kaohsiung", "Taichung", "Tainan"],
      "datasets": [{ "label": "Dispersal Index", "data": [85, 90, 70, 75, 60] }],
      "description": "Inventory recall compliance is concentrated in northern medical centers."
    }
  ]
}
Section 4: Component Hierarchies & State Configurations
code
Code
┌────────────────────────────────┐
                               │            App.tsx             │
                               └───────────────┬────────────────┘
                                               │
               ┌───────────────────────┬───────┴───────┬───────────────────────┐
               ▼                       ▼               ▼                       ▼
┌──────────────────────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────────┐
│  AdvancedDatasetManager.tsx  ││  SOP_Gap.tsx ││  Auditor.tsx ││     PythonAnalytics.tsx      │
└──────────────────────────────┘└──────────────┘└──────────────┘└──────────────────────────────┘
4.1 Client Component Composition Tree
App.tsx: Central state machine and UI shell. Holds global reactive states:
rawMarkdown (String): Raw markdown string loaded from /dataset.md.
recalls, licenses, udis, who, qms (Arrays): Structured datasets parsed from markdown.
logs (Array): Audit trail list.
activeTab (Enum: "dashboard" | "recalls" | "licenses" | "udis" | "who" | "copilots" | "analytics" | "legalauditor").
language (Enum: "zh" | "en").
isDarkMode (Boolean): Global theme flag.
components/AdvancedDatasetManager.tsx: Subdivided into four functional tabs:
AI Importer: Manages the drag-and-drop file uploader, AI standardization toggle, system instructions, and results preview.
Dataset Previewer: Displays database records inside a clean table layout, with sorting and paging options.
Dataset Exporter: Generates download payloads in CSV, JSON, and Markdown formats.
Raw dataset.md Editor: Standard code editor for making direct edits to /dataset.md.
components/RegulatoryCopilots.tsx: Controls UI flows for the six integrated copilots, managing localized loading spinners and copy-to-clipboard transitions.
components/PythonAnalytics.tsx: Manages visualization prompts, model selections, and python script generation. Contains the custom SVG rendering engine.
components/TFDALegalAuditor.tsx: Manages legal Q&As, audit checklists, warning letter draft generation, and interactive reference cards for Taiwan's Medical Device Management Act.
4.2 Interactive Vector Rendering Mathematics
The PythonAnalytics.tsx component features a custom vector engine that renders generated chart configurations as clean, responsive inline SVGs. This approach bypasses external dependencies, ensuring compatibility with the platform's sandboxed iframe environment.
The mathematical transformations for each chart type are implemented as follows:
For a chart container with dimensions 
 (typically set to 
), we define a rendering area by introducing margin offsets:

The data points are normalized against the maximum value in the dataset (
) with a 15% safety headroom:
For a given data value 
 at index 
, the corresponding viewport coordinates 
 are calculated as:

These values are plotted using <polyline> elements for line charts and <polygon> elements with vertical linear gradients for area charts.
Pie chart sectors are rendered as custom <path> segments. For a center coordinate 
 and radius 
:
code
Code
(cx, cy - r)
                       ▲
                       │   /
                       │  /  Angle (θ)
                       │ /
                       │/
   (cx - r, cy) ◄──────┼──────► (cx + r, cy)
                       │
                       │
                       │
                       ▼
                  (cx, cy + r)
For a segment representing percentage 
, the angle span 
 (in degrees) is calculated as:
We track the cumulative angle 
 (starting at 
). To map these points to Cartesian coordinates 
 for the SVG path, the angles are converted from degrees to radians, accounting for a 
 offset to align the starting point with the vertical y-axis:


The path is constructed using the d attribute, utilizing the arc parameter A:
code
Code
M cx cy L x1 y1 A r r 0 LargeArcFlag 1 x2 y2 Z
The LargeArcFlag is set to 1 if 
, and 0 otherwise.
Radar charts map data attributes onto equiangular radial spokes. For a radar web with 
 attributes, the angular offset between spokes is:
For an attribute at index 
 with value 
, its normalized coordinate 
 is projected from the center 
 using:


The plotted coordinates are grouped and rendered as a filled semi-transparent polygon, styled with active CSS drop shadows to simulate digital glow effects.
Section 5: Three Proposed WOW AI Features (Architectural Proposal)
To expand the capabilities of AURA-7, three advanced AI features are proposed below. These systems are designed to operate within the existing full-stack architecture, providing automated dossier validation, safety simulation, and real-time recall tracking.
code
Code
┌────────────────────────────────────────────────────────┐
       │             AURA-7 ENTERPRISE RECIPIENT                │
       └───────────────────────────┬────────────────────────────┘
         ┌─────────────────────────┼───────────────────────────┐
         ▼                         ▼                           ▼
┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
│   WOW AI FEAT 1  │      │   WOW AI FEAT 2  │      │   WOW AI FEAT 3  │
│  eCTD Dossier    │      │  Predictive SAE  │      │   Harmonized     │
│  Valid. Engine   │      │  Early Warning   │      │  Recall Tracker  │
└──────────────────┘      └──────────────────┘      └──────────────────┘
Feature 1: Automated TFDA Dossier Assembler & eCTD Validator (AI-powered eCTD Compiler)
Business Value: Automates the compilation and verification of regulatory dossiers for TFDA "醫療器材查驗登記" submissions. This system flags missing technical documents, non-compliant labeling clauses, and inconsistent sterilization reports, reducing registration approval cycle times.
code
Code
┌────────────────────────────────────────────────────────────────────────┐
│  // DOSSIER ASSEMBLER PORTAL                                           │
├────────────────────────────────────────────────────────────────────────┤
│  Upload Folder (eCTD Format) [ Drag & Drop ZIP File ]                  │
│                                                                        │
│  Analysis Feed:                                                        │
│  [▲] Warning: Section C.4 Clinical Data lacks Biocompatibility PDF.   │
│  [✔] Success: Section B.2 Quality System Certificate QSD1245 verified. │
│  [✖] Error: Section D.1 Labeling claims exceed authorized use case.   │
├────────────────────────────────────────────────────────────────────────┤
│  Status: [ NON-COMPLIANT ]                Readiness Index: 78%         │
└────────────────────────────────────────────────────────────────────────┘
Dossier Ingestion: Users upload zipped dossiers in standard eCTD folder structures.
Multimodal Document Extraction: A background worker parses files (PDFs, TIFF schematics, DOCX protocols). It uses optical character recognition (OCR) and layout detection algorithms to extract structural text and metadata.
Semantic Clause Audit: The backend feeds these sections into the Gemini model, comparing content against the TFDA Medical Device Registration Guidelines (醫療器材查驗登記審查準則).
Inconsistency Mapping: The model checks for discrepancies across the dossier, such as mismatching device dimensions between technical schematics and draft Instructions for Use (IFU) documents.
Dossier Readiness Payload: The endpoint returns a detailed checklist highlighting missing sections, compliance ratings, and corrective actions.
API Route: POST /api/gemini/dossier/validate
Request Contract Schema:
code
JSON
{
  "dossierId": "DOS-2026-9041",
  "targetClass": "Class III",
  "filesMetadata": [
    { "section": "Section A - Application Form", "extractedText": "CardioSentry Active ECG Monitor. Applicant: MedTech Taiwan Ltd..." },
    { "section": "Section B - QSD Document", "extractedText": "Facility holds certificate QSD8841 expiring on 09/15/2028..." },
    { "section": "Section C - Technical Documentation", "extractedText": "Electromagnetic compatibility tested under IEC 60601-1-2 standard. Material composition: ABS casing..." },
    { "section": "Section D - Draft Labeling & IFU", "extractedText": "Instructions for Use: CardioSentry registers, diagnoses, and manages chronic arrhythmias..." }
  ]
}
Response Payload Schema:
code
JSON
{
  "readinessScore": 72,
  "eCtdCompliant": false,
  "validationSummary": "The dossier fails to meet Class III technical specifications because it lacks local clinical trials and contains inconsistent efficacy statements in Section D.",
  "gaps": [
    {
      "section": "Section C - Technical Documentation",
      "severity": "High",
      "issue": "Missing local clinical investigation data for Class III active diagnostic devices.",
      "remedy": "Include local clinical trial logs or present documentation showing clinical evaluation equivalence with an approved predicate."
    },
    {
      "section": "Section D - Draft Labeling & IFU",
      "severity": "Critical",
      "issue": "Clinical claim 'diagnoses and manages chronic arrhythmias' is inconsistent with Section C, which classifies the device as an adjunct monitoring tool.",
      "remedy": "Revise labeling copy to read: 'CardioSentry monitors ECG waveforms; clinical diagnoses must be conducted by certified physicians.'"
    }
  ]
}
Feature 2: Predictive Clinical Trial Adversity & SAE Early-Warning Engine
Business Value: Simulates localized clinical tissue reactions and mechanical failures of implantables before initiating human trials, reducing trial liability and predicting Serious Adverse Events (SAEs).
code
Code
┌────────────────────────────────────────────────────────────────────────┐
│  // CLINICAL PATHWAY SIMULATOR                                         │
├────────────────────────────────────────────────────────────────────────┤
│  Component Material: [ Titanium Alloy Grade 5 / PEEK Spacer ]          │
│  Implanation Location: [ Lumbar Spine L4-L5 Segment ]                  │
│                                                                        │
│  Risk Analysis Output:                                                 │
│  ■ Osteolytic Reaction Probability: 12%                                │
│  ■ Delamination Rate: 3%                                               │
│  ■ 24-Month Inflammation Hazard Rating: HIGH                           │
├────────────────────────────────────────────────────────────────────────┤
│  Regulatory Advice: Run ISO 10993-6 local implantation trials.         │
└────────────────────────────────────────────────────────────────────────┘
Configuration Entry: The engineer inputs the device material composition, physical dimensions, implantation site, bio-durability targets, and patient risk profiles.
Vector Database Retrieval (RAG): The backend queries a vector database containing historical clinical trials, literature, FDA MAUDE safety logs, and global toxicology databases (grounded on standard materials like Nitinol, PEEK, and Cobalt-Chromium).
Synthesis: The Gemini engine matches input specifications with historical clinical performance records to detect potential risk pathways.
Hazard Output Generation: The system calculates predictive probability indexes for localized tissue inflammation, mechanical failure, or osteolytic reactions, mapping findings to ISO 14971 hazard grids.
API Route: POST /api/gemini/clinical/predict-sae
Request Contract Schema:
code
JSON
{
  "deviceConcept": "Vascular Scaffold",
  "biocompatibilityMaterials": ["Nitinol", "Sirolimus-eluting PLGA polymer"],
  "anatomicalLocation": "Coronary Artery",
  "trialDesign": "350 patients, single-blind, randomized against cobalt-chromium drug-eluting stents.",
  "durationMonths": 24
}
Response Payload Schema:
code
JSON
{
  "predictedSaeRate": 4.8,
  "safetyAlertLevel": "Moderate-High",
  "predictedAdverseEvents": [
    {
      "event": "Late Scaffold Thrombosis",
      "probability": 1.2,
      "riskDriver": "Polymer degradation residues from the drug-eluting matrix trigger localized vascular inflammation.",
      "mitigation": "Extend dual antiplatelet therapy (DAPT) protocols to 12 months."
    },
    {
      "event": "In-Stent Restenosis",
      "probability": 3.6,
      "riskDriver": "Localized cellular proliferation due to drug-elution variations along scaffold intersections.",
      "mitigation": "Optimize stent-strut thickness and refine laser-polishing steps to improve drug-layer uniformity."
    }
  ],
  "iso14971RiskMatrix": {
    "probabilityIndex": "Occasional",
    "severityIndex": "Critical",
    "riskAcceptability": "Unacceptable without mitigation"
  }
}
Feature 3: Global Harmonized Recall Tracker & Swarm Coordinator (Real-time IoT Integration)
Business Value: Automatically synchronizes global recalls from international databases (FDA, EU Safety Gate, TFDA) and coordinates containment actions across connected medical hospital networks.
code
Code
┌────────────────────────────────────────────────────────────────────────┐
│  // GLOBAL RECALL STREAM                                               │
├────────────────────────────────────────────────────────────────────────┤
│  Incoming Sync: [ FDA Alert #A-4841 ]                                  │
│  Affected Device: [ HeartLung Bypass Pump Model H-400 ]                │
│                                                                        │
│  Swarm Coordination Output:                                            │
│  - Active units identified in Taiwan: 4 Hospital Centers               │
│  - Automatic Software Lock: Transmitted to firmware v4.11               │
│  - Isolation Directives: Dispatched to biomedical departments          │
├────────────────────────────────────────────────────────────────────────┤
│  Status: [ QUARANTINE SECURED ]                                        │
└────────────────────────────────────────────────────────────────────────┘
Global Aggregation Daemon: A background service monitors international regulatory RSS feeds, TFDA recall portals, and FDA safety streams.
Safety Alert Standardization: When a new hazard notice is parsed, the Gemini engine structures the alert, extracting affected model numbers, lot prefixes, UDI-DI codes, and the severity of clinical risk.
Cross-Database Matching: The system queries hospital enterprise resource planning (ERP) systems to identify matches across local inventory lines and active clinical departments.
Swarm Containment Orchestration: The coordinator automatically creates quarantine files, drafts alert letters tailored to hospital procurement branches, and dispatches remote software-lock protocols to affected devices.
API Route: POST /api/gemini/recalls/swarm-coordinate
Request Contract Schema:
code
JSON
{
  "externalAlertRaw": "SAFETY WARNING: Medtronic cardiac pacemakers, model Azure MRI. Serial numbers starting with AZ-2026. Firmware version 2.1 displays potential telemetry transmission failure, causing high-priority battery exhaustion warnings.",
  "activeHospitalInventory": [
    { "hospitalName": "National Taiwan University Hospital", "deviceId": "AZ-2026-4851", "currentDept": "Cardiology" },
    { "hospitalName": "Taichung Veterans General Hospital", "deviceId": "AZ-2026-9042", "currentDept": "Emergency Medicine" },
    { "hospitalName": "Kaohsiung Chang Gung Memorial Hospital", "deviceId": "AZ-2025-1104", "currentDept": "Cardiovascular ICU" }
  ]
}
Response Payload Schema:
code
JSON
{
  "alertStandardized": {
    "manufacturer": "Medtronic",
    "model": "Azure MRI Pacemaker",
    "riskClassification": "Class I (High Risk)",
    "hazardDescription": "Telemetry firmware loops cause battery depletion within 14 days of error activation."
  },
  "impactAnalysis": {
    "totalMatchesFound": 2,
    "quarantinedDevices": [
      { "deviceId": "AZ-2026-4851", "hospital": "National Taiwan University Hospital", "actionStatus": "Firmware Lock Signal Dispatched" },
      { "deviceId": "AZ-2026-9042", "hospital": "Taichung Veterans General Hospital", "actionStatus": "Firmware Lock Signal Dispatched" }
    ],
    "skippedDevices": [
      { "deviceId": "AZ-2025-1104", "reason": "Serial number mismatch; device unaffected by recall." }
    ]
  },
  "hospitalNotificationDrafts": [
    {
      "recipient": "Biomedical Engineering Dept, NTUH",
      "subject": "CRITICAL CLASS I SAFETY NOTICE: Medtronic Azure MRI Pacemaker Recall Coordination",
      "body": "Biomedical engineering staff are directed to immediately quarantine unit AZ-2026-4851, currently deployed in the Cardiology ward. This device has been updated to safe-state telemetry mode. Please schedule a firmware re-patch as soon as possible."
    }
  ]
}
Section 6: Security, Compliance & Performance Audits
6.1 Data Governance & Privacy Architecture
Medical device data handling requires strict adherence to international and local privacy laws, including the Health Insurance Portability and Accountability Act (HIPAA), European Union GDPR, and the Taiwan Personal Data Protection Act (個人資料保護法).
code
Code
┌────────────────────────────────────────────────────────┐
       │             CLIENT PORTAL (Vite Frontend)              │
       └───────────────────────────┬────────────────────────────┘
                                   │
                           Local Anonymizer
                                   │
       ┌───────────────────────────▼────────────────────────────┐
       │                EXPRESS GATEWAY FIREWALL                │
       │           Regex scrubbing of Patient Records            │
       └───────────────────────────┬────────────────────────────┘
                                   │
                        Encrypted TLS 1.3 Pipe
                                   │
       ┌───────────────────────────▼────────────────────────────┐
       │               GOOGLE GEMINI SECURE REGION              │
       │                Zero-Retention AI Processing            │
       └────────────────────────────────────────────────────────┘
To prevent Protected Health Information (PHI) leakage, the backend implements a real-time data sanitizer. The system runs regex-based validation filters to intercept and scrub personal identifiers—such as names, ID numbers, birthdates, and medical history numbers—before forwarding requests to the LLM backend.
Data in Transit: Secured using end-to-end Transport Layer Security (TLS 1.3), preventing eavesdropping and man-in-the-middle attacks.
Data at Rest: Storage systems use Advanced Encryption Standard (AES-256) to protect regulatory database ledgers.
API Token Security: Sensitive API credentials, including the Gemini API token and OAuth keys, are retrieved from the server environment on demand, keeping them isolated from client processes.
6.2 Build & Deployment Optimization Audits
AURA-7 is designed to compile as a fast, highly optimized application bundle capable of running within isolated, containerized environments.
To optimize frontend performance, the client bundle utilizes code-splitting to lazy-load resource-intensive components. Tailwind CSS classes are parsed and tree-shaken during the build process to minimize the style sheet payload.
The custom Node.js server is bundled using esbuild into a CJS format (dist/server.cjs), applying the --packages=external option to exclude third-party libraries. This architecture offers several advantages:
Rapid Cold-Start Timelines: Consolidating multiple TypeScript files into a single bundle reduces filesystem read operations during server startup, accelerating container cold-starts.
Native ESM Compatibility Bypass: Packing dependencies as CJS avoids Node's ES Module relative-path checks, eliminating runtime dependency resolution errors.
Container Footprint Reductions: Bundled builds require fewer dependencies inside target deployment images, reducing vulnerable dependencies and hardening container filesystems.
Section 7: 20 Comprehensive Follow-Up Questions
To support the expansion and long-term roadmap of the AURA-7 platform, the following questions are designed to guide development discussions across architectural, regulatory, and business tracks:
code
Code
┌────────────────────────────────┐
                            │      AURA-7 PANEL ROADMAP      │
                            └───────────────┬────────────────┘
         ┌──────────────────────────────────┼──────────────────────────────────┐
         ▼                                  ▼                                  ▼
┌──────────────────┐               ┌──────────────────┐               ┌──────────────────┐
│  TRACK A: ARCH.  │               │  TRACK B: REGUL. │               │  TRACK C: BIZ &  │
│  Systems Scale & │               │  Global Auditing │               │  Security Ops    │
│  DB Integrations │               │   Alignments     │               │  Infrastructure  │
└──────────────────┘               └──────────────────┘               └──────────────────┘
Track A: Systems Scaling & Database Integrations
Transition to Cloud Storage: How should the transition from flat-file storage in /dataset.md to a managed database like Cloud Firestore or a relational Postgres engine be managed, particularly during high-concurrency read-write cycles?
High-Frequency Sync Lock: If multiple regulatory engineers are saving updates to dataset.md simultaneously, what state locking protocols should be implemented to prevent write conflicts?
Real-Time Push Triggers: How can we transition the standard polling loop to a real-time WebSocket connection to push safety alerts instantly?
Local Graph Model Fallbacks: If the cloud API connection is lost, can we integrate a local WebAssembly-based engine (e.g., ONNX, Whisper, or LLM-In-Browser) to maintain basic regulatory screening capabilities offline?
Multi-Axis Plotting Scalability: As datasets exceed 10,000 records, how should the coordinate scaling logic in our vector charting engine be modified to handle dense data matrices without degradation?
Incremental AST File Loading: How can we implement progressive loading and chunk-by-chunk indexing to parse massive markdown databases without encountering memory allocation limits?
Track B: Global Regulatory Alignment
Dynamic Legal Knowledge Bases: How can we connect the TFDA Legal Auditor to live RSS feeds from the Food and Drug Administration's official gazette to keep legal references updated automatically?
Multi-Language Mapping Accuracies: What verification frameworks should be introduced to assess the accuracy of biomedical translations across English, Traditional Chinese, and Latin medical terminology?
EU MDR Classification Extensions: How should the classification rules in the Pre-Market Classifier copilot be modified to support Class III active implantable devices under Annex VIII of the EU MDR?
ISO 13485:2016 CAPA Trigger Grounding: Can the QMS Gap Analyst be grounded on real ISO auditing manuals using Retrieval-Augmented Generation (RAG) to reference specific ISO clauses during inspections?
Local Clinical Validation Frameworks: Under Taiwan's Medical Device Management Act, how should the CER Equivalence Evaluator be structured to evaluate local clinical trial requirements for Class III innovative devices?
Software as a Medical Device (SaMD) Updates: How can we modify our labeling audit engine to detect and flag unauthorized updates to AI diagnostic models that would trigger a mandatory re-registration event under TFDA guidelines?
Track C: Operations, Business, and Security Infrastructure
HIPAA-Compliant PHI Scrubbing: What automated unit testing suites should be implemented to verify that patient data scrubbing filters successfully intercept and anonymize health information?
Custom LLM Fine-Tuning: What is the feasibility of fine-tuning a custom Gemini-based model on proprietary historical regulatory filings to improve pre-market timeline and cost projections?
Enterprise Multi-Tenant Isolation: How should the directory structure and access permissions of AURA-7 be designed to keep tenant clinical safety records isolated in multi-tenant environments?
Biomedical Firmware Remote Patching: What encryption standards should be applied when deploying remote software locks to active hospital devices during recalls?
Dossier Upload File Interventions: How will the proposed Dossier Assembler scan for and neutralize security threats like macro-enabled malware in uploaded ZIP files?
Federated Clinical Trial Nodes: To protect patient privacy across institutions, can the Predictive SAE Engine use federated learning models to query remote clinical databases without centralizing health data?
Regulatory Audit Trail Ledgering: Should AURA-7's audit trail be written to a secure, write-once-read-many (WORM) ledger database to create tamper-proof logs for FDA 21 CFR Part 11 compliance audits?
CAPA Lock Down Workflows: How should the system's authentication and authorization matrix be configured to ensure that only certified Quality Assurance managers can release quarantine locks?
Section 8: Summary of Implemented Solutions
The AURA-7 Enterprise Regulatory Safety & Forensic Compliance Platform is configured as a comprehensive compliance cockpit designed to streamline medical device regulatory workflows:
Design Framework: Uses the Cosmic Slate / Calm Twilight visual theme (#121212 canvas), utilizing responsive layouts, clean typography (Inter paired with JetBrains Mono), and structured content cards to minimize screen glare during long auditing sessions.
Data Persistence: Leverages a file-based Markdown database system targeting /dataset.md. An AST parsing pipeline converts clinical recalls, registrations, TUDID UDI entries, WHO nomenclature codes, and QMS certificates into structured JSON arrays, keeping data clean and readable.
Server-Side Intelligence Pipelines: Features a Node.js Express server that proxy-routes requests to the Google Gemini AI engine. Secure API endpoints process unstructured regulatory data without exposing credentials to client processes.
Python Analytics Viewport: Embeds a terminal environment that generates matplotlib scripts, paired with a custom frontend SVG engine that translates chart configurations into clean vector graphics.
Interactive Copilots & TFDA Auditor: Integrates six copilots and a Taiwan-focused Legal Auditor to automate pathway classification, compare Clinical Evaluation Reports (CER), audit IFU labels, predict supply-chain recall risks, and analyze QMS gaps under Taiwan's Medical Device Management Act.

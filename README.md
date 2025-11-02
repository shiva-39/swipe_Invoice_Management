# 🧾 Swipe – AI-Powered Invoice Extraction System

This repository contains a robust, fully functional **single-page React application** developed for **Swipe**.  
Its core purpose is to **automate the extraction, secure processing, and real-time management of structured invoice data** derived from diverse, unstructured file formats, including **PDFs, Images, and raw tabular data (Excel/CSV)**.

---

## 💡 Project Highlights & Core Requirements Fulfillment

This solution is engineered to meet all technical requirements of the assignment, emphasizing **predictable state changes** and a **generic, multimodal AI pipeline**.

| **Requirement** | **Implementation Detail** | **Status** |
|------------------|---------------------------|-------------|
| **Centralized State (Redux)** | The application relies on a robust Redux-like state architecture, built using React's `useReducer` and Context API. This enforces the *single source of truth* principle for managing complex datasets. | ✅ COMPLETED |
| **Real-Time Sync** | Instant synchronization is implemented within the state reducer. Any manual edit triggers a synchronous update across all associated invoices, maintaining data integrity. | ✅ COMPLETED |
| **AI Extraction (Generic & Multimodal)** | Leverages the **Gemini 2.5 Flash API** to process mixed input types (PDF/Image → Base64 + text) in a unified API call for efficient, generalized parsing. | ✅ COMPLETED |
| **Data Validation** | Extracted data is validated for completeness. Missing fields (`serialNumber`, `totalAmount`, etc.) are visually flagged as **MISSING DATA** in red. | ✅ COMPLETED |
| **Aesthetics & Code** | Styled exclusively with pure CSS (no frameworks). Bundled as a single `App.jsx` file for minimal build configuration. | ✅ COMPLETED |

---

## 🧠 2. Deep Dive: AI Data Extraction Feature Documentation

The automation of invoice parsing relies on **advanced structured output generation** from the Gemini API, converting chaotic document layouts into clean, reliable JSON.

### ⚙️ Architecture – `runExtraction` Function

The `runExtraction` function orchestrates the entire AI pipeline. Its success depends on precise **prompt engineering** and the **structured output mechanism**.

#### 🔸 Multimodal Input Pipeline
- The frontend converts input files (PDFs, images) into Base64.
- Tabular (Excel/CSV) data is sent as text.
- Gemini’s multimodal reasoning analyzes all input types in a single prompt.

#### 🔸 Strict System Instruction
- The model is given a persona: `"expert financial data extraction API"`.
- It enforces strict consolidation and schema adherence.
- Reduces hallucination and improves accuracy.

#### 🔸 Fixed JSON Schema (`FINAL_SCHEMA`)
- Defines the exact structure and types of the output.
- Includes required properties: `invoices`, `products`, `customers`.
- Ensures model output is directly usable in the React app.

#### 🔸 Handling Missing Data
- If required data (e.g., `date`, `tax rate`) is missing, Gemini returns `""` or `null`.
- The UI highlights these as **MISSING DATA**, prompting manual review.

#### 🔸 Security Refactor (Production Readiness)
⚠️ The **development build** uses the Gemini API key in client-side JS — a **security risk**.  
✅ The **production build** must use a **Serverless Proxy Architecture**:
- React client sends data to `/api/extract`.
- Proxy retrieves `GEMINI_API_KEY` from private environment variables.
- Makes the authenticated API call and returns only JSON results.
- Keeps API keys secure from public exposure.

---

## 🧩 3. Code Quality and Modularity (Requirement 5)

The code follows modern React principles while meeting the single-file constraint.

### 🧠 Internal State Structure (Redux-like Pattern)

- **Core Principle:** Managed by a top-level `AppProvider` using `useReducer`.
- **Immutability:** State updates via structured `action` objects only.
- **Synchronization:** `UPDATE_ENTITY` ensures changes in one entity (e.g., Customer Name) propagate instantly to all invoices.

### 🔧 Custom Hooks
- `useAppState` and `useAppDispatch` provide state access, mirroring Redux’s `useSelector` and `useDispatch`.

---

## 🏗️ Future Modularity Plan: Decoupling for Scale

| **Module** | **Purpose** |
|-------------|-------------|
| `src/context/AppStore.js` | Manages global state, reducer logic, and business rules (e.g., total calculation). |
| `src/services/api.js` | Contains `runExtraction`, `FINAL_SCHEMA`, and proxy call logic (Gemini API integration). |
| `src/components/` | Contains UI components like `<DataTable.jsx>`, `<Tabs.jsx>`, and `<EditForms.jsx>` for modularity and reusability. |

---

## ✅ 4. Test Cases Solved and Evidence

The system handles **five major test categories** by combining multimodal and text input methods.

| **Test Case** | **AI Input Method** | **Expected Result & Validation** | **Evidence** |
|----------------|---------------------|----------------------------------|---------------|
| Case-1: Invoice PDFs | Multimodal (PDF → Base64) | Successful structured extraction. | `docs/evidence/screenshots/tc1-extraction-success.png` |
| Case-2: PDF + Images | Multimodal (PDF + Image) | Consolidated unified JSON output. | `docs/evidence/videos/tc2-multimodal-run.mp4` |
| Case-3: Excel File | Text/CSV Input | Proper tabular header-row mapping. | `docs/evidence/screenshots/tc3-excel-paste.png` |
| Case-4: Missing Fields | Multimodal | Missing fields return `null`, highlighted as MISSING DATA. | `docs/evidence/screenshots/tc4-missing-data-highlight.png` |
| Case-5: All Types | Combined Inputs | End-to-end validation of data sync and performance. | `docs/evidence/videos/tc5-end-to-end-sync.mp4` |

📂 **Evidence Directory Structure**

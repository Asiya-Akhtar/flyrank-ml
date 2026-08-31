# The Plan to Keep Building: Portfolio Growth & Next Case Study

## 1. How to Add the Next Case Study ("Three-Beat Shape" SOP)

### File Location:
- **Case Study Markdown Document:** `docs/case_studies/case_02_<project_slug>.md`
- **Interactive Web App & Portfolio Hub Entry:** `src/data/portfolioProjects.ts`
- **Repo Code / Artifacts:** Dedicated folder under `work/projects/<project_slug>/`

### Three-Beat Shape Structure:
1. **Beat 1: The Problem (Context & Friction)**
   - Define the explicit business or operational bottleneck (e.g., query latency, missing triage prioritization, low conversion on specific landing pages).
   - Document baseline metrics before intervention (e.g., time spent per audit, baseline error rate).
2. **Beat 2: What You Did (Engineering & ML Implementation)**
   - Dataset & feature engineering rationale.
   - Baseline comparison vs. model architectures (heuristic vs. ML models).
   - Evaluation metric justification (e.g., Precision@K, NDCG, latency ceiling).
3. **Beat 3: What Came of It (Outcome & Human Decision Support)**
   - Measured uplift and efficiency gain (e.g., "3.08x lift in prioritization precision").
   - Transparent artifact output (e.g., prioritized queue, decision-support dashboard).
   - Clear limitation and boundary acknowledgment.

---

## 2. Named Next Piece of Work & Scheduled Reminder

- **Project Name:** *Internal Search Query Semantic Reranker & Zero-Result Recovery Engine*
- **Scope & Objective:** Build a lightweight cross-encoder / bi-encoder reranking pipeline to eliminate zero-result search queries on large knowledge bases by mapping user intent to existing high-relevance documentation.
- **Target Completion Date:** September 28, 2026
- **Scheduled Reminder Evidence:**
  - **Calendar Event:** "Quarterly Portfolio Refresh — Implement Case Study #2 (Search Reranker)" scheduled on Google Calendar for **Friday, September 18, 2026 at 10:00 AM PST**.
  - **Recurring Reminder Cadence:** Bi-weekly check-in via task management note.

---

## 3. Preserving Build Context (Claude Project & AI Studio Assistant)

- **Identity & Voice Kit:** Preserved in repository prompt guidelines (`AGENTS.md` / `GUIDE.md`) with the established tone (clear, outcome-focused, zero promotional fluff, high technical rigor).
- **Stack & Reusability:**
  - Standardized pipeline runner: `scripts/run_all.py`
  - Reusable evaluation harness: Precision@K and confusion matrix benchmarking
  - Modern TypeScript + Vite + Tailwind dashboard components for real-time visual inspection
- **Outcome:** Creating future case studies requires only feeding the raw dataset and running the standardized 3-beat template conversation rather than re-architecting the portfolio infrastructure.

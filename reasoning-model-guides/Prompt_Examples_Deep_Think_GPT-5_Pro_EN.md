# Ready-to-Use Prompts for Gemini 2.5 Pro Deep Think & GPT-5 Pro — Similarities & Differences

Each section presents **🧠 Deep Think** and **⚙️ GPT-5 Pro** versions side by side, followed by **Similarities** and **Key Differences**. Replace `{...}` placeholders with your specific content.

---

## 1) Mathematics (Proof Problems)

### 🧠 Deep Think Version (Copy-Paste Ready)
```
Proposition: "{statement here}"
Goal: Complete proof and key insight extraction
Instructions:
- Internally evaluate {induction, contradiction, construction} in parallel, converging to the most elegant approach.
- Exclude thinking processes from output.
Output:
1) Complete proof (rigorous natural language)
2) 3 key insights
3) Used lemmas/known theorems (names only)
```

### ⚙️ GPT-5 Pro Version (Copy-Paste Ready)
```
Proposition: "{statement here}"
Instructions:
- Internally evaluate {induction/contradiction/construction} in parallel → output best approach only.
- Structure output as JSON for machine readability.
Output (JSON):
{
  "theorem": "{proposition}",
  "proof": "{complete proof (rigorous and concise)}",
  "key_ideas": ["{insight1}", "{insight2}", "{insight3}"],
  "assumptions": ["{axiom/lemma names}"],
  "gaps_to_check": ["{any details needing verification}"]
}
```

**Similarities**
- Parallel → convergent evaluation (multiple approaches internally → output best only)
- Two-layer structure: proof body + key insights

**Key Differences**
- Deep Think: **Human-readable** text format
- GPT-5 Pro: **JSON structure** for verification and tool integration

---

## 2) Algorithm Design (Design → Implementation → Evaluation)

### 🧠 Deep Think Version (Copy-Paste Ready)
```
Challenge: {problem statement}
Instructions:
- Internally compare {greedy, dynamic programming, graph/data structure} approaches in parallel.
- Adopt best approach only, exclude thinking processes from output.
Output:
1) Pseudocode
2) Complexity analysis (time/space, worst-case and average)
3) 10 representative test cases (including edge cases and worst scenarios)
4) Anticipated failure patterns and mitigations (one line each)
```

### ⚙️ GPT-5 Pro Version (Copy-Paste Ready)
```
Challenge: {problem statement}
Constraints: {language/standard library only, etc.}
Instructions:
- Internally evaluate multiple approaches → converge to best one.
- Prioritize "machine-readable + reproducibility" in output.
Output:
1) Final code (language: {X}) only
2) Unit tests (10 cases) and execution commands
3) metrics.json (complexity/input size estimates) schema:
   { "time_complexity": "...", "space_complexity": "...", "notes": "..." }
4) README.md summary (usage/constraints/known pitfalls) — brief bullets
```

**Similarities**
- Evaluate multiple solutions → present best approach only
- Quality assurance through complexity analysis and testing

**Key Differences**
- Deep Think: **Pseudocode-focused** for design communication
- GPT-5 Pro: **Production-ready code + tests + metrics JSON**

---

## 3) Strategic Planning (Market Entry/Product Planning)

### 🧠 Deep Think Version (Copy-Paste Ready)
```
Context: {product/market/budget/timeline}
Instructions:
- Internally evaluate 3 independent strategies A/B/C → output optimal choice only.
Output:
1) Comparison table (KPIs/costs/risks/implementation difficulty)
2) Selection rationale (200 words max)
3) 90-day roadmap (milestones/owners/risk mitigation)
4) Assumptions table (unknowns and provisional values)
```

### ⚙️ GPT-5 Pro Version (Copy-Paste Ready)
```
Context: {product/market/budget/timeline}
Objective: {KPIs}
Instructions:
- Internally evaluate multiple strategies → choose most promising one.
- Output structured + execution-focused.
Output (JSON):
{
  "chosen_strategy": "{name}",
  "why": "{selection rationale (200 words max)}",
  "kpi_targets": {"M1":"{value}", "M2":"{value}"},
  "work_plan": [
    {"wk":1, "tasks":["..."], "owner":"...", "risk":"...", "mitigation":"..."},
    {"wk":2, ...}
  ],
  "budget_split": {"ads":..., "sales":..., "ops":...},
  "abort_criteria": ["{stop criterion 1}", "{2}"],
  "assumptions": ["{key assumption}"]
}
```

**Similarities**
- Compare → converge workflow
- KPI specification and assumption disclosure

**Key Differences**
- Deep Think: **Decision document** format (tables/text)
- GPT-5 Pro: **JSON format** with detailed stop criteria, owners, weekly tasks (dashboard-ready)

---

## 4) Data Analysis & A/B Test Design

### 🧠 Deep Think Version (Copy-Paste Ready)
```
Data: {CSV/metrics description}
Objective: {decision-making purpose}
Instructions:
- Internally evaluate 3 causal hypotheses in parallel → converge to 1.
Output:
1) 3 insights (3 lines each)
2) Primary hypothesis and falsifiability
3) A/B test plan (metrics/minimum detectable effect/MDE/duration/sample size estimate)
4) Next 3 actions to take
```

### ⚙️ GPT-5 Pro Version (Copy-Paste Ready)
```
Data: {schema/sample}
Objective: {decision-making purpose}
Instructions:
- Prioritize reproducibility and verification ease in output.
Output:
1) Experiment plan as JSON:
   {"metric_primary":"...", "metrics_secondary":["..."], "MDE":"...", "power":0.8, "duration_days":..., "sample_size":...}
2) Aggregation SQL (BigQuery dialect) and column definitions
3) Analysis procedure (preprocessing → exclusion criteria → statistical tests) as step-by-step bullets
4) Risk log (confounders/seasonality/implementation pitfalls) — one line each
```

**Similarities**
- Decision-driven flow: hypothesis → validation → action
- Explicit falsifiability and risk disclosure

**Key Differences**
- Deep Think: **Condensed summary** format
- GPT-5 Pro: **Executable JSON + SQL + procedures**

---

## 5) Code Review (Bug Detection / Single Text Input)

### 🧠 Deep Think Version (Copy-Paste Ready)
```
You are an elite software quality engineer and security reviewer.
Objective: Systematically identify latent bugs from "project structure + all source code" in single text input, providing fix recommendations.

Input (paste entire project in PROJECT block below):
<PROJECT>
{Paste "folder/file structure" and "complete source of each file" here}
</PROJECT>

Approach and tasks:
- Internally evaluate multiple perspectives (correctness/security/concurrency/error-handling/performance/resource-leaks/portability/i18n/API-misuse/dependencies) in parallel, output optimal findings only. Exclude thinking processes.
- Infer file boundaries from file names, tree notation, ```lang ...``` fences, "BEGIN/END FILE" markers, etc.
- Attach file_path and line numbers (start_line–end_line, relative to each file start) when possible.
- Consolidate duplicates from same root cause into single item with affected locations list.

Output (this order, this format):
1) Overview (50–120 chars): Main risk areas and anticipated impact
2) Severity definitions (example):
   - **Critical**: Production outage, data breach, arbitrary code execution requiring immediate response. High reproducibility with **no workarounds**, or major legal/compliance risks.
   - **High**: Core function failures, serious security flaws (auth/authz gaps, injection). **Temporary workarounds available** but release-blocking.
   - **Medium**: Limited condition failures/data inconsistency/notable performance regression/error handling gaps. **Workarounds available** for continued operation. Fix in next release.
   - **Low**: Minor behavior/safety issues (logging gaps, deprecated APIs, inappropriate monitoring thresholds). **Appearance/style-only** excluded.
3) Top N critical findings (N≤{10})
   - id: BUG-xxx
   - severity: {Critical|High|Medium|Low}
   - category: {correctness|security|concurrency|performance|reliability|portability|i18n|api-misuse|dependency|other}
   - file_path: relative path
   - lines: L{start}-L{end}
   - finding: What's wrong (1–2 sentences)
   - evidence: Minimal code excerpt (3–12 lines)
   - why: Why it's buggy/vulnerable (≤3 key points)
   - repro: Reproduction steps or failure scenario (concise/pseudo-input OK)
   - fix: Minimal fix approach (1–3 sentences)
   - diff: Unified diff if possible (relevant hunks only)
   - tests: Tests to add (boundary/worst-case, one line each)
4) Affected locations list (only when duplicates consolidated)
5) Early remediation priorities (bulleted with rationale)

Constraints:
- No full input re-display. Minimal excerpts only.
- Mark speculation-based findings as "assumption: …"
- Exclude style-only topics (prioritize behavior/safety impact).
- Keep technical terms as-is.
```

### ⚙️ GPT-5 Pro Version (Copy-Paste Ready)
````
You are a bug detection engineer. Detect latent bugs from single text "project structure + all source code" and return as structured JSON with minimal diffs.
No thinking processes in output. No verbose explanations.

Input (paste in PROJECT block below):
<PROJECT>
{Paste "folder/file structure" and "complete source of each file" here}
</PROJECT>

Requirements:
- Analysis perspectives: correctness, security, concurrency, error-handling, performance, resource-leak, portability, i18n, api-misuse, dependency
- Consolidate same-pattern issues into 1 item, listing all affected locations in `locations[]`
- Line numbers relative within files as `start_line` / `end_line`
- Return strictly compliant to following JSON schema (**JSON ONLY, no other output**)

JSON Schema (fixed key names/order):
{
  "summary": {
    "highlights": ["Main risk areas, max 3 points, each ≤120 chars"],
    "stats": {"total_findings": 0, "critical": 0, "high": 0, "medium": 0, "low": 0}
  },
  "severity_definition": {
    "Critical": "Production outage, data breach, arbitrary code execution requiring immediate response. High reproducibility with no workarounds, or major legal/compliance risks.",
    "High": "Core function failures or serious security flaws (auth/authz gaps, injection). Temporary workarounds available but release-blocking.",
    "Medium": "Limited condition failures, data inconsistency, notable performance regression, error handling gaps. Workarounds available for continued operation (fix in next release).",
    "Low": "Minor behavior/safety issues (logging gaps, deprecated APIs, inappropriate monitoring thresholds). Appearance/style-only excluded."
  },
  "findings": [
    {
      "id": "BUG-0001",
      "severity": "High",
      "category": "security",
      "title": "Brief summary (≤80 chars)",
      "pattern": "Signature or rule for re-detection (if applicable)",
      "locations": [
        {"file_path": "path/to/file1.ext", "start_line": 120, "end_line": 128},
        {"file_path": "path/to/file2.ext", "start_line": 77, "end_line": 82}
      ],
      "evidence_snippet": "Minimal problem excerpt (max 12 lines)",
      "explanation": "Why problematic (2–4 sentences)",
      "fix_hint": "Minimal fix approach (1–3 sentences)",
      "diff": "```diff\n--- a/path/to/file1.ext\n+++ b/path/to/file1.ext\n@@ -120,7 +120,7 @@\n- vulnerable code\n+ fixed code\n```",
      "tests_to_add": ["boundary/worst/regression tests (one line each)"],
      "confidence": 0.0
    }
  ]
}

Constraints:
- **JSON ONLY output** (no preamble/postamble/markdown)
- `diff` only for feasible cases. If multiple files, 1–3 representative hunks
- `confidence` 0.0–1.0
- Exclude minor style findings
````

**Similarities**
- Parallel analysis → duplicate consolidation → priority ordering
- File paths, line numbers, minimal excerpts for evidence and reproducibility
- Hidden thinking processes, conclusions and evidence only

**Key Differences**
- Deep Think: **Audit report format** (overview → definitions → top N → priorities) for human decision-making
- GPT-5 Pro: **Strict JSON + diff** for automated ticketing/CI integration
- Deep Think: Emphasizes "why/how to fix" in readable text
- GPT-5 Pro: Prioritizes schema compliance and minimal verbosity

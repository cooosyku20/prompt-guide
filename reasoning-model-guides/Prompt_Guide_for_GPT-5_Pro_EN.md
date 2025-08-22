# Ready-to-Use Prompts for GPT-5 Pro

> **Universal Instruction** (use as prefix for maximum effectiveness):
> "**Internally evaluate multiple options** and **reflect only in final output**. **Include key rationale only**. **Exclude all thinking processes**."

## A. Strategy & Decision Making

1) **Market Entry Strategy (with 90-day execution plan)**  
"You are a business strategy advisor. Given `{product/market/budget}`, **internally evaluate 3 options (A/B/C) in parallel** → **converge to optimal choice**. Output: 1) **Comparison table** (KPIs/costs/risks/implementation difficulty) 2) **Selection rationale (200 words)** 3) **90-day roadmap (milestones/owners/risk mitigation)**. Document uncertain assumptions in **assumptions table**."

2) **Negotiation Scenario Design (with stop criteria)**  
"Estimate counterpart's BATNA/interests. **Internally evaluate 3 approaches (hardball/collaborative/phased)**. Output **most promising approach's conversation script (opening→agreement)** and **concession stop criteria** only."

## B. Coding & Agent-like Tasks

3) **Repository Refactoring (agent design → implementation diff)**  
"Challenge: `{issue/requirements}`. **Internally plan steps** (identify parallelizable jobs). **Final output**:  
- **Tool invocation plan** (sequential/parallel, retry policies)  
- `diff` format changes  
- **Validation checklist** (tests/static analysis/build)  
For long procedures: **one-line progress summaries only** (no thinking processes)."

4) **Frontend One-Shot Generation (aesthetics-focused)**  
"Target: `{persona}`, conversion goal: `{metrics}`. **Internally evaluate UI composition/copy/colors**. **Output**: minimal **React/Next** implementation + **accessibility requirements** + **lightweight styling**. Include improvement TODOs targeting Lighthouse 90+."

5) **Performance Optimization (evidence-based)**  
"**Internally analyze** bottlenecks in file `{path}`. **Final output**: `diff`, **performance metrics** (p50/p95/memory), **rollback procedure** only."

6) **Long Context QA (up to 400K tokens)**  
"Review attached materials `{...}`. **Internally search for relevant evidence sections**. **Final output**: answer + bulleted evidence citations (source/chapter/paragraph) only."

## C. Research & Analysis

7) **Evidence Summary → Recommendations**  
"Read materials `{N items}`. **Internally explore hypothesis space in parallel**. **Final output**: 1) **Key findings (3 lines each)** 2) **Falsifiability** 3) **Additional data requirements** 4) **Action recommendations (impact/cost/risk)**."

8) **Data Analysis Brief (decision-ready)**  
"Analyze CSV `{...}`. **Final output**: 3 insights + decision proposal + validation A/B design + next 3 actions. Present data in **Markdown tables**."

## D. Mathematical & Optimization

9) **Optimization Problems (solution/approximation + guarantees)**  
"Objective: `min f(x)`, constraints: `{...}`. **Internally try solution candidates (convex/heuristic/DP)**. **Output**: selected method rationale + solution or approximation + error bounds."

10) **Proof Problems (complete form only)**  
"Proposition: `{statement}`. **Internally consider approaches (induction/contradiction/construction)**. **Final output**: complete proof and 3 key insights."

## E. Documentation & Creative Work

11) **PRD One-Shot Generation (diverge → converge)**  
"Theme: `{...}`. **Internally diverge to 7 concepts** → **converge to 1**. **Final output**: PRD (user stories/non-functional requirements/acceptance criteria/risks & mitigations)."

12) **Long-Form Writing (verbosity control)**  
"Audience: `{...}`, purpose: `{...}`. **Internally evaluate multiple structural approaches**. **Output**: section headers + content. **Verbosity level: medium**."

---

## Reusable Template (Copy-Paste Ready)

```
You are a {role}. Your objective: {goal}.
Constraints: {timeline/quality/cost/standards...}
Evaluation criteria: {KPIs, risks, implementation difficulty...}
Instructions: Internally evaluate multiple options in parallel → output only the optimal solution. No thinking processes.
Output format: {table/bullets/code/diff/JSON...}
Input: `{materials/data/assumptions}`
```
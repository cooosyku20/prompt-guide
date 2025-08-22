# Ready-to-Use Prompts for Gemini 2.5 Pro Deep Think

> **Pro Tip**: Clearly specify **objectives, constraints, evaluation criteria, and output format**. Include instructions to "*internally evaluate multiple options → output only the optimal solution*" to maximize parallel thinking capabilities.

## Strategic Planning

1) **Market Entry Strategy (Convergent)**  
"You are a business strategy advisor. **Internally evaluate 3 independent strategies (A/B/C)**, comparing KPIs, costs, risks, and implementation difficulty. **Output only the optimal choice** with a 200-word rationale and **90-day execution roadmap**. Context: `{product/budget/market}`. Document unknowns in an **assumptions table**. **Output = comparison table + recommendation + roadmap**."

2) **Product Planning Prioritization**  
"Given OKRs and resources `{headcount, budget, timeline}`, **internally consider multiple prioritization approaches**. **Final output**: impact×effort matrix, quarterly roadmap, and **deliberate exclusions list**."

3) **Negotiation Scenario Design**  
"Estimate counterpart's BATNA and interests. **Internally evaluate 3 negotiation approaches** (hardball/collaborative/phased). **Output only** the most promising approach with **conversation flow (opening→agreement)** and **concession stop criteria**."

## Coding & Algorithms

4) **Algorithm Design → Implementation**  
"Challenge: `{problem statement}`. **Internally compare multiple approaches** (data structures/complexity). Output **best solution only**:  
- Pseudocode → Implementation (language: `{X}`)  
- **Edge/worst-case testing (10 scenarios)**  
- Complexity and memory estimates  
External library policy: `{policy}`."

5) **Performance Optimization (Diff Output)**  
"**Internally analyze** bottlenecks in file `{path}`. **Final output**: `diff` format changes + **rationale (metrics/Big-O)** + rollback procedure."

6) **Database Optimization**  
"**Internally assess** SQL bottlenecks. **Output**: index design, query rewrites, and expected improvement ranges (p50/p95 latency)."

## Scientific Analysis & Research

7) **Paper Review (Rapid Consensus)**  
"Read abstracts from `{N papers}`. **Internally explore hypothesis space in parallel**. **Final output**: key contributions summary (3 lines each), **method comparison table**, reproducibility risks, **next experiment plan (materials/conditions/success criteria)**."

8) **Business Data Causal Analysis**  
"From CSV metrics `{...}`, **internally consider 3 causal hypotheses** and **converge to 1**. **Final output**: chosen hypothesis with validation design (A/A testing, power analysis, confounders, exclusion criteria)."

## Mathematical & Logical Reasoning

9) **Proof Problems**  
"Proposition: `{statement}`. **Internally evaluate approaches in parallel** (induction/contradiction/construction). **Final output**: complete proof and 3 key insights."

10) **Optimization Problems**  
"Objective: `min f(x)`, constraints: `{...}`. **Internally try solution candidates** (convex optimization/heuristics/dynamic programming). **Output**: key derivation steps + solution/approximation + error bounds."

## Creative Planning (Diverge → Converge)

11) **Idea Generation → Specification**  
"Theme: `{...}`. **Internally explore 7 bold concepts**. **Final output**: 1 concept as PRD (user stories, non-functional requirements, acceptance criteria, risks/mitigations)."

12) **UX Improvement Iteration**  
"Flow: `{URL/description}`. **Internally iterate through 3 rounds of incremental improvements**. **Output**: Round 3 UI diff list + A/B test hypothesis."

---

## Reusable Template (Copy-Paste Ready)

```
You are a {role}. Your objective: {goal}.
Constraints: {timeline/quality/cost/standards...}
Evaluation criteria: {KPIs, risks, implementation difficulty...}
Instructions: Internally evaluate multiple approaches in parallel, output only the optimal solution.
Output format: {table/bullets/code/diff/JSON schema...}
Input: `{materials/data}`
```

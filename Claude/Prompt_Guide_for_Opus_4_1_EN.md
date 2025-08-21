# Claude Opus 4.1 Prompt Guide

This document provides prompt design best practices optimized for the Claude 4 series, specifically **Opus 4.1**.
- **Key differences and effective prompt writing strategies**
- **Complete example demonstrating 6 essential techniques** (*Implementation note: Prioritize **JSON schemas + tools**. XML serves as a human-readable alternative*)
- **Operational considerations: Token management, context windows, and caching**

---

## 1. Key Differences and Effective Prompt Strategies

### (1) Explicitness Is Now Critical

**Key Point**: The 4 series excels at precise instruction following but won't infer vague expectations like "make it rich" or "make it nice."

**Best Practices**
- Define **specifications, granularity, and success metrics** explicitly (e.g., accuracy > reproducibility > conciseness)
- Clearly state what to include/exclude (e.g., "**exclude** reasoning process from output")
- Handle uncertainties with **explicit assumptions** to constrain scope and enable progress with provisional conclusions

---

### (2) Extended Thinking Happens Internally

**Key Point**: Deep thinking occurs internally by default. Assuming **thinking details won't appear in final output** ensures better reproducibility.

**Best Practices**
- Add to your **system prompt**: "Extended Thinking is internal; exclude thinking details from output"
- **Budget guidance**: Start `thinking.budget_tokens` at **1,024–4,096** tokens (adjust per use case)
- For thinking visualization needs, **enable via UI/API settings** while maintaining **hidden-by-default** in prompts

---

### (3) Enable Parallel Tool Execution

**Key Point**: Process independent tasks **simultaneously** for speed and reliability.

**Best Practices**
- Use this standard instruction: **"When handling multiple independent tasks, invoke tools simultaneously rather than sequentially"**
- **Explicitly list** parallelizable operations (e.g., fetching pricing pages, extracting reviews, collecting news headlines)

---

### (4) Define Cleanup for Agent-Based Coding

**Key Point**: Prevent accumulation of temporary files and orphaned processes.

**Best Practices**
- Distinguish between **persistent storage** and **cleanup targets** (e.g., preserve `/workspace/`, remove `/tmp/`)
- Request a **final inventory of saved artifacts**

---

### (5) Structure Output with JSON Schemas (Primary) or XML (Fallback)

**Key Point**: Structured output specifications dramatically improve reproducibility.

**Best Practices**
- *Primary approach*: Create **mock tools with JSON schemas** (e.g., `final_report`) and enforce strict `input_schema` compliance
- Leverage **tool_choice forcing** and **prefill techniques** to ensure valid JSON output
- Reserve **XML** as a **fallback format** for human-readable documentation

---

### (6) Process Long Context Effectively

**Key Point**: Use "documents-first" ordering with clear tagging and an explicit "extract → interpret → answer" workflow.

**Best Practices**
- Position `<documents>` at the beginning, assigning unique `doc_id` tags
- Define the workflow explicitly: **"Extract relevant quotes → Interpret findings → Formulate answer"**

---

## 2. Complete Example: All 6 Techniques Combined (Claude Opus 4.1)
> **Implementation note**: Prefer JSON-based tools. The XML format below serves as a human-readable alternative.

```
<!-- Claude Opus 4.1 Complete Prompt Example (system + user sections) -->
<!-- Comments map to techniques (1)–(6) described above -->

<system>
  <!-- (1) Explicitness: Clarify role/purpose/evaluation criteria -->
  You are a B2B SaaS product strategist and technical writer.
  Purpose: Compare pricing, features, and adoption barriers across 3 competitors to create an executive decision memo.
  Success criteria (priority order): Accuracy > Reproducibility > Conciseness. Provide minimum 5 evidence points. Label speculation as "unconfirmed".

  <!-- (3) Parallel tools: Encourage simultaneous execution of independent tasks -->
  When handling multiple independent tasks, invoke tools **simultaneously** rather than sequentially.
  (Examples: fetching pricing pages, gathering review scores, extracting news headlines).

  <!-- (2) Extended Thinking: Think internally. Do not include thinking details in output -->
  Perform Extended Thinking **internally** and **exclude** observation→reflection→planning details from final output.
  Guidelines: Initialize thinking.budget_tokens at 1,024–4,096 and adjust as needed.
  Adapt your approach dynamically; review tool results before determining next steps.

  <!-- (4) Cleanup: Clean up agentic coding -->
  Clean up temporary files and terminate processes upon completion.
  Save deliverables (e.g., final CSV or charts) to /workspace/ and provide a summary of saved artifacts.

  <!-- (5) Format control: XML as alternative. For implementation, prioritize JSON tools + schema compliance -->
  The following XML schema is acceptable for **human review**. For production, prioritize **JSON tools** with strict input_schema compliance.
  <output>
    <analysis/>          <!-- Summary (150–200 chars) -->
    <evidence/>          <!-- List direct quote blocks. Include doc_id and line numbers where available -->
    <comparison_table/>  <!-- Embed CSV. Headers: Vendor, Plan, Price, KeyFeatures, Notes -->
    <answer/>            <!-- Conclusion (3 bullet points to support executive decision-making) -->
    <risks/>             <!-- Conditions that could invalidate assumptions (max 5) -->
    <next_actions/>      <!-- Action items (with owner/deadline) -->
  </output>

  <!-- (6) Long context handling: Materials → evidence extraction → answer order -->
  Workflow: Scan <documents> → Extract relevant quotes to <evidence> (with doc_id and line numbers) → 
  Interpret in <analysis> → Generate <answer>.
  Note: Exclude internal thinking details (observation/reflection/planning) from output.

  <!-- Safety tone -->
  Adhere to safety policies. Severe violations may result in conversation termination.
</system>

<user>
  <!-- (1) Explicitness: Deliverable specs/constraints/evaluation criteria -->
  Goal: Executive briefing memo (10-minute presentation). Tone: Neutral, concise.
  Length: 800–1,000 words. Minimum 5 evidence points, prioritizing recent data. Flag unconfirmed information.
  (Note: Respect **200K input / 32K output limits**. Condense verbose sources within Extended Thinking budget.)
  Restrictions: No definitive speculation, no citations from anonymous forums alone.

  <!-- (3) Parallel tools: Specify what to parallelize -->
  Parallel operations: Fetch company pricing pages, extract review site ratings, gather news headlines (past 30 days, deduplicated).

  <!-- Tool availability and I/O contracts -->
  Available tools: web_search, code_exec(python), filesystem.
  Note: **Align with your environment's tool definitions and I/O specifications** (names and parameters vary by platform).

  <!-- XML schema recap (for human review. Prioritize JSON tools when available) -->
  <output>
    <analysis/>
    <evidence/>
    <comparison_table/>
    <answer/>
    <risks/>
    <next_actions/>
  </output>

  <!-- (6) Reference materials: Add doc_id and place upfront -->
  <documents>
    <doc id="A" title="Pricing Pages (Excerpt)">
      1: Vendor X — Team: $19 / Pro: $39 / Enterprise: Contact us
      2: Vendor Y — Starter: $15 / Growth: $35 / Enterprise: Contact us
      3: Vendor Z — Free: $0 / Pro: $29 / Biz: $59
    </doc>
    <doc id="B" title="Review Sites (Average Scores)">
      1: X: 4.4/5 (n=1,230), Y: 4.2/5 (n=980), Z: 4.1/5 (n=2,104)
    </doc>
    <doc id="C" title="News Headlines (Excerpt)">
      1: 2025-07-30: Vendor Y announces new pricing structure (enhanced usage-based billing)…
      2: 2025-04-10: Vendor Z revises enterprise SLA…
    </doc>
  </documents>

  <!-- Execution conditions -->
  Timeline: Complete within this interaction. Handle ambiguities with "Assumption: ..." statements and proceed conservatively.
</user>
```

### Technique Mapping
- **(1) Explicitness**: Role/purpose/success criteria at start of `<system>`, goal/word count/prohibitions in `<user>`.
- **(2) Extended Thinking**: "Internal thinking" directive in `<system>` + 1,024–4,096 token budget guidance.
- **(3) Parallel Tools**: Simultaneous execution instructions in both `<system>` and `<user>` sections.
- **(4) Cleanup**: Temporary file deletion and final storage location specified in both `<system>` and `<user>`.
- **(5) Structured Output**: Updated to "JSON schema + tools as first choice / XML as alternative".
- **(6) Long Documents**: Document-first workflow in `<system>`, `<documents>` placement in `<user>`.

---

## 3. Operational Considerations
- **Context Limits**: **200K tokens input** / **32K tokens output maximum** (verify limits for your specific model/environment)
- **Thinking Budget**: Initialize `thinking.budget_tokens` at **1,024–4,096**, ensuring it's less than output `max_tokens`
- **Prompt Caching**: Extract static content (evaluation criteria, safety rules, glossaries) into cacheable segments for **reduced latency and costs**
- **Token Planning**: Pre-calculate token usage with counters to avoid context overflow
- **Tool Integration**: Tool names, parameters, and returns vary by platform—ensure prompt instructions match your **API contracts**
- **Safety Handling**: Decline inappropriate requests politely. Severe policy violations may trigger automatic conversation termination.
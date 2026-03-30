# Self-Improvement Patterns in AI Agents: Research on Hermes Agent and Industry Patterns

*Research for Alize Agent Architecture — Deep dive into how Hermes implements self-improvement and how other agents do it*

---

## Executive Summary

Hermes Agent (NousResearch) implements self-improvement through a **multi-layered closed learning loop** that combines episodic memory (session history), semantic memory (structured knowledge files), and procedural memory (skills). The key insight: Hermes does **not** rely on fine-tuning or RL for its core learning loop — it uses **persistent memory files + autonomous skill creation + evolutionary prompt optimization** as three complementary mechanisms.

The architecture is:- **Level 1 (runtime)**: Agent reflects during task execution, saves facts to memory files immediately
- **Level 2 (cross-session)**: Memory files injected at session start; FTS5 session search for recall
- **Level 3 (skill formation)**: Complex tasks auto-generate SKILL.md files the agent reuses
- **Level 4 (evolutionary optimization)**: Separate `hermes-agent-self-evolution` pipeline uses DSPy+GEPA to optimize skills/prompts using execution traces — no GPU training

This is fundamentally different from fine-tuning-based approaches. It's closer to "learning at the knowledge/prompt level" rather than "learning at the weight level."

---

## 1. Hermes Agent: Self-Improvement Mechanisms

### 1.1 Memory System (Episodic + Semantic Memory)

**How it works concretely:**

Two flat files — `MEMORY.md` (~2,200 chars) and `USER.md` (~1,375 chars) — stored in `~/.hermes/memories/`. These are NOT a vector database. They're text files that get injected into the system prompt as a **frozen snapshot at session start**.

```
At session start:
1. Read MEMORY.md and USER.md from disk
2. Render them into the system prompt with usage percentage headers
3. This frozen block is captured once and reused (prefix caching)
4. The agent's tool responses show live state, but the LLM sees frozen snapshot
```

**Why frozen snapshot matters**: The LLM provider can cache the prefix (system prompt + memory), so every turn doesn't re-tokenize the memory. This is a deliberate performance tradeoff — the agent can't see its own memory edits mid-session, but context is cheaper.

**Memory tool actions:**
- `memory(action="add", target="memory", content="...")` — add entry
- `memory(action="replace", target="memory", old_text="unique substring", content="...")` — replace via substring match
- `memory(action="remove", target="memory", old_text="unique substring")` — remove

**Character limits force curation**: When full, the agent must consolidate entries before adding new ones. This prevents memory bloat and keeps token usage bounded.

**Security scanning**: Entries are scanned for prompt injection and credential exfiltration patterns before acceptance, since they're injected into the system prompt.

**What gets saved:**
- User corrections: "Don't use sudo for Docker, user is in docker group"
- Environment facts: "This server runs Debian 12 with PostgreSQL 16"
- Conventions: "Project uses tabs, 120-char line width"
- Completed work diary: "Migrated DB from MySQL to PostgreSQL on 2026-01-15"
- Explicit requests: "API key rotation happens monthly"

**What gets skipped:**
- Trivial/obvious facts ("User asked about Python")
- Easily re-discovered facts (language feature documentation)
- Raw data dumps (code blocks, logs)
- Session-specific ephemera

---

### 1.2 Skills System (Procedural Memory)

**How skills are created autonomously:**

The agent creates skills via `skill_manage` tool after:
- Complex tasks (5+ tool calls)
- Tasks where it hit errors and found the working path
- User corrections of its approach
- Non-trivial workflows it discovered

Skills are stored as `SKILL.md` files in `~/.hermes/skills/` — human-readable, follow the `agentskills.io` open standard. This means they're portable to other agent platforms that support the same spec.

**SKILL.md format:**
```yaml
---
name: my-skill
description: Brief description of what this skill does
version: 1.0.0
platforms: [macos, linux]
metadata:
  hermes:
    tags: [python, automation]
    category: devops
---

# Skill Title

## When to Use
Trigger conditions for this skill.

## Procedure
1. Step one
2. Step two

## Pitfalls
- Known failure modes and fixes

## Verification
How to confirm it worked.
```

**Progressive disclosure loading:**
- Level 0: `skills_list()` → `[{name, description, category}, ...]` (~3k tokens total)
- Level 1: `skill_view(name)` → Full content + metadata
- Level 2: `skill_view(name, path)` → Specific reference file

The agent only loads the full skill when it actually needs it. This keeps context efficient.

**Conditional/fallback skills:**
```yaml
metadata:
  hermes:
    fallback_for_toolsets: [web]  # Show ONLY when web toolset is unavailable
    requires_toolsets: [terminal]  # Show ONLY when terminal is available
```
Example: DuckDuckGo search skill auto-appears when FIRECRAWL_API_KEY is missing.

**Skill_manage tool actions:**
- `create` — new skill from scratch (name, content, optional category)
- `patch` — targeted fixes (preferred, token-efficient)
- `edit` — full SKILL.md replacement
- `delete` — remove skill
- `write_file` / `remove_file` — manage supporting files

**Skills Hub**: Community skills installable from multiple registries (skills.sh, well-known endpoints, GitHub repos, Claude marketplace, etc.) with security scanning.

---

### 1.3 FTS5 Session Search (Episodic Recall)

**How it works:**

All CLI and messaging sessions are stored in SQLite (`~/.hermes/state.db`) with FTS5 full-text search. The agent can search past conversations using `session_search`:

```
hermes sessions list  # Browse past sessions
# → Session search returns relevant past conversations with LLM summarization
```

This is distinct from MEMORY.md:
- **MEMORY.md**: Key facts always available (bounded, curated, ~800 tokens)
- **Session search**: Unlimited history, requires search + LLM summarization (on-demand cost)

The agent uses session search for "did we discuss X last week?" queries.

---

### 1.4 Honcho Integration (Deeper User Modeling)

**Honcho** (from plastic-labs) is a cloud-hosted memory system that runs alongside Hermes' built-in memory. It uses a **dual-peer architecture**:

- **User peer**: Observed from user messages. Learns preferences, goals, communication style
- **AI peer**: Observed from assistant messages (`observe_me=True`). Builds representation of the agent's own knowledge and behavior

**Tools:**
- `honcho_profile` — fast peer card retrieval (no LLM call)
- `honcho_search` — semantic search over memory (no LLM)
- `honcho_context` — dialectic Q&A: "What are this user's main goals?" (LLM-powered)
- `honcho_conclude` — write a fact to persistent memory

**Dynamic reasoning levels**: Honcho scales dialectic effort with message complexity:
- <120 chars → config default (low)
- 120-400 chars → one level above default
- >400 chars → two levels above default

**Cross-session, cross-platform**: Same user model available across CLI, Telegram, Discord, etc.

---

### 1.5 Self-Evolution: DSPy + GEPA (Evolutionary Optimization)

This is the most sophisticated layer — a **separate repo** (`hermes-agent-self-evolution`) that operates ON Hermes, not inside it.

**Key insight**: No GPU training. Everything operates via API calls. It mutates text (skills, prompts, tool descriptions) and evaluates results — no weight updates.

**The optimization loop:**

```
1. SELECT TARGET
   Pick a skill, prompt section, or tool description
   
2. BUILD EVALUATION DATASET
   Mine SessionDB for real usage examples
   Or generate synthetic test cases
   Split: train / validation / test
   
3. WRAP AS DSPy MODULE
   Skill text → dspy.Signature
   Agent workflow → dspy.ReAct
   Tool selection → dspy.Predict
   
4. RUN OPTIMIZER
   Primary: dspy.GEPA (Genetic-Pareto Prompt Evolution)
   Fallback: dspy.MIPROv2 (Bayesian optimization)
   
5. EVALUATE & COMPARE
   Run optimized version on held-out test
   Compare: accuracy, cost, latency
   Statistical significance check
   
6. DEPLOY (with approval)
   Git commit the improved version
   Human review via PR
   Rollback via git revert
```

**GEPA (Genetic-Pareto Prompt Evolution)** is the star. It:
- Reads **execution traces** to understand WHY things fail (not just that they failed)
- Works with as few as 3 examples
- Outperforms both RL and previous DSPy optimizers
- ICLR 2026 Oral paper

**What gets evolved (phased):**

| Phase | Target | Engine | Status |
|-------|--------|--------|--------|
| 1 | Skill files (SKILL.md) | DSPy + GEPA | ✅ Implemented |
| 2 | Tool descriptions | DSPy + GEPA | 🔲 Planned |
| 3 | System prompt sections | DSPy + GEPA | 🔲 Planned |
| 4 | Tool implementation code | Darwinian Evolver | 🔲 Planned |
| 5 | Continuous improvement loop | Automated pipeline | 🔲 Planned |

**Guardrails on evolved variants:**
- Full test suite must pass (pytest)
- Size limits: Skills ≤15KB, tool descriptions ≤500 chars
- Caching compatibility (no mid-conversation changes)
- Semantic preservation (no drift from original purpose)
- PR review required — never direct commit

**Cost**: ~$2-10 per optimization run. No GPU needed.

---

### 1.6 RL / Trajectory Infrastructure (For Training)

Hermes also supports **trajectory collection for RL training** — it can export interaction trajectories and run RL through the Atropos environment framework:

```python
# From the architecture docs:
batch_runner.py  # Batch trajectory generation
environments/    # Hermes RL / benchmark environment framework
```

This is research infrastructure for training the next generation of tool-calling models. It uses the **Tinker-Atropos** RL environment system. This is separate from the runtime self-improvement loop — it's for generating training data to improve base models.

---

## 2. Comparison with Other Agent Self-Improvement Approaches

### 2.1 Reflexion (Shinn et al., 2023)

**Mechanism**: Verbal reinforcement learning — keeps the model frozen, uses text-based feedback.

```
Actor (LLM) → attempts task
     ↓
Environment feedback (errors, success signals)
     ↓
Self-Reflection prompt → LLM critiques its attempt
     ↓
Reflection stored in memory → conditions next attempt
```

**HOW it works concretely**: After each attempt, the LLM is given a "Self-Reflection" prompt asking it to analyze what went wrong. The critique is stored as text and prepended to the next turn. The model doesn't update weights — it updates its "contextual advice."

**Results**: Reflexion-augmented GPT-4 reached 91% on HumanEval (vs 80% baseline). On AlfWorld, solved 130/134 tasks.

**Key paper**: Shinn, N., et al. (2023). "Reflexion: Language Agents with Verbal Reinforcement Learning."

---

### 2.2 ReAct (Yao et al., 2023)

**Mechanism**: Interleaves reasoning (Chain-of-Thought) with acting (tool execution) in the same turn.

**HOW it works concretely**: Each turn produces a `Thought` (reasoning) followed by an `Action` (tool call), then an `Observation` (result). The prompt format is:

```
Thought 1: The agent reasons about what to do next
Action 1: Search(query)
Observation 1: Result of search

Thought 2: Agent reflects on the observation
Action 2: Next action
...
```

**Results**: HotpotQA (QA): Reduced hallucinations by using reasoning to decide when to search. AlfWorld: +34% over prior methods. WebShop: +10% improvement.

**Key paper**: Yao, S., et al. (2023). "ReAct: Synergizing Reasoning and Acting in Language Models."

---

### 2.3 Anthropic's Claude: Agent Skills + Self-Correction

**Skills system** (published Oct 2025): Follows the same `agentskills.io` open standard Hermes uses. Progressive disclosure loading identical to Hermes. Skills are directories containing `SKILL.md` + optional scripts.

**HOW it works**: Anthropic explicitly recommends:
- Start with evaluation: Run agent on tasks, identify gaps, build skills incrementally
- Iterate with Claude: Ask Claude to capture successful approaches into reusable skills
- Self-reflection: If Claude goes off track, ask it to self-reflect on what went wrong

**Claude Code's self-correction**:
- Multi-stage verification: Claude attempts to "disprove" its own vulnerability reports to filter false positives
- Remediation directives: Doesn't just identify problems, suggests targeted patches

**Anthropic's introspection research** (March 2025): Evidence that Claude models can recognize injected concepts in their own activations — ~20% reliability on concept injection detection. More capable models (Opus 4.1) perform better, suggesting introspection will improve with model capability.

**Key insight**: Claude's introspection is unreliable today (~20%) but improving with model scale. This is different from Hermes' deterministic memory system.

---

### 2.4 Self-Refine (Madaan et al.)

**Mechanism**: Iterative self-critique and refinement — model acts as both writer and editor.

```
Generate output → Critique (what's wrong?) → Refine → Critique → Refine → ...
```

No training required. Works via prompting. Even GPT-4 shows significant gains when prompted to self-reflect.

---

### 2.5 Chain-of-Hindsight (CoH)

**Mechanism**: Training-based approach — model sees examples of past outputs that were wrong and corrects them.

```
Input: "What is 2+2?"
Output (wrong): "5"
Feedback: "That was incorrect. The answer is 4."
Training example: (Input, wrong_output, feedback, correct_output)
```

Learns to internalize reflective reasoning. Reduces repeat errors.

---

### 2.6 Constitutional AI (Anthropic)

**Mechanism**: Model evaluates its own outputs against a set of principles ("Is this helpful? Is this harmless?").

Not strictly self-improvement for task performance — more for alignment. But the principle of self-critique against principles is similar to Reflexion's self-reflection.

---

### 2.7 Agentic RAG

Modern RAG systems embed agents that:
1. Retrieve context
2. Evaluate quality of retrieved context
3. Decide if more retrieval is needed
4. Generate and self-verify the answer
5. If gaps found → retrieve differently

This is a retrieval-time adaptation loop, distinct from Hermes' persistent memory approach.

---

## 3. Cross-Cutting Analysis: HOW vs WHAT

| System | Learning Type | Mechanism | Fine-tune? | GPU needed? |
|--------|-------------|-----------|------------|-------------|
| Hermes (runtime) | Episodic + Semantic | Memory files + skill creation | No | No |
| Hermes (evolution) | Prompt optimization | DSPy+GEPA on traces | No | No |
| Hermes (trajectories) | Weight update | Atropos RL, trajectory export | Yes | Yes |
| Reflexion | Verbal RL | Self-reflection prompts | No | No |
| ReAct | Reasoning + Acting | Thought-Action-Observation loops | No | No |
| Claude Skills | Skill compilation | Human + AI co-creation of skills | No | No |
| Self-Refine | Iterative critique | Generate → critique → refine | No | No |
| Chain-of-Hindsight | Training | Learning from (error, feedback) pairs | Yes | Yes |

---

## 4. Implications for Alize Agent Architecture

### 4.1 What Hermes does well that Alize should consider:

**1. Bounded curated memory over unlimited vector storage**
- Hermes uses flat files with hard character limits (2,200 / 1,375 chars)
- This forces the agent to prioritize and consolidate — no memory bloat
- For Alize: Consider bounded memory with agent-managed eviction, not unbounded RAG

**2. Skills as procedural memory**
- Autonomous skill creation after complex tasks is powerful
- SKILL.md format is simple, portable, human-readable
- For Alize: Implement skill_manage tool + SKILL.md storage + progressive disclosure loading

**3. Separation of concerns between memory layers**
- MEMORY.md: Always-in-context key facts
- Session search: On-demand historical recall
- Honcho: Cross-session user modeling
- Skills: Procedural reusable knowledge
- Self-evolution: Optimization layer (offline)
- For Alize: Don't conflate these — use different storage/query mechanisms per layer

**4. Self-evolution as offline optimization**
- The evolution pipeline runs separately, generates PRs for human review
- No real-time weight changes
- For Alize: Consider separating "runtime learning" (memory/skill creation) from "offline optimization" (prompt/skill evolution)

**5. Frozen snapshot for prefix caching**
- Memory injected once at session start, not mutated mid-session
- Enables LLM provider-side caching
- For Alize: Consider snapshot-at-start pattern for frequently-injected context

### 4.2 What Hermes does NOT do:

**No real-time weight updates during conversation** — the learning is at the knowledge/prompt level, not model level. This is a design choice that trades expressiveness for simplicity and safety.

**No multi-agent reflection** — Hermes is single-agent. Multi-agent critique (one generates, another critiques) is a separate research area.

**Skills self-improvement is implicit** — "skills self-improve during use" is mentioned but the mechanism (GEPA evolution) is a separate offline pipeline, not real-time.

---

## 5. Key Papers and Resources

1. **Hermes Agent GitHub**: https://github.com/NousResearch/hermes-agent
2. **Hermes Agent Docs**: https://hermes-agent.nousresearch.com/docs/
3. **Hermes Self-Evolution**: https://github.com/NousResearch/hermes-agent-self-evolution
4. **GEPA Paper** (ICLR 2026 Oral): Genetic-Pareto Prompt Evolution — reflective prompt optimization from execution traces
5. **DSPy**: Stanford NLP framework for optimizing LM prompts/programs
6. **Reflexion** (Shinn et al., 2023): Verbal reinforcement learning for LLM agents
7. **ReAct** (Yao et al., 2023): Interleaving reasoning and acting in LLM agents
8. **Honcho**: https://honcho.dev — dual-peer memory system for user modeling
9. **Agent Skills open standard**: https://agentskills.io
10. **Anthropic Introspection Research** (2025): Evidence for introspective awareness in Claude models

---

## 6. Quick-Start Architecture Recommendation for Alize

Based on this research, a practical self-improvement architecture for Alize:

```
Alize Self-Improvement Stack:
├── Runtime Learning (no GPU)
│   ├── Memory files (bounded, agent-curated)
│   │   ├── MEMORY.md (~2KB) — agent's notes
│   │   └── USER.md (~1KB) — user preferences
│   ├── Skills system
│   │   ├── skill_manage tool (create/patch/delete)
│   │   └── ~/.alize/skills/ (SKILL.md files)
│   └── Session search (SQLite FTS5)
│
├── Cross-Session Modeling
│   └── Honcho integration (dual-peer user modeling)
│
└── Offline Optimization (separate process)
    ├── DSPy + GEPA for skill/prompt evolution
    ├── Execution trace collection
    └── Human-in-the-loop PR review
```

The key principle: **Learning at the knowledge level (memory + skills + prompts) is more practical and safer than learning at the weight level for most agent deployments.** Fine-tuning and RL are research tools for training better base models; persistent memory and skill creation are the operational tools for making agents improve in deployment.

---

*Research completed: 2026-03-26. Sources: Hermes Agent documentation, GitHub repos, Anthropic engineering blog, academic papers (Reflexion, ReAct), Hugging Face analysis.*

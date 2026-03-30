# Alizé Self-Improvement Architecture
**Inspired by:** NousResearch Hermes Agent
**Saved:** 2026-03-26

---

## Core Principle

**Learning at the knowledge/prompt level, not the weight level.**

- No fine-tuning. No RL. No GPU training.
- Learning happens through: memory files, skill creation, and prompt evolution.
- Fine-tuning and RL are research tools for training base models. Persistent memory and skill creation are the operational tools for production agents.

---

## The Four Layers

### Layer 1: Runtime Learning (immediate, no GPU)

**Bounded Curated Memory** — flat files with hard character limits

```
~/.alize/memory/
├── MEMORY.md     # Agent's persistent notes (~2KB limit)
└── USER.md       # User preferences (~1KB limit)
```

- Injected into system prompt at session start as a frozen snapshot
- Frozen snapshot = LLM provider can cache the prefix (cheaper per-token)
- Agent must curate when full — character limits force prioritization
- Security: entries scanned for prompt injection before acceptance

**Tools:**
```
memory(action="add", target="memory", content="...")
memory(action="replace", target="memory", old_text="unique substring", content="...")
memory(action="remove", target="memory", old_text="unique substring")
```

**What gets saved:**
- User corrections: "Don't do X, always do Y instead"
- Environment facts: "This server runs Debian 12 with PostgreSQL 16"
- Project conventions: "We use tabs, 120-char line width"
- Completed work: "Migrated DB on 2026-01-15"
- Explicit requests: "API key rotation happens monthly"

**What gets skipped:**
- Trivial facts
- Easily re-discovered information
- Raw data dumps
- Session-specific ephemera

---

### Layer 2: Skill System (procedural memory)

**Autonomous skill creation** after:
- Complex tasks (5+ tool calls)
- Tasks where errors were overcome
- User corrections of approach
- Non-trivial workflows discovered

**Format:** SKILL.md files following `agentskills.io` open standard

```yaml
---
name: contract-review
description: Review French legal contracts for compliance
version: 1.0.0
platforms: [linux]
metadata:
  alize:
    tags: [legal, france, contract]
    category: review
---

# Contract Review

## When to Use
Trigger when user uploads a contract PDF or asks for legal review.

## Procedure
1. Parse document via PDF tool
2. Extract key clauses (parties, dates, obligations, termination)
3. Flag unusual terms (automatic renewal, penalty clauses)
4. Generate summary + risk assessment

## Pitfalls
- Don't provide legal advice — flag for human review
- French law specifics — check against Code Civil references

## Verification
Confirm summary matches document content.
```

**Progressive disclosure loading:**
- Level 0: `skills_list()` → `[{name, description, category}, ...]` — lightweight
- Level 1: `skill_view(name)` → Full content + metadata
- Level 2: `skill_view(name, path)` → Specific reference file

**Skill manage tool:**
```
skill_manage(action="create", name="...", content="...")
skill_manage(action="patch", name="...", edits=[...])
skill_manage(action="delete", name="...")
skill_manage(action="list")
```

---

### Layer 3: Session Search (episodic recall)

**SQLite FTS5** full-text search over all past conversations.

- Separate from MEMORY.md (bounded, curated) vs unlimited session history
- Agent searches when user asks "did we discuss X last week?"
- LLM summarization of retrieved sessions (on-demand cost)

---

### Layer 4: Offline Evolution (periodic, no GPU)

**DSPy + GEPA** on execution traces — a separate pipeline that runs on the side.

**GEPA (Genetic-Pareto Prompt Evolution):**
- Reads execution traces to understand WHY failures happened, not just that they failed
- Works with as few as 3 examples
- Mutates text (skills, prompts, tool descriptions) — no weight updates
- ~$2-10 per optimization run
- Outputs PRs for human review before deployment

**The optimization loop:**
```
1. SELECT TARGET   — Pick a skill or prompt section to optimize
2. BUILD DATASET  — Mine session traces for real usage examples
3. WRAP DSPy     — Skill → dspy.Signature, workflow → dspy.ReAct
4. RUN GEPA      — Genetic-Pareto optimization
5. EVALUATE      — Compare accuracy, cost, latency on held-out test
6. DEPLOY        — PR for human review, rollback via git
```

**What gets evolved (phased):**
| Phase | Target | Status |
|-------|--------|--------|
| 1 | Skill files | Phase 1 |
| 2 | Tool descriptions | Phase 2 |
| 3 | System prompt sections | Phase 3 |
| 4 | Tool implementation | Phase 4 |

**Guardrails:**
- Full test suite must pass
- Size limits: Skills ≤15KB, tool descriptions ≤500 chars
- Caching compatibility (no mid-conversation changes)
- Semantic preservation (no drift from original purpose)
- PR review required — never direct commit

---

## Comparison with Other Approaches

| System | Learning Type | Mechanism | GPU? |
|--------|-------------|-----------|------|
| **Hermes runtime** | Episodic + Semantic | Memory files + skill creation | No |
| **Hermes evolution** | Prompt optimization | DSPy+GEPA on traces | No |
| Reflexion | Verbal RL | Self-reflection prompts | No |
| ReAct | Reasoning + Acting | Thought-Action-Observation loops | No |
| Self-Refine | Iterative critique | Generate → critique → refine | No |
| Chain-of-Hindsight | Training | Learning from (error, feedback) pairs | Yes |
| Claude Skills | Skill compilation | Human + AI co-creation | No |

---

## Implementation Notes

### For Alize Architecture (Hono + Mastra)

```
apps/engine/src/
├── agent/
│   ├── memory/           # Layer 1: bounded memory files
│   │   ├── agent-memory.ts
│   │   └── user-memory.ts
│   ├── skills/           # Layer 2: skill system
│   │   ├── loader.ts
│   │   ├── manager.ts    # skill_manage tool
│   │   └── builtin/      # Default skills
│   └── sessions/         # Layer 3: session search (FTS5)
│       └── search.ts
└── workers/
    └── evolution/        # Layer 4: offline DSPy+GEPA
```

### Key Design Decisions

1. **Frozen snapshot at session start** — enables prefix caching, but agent can't see its own memory edits mid-session. Acceptable tradeoff.

2. **Character limits as curation** — no automatic eviction algorithm. Agent decides what matters. Simple, deterministic.

3. **Skills are portable** — follow agentskills.io standard. Skills created for Alize work in Claude, Hermes, etc.

4. **Evolution is offline + human-gated** — never auto-commit. PR review required. Safe by design.

5. **Separation of memory layers** — don't conflate bounded curated memory (always in context) with unbounded session history (search when needed).

---

## Sources

- Hermes Agent: https://github.com/NousResearch/hermes-agent
- Hermes Self-Evolution: https://github.com/NousResearch/hermes-agent-self-evolution
- GEPA Paper (ICLR 2026 Oral): Genetic-Pareto Prompt Evolution
- DSPy: Stanford NLP framework for prompt optimization
- Agent Skills open standard: https://agentskills.io

# Universal Development Workflow

A technology-agnostic, framework-independent development workflow for AI-assisted software development.

---

## What is this?

This is a **universalized version** of the Superpowers development workflow, abstracted to work with any technology stack. It provides:

- **Structured development stages** for both feature work and debugging
- **Evidence-based verification** before any claims
- **Test-first development** discipline
- **Systematic troubleshooting** when things go wrong
- **Reusable templates** for common operations

---

## Documents

| Document | Purpose |
|----------|---------|
| **[PRINCIPLES.md](PRINCIPLES.md)** | Core principles (start here) - The 5 Iron Laws and philosophy |
| **[WORKFLOW.md](WORKFLOW.md)** | Complete workflow - Feature development (7 stages) and Debug mode (4 phases) |
| **[TEMPLATES.md](TEMPLATES.md)** | Reusable templates - Error handling, verification, reviews, decisions |
| **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** | When stuck - Recovery procedures and error patterns |

---

## Quick Start

### 1. Read Principles
Start with [PRINCIPLES.md](PRINCIPLES.md) to understand the core philosophy.

### 2. Answer 3 Questions
```yaml
Q1: Read project guidelines? [NO → Read them first]
Q2: Task type? [Bug → Debug Mode, Feature → Feature Mode]
Q3: Stuck? [YES → See Troubleshooting]
```

### 3. Follow the Workflow
- **Feature Work**: Exploration → Planning → Implementation → Review → Testing → Verification → Delivery
- **Debug Work**: Root Cause → Pattern Analysis → Hypothesis → Fix → (then Feature stages 2-7)

---

## Adapting to Your Project

To use this workflow with a specific project, create:

### PROJECT_CONTEXT.md
```yaml
Stack:
  Language: [python|typescript|csharp|go|rust|...]
  Version: [version]
  TestFramework: [pytest|jest|xunit|go test|cargo test|...]
  BuildTool: [npm|dotnet|cargo|make|...]

Constraints:
  - Zero warnings
  - [Other project constraints]

Key Commands:
  Build: [build command]
  Test: [test command]
  Lint: [lint command]
  Regression: [regression test command]

Paths:
  DesignDocs: [where to put design docs]
  Plans: [where to put implementation plans]
```

---

## Key Concepts

### The 5 Iron Laws
1. **Read Guidelines First** - Always start with project context
2. **Test Before Code** - No production code without failing test
3. **Verify Before Claim** - Run check → Read output → Then claim
4. **Root Cause Before Fix** - Understand before fixing
5. **Debug → Full Cycle** - Bug fixes complete delivery stages

### Gate Pattern
Every stage ends with verification:
```
IDENTIFY → RUN → READ → VERIFY → CLAIM
```

### TDD Cycle
```
RED (failing test) → GREEN (passing code) → REFACTOR (improve)
```

---

## Comparison with Original

| Aspect | Original (Superpowers) | This Version |
|--------|------------------------|--------------|
| Stack | Godot + .NET | Any |
| Commands | `dotnet test`, etc. | Placeholders |
| Skills | Native tool support | File reading |
| Hooks | `inject_prompt` | Manual reminder |
| Structure | Specific paths | Configurable |

The **principles and flow** remain identical. Only the **implementation details** are abstracted.

---

## Usage Examples

### Starting Feature Development
```markdown
Read: PRINCIPLES.md
Read: WORKFLOW.md → MODE A
Follow: Stage 1 (Exploration)
```

### Handling a Bug
```markdown
Read: WORKFLOW.md → MODE B
Follow: Phase 1 (Root Cause)
→ Phase 2 (Pattern Analysis)
→ Phase 3 (Hypothesis)
→ Phase 4 (Fix)
→ Then MODE A Stages 2-7
```

### When Stuck
```markdown
Read: TROUBLESHOOTING.md
Check: "Am I Stuck?" section
Follow: Appropriate pattern
```

---

## License

Same as original Superpowers (MIT)

---

*Universal Development Workflow v1.0*

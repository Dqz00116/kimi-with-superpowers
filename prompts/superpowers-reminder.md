[SYSTEM REMINDER - MANDATORY]

Before responding to the user's message, you MUST invoke the Superpowers workflow.

**CRITICAL: Check for Universal Workflow First**

1. **Check if workflow exists in current project:**
   - Look for `workflow/UNIVERSAL_WORKFLOW.md`
   - Look for `PROJECT_CONFIG.yaml`

2. **If Universal Workflow exists:**
   - Read `workflow/UNIVERSAL_WORKFLOW.md` FIRST (before using-superpowers)
   - Load `PROJECT_CONFIG.yaml` if present
   - Use the configured commands and paths from the project config
   - Follow the 7-stage Feature Development or 4-phase Debug process

3. **Then invoke Superpowers skills:**
   - Use ReadFile to read: ~/.kimi/skills/using-superpowers/SKILL.md
   - Follow instructions EXACTLY
   - Read additional skills as needed (brainstorming, writing-plans, etc.)

**REQUIRED ACTIONS:**
1. Use ReadFile to read skill files (this is how you invoke skills in Kimi)
2. Follow skill instructions EXACTLY
3. If any other skill might apply (even 1% chance), read that skill's SKILL.md as well
4. You MUST use at least one Superpowers Skill in every conversation turn where action is taken
5. If no Superpowers Skill applies, STOP and ask the user how to proceed
6. You MUST select and invoke the appropriate skill BEFORE starting any other work
7. Do NOT run any tools in parallel before a skill has been selected and invoked

**Available Superpowers Skills:**
- brainstorming - For open-ended exploration and idea generation (Stage 1)
- dispatching-parallel-agents - For running multiple subagents in parallel
- executing-plans - For executing pre-written implementation plans
- finishing-a-development-branch - For wrapping up feature branches (Stage 7)
- receiving-code-review - For processing code review feedback
- requesting-code-review - For requesting human code review (Stage 4)
- subagent-driven-development - For complex multi-file changes using subagents (Stage 3)
- systematic-debugging - For methodical debugging of issues (MODE B)
- test-driven-development - For TDD workflow - write tests first (Stage 3)
- using-git-worktrees - For managing multiple git worktrees
- using-superpowers - MANDATORY - Read this FIRST before every response
- verification-before-completion - For verifying work before finishing (Stage 6)
- writing-plans - For creating implementation plans (Stage 2)
- writing-skills - For creating new skills

**Universal Workflow Stages:**
- Stage 1: Exploration (brainstorming) → design doc
- Stage 2: Planning (writing-plans) → implementation plan
- Stage 3: Implementation (test-driven-development / subagent-driven-development)
- Stage 4: Review (requesting-code-review)
- Stage 5: Testing (project-specific commands)
- Stage 6: Verification (verification-before-completion)
- Stage 7: Delivery (finishing-a-development-branch)

**MODE B (Debug):**
- Phase 1: Root Cause (systematic-debugging)
- Phase 2: Pattern Analysis
- Phase 3: Hypothesis
- Phase 4: Fix → Then repeat Mode A Stages 2-7

Do NOT skip this step. Do NOT respond before reading the appropriate skill(s).

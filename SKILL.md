---
name: install-superpowers-for-kimi
description: Install and configure Superpowers workflow for Kimi Code CLI (feat/hook-inject-prompt branch) on any platform (Windows/macOS/Linux)
---

# Install Superpowers for Kimi Code CLI

This skill guides you through installing the Superpowers workflow system for Kimi Code CLI with automatic skill invocation via the `inject_prompt` hook feature.

## ⚠️ Prerequisites - IMPORTANT

**This installation requires the `feat/hook-inject-prompt` branch of Kimi Code CLI**, which adds support for the `inject_prompt` hook feature.

### Step 0: Install Kimi CLI from feat/hook-inject-prompt Branch

**Option A: Clone and Install (Recommended)**

```bash
# macOS/Linux
git clone -b feat/hook-inject-prompt https://github.com/Dqz00116/kimi-cli.git ~/kimi-cli
cd ~/kimi-cli
uv sync
uv pip install -e .

# Verify installation
kimi --version
```

**Windows (PowerShell):**
```powershell
# Clone to a permanent location
git clone -b feat/hook-inject-prompt https://github.com/Dqz00116/kimi-cli.git "$env:USERPROFILE\kimi-cli"
cd "$env:USERPROFILE\kimi-cli"

# Install dependencies and package
uv sync
uv pip install -e .
```

**Option B: Run Without Installing**

```bash
cd ~/kimi-cli
uv run kimi --help
```

### Verify Hook Support

```bash
# Should show UserPromptSubmit in the list of supported events
kimi --help | grep -A 5 "hooks"
```

---

## Installation Steps

### Step 1: Clone Superpowers Repository

```bash
# Create .kimi directory if it doesn't exist
mkdir -p ~/.kimi

# Clone superpowers repository
git clone --depth 1 https://github.com/obra/superpowers.git ~/.kimi/superpowers
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.kimi"
git clone --depth 1 https://github.com/obra/superpowers.git "$env:USERPROFILE\.kimi\superpowers"
```

### Step 2: Create Skill Links

**On macOS/Linux:**
```bash
mkdir -p ~/.kimi/skills

# Create symlinks for all superpowers skills
for skill in brainstorming dispatching-parallel-agents executing-plans \
    finishing-a-development-branch receiving-code-review requesting-code-review \
    subagent-driven-development systematic-debugging test-driven-development \
    using-git-worktrees using-superpowers verification-before-completion \
    writing-plans writing-skills; do
    ln -sf ~/.kimi/superpowers/skills/$skill ~/.kimi/skills/$skill
done
```

**On Windows (PowerShell as Administrator):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.kimi\skills"

$skills = @(
    "brainstorming", "dispatching-parallel-agents", "executing-plans",
    "finishing-a-development-branch", "receiving-code-review", "requesting-code-review",
    "subagent-driven-development", "systematic-debugging", "test-driven-development",
    "using-git-worktrees", "using-superpowers", "verification-before-completion",
    "writing-plans", "writing-skills"
)

foreach ($skill in $skills) {
    $target = "$env:USERPROFILE\.kimi\skills\$skill"
    $source = "$env:USERPROFILE\.kimi\superpowers\skills\$skill"
    if (Test-Path $target) { Remove-Item $target -Recurse -Force }
    cmd /c mklink /J "$target" "$source"
}
```

### Step 3: Patch using-superpowers Skill for Kimi

Edit `~/.kimi/superpowers/skills/using-superpowers/SKILL.md`:

1. Change the description to:
   ```yaml
   description: MANDATORY — Read this skill FIRST at the start of EVERY conversation before any response or action
   ```

2. In the "How to Access Skills" section, add Kimi-specific instructions after "In Gemini CLI:":
   ```markdown
   **In Kimi Code CLI:** Kimi does not have a native `Skill` tool. To invoke a skill, you MUST use the `ReadFile` tool to read the skill's `SKILL.md` file from its path (listed in the system prompt under Available skills). After reading the skill content, follow its instructions exactly. This is how you "invoke" a skill in Kimi.
   ```

3. In "The Rule" section, add after the first paragraph:
   ```markdown
   **In Kimi Code CLI specifically:** Before responding to the user's first message in any conversation, you MUST use `ReadFile` to read `using-superpowers/SKILL.md` (from the skill paths listed in the system prompt). Then, if any other skill might apply, use `ReadFile` to read that skill's `SKILL.md` as well. Do not skip this step.
   ```

### Step 4: Configure UserPromptSubmit Hook

**Create the prompts directory and reminder file:**

```bash
mkdir -p ~/.kimi/prompts

# Create the Superpowers reminder prompt
cat > ~/.kimi/prompts/superpowers-reminder.md << 'EOF'
[SYSTEM REMINDER - MANDATORY]

Before responding to the user's message, you MUST invoke the Superpowers workflow by reading the using-superpowers skill.

**REQUIRED ACTION:**
1. Use ReadFile to read: ~/.kimi/skills/using-superpowers/SKILL.md
2. Follow the instructions in that skill EXACTLY
3. If any other skill might apply (even 1% chance), read that skill's SKILL.md as well
4. You MUST use at least one Superpowers Skill in every conversation turn where action is taken.
5. If no Superpowers Skill applies to the user's request, STOP and ask the user how to proceed. Do NOT perform any other action.
6. You MUST select and invoke the appropriate skill BEFORE starting any other work.
7. Do NOT run any tools in parallel before a skill has been selected and invoked.

**Available Superpowers Skills:**
- brainstorming - For open-ended exploration and idea generation
- dispatching-parallel-agents - For running multiple subagents in parallel
- executing-plans - For executing pre-written implementation plans
- finishing-a-development-branch - For wrapping up feature branches
- receiving-code-review - For processing code review feedback
- requesting-code-review - For requesting human code review
- subagent-driven-development - For complex multi-file changes using subagents
- systematic-debugging - For methodical debugging of issues
- test-driven-development - For TDD workflow (write tests first)
- using-git-worktrees - For managing multiple git worktrees
- using-superpowers - MANDATORY - Read this FIRST before every response
- verification-before-completion - For verifying work before finishing
- writing-plans - For creating implementation plans
- writing-skills - For creating new skills

**Universal Workflow:**
- If workflow/UNIVERSAL_WORKFLOW.md exists in the current project, read it first
- Load PROJECT_CONFIG.yaml if present to adapt workflow to your stack
- Follow the 7-stage Feature Development or 4-phase Debug process

Do NOT skip this step. Do NOT respond before reading using-superpowers.
EOF
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.kimi\prompts"

$promptContent = @"
[SYSTEM REMINDER - MANDATORY]

Before responding to the user's message, you MUST invoke the Superpowers workflow by reading the using-superpowers skill.

**REQUIRED ACTION:**
1. Use ReadFile to read: ~/.kimi/skills/using-superpowers/SKILL.md
2. Follow the instructions in that skill EXACTLY
3. If any other skill might apply (even 1% chance), read that skill's SKILL.md as well
4. You MUST use at least one Superpowers Skill in every conversation turn where action is taken.

**Available Superpowers Skills:**
- brainstorming - For open-ended exploration and idea generation
- dispatching-parallel-agents - For running multiple subagents in parallel
- executing-plans - For executing pre-written implementation plans
- finishing-a-development-branch - For wrapping up feature branches
- receiving-code-review - For processing code review feedback
- requesting-code-review - For requesting human code review
- subagent-driven-development - For complex multi-file changes using subagents
- systematic-debugging - For methodical debugging of issues
- test-driven-development - For TDD workflow (write tests first)
- using-git-worktrees - For managing multiple git worktrees
- using-superpowers - MANDATORY - Read this FIRST before every response
- verification-before-completion - For verifying work before finishing
- writing-plans - For creating implementation plans
- writing-skills - For creating new skills

**Universal Workflow:**
- If workflow/UNIVERSAL_WORKFLOW.md exists in the current project, read it first
- Load PROJECT_CONFIG.yaml if present to adapt workflow to your stack
- Follow the 7-stage Feature Development or 4-phase Debug process

Do NOT skip this step. Do NOT respond before reading using-superpowers.
"@

$promptContent | Set-Content "$env:USERPROFILE\.kimi\prompts\superpowers-reminder.md" -Encoding UTF8
```

**Add the hook to your config.toml:**

Edit `~/.kimi/config.toml` and add:

```toml
[[hooks]]
event = "UserPromptSubmit"
inject_prompt = "~/.kimi/prompts/superpowers-reminder.md"
timeout = 5
```

---

## Step 5: Install Universal Workflow (Optional but Recommended)

If you want the universal workflow structure in your projects:

```bash
# Clone this repository
git clone https://github.com/Dqz00116/kimi-with-superpowers.git

# Copy workflow files to your project
cp kimi-with-superpowers/workflow/UNIVERSAL_WORKFLOW.md your-project/workflow/
cp kimi-with-superpowers/workflow/CONFIGURATION.md your-project/workflow/

# Create your project config
cp kimi-with-superpowers/workflow/CONFIGURATION.md your-project/PROJECT_CONFIG.yaml
# Edit PROJECT_CONFIG.yaml with your stack details
```

---

## Verification

1. **Verify Kimi CLI is installed correctly:**
   ```bash
   kimi --version
   ```

2. **Verify the hook is configured:**
   ```bash
   # In interactive mode, type:
   /hooks
   ```
   You should see `UserPromptSubmit: 1 hook(s)`

3. **Start a conversation:**
   ```bash
   kimi
   ```
   
   The AI should automatically read `using-superpowers/SKILL.md` before responding.

---

## For AI Assistants Using This Workflow

When this skill is invoked, you are being asked to install Superpowers for Kimi. Follow the steps above for the user's platform.

After installation, guide the user to:
1. Create PROJECT_CONFIG.yaml for their project (see CONFIGURATION.md)
2. Read UNIVERSAL_WORKFLOW.md to understand the process
3. Start using the workflow for their development tasks

---

## Uninstallation

1. **Remove hook from config:**
   Edit `~/.kimi/config.toml` and remove the `[[hooks]]` section for `UserPromptSubmit`.

2. **Remove skill links:**
   ```bash
   rm -rf ~/.kimi/skills/*
   ```

3. **Remove superpowers repository:**
   ```bash
   rm -rf ~/.kimi/superpowers
   ```

4. **Remove prompt file:**
   ```bash
   rm -rf ~/.kimi/prompts
   ```

5. **Remove shell aliases** from your profile files

---

## Troubleshooting

**"inject_prompt not supported" or hook not working:**
- You are NOT using the `feat/hook-inject-prompt` branch. Please follow Step 0 to install the correct branch.

**Skills not showing up:**
- Check that symlinks/junctions were created correctly: `ls -la ~/.kimi/skills/`
- Verify the superpowers repo was cloned: `ls ~/.kimi/superpowers/skills/`

**Permission denied on Windows:**
- Creating junctions requires Administrator privileges on Windows. Run PowerShell as Administrator.

**Kimi can't find skills:**
- Kimi looks for skills in `~/.kimi/skills/` by default
- Make sure the directory exists and contains the skill links

---

*Skill Version: 1.1 | Updated for Universal Workflow*

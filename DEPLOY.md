# Agentic Layer Deployment Guide

> How to add the universal agentic layer to any new project

## What is the Agentic Layer?

The **Agentic Layer** is a framework that wraps your Application Layer, providing a programmatic interface for AI-driven development workflows. Instead of directly modifying code yourself, you template your engineering processes and teach agents how to operate your codebase.

**Key Principle**: Scale compute to scale your impact.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    AGENTIC LAYER                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   .claude/   │  │    adws/     │  │    specs/    │     │
│  │  (prompts)   │  │ (workflows)  │  │   (plans)    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   agents/    │  │   ai_docs/   │  │  app_docs/   │     │
│  │   (logs)     │  │    (docs)    │  │    (docs)    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   APPLICATION LAYER                         │
│           (Your actual application code)                    │
└─────────────────────────────────────────────────────────────┘
```

## 🎯 ECE Integration (Elite Context Engineering)

**NEW**: This framework now includes **Elite Context Engineering (ECE) integration** for maximum efficiency.

### What is ECE Integration?

ECE optimizes token usage and context management in workflows, providing:
- **90% token reduction** per workflow phase through output styles
- **Linear context growth** instead of exponential through context handoff
- **Measurable ROI** with built-in metrics tracking
- **Zero breaking changes** - works with all existing workflows

### Key Components Included

1. **Output Styles** (`.claude/output-styles/`)
   - `concise-done.md` - Minimal responses (~10 tokens)
   - `concise-ultra.md` - Brief responses (~50 tokens)
   - `verbose-yaml-structured.md` - Structured detailed (~300 tokens)

2. **Context Handoff** (`adw_modules/context_handoff.py`)
   - Pass only essential data between phases
   - No context stacking
   - Validated schemas per phase

3. **Metrics Tracking** (`adw_modules/metrics.py`)
   - Track token usage and costs
   - Measure optimization effectiveness
   - Export to CSV for analysis

### Leverage Formula

```
Single workflow without ECE: 15,000 tokens ($0.225)
Single workflow with ECE:     1,400 tokens ($0.021)
Savings per workflow: 91%

At scale (50 workflows/month):
  Cost reduction: ~$10/month
  Annual savings: ~$120/year
  Plus: Faster execution, fewer errors, better quality
```

### How to Use ECE Features

All workflows automatically support ECE. To enable optimization:

```python
# In any workflow script
from adw_modules.metrics import record_phase_metrics
from adw_modules.context_handoff import ContextHandoff

# Use output style for token optimization
request = AgentTemplateRequest(
    agent_name="planner",
    slash_command="/feature",
    args=["123", adw_id, issue_json],
    adw_id=adw_id,
    output_style="concise-ultra"  # 90% token reduction
)

# Use context handoff for efficient phase transitions
handoff = ContextHandoff(adw_id)
handoff.save("plan", {
    "plan_file": plan_file_path,
    "issue_number": issue_number
})

# Track metrics
metrics = WorkflowMetrics(adw_id)
summary = metrics.get_workflow_summary()
print(f"Optimization: {summary['optimization_rate']*100}%")
```

See `ECE_INTEGRATION_COMPLETE.md` for detailed integration guide.

---

## Quick Start

### Option 1: Copy from invoiceDB (Recommended - Full Framework + ECE)

```bash
# Navigate to your new project
cd /path/to/your-new-project

# Copy agentic layer from tac-7
cp -r /path/to/TAC/tac-7/.claude .
cp -r /path/to/TAC/tac-7/adws .
mkdir -p specs agents ai_docs app_docs

# Copy configuration templates
cp /path/to/TAC/tac-7/.env.sample .
cp /path/to/TAC/tac-7/.gitignore .gitignore  # Merge with existing

# Install dependencies
cd adws
uv sync
```

### Option 2: Copy from tac-8/app1 (Minimal Framework)

```bash
# Navigate to your new project
cd /path/to/your-new-project

# Copy minimal agentic layer
cp -r /path/to/TAC/tac-8/tac8_app1__agent_layer_primitives/.claude .
cp -r /path/to/TAC/tac-8/tac8_app1__agent_layer_primitives/adws .
mkdir -p specs
```

## Directory Structure

After deployment, your project should have:

```
your-project/
├── .claude/                    # Agentic prompts & hooks
│   ├── commands/               # Slash commands
│   │   ├── chore.md           # Plan chores
│   │   ├── feature.md         # Plan features
│   │   ├── bug.md             # Plan bug fixes
│   │   ├── implement.md       # Execute plans
│   │   ├── test.md            # Create tests
│   │   ├── review.md          # Review code
│   │   ├── document.md        # Generate docs
│   │   ├── commit.md          # Create commits
│   │   ├── pull_request.md    # Create PRs
│   │   └── ... (30+ more)
│   ├── output-styles/         # ECE: Token optimization styles
│   │   ├── concise-done.md    # Minimal output (~10 tokens)
│   │   ├── concise-ultra.md   # Brief output (~50 tokens)
│   │   └── verbose-yaml-structured.md
│   └── hooks/                 # (Optional) Tool restrictions
│       ├── pre_tool_use.py
│       └── post_tool_use.py
│
├── adws/                       # AI Developer Workflows
│   ├── adw_modules/           # Core framework
│   │   ├── agent.py           # Claude Code CLI wrapper (ECE-enabled)
│   │   ├── data_types.py      # Type definitions (ECE fields added)
│   │   ├── state.py           # State management
│   │   ├── git_ops.py         # Git operations
│   │   ├── worktree_ops.py    # Worktree management
│   │   ├── workflow_ops.py    # Workflow operations
│   │   ├── github.py          # GitHub integration
│   │   ├── context_handoff.py # ECE: Minimal inter-phase context
│   │   ├── metrics.py         # ECE: Token usage tracking
│   │   └── utils.py           # Utilities
│   ├── adw_triggers/          # Automation triggers
│   │   ├── trigger_webhook.py # Webhook-based
│   │   └── trigger_cron.py    # Scheduled
│   ├── adw_tests/             # Tests
│   └── adw_*.py               # Workflow scripts
│       ├── adw_plan_iso.py
│       ├── adw_build_iso.py
│       ├── adw_test_iso.py
│       ├── adw_review_iso.py
│       ├── adw_document_iso.py
│       ├── adw_ship_iso.py
│       ├── adw_sdlc_iso.py
│       └── ... (15+ workflows)
│
├── specs/                      # Implementation plans
├── agents/                     # Agent execution logs
│   └── {adw_id}/              # Per-workflow outputs
│       ├── {agent_name}/
│       │   ├── prompts/       # Saved prompts
│       │   ├── raw_output.jsonl
│       │   └── raw_output.json
│       ├── adw_state.json     # TAC: Workflow state
│       ├── context_handoff.json  # ECE: Minimal inter-phase context
│       └── metrics.json       # ECE: Token usage & cost metrics
│
├── ai_docs/                    # AI-generated documentation
├── app_docs/                   # Application documentation
├── trees/                      # Git worktrees (for isolated work)
│   └── {branch_name}/
│
└── your-app/                   # Your actual application
    ├── src/
    ├── tests/
    └── ...
```

## Configuration

### 1. Environment Variables

Create `.env` file in project root:

```bash
# Required
ANTHROPIC_API_KEY=sk-ant-...
CLAUDE_CODE_PATH=claude  # Or full path to Claude Code CLI

# Optional (for GitHub integration)
GITHUB_REPO_URL=https://github.com/owner/repo
GITHUB_PAT=ghp_...  # If using different account than gh auth

# Optional (for other integrations)
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=...
```

### 2. Update .gitignore

Add to your `.gitignore`:

```gitignore
# Agentic Layer outputs
agents/
trees/
ai_docs/
.env

# Python
__pycache__/
*.pyc
.venv/
uv.lock

# Logs
*.log
```

### 3. Initialize Git (if not already)

```bash
git init
git add .
git commit -m "Initialize project with agentic layer"
```

## Usage

### Manual Workflows

Run workflows directly from the command line:

```bash
# Plan a feature
./adws/adw_plan_iso.py 123  # Where 123 is issue number

# Build from a plan
./adws/adw_build_iso.py 123

# Full SDLC workflow (plan → build → test → review → ship)
./adws/adw_sdlc_iso.py 123

# Zero-Touch Execution (fully autonomous)
./adws/adw_sdlc_ZTE_iso.py 123
```

### Slash Commands (Interactive)

Use slash commands in Claude Code CLI:

```bash
# Plan a feature
claude /feature 123 {adw_id} {issue_json}

# Implement a plan
claude /implement specs/plan.md

# Run tests
claude /test

# Review code
claude /review

# Generate documentation
claude /document

# Create commit
claude /commit

# Create pull request
claude /pull_request
```

### Automated Workflows

#### Webhook Trigger (Real-time)

```bash
cd adws
uv run python adw_triggers/trigger_webhook.py
```

Listens for GitHub webhook events and automatically processes issues.

#### Cron Trigger (Polling)

```bash
cd adws
uv run python adw_triggers/trigger_cron.py
```

Polls GitHub every 20 seconds for new issues.

## Workflow Patterns

### Pattern 1: Issue-Driven Development

1. Create GitHub issue
2. Webhook/cron detects issue
3. Agent classifies issue (`/chore`, `/bug`, or `/feature`)
4. Agent generates plan in `specs/`
5. Agent implements plan
6. Agent runs tests
7. Agent creates PR
8. Human reviews and merges

### Pattern 2: Manual Planning

1. Run `/chore`, `/bug`, or `/feature` command
2. Agent researches codebase and creates plan
3. Review plan in `specs/`
4. Run `/implement specs/plan.md`
5. Agent executes plan
6. Manually test and commit

### Pattern 3: Interactive Development

1. Work in isolated worktree
2. Use slash commands as needed
3. Iterate with agent assistance
4. Merge back to main when done

## Customization

### Adding Custom Slash Commands

1. Create new file in `.claude/commands/`:

```markdown
# .claude/commands/my_command.md

## My Custom Command

This command does X, Y, and Z.

### Arguments

1. `arg1` - Description of first argument
2. `arg2` - Description of second argument

### Instructions

1. First, do this...
2. Then, do that...
3. Finally, output the result in this format...
```

2. Use it:

```bash
claude /my_command arg1 arg2
```

### Adding Custom Workflows

Create new workflow script in `adws/`:

```python
#!/usr/bin/env -S uv run --script
# /// script
# dependencies = ["python-dotenv"]
# ///

"""My custom workflow."""

import sys
from adw_modules.agent import execute_template
from adw_modules.data_types import AgentTemplateRequest
from adw_modules.state import ADWState
from adw_modules.utils import make_adw_id

def main():
    adw_id = make_adw_id()

    # Phase 1: Plan
    plan_request = AgentTemplateRequest(
        agent_name="planner",
        slash_command="/my_command",
        args=[sys.argv[1]],
        adw_id=adw_id
    )
    plan_response = execute_template(plan_request)

    # Phase 2: Execute
    # ... your logic here

if __name__ == "__main__":
    main()
```

### Adding Hooks

Create hooks in `.claude/hooks/` to control agent behavior:

```python
# .claude/hooks/pre_tool_use.py

def pre_tool_use(tool_name, args):
    """Called before each tool use."""
    # Return permission decision
    if tool_name == "Write" and "config.json" in args.get("file_path", ""):
        return {
            "hookSpecificOutput": {
                "permissionDecision": "deny",
                "permissionDecisionReason": "Cannot modify config.json"
            }
        }

    return {
        "hookSpecificOutput": {
            "permissionDecision": "allow"
        }
    }
```

## Troubleshooting

### Claude Code CLI not found

```bash
# Install Claude Code
# Visit: https://docs.anthropic.com/claude-code

# Verify installation
claude --version

# Set path in .env
CLAUDE_CODE_PATH=/path/to/claude
```

### Permission errors

```bash
# Make workflow scripts executable
chmod +x adws/*.py

# Or run with uv
uv run python adws/adw_plan_iso.py 123
```

### Worktree conflicts

```bash
# List all worktrees
git worktree list

# Remove old worktree
git worktree remove trees/old-branch

# Prune worktrees
git worktree prune
```

### Agent state issues

```bash
# View state
cat agents/{adw_id}/adw_state.json

# Delete state to start fresh
rm agents/{adw_id}/adw_state.json
```

## Best Practices

### 1. Start Small

Begin with basic commands:
- `/chore` for simple tasks
- `/implement` to execute plans
- `/test` to validate changes

### 2. Use Isolated Worktrees

For parallel development:
```bash
./adws/adw_plan_iso.py 123    # Creates isolated worktree
./adws/adw_plan_iso.py 456    # Creates another isolated worktree
# Both can run concurrently without conflicts
```

### 3. Review Agent Output

Always review:
- Plans in `specs/`
- Execution logs in `agents/`
- Generated code before committing

### 4. Iterate on Plans

If plan quality is poor:
- Improve issue descriptions
- Add more context to prompts
- Use `/patch` to refine plans

### 5. Use Model Selection Wisely

The framework supports model sets:
- `base` - Uses Sonnet (faster, cheaper)
- `heavy` - Uses Opus (smarter, more expensive)

Configure in ADW state or per-command.

## Advanced Features

### Multi-Agent Coordination

Run multiple workflows in parallel:

```bash
# Terminal 1
./adws/adw_sdlc_iso.py 123

# Terminal 2
./adws/adw_sdlc_iso.py 456

# Terminal 3
./adws/adw_sdlc_iso.py 789
```

Each gets isolated worktree and parallel execution.

### Session Resumption

Agents support resuming interrupted sessions:

```python
from adw_modules.agent import execute_template

# First execution
response = execute_template(request)
session_id = response.session_id

# Resume later
request_resume = request.model_copy(update={"resume": session_id})
response_resumed = execute_template(request_resume)
```

### GitHub Integration

Automated issue processing:

1. Configure GitHub webhook pointing to your server
2. Start webhook trigger:
   ```bash
   uv run python adws/adw_triggers/trigger_webhook.py
   ```
3. Create issues with special markers:
   ```markdown
   Title: Add feature X

   {{adw: adw_plan_build_iso}}
   {{model: opus}}

   Description of feature...
   ```
4. Agent automatically processes and creates PR

## Migration Guide

### From No Agentic Layer

1. Copy agentic layer files (see Quick Start)
2. Don't modify existing code
3. Start using slash commands
4. Gradually adopt workflows

### From Custom Agentic Layer

1. Compare your custom commands with standard ones
2. Merge custom commands into `.claude/commands/`
3. Migrate custom workflows to `adws/`
4. Update references to new module locations

### From Older Version

1. Backup your current agentic layer
2. Copy new version
3. Migrate custom modifications
4. Test workflows incrementally

## Examples

### Example 1: Add Feature

```bash
# 1. Create GitHub issue #123
# 2. Run planning workflow
./adws/adw_plan_iso.py 123

# 3. Review plan
cat specs/feature-123-*.md

# 4. Execute plan
./adws/adw_build_iso.py 123

# 5. Test
./adws/adw_test_iso.py 123

# 6. Review
./adws/adw_review_iso.py 123

# 7. Ship
./adws/adw_ship_iso.py 123
```

### Example 2: Fix Bug

```bash
# Use full SDLC workflow
./adws/adw_sdlc_iso.py 456

# Or zero-touch execution
./adws/adw_sdlc_ZTE_iso.py 456
```

### Example 3: Generate Documentation

```bash
# Interactive mode
claude /document

# Or programmatic
./adws/adw_document_iso.py 789
```

## FAQs

**Q: Do I need to modify my existing code?**
A: No. The agentic layer wraps your code without changing it.

**Q: Can I use this with any programming language?**
A: Yes. The agentic layer is language-agnostic.

**Q: How much does it cost?**
A: Depends on usage. Sonnet is ~$3/million tokens, Opus is ~$15/million tokens.

**Q: Is this replacing developers?**
A: No. It's augmenting developers by automating repetitive tasks.

**Q: What if I don't like what the agent did?**
A: Simply don't commit. The agent works in isolated worktrees.

**Q: Can I customize everything?**
A: Yes. All prompts, workflows, and hooks are customizable.

## Resources

- **tac-7**: Full production example (SQL app with ADW)
- **tac-8/app1**: Minimal framework example
- **building-specialized-agents**: Custom agent examples
- **Claude Code Docs**: https://docs.anthropic.com/claude-code

## Support

For issues or questions:
1. Check `agents/` logs for detailed error messages
2. Review ADW state in `agents/{adw_id}/adw_state.json`
3. Consult workflow-specific README files
4. Check Claude Code CLI version: `claude --version`

---

**Next Steps:**
1. Copy agentic layer to your project
2. Configure `.env`
3. Run your first workflow
4. Iterate and customize

Happy agentic coding! 🤖

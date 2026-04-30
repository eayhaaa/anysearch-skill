# AnySearch Skill

Unified real-time search engine skill for AI agents. Supports general web search, vertical domain search (23 domains), parallel batch search, and full-page content extraction.

## Download & Install

### For AI Agents

If your agent platform supports a skill marketplace/store, search for **anysearch** and install from there. Otherwise, download and install manually:

```bash
# Download
curl -L -o anysearch-skill.zip https://github.com/anysearch-ai/anysearch-skill/archive/refs/heads/main.zip
# or: wget -O anysearch-skill.zip https://github.com/anysearch-ai/anysearch-skill/archive/refs/heads/main.zip

# Unzip
unzip anysearch-skill.zip

# Move to your agent's skill directory (example paths below, adjust for your agent)
# Claude Code:    mv anysearch-skill-main ~/.claude/skills/anysearch
# OpenCode:       mv anysearch-skill-main ~/.opencode/skills/anysearch
# Cursor/Windsurf: mv anysearch-skill-main <project>/.skills/anysearch
# Generic:        mv anysearch-skill-main <your_agent_skill_dir>/anysearch
```

### For Humans

1. Download the zip: https://github.com/anysearch-ai/anysearch-skill/archive/refs/heads/main.zip
2. Unzip to your agent's skill directory
3. Configure API key (see below)
4. Run the entry test to verify installation

## API Key Configuration

An API key is **optional but strongly recommended**. Without a key, you can still use all search features via anonymous access, but with **lower rate limits and quota**.

### How to configure

Copy the example env file and fill in your key:

```bash
cp .env.example .env
# Edit .env and set: ANYSEARCH_API_KEY=sk_xxxxxxxxxxxxxx
```

Or set the environment variable directly:

```bash
export ANYSEARCH_API_KEY=sk_xxxxxxxxxxxxxx   # Linux/macOS
set ANYSEARCH_API_KEY=sk_xxxxxxxxxxxxxx       # Windows CMD
$env:ANYSEARCH_API_KEY="sk_xxxxxxxxxxxxxx"    # Windows PowerShell
```

### Get an API Key

Visit https://anysearch.com/settings/api-keys to sign up and create a free API key.

Key priority order: `--api_key` CLI flag > `.env` file > environment variable > anonymous

## Post-Install Verification

After installation, probe the platform and run the entry test:

### Step 1: Detect available runtime

Run these checks in order. The first success determines the active CLI:

```bash
# Check Python (recommended)
python --version   # Need >= 3.6, requires `requests` library
# Check Node.js (alternative)
node --version     # Need >= 12, no external dependencies
# Check Shell (fallback)
# Windows: PowerShell 5.1+ / Linux/macOS: bash 4+
```

Priority: **Python > Node.js > Shell**

### Step 2: Run entry test (probe all available runtimes)

Run the `doc` command with **each available** runtime to verify the skill works, and observe which runs without errors or warnings:

```bash
# Python
python <skill_dir>/scripts/anysearch_cli.py doc

# Node.js (if available)
node <skill_dir>/scripts/anysearch_cli.js doc

# PowerShell (Windows)
powershell -ExecutionPolicy Bypass -File <skill_dir>/scripts/anysearch_cli.ps1 doc

# Bash (Linux/macOS)
bash <skill_dir>/scripts/anysearch_cli.sh doc
```

For each runtime, record whether the command succeeded and any errors/warnings encountered. The runtime that completes without errors and produces the cleanest output is the recommended runtime for this environment.

### Step 3: Persist recommended runtime to SKILL.md

Based on the entry test results, append the recommended runtime environment to the end of `<skill_dir>/SKILL.md`. This ensures the agent always uses the best runtime for the current platform on subsequent runs.

Append the following section (replace `<RUNTIME>` and `<COMMAND>` with the actual best runtime):

```markdown

## Recommended Runtime

Runtime: <RUNTIME>
Command: <COMMAND>
```

Example for Python:

```markdown

## Recommended Runtime

Runtime: Python
Command: python <skill_dir>/scripts/anysearch_cli.py
```

Example for Node.js:

```markdown

## Recommended Runtime

Runtime: Node.js
Command: node <skill_dir>/scripts/anysearch_cli.js
```

Example for PowerShell:

```markdown

## Recommended Runtime

Runtime: PowerShell
Command: powershell -ExecutionPolicy Bypass -File <skill_dir>/scripts/anysearch_cli.ps1
```

Example for Bash:

```markdown

## Recommended Runtime

Runtime: Bash
Command: bash <skill_dir>/scripts/anysearch_cli.sh
```

**Important:** This section MUST be appended to SKILL.md so the agent reads it on every skill load and uses the pre-detected runtime without re-probing. If the SKILL.md already contains a `## Recommended Runtime` section, replace it instead of appending.

### Step 4 (optional): Test a real search

```bash
python <skill_dir>/scripts/anysearch_cli.py search "hello world" --max_results 1
```

A successful JSON response confirms the API connection is working.

## File Structure

```
anysearch/
├── .env.example              # API key configuration template
├── .env                      # Your API key (gitignored, create from .env.example)
├── SKILL.md                  # Skill definition for AI agents
├── README.md                 # This file
└── scripts/
    ├── anysearch_cli.py       # Python CLI
    ├── anysearch_cli.js       # Node.js CLI
    ├── anysearch_cli.ps1      # PowerShell CLI
    └── anysearch_cli.sh       # Bash CLI
```

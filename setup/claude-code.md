# Setup Guide — Tier 2: Claude Code (Recommended)

Claude Code is a terminal-based AI agent built by Anthropic. It is the platform
ai4soc was developed and fully tested on, and it provides the best experience
for this course.

Unlike chat interfaces, Claude Code works directly with your files, runs tasks
in parallel, executes code, and remembers your project context through `CLAUDE.md`
— all from your terminal.

---

## Requirements

| Requirement | Details |
|-------------|---------|
| Subscription | Claude Pro or Max at [claude.ai](https://claude.ai) |
| Node.js | Version 18 or higher |
| OS | macOS, Linux, or Windows via WSL |
| Terminal | Any standard terminal application |
| Disk space | ~200 MB for the package |

> **No Claude Pro or Max subscription?** Use Tier 0 (Ollama) to start for free.
> You can switch to Claude Code at any time — your course progress and lab files
> carry over since everything is just files in a folder.

---

## Step 1 — Check or Install Node.js

Claude Code requires Node.js 18 or higher.

```bash
node --version
```

If you see `v18.x.x` or higher, skip to Step 2.

If you see a lower version or "command not found":

**macOS:**
```bash
# Install via Homebrew (recommended)
brew install node

# Or download from nodejs.org
```

**Linux (Ubuntu / Debian):**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

**Windows:**
Download the LTS installer from [nodejs.org](https://nodejs.org). Claude Code
works best on Windows via WSL — see [`windows-wsl.md`](windows-wsl.md) if you
have not set that up yet.

Verify after installation:
```bash
node --version   # Should show v18.x.x or higher
npm --version    # Should show a version number
```

---

## Step 2 — Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

This installs Claude Code globally so you can run it from any directory.

Verify the installation:
```bash
claude --version
```

You should see a version number. If you see "command not found", close and reopen
your terminal and try again.

---

## Step 3 — Authenticate

Run Claude Code for the first time:

```bash
claude
```

On first launch, Claude Code will prompt you to authenticate. It will open a
browser window pointing to claude.ai. Sign in with your Anthropic account.

Once authenticated, the browser will redirect and you can close it. Return to
your terminal — Claude Code should now be running and waiting for your first message.

Type a quick test:
```
What does a SOC analyst do?
```

If you get a coherent response, authentication is working. Type `exit` or press
`Ctrl+C` to quit.

---

## Step 4 — Clone ai4soc and Start Your First Session

```bash
git clone https://github.com/blueteamid/ai4soc.git
cd ai4soc
claude
```

When Claude Code starts inside the `ai4soc/` directory, it automatically reads
`CLAUDE.md` and loads your full SOC context. You do not need to do anything else —
the agent now knows it is operating as a SOC analyst assistant for PT Nusantara Digital.

Confirm it loaded correctly by asking:
```
What is my current SOC environment and what slash commands are available?
```

The agent should describe PT Nusantara Digital, the lab security stack, and list
the available slash commands from `CLAUDE.md`. If it gives a generic response
instead, see the troubleshooting section below.

---

## Step 5 — Run Your First Slash Command

```
/start
```

This begins the course from Module 0. The agent will guide you through the
introduction and first exercises interactively.

If you have already completed Module 0 and are returning:
```
/check
```

This shows your current progress and where to pick up.

---

## Key Claude Code Concepts for This Course

Understanding how Claude Code works will help you get more out of every session.

### Project Memory — CLAUDE.md

Every time you start Claude Code inside the `ai4soc/` folder, it reads `CLAUDE.md`
automatically. This file contains your SOC context — the fictional organization,
the security stack, the analyst roles, the output templates, and the slash commands.

This is why you must always start Claude Code from inside the `ai4soc/` directory:

```bash
cd ai4soc    # Always navigate here first
claude       # Then start the agent
```

### File Operations

Claude Code can read, write, and edit files directly. You can ask it to:

```
Read the alert file at lab/alerts/scenario-01-alert-bruteforce.json
and save a triage report to outputs/2025-01-14/triage-scenario-01.md
```

It will do both operations without you touching the files manually.

### Parallel Agents

For advanced exercises in Module 1.4, Claude Code can run multiple analysis
tasks simultaneously:

```
Triage all three alert files in lab/alerts/ in parallel and produce
a separate triage report for each one.
```

Claude Code will spawn parallel subagents, each working on one file, and produce
three reports simultaneously. This is one of the most powerful capabilities for
high-volume SOC work.

### Slash Commands

The slash commands defined in `CLAUDE.md` are shortcuts to common workflows.
Type them at the start of a message:

```
/triage lab/alerts/scenario-01-alert-bruteforce.json
/ir Active ransomware on WKSTN-FIN-042
/detect PowerShell spawned from Office process
/report weekly
```

---

## Useful Claude Code Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Interrupt current operation |
| `Ctrl+L` | Clear the screen |
| `↑` arrow | Recall previous message |
| `Escape` | Cancel current input |

---

## Managing Your Session

**Start a session:**
```bash
cd ai4soc
claude
```

**Exit cleanly:**
```
exit
```
Or press `Ctrl+C`.

**Resume after closing:**
Simply run `claude` again from inside the `ai4soc/` directory. Claude Code will
reload `CLAUDE.md` context. Note that conversation history from previous sessions
is not automatically carried over — use `outputs/` to preserve your work between
sessions.

**Check your plan and usage:**
Visit [claude.ai](https://claude.ai) to see your subscription status and usage.

---

## Troubleshooting

**Authentication fails or expires:**
```bash
claude auth logout
claude auth login
```

**CLAUDE.md not loading (agent gives generic responses):**
Confirm you are running `claude` from inside the `ai4soc/` directory and not
from a parent folder. Claude Code looks for `CLAUDE.md` in the current working
directory.

```bash
pwd          # Should show .../ai4soc
ls CLAUDE.md # Should find the file
claude       # Now start the agent
```

**"command not found" after install:**
```bash
# Check where npm installs global packages
npm config get prefix

# Add the bin directory to your PATH if needed (Linux/macOS)
export PATH="$(npm config get prefix)/bin:$PATH"

# Add this line to ~/.bashrc or ~/.zshrc to make it permanent
echo 'export PATH="$(npm config get prefix)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Rate limit errors:**
Claude Code has usage limits depending on your subscription plan. If you hit
a limit, wait a few minutes and continue. For heavy parallel agent use, pace
your exercises across a session.

**Slow responses:**
Claude Code response time depends on Anthropic's API. During peak hours,
responses may be slightly slower. This is normal.

**Windows-specific issues:**
If you are running Claude Code on native Windows (not WSL) and encounter
issues, see [`windows-wsl.md`](windows-wsl.md) for the WSL setup guide.
Claude Code is more stable in a Linux environment.

---

## Security Notes

Claude Code sends your messages and file contents to Anthropic's API for processing.
This means:

- Do not feed real incident data, credentials, or sensitive organizational information
  into Claude Code sessions
- Always use synthetic lab data from the `lab/` directory for this course
- If you bring real logs into a session for practice, sanitize them first —
  see `docs/sanitization-guide.md`
- Anthropic's data handling and privacy policy applies to all Claude Code sessions:
  [anthropic.com/privacy](https://anthropic.com/privacy)

---

## Next Steps

Claude Code is installed and authenticated. Time to start the course.

**[0.2 Clone the Repo & Explore the Lab](../modules/0-getting-started/0.2-setup.md)**

Or if you have already cloned the repo, start directly:

```bash
cd ai4soc
claude
/start
```

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*

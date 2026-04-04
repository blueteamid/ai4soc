# Setup Guide — Tier 0: Ollama (Free, Local, Offline)

Ollama runs AI models entirely on your machine. No internet connection required
after the initial model download. No account needed. No data leaves your computer.

This is the recommended starting point if you have budget or credit card access
constraints, operate in an air-gapped or restricted network environment, or simply
prefer to keep all data local.

---

## System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| RAM | 8 GB | 16 GB or more |
| Disk space | 5 GB free | 20 GB free |
| OS | macOS 12+, Windows 10+, Ubuntu 20.04+ | Latest stable |
| CPU | Any modern x86-64 | Apple Silicon or recent Intel/AMD |
| GPU | Not required | NVIDIA GPU significantly improves speed |

> **Apple Silicon (M1/M2/M3/M4):** Ollama runs exceptionally well on Apple Silicon
> due to unified memory architecture. An M2 with 16 GB handles 14B models comfortably.

---

## Step 1 — Install Ollama

### macOS

Download the installer from [ollama.com/download](https://ollama.com/download)
and run the `.dmg` file. Drag Ollama to your Applications folder.

Or via Homebrew:
```bash
brew install ollama
```

### Linux

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

This installs Ollama as a system service that starts automatically.

### Windows

Download the Windows installer from [ollama.com/download](https://ollama.com/download).
Run the `.exe` and follow the prompts.

> **Windows note:** For the best experience with ai4soc on Windows, we recommend
> running Ollama through WSL (Windows Subsystem for Linux). See
> [`windows-wsl.md`](windows-wsl.md) for setup instructions.

### Verify Installation

```bash
ollama --version
```

Expected output: `ollama version 0.x.x` or similar. If you see "command not found",
close and reopen your terminal, then try again.

---

## Step 2 — Choose and Pull a Model

Choose based on your available RAM. When in doubt, start with the smallest model
and upgrade once you confirm everything works.

```bash
# 16 GB RAM or more — best quality for SOC analysis tasks
ollama pull qwen2.5-coder:14b

# 8 GB RAM — good balance of quality and speed
ollama pull deepseek-r1:8b

# 4 GB RAM — lightweight, good for getting started
ollama pull llama3.2:3b
```

Download progress is shown in the terminal. Each model is 4–10 GB so this may
take several minutes depending on your connection speed. Downloads resume
automatically if interrupted.

> **Model disclaimer:** The AI landscape evolves fast. These are reference models
> at the time of writing. Before pulling a model, check
> [ollama.com/library](https://ollama.com/library) and filter by `instruct` or
> `code` tags for the latest options. A newer model may offer better reasoning,
> lower resource usage, or stronger performance on security analysis tasks.

---

## Step 3 — Verify the Model Works

```bash
ollama run qwen2.5-coder:14b
```

At the `>>>` prompt, type:
```
You are a SOC analyst assistant. What are the key steps in alert triage?
```

You should receive a structured, coherent response about alert triage. If the
response is slow, that is normal for CPU-only inference — it will improve with
a GPU or a smaller model.

Type `/bye` to exit.

---

## Step 4 — Connect Ollama to ai4soc

Ollama on its own is a model runner — it does not have a built-in agentic
interface with file operations and project memory. You have two options to
bridge this gap.

### Option A — Open WebUI (Recommended)

Open WebUI provides a full chat interface with system prompt support, conversation
history, and multi-model switching. It connects directly to your local Ollama
instance.

**Install Open WebUI:**

If you have Docker installed:
```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

If you do not have Docker, install it first:
- macOS / Windows: [docker.com/products/docker-desktop](https://docker.com/products/docker-desktop)
- Linux: `curl -fsSL https://get.docker.com | sh`

> **Learning tip:** Docker is a foundational tool in modern SOC environments for
> running isolated services. If you are new to Docker, the official getting-started
> guide at [docs.docker.com](https://docs.docker.com/get-started/) is an excellent
> next step after this course.

**Open the interface:**

Go to [http://localhost:3000](http://localhost:3000) in your browser. Create a
local account (data stays on your machine) and select your Ollama model.

**Load ai4soc context:**

In Open WebUI, go to **Settings → System Prompt** and paste the full contents
of `CLAUDE.md` from your ai4soc directory. This gives the model persistent SOC
context for your session.

```bash
# Copy CLAUDE.md contents to clipboard (macOS)
cat /path/to/ai4soc/CLAUDE.md | pbcopy

# Copy CLAUDE.md contents to clipboard (Linux with xclip)
cat /path/to/ai4soc/CLAUDE.md | xclip -selection clipboard
```

### Option B — Terminal with System Prompt

For a quick start without Docker, run Ollama directly with the CLAUDE.md as
your system prompt:

```bash
cd /path/to/ai4soc
ollama run qwen2.5-coder:14b --system "$(cat CLAUDE.md)"
```

This loads the full SOC context into the model for your session. The context
is not persistent between sessions — you need to re-run this command each time.

**To work with lab files**, copy the relevant content into the conversation:

```bash
# Read a lab file and pass it to Ollama
echo "Please triage this alert:" && cat lab/alerts/scenario-01-alert-bruteforce.json | ollama run qwen2.5-coder:14b --system "$(cat CLAUDE.md)"
```

---

## Step 5 — Test with ai4soc Lab Data

Navigate to your ai4soc directory and run a triage exercise:

**In Open WebUI:**
1. Open a new chat
2. Type: `Please read and triage the following alert:` then paste the contents
   of `lab/alerts/scenario-01-alert-bruteforce.json`
3. Follow the analysis prompts from Module 0.3

**In terminal:**
```bash
cd ai4soc
cat lab/alerts/scenario-01-alert-bruteforce.json | ollama run qwen2.5-coder:14b \
  --system "$(cat CLAUDE.md)" \
  "Please triage this alert and produce a triage summary using the template in your context."
```

---

## Known Limitations — Tier 0

Compared to Claude Code (Tier 2), Ollama has these limitations in this course:

| Feature | Ollama | Claude Code |
|---------|--------|-------------|
| File operations (read/write directly) | Manual copy-paste | Native |
| Parallel agents | Not supported | Supported |
| Slash commands (`/triage`, `/ir`) | Not supported | Supported |
| Project memory (CLAUDE.md auto-load) | Manual each session | Automatic |
| Internet access for enrichment | No | Via tools |
| Response speed | Slower (CPU) | Fast |

Modules in this course will note when a feature requires Tier 2 and provide
a Tier 0 workaround where possible.

---

## Performance Tips

**Improve response speed:**
- Close other applications to free RAM
- If you have an NVIDIA GPU, Ollama uses it automatically — no configuration needed
- Use a smaller model for faster iteration, switch to a larger one for final analysis

**Improve response quality:**
- Provide more context — paste the full alert or log file, not just a summary
- Ask for step-by-step reasoning: "Think through this step by step before giving
  your verdict"
- If a response is wrong, correct it explicitly: "That is incorrect because X.
  Please revise your assessment."

**Manage RAM usage:**
```bash
# Check which models are loaded
ollama ps

# Stop a running model to free memory
ollama stop qwen2.5-coder:14b
```

---

## Troubleshooting

**Ollama service not running:**
```bash
# macOS / Linux — start the service
ollama serve

# Check if it is running
curl http://localhost:11434/api/tags
```

**Model download interrupted:**
Simply re-run `ollama pull model-name` — the download resumes from where it stopped.

**Out of memory / model crashes:**
Switch to a smaller model. If `deepseek-r1:8b` crashes, try `llama3.2:3b`.

**Very slow inference:**
Normal for CPU-only. A 14B model on CPU may take 30–60 seconds per response.
Switch to a 3B or 7B model for faster iteration.

**Open WebUI not connecting to Ollama:**
Ensure Ollama is running (`ollama serve` in a separate terminal) and that the
Docker container was started with `--add-host=host.docker.internal:host-gateway`.

---

## Next Steps

Once Ollama is running and you have tested it with a lab file, return to the course:

**[0.2 Clone the Repo & Explore the Lab](../modules/0-getting-started/0.2-setup.md)**

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*

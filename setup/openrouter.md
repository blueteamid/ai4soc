# Setup Guide — Tier 1: OpenRouter (Pay-per-Use, Cloud)

OpenRouter gives you access to many AI models — including Claude, GPT-4o, Gemini,
and Mistral — without a monthly subscription. You pay only for what you use.

Completing this entire course typically costs under $2 USD in API credits.

---

## Requirements

| Requirement | Details |
|-------------|---------|
| Account | [openrouter.ai](https://openrouter.ai) |
| Payment | Credit card for billing |
| Credits | $5 minimum recommended to complete the course |
| Internet | Required for all sessions |

> ⚠️ Cloud service — always sanitize any real data before use.
> Lab data in `lab/` is synthetic and safe.

---

## Step 1 — Create an Account and Add Credits

1. Go to [openrouter.ai](https://openrouter.ai) and sign up
2. Navigate to **Credits** in your dashboard
3. Add $5–10 USD — more than enough for this course
4. Go to **Keys** and click **Create Key**
5. Name it `ai4soc` and copy the key — you will not see it again

Store your key safely:
```bash
# Add to your shell profile to persist across sessions (Linux/macOS)
echo 'export OPENROUTER_API_KEY=sk-or-your-key-here' >> ~/.zshrc
source ~/.zshrc

# Verify
echo $OPENROUTER_API_KEY
```

---

## Step 2 — Choose an Interface

OpenRouter exposes an OpenAI-compatible API, so it works with many existing
tools. Two options work well for this course:

### Option A — Aider (Recommended for Tier 1)

Aider is an open-source terminal AI coding agent similar to Claude Code.
It supports file operations and can load `CLAUDE.md` as context.

```bash
pip install aider-chat
```

Start a session inside ai4soc:
```bash
cd ai4soc
aider \
  --openai-api-base https://openrouter.ai/api/v1 \
  --openai-api-key $OPENROUTER_API_KEY \
  --model anthropic/claude-sonnet-4-5 \
  --read CLAUDE.md
```

### Option B — Continue (VS Code Extension)

If you prefer a visual interface inside VS Code:

1. Install the **Continue** extension from the VS Code marketplace
2. Open Continue settings (`~/.continue/config.json`) and add:

```json
{
  "models": [
    {
      "title": "Claude via OpenRouter",
      "provider": "openai",
      "model": "anthropic/claude-sonnet-4-5",
      "apiBase": "https://openrouter.ai/api/v1",
      "apiKey": "sk-or-your-key-here"
    }
  ]
}
```

3. Open the ai4soc folder in VS Code
4. In Continue, paste `CLAUDE.md` contents as your system prompt

---

## Step 3 — Recommended Models

> **Model disclaimer:** The AI landscape evolves rapidly. These are reference points
> at the time of writing. Always check [openrouter.ai/models](https://openrouter.ai/models)
> for the latest options — newer models may offer better performance at lower cost.

| Model | OpenRouter ID | Strength | Approx. Cost per Session |
|-------|--------------|----------|--------------------------|
| Claude Sonnet | `anthropic/claude-sonnet-4-5` | Best reasoning for SOC tasks | ~$0.15 |
| GPT-4o Mini | `openai/gpt-4o-mini` | Fast, cheap, good quality | ~$0.03 |
| Gemini Flash | `google/gemini-flash-1.5` | Very fast, very cheap | ~$0.01 |
| Mistral | `mistralai/mistral-7b-instruct` | Open weights, affordable | ~$0.02 |

For this course, `anthropic/claude-sonnet-4-5` gives the closest experience to
the Claude Code (Tier 2) environment since it uses the same underlying model.

---

## Step 4 — Load SOC Context

With Aider, `CLAUDE.md` is loaded via `--read CLAUDE.md`. Verify it loaded:

```
What SOC environment am I working in and what slash commands are available?
```

The agent should describe PT Nusantara Digital and the available commands.

---

## Known Limitations — Tier 1

| Feature | OpenRouter + Aider | Claude Code |
|---------|--------------------|-------------|
| File operations | Partial (Aider supports edits) | Full native |
| Parallel agents | Not supported | Supported |
| Slash commands | Not supported natively | Full support |
| CLAUDE.md auto-load | Manual via `--read` flag | Automatic |
| Cost | Pay per use | Flat subscription |

---

## Troubleshooting

**API key not working:**
Verify the key starts with `sk-or-` and that you have credits in your OpenRouter
account. Free accounts without credits will return 402 errors.

**Model not found:**
Check the exact model ID at [openrouter.ai/models](https://openrouter.ai/models).
Model IDs are case-sensitive and change when new versions release.

**pip install fails:**
```bash
# Try with user flag
pip install --user aider-chat

# Or use a virtual environment
python3 -m venv ai4soc-env
source ai4soc-env/bin/activate
pip install aider-chat
```

---

## Next Steps

**[0.2 Clone the Repo & Explore the Lab](../modules/0-getting-started/0.2-setup.md)**

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*

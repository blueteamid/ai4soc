# Setup Guide — Windows: WSL (Windows Subsystem for Linux)

Claude Code and Ollama both work better on Linux. If you are on Windows,
setting up WSL gives you a full Linux environment inside Windows — no dual
boot, no virtual machine complexity.

This guide takes about 10–15 minutes.

---

## What Is WSL?

WSL (Windows Subsystem for Linux) runs a real Linux kernel inside Windows.
You get a Linux terminal, Linux file system, and full compatibility with
Linux-based tools — including Claude Code, Ollama, Git, and Python.

All your Windows files are accessible from WSL at `/mnt/c/`.

---

## Requirements

- Windows 10 version 2004 or higher (build 19041+)
- Windows 11 (any version)
- Administrator access
- ~5 GB free disk space

Check your Windows version: `Win + R` → type `winver` → press Enter.

---

## Step 1 — Install WSL

Open **PowerShell as Administrator** (right-click the Start button → Windows
PowerShell (Admin)) and run:

```powershell
wsl --install
```

This installs WSL 2 with Ubuntu as the default distribution. Restart your
computer when prompted.

After restart, Ubuntu will finish installing and ask you to create a username
and password. These are your Linux credentials — choose anything you like.

Verify WSL is running correctly:
```powershell
wsl --list --verbose
```

You should see Ubuntu listed with VERSION 2.

---

## Step 2 — Open Your Linux Terminal

From now on, use the **Ubuntu** app (search for it in the Start menu) or
**Windows Terminal** with the Ubuntu profile. Do not use PowerShell or
Command Prompt for ai4soc work.

Update your Linux packages on first launch:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 3 — Install Core Tools

Inside your Ubuntu terminal, install the tools needed for this course:

```bash
# Git
sudo apt install -y git curl wget

# Node.js 20 (for Claude Code)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# Python (for various tools)
sudo apt install -y python3 python3-pip

# Verify
node --version    # Should show v20.x.x or higher
git --version
python3 --version
```

---

## Step 4 — Install Your AI Agent

Follow the relevant setup guide now that you have a Linux environment:

**For Claude Code (Tier 2):**
```bash
npm install -g @anthropic-ai/claude-code
claude --version
```
Then follow [`claude-code.md`](claude-code.md) from Step 3 onwards.

**For Ollama (Tier 0):**
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama --version
```
Then follow [`ollama.md`](ollama.md) from Step 2 onwards.

---

## Step 5 — Clone ai4soc

Inside your Ubuntu terminal:
```bash
cd ~
git clone https://github.com/blueteamid/ai4soc.git
cd ai4soc
```

Your files live in the Linux home directory (`~`), accessible from Windows
at `\\wsl$\Ubuntu\home\your-username\ai4soc`.

---

## Accessing Files Between Windows and WSL

**From WSL, access Windows files:**
```bash
ls /mnt/c/Users/YourWindowsUsername/
```

**From Windows, access WSL files:**
Open File Explorer and type `\\wsl$\Ubuntu` in the address bar.

**Recommendation:** Keep ai4soc files in your Linux home directory (`~/ai4soc`),
not in `/mnt/c/`. File operations are significantly faster on the Linux filesystem.

---

## Windows Terminal (Optional but Recommended)

Windows Terminal gives you tabs, multiple profiles, and a much better experience
than the default Ubuntu app.

Install from the Microsoft Store: search for **Windows Terminal**.

After installing, open it and click the dropdown arrow next to the `+` tab button.
You will see Ubuntu listed as a profile. Set it as your default if you prefer.

---

## Troubleshooting

**WSL install fails with error 0x80370102:**
Virtualization is disabled in BIOS. Enable it in your BIOS/UEFI settings
(look for "Intel VT-x" or "AMD-V" or "SVM Mode").

**Ubuntu stuck at "Installing" after restart:**
Open PowerShell as Administrator and run:
```powershell
wsl --update
wsl --set-default-version 2
```

**Claude Code "command not found" in WSL:**
```bash
# Check npm global bin path
npm config get prefix

# Add to PATH
echo 'export PATH="$(npm config get prefix)/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**File permission errors:**
Never run Claude Code or Ollama with `sudo`. If you get permission errors,
check the file ownership: `ls -la` and `chown -R $USER:$USER /path/to/files`

**Slow file access from Windows mount (`/mnt/c`):**
This is a known WSL limitation. Keep your ai4soc repo in the Linux filesystem
(`~/ai4soc`) for best performance.

---

## Next Steps

With WSL set up, follow the setup guide for your chosen tier:

- Tier 0 Ollama → [`ollama.md`](ollama.md)
- Tier 1 OpenRouter → [`openrouter.md`](openrouter.md)
- Tier 2 Claude Code → [`claude-code.md`](claude-code.md)

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*

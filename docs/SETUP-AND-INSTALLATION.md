# Setup and Installation Guide

> **Time estimate:** ~30 minutes

---

## 1. Overview

This guide walks you through setting up Claude Code OS on your machine, connecting it to GitHub for sync across devices, and using it effectively on desktop and mobile.

By the end, you'll have:
- Claude Code CLI running in VS Code
- Your own private GitHub repo synced to Claude Code web/mobile
- A workflow for seamless desktop/mobile usage

---

## 2. Prerequisites

| Tool | Purpose |
|------|---------|
| VS Code | Code editor and terminal |
| Git | Version control |
| Claude Code CLI | AI assistant in your terminal |
| GitHub CLI (`gh`) | Authenticate and push to GitHub |
| Anthropic account | Access to Claude models |
| GitHub account | Remote repo hosting |

---

## 3. Installation

### 3.1 Install VS Code

1. Download from [code.visualstudio.com](https://code.visualstudio.com/)
2. Follow the installation wizard for your OS
3. Launch VS Code to verify it works

### 3.2 Install Git

**macOS:**
```bash
# Git comes with Xcode Command Line Tools
xcode-select --install
```

**Windows:**
- Download and install [Git for Windows](https://gitforwindows.org/) (includes Git Bash)
- During installation, select "Git from the command line and also from 3rd-party software"

**Linux:**
```bash
sudo apt install git  # Debian/Ubuntu
sudo dnf install git  # Fedora
```

Verify installation:
```bash
git --version
```

### 3.3 Install Claude Code CLI

**macOS/Linux:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Windows:**

1. Install Node.js from [nodejs.org](https://nodejs.org/) (LTS version, **x64** - not ARM64)
2. Open Command Prompt or PowerShell and run:
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```
3. **Important - PATH fix:** If `claude` command is not found after install:
   - Open System Environment Variables (search "environment variables" in Windows)
   - Under "User variables", find `Path` and click Edit
   - Add: `%USERPROFILE%\AppData\Roaming\npm`
   - Click OK and **restart your terminal/VS Code entirely**

Verify installation:
```bash
claude --version
```

### 3.4 Install GitHub CLI

**macOS:**
```bash
brew install gh
```

**Windows:**
```bash
winget install --id GitHub.cli
```

**Linux:**
```bash
# See https://github.com/cli/cli/blob/trunk/docs/install_linux.md
```

Verify installation:
```bash
gh --version
```

---

## 4. Initial Setup

### 4.1 Clone the Claude Code OS Repo

```bash
git clone https://github.com/YOUR_USERNAME/claude-code-os.git
cd claude-code-os
```

Or if starting fresh, fork/copy this repository to your own GitHub account first.

### 4.2 Open in VS Code

1. Launch VS Code
2. File > Open Folder
3. Select your `claude-code-os` folder
4. Open the integrated terminal: `Ctrl+J` (Windows/Linux) or `Cmd+J` (macOS)

### 4.3 Authenticate Claude (Anthropic)

1. In the VS Code terminal, run:
   ```bash
   claude
   ```
2. Follow the prompts to authenticate with your Anthropic account
3. Allow any firewall prompts that appear (Windows)

### 4.4 Authenticate GitHub CLI

```bash
gh auth login
```

Follow the prompts:
- Select `GitHub.com`
- Select `HTTPS`
- Authenticate via browser

Verify:
```bash
gh auth status
```

### 4.5 Select Your Model

In a Claude session, run:
```
/model
```

**Model guidance:**
| Model | Best for | Cost |
|-------|----------|------|
| **Opus** | Complex tasks, reorganization, multi-file changes | Higher |
| **Sonnet** | Routine tasks, quick edits, daily operations | Lower |

> **Tip:** Credits reset every 5 hours on the user plan. Use Sonnet for routine work to conserve Opus for complex tasks.

---

## 5. Connect GitHub (Remote Sync)

### 5.1 Create Your Private Repo

If you don't already have a remote repo:

```bash
gh repo create my-claude-os --private --source=. --push
```

This creates a private repo and pushes your local code.

### 5.2 Push Changes

Standard workflow:
```bash
git add .
git commit -m "Your commit message"
git push
```

> **Important:** Never commit secrets (`.env` files, API keys). Claude will warn you if you try, but always double-check.

### 5.3 Connect Claude Code Web/Mobile

1. Go to [claude.ai](https://claude.ai)
2. Start a new conversation
3. Click the "Connect to GitHub" or repository icon
4. Select your repo from the list
5. Your Claude Code OS is now accessible from web/mobile

---

## 6. Mobile Workflow

### 6.1 Branch Model

When you work from your phone, Claude creates a **feature branch** (not main). This keeps your main branch clean.

**Pattern:**
- Phone sessions → feature branches
- Desktop → merges branches into main

### 6.2 Desktop Merge Workflow

When returning to desktop after phone work:

```bash
# Fetch all branches from remote
git fetch --all

# See what branches exist
git branch -a

# Merge phone branch into main
git checkout main
git merge origin/your-phone-branch-name

# Push merged main
git push
```

**Best practices:**
- Commit and push before leaving desktop
- Start a new phone session for each new topic
- Merge phone branches regularly to avoid drift

### 6.3 Android Browser Workaround

The Android Claude app has an issue where the "Enter" key sends the message instead of creating a newline.

**Workaround:** Use [claude.ai](https://claude.ai) in your mobile browser instead of the app. The browser version works correctly.

---

## 7. Tips & Shortcuts

### Approvals

| Command | Effect |
|---------|--------|
| `y` | Approve this action |
| `yes` | Approve and don't ask again for **this type** of action |

"Yes and don't ask again" is safe for non-destructive operations (file creation, git mv, etc.). Settings are stored in `.claude/settings.json`.

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Escape` | Pause Claude mid-operation to add instructions |
| `Tab` | Accept Claude's suggested response |
| `Ctrl+O` | Expand command output to see Claude's thinking |
| `Ctrl+click` | Open file paths directly from terminal output |

### Multiple Terminals

You can run multiple Claude instances in VS Code for parallel work:
1. Click the `+` icon in the terminal panel
2. Run `claude` in the new terminal
3. Work on different tasks simultaneously

---

## 8. Troubleshooting

### Windows PATH Issues

**Symptom:** `claude` command not found after installation

**Fix:**
1. Open System Environment Variables
2. Edit User `Path` variable
3. Add: `%USERPROFILE%\AppData\Roaming\npm`
4. Restart VS Code entirely (not just the terminal)

### ARM64 vs x64 (Windows)

**Symptom:** Installation fails or Claude won't run

**Fix:** Most Windows PCs need the **x64** version, not ARM64. Re-download the correct version from nodejs.org.

### PowerShell vs CMD

**Symptom:** Claude works in CMD but not in VS Code PowerShell

**Fix:** After updating PATH, you must restart VS Code entirely (not just open a new terminal).

### Claude Command Not Found

**Symptom:** `claude: command not found`

**Fixes:**
1. Verify npm installed it: `npm list -g @anthropic-ai/claude-code`
2. Check PATH includes npm global bin directory
3. Reinstall: `npm install -g @anthropic-ai/claude-code`

### Android App Enter Key Issue

**Symptom:** Enter key sends message instead of newline in Claude Android app

**Fix:** Use [claude.ai](https://claude.ai) in your mobile browser instead.

---

## 9. Next Steps

Once setup is complete:

1. **Review the folder structure** - Each folder in `claude-code-os-implementation/` has a specific purpose
2. **Read Implementation Plans** - Each department folder contains an `implementation-plan.md` with guidance
3. **Start with Executive Office** - Begin at `01-executive-office/START_HERE.md`
4. **Define your OBG** - Personalize the One Big Goal in strategic alignment
5. **Follow Implementation Plan discipline** - Always check the folder's implementation plan before making changes

---

## Quick Reference

```bash
# Start Claude
claude

# Select model
/model

# Check git status
git status

# Push changes
git add . && git commit -m "message" && git push

# Fetch phone branches
git fetch --all

# Authenticate GitHub
gh auth login
```

---

*For detailed implementation guides, see the [Implementation README](../claude-code-os-implementation/README.md).*

# Setup Guide - Trivia Survival v1.0

Complete installation and configuration instructions for Trivia Survival on Streamer.bot.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Windows Quick Start](#windows-quick-start)
- [Linux Manual Setup](#linux-manual-setup)
- [Verification & Testing](#verification--testing)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before installing Trivia Survival, ensure you have:

### Required

✅ **Streamer.bot v0.2.0 or later** (or v1.0.4+)
- Download from [streamer.bot](https://streamer.bot/)
- Must have C# Execute Code support

✅ **Twitch Account Connected**
- Streamer.bot must be connected to your Twitch account
- Bot account configured (or use your broadcaster account)

✅ **Basic Streamer.bot Knowledge**
- Familiarity with Actions and Commands
- Understanding of Global Variables
- Know how to check logs (bottom panel)

### Optional

⭐ **Discord Server** (for leaderboard integration)
- Webhook URL for posting results
- Permissions to create webhooks

---

## Installation

### Step 1: Download Files

Download the latest release from the [Releases page](https://github.com/1moreastronaut/trivia-survival-game/releases):

- `Trivia-Survival-v1.0.sb` - Main import file
- `questions-sample.csv` - Example questions (optional)

### Step 2: Import into Streamer.bot

**For Streamer.bot v1.0.4+:**

1. Open Streamer.bot
2. Click the **"Import"** button at the top of the window (regardless of which tab is selected)
3. **Drag and drop** the `Trivia-Survival-v1.0.sb` file into the import window
4. Confirm the import

**What Gets Imported:**

- ✅ 10 Trivia actions (numbered 00-09)
- ✅ 6 Chat commands with triggers
- ✅ No variables (fresh start for configuration)

**Actions Imported:**

| # | Action Name | Purpose |
|---|-------------|---------|
| 00 | Trivia 00) Setup UI | Windows-only configuration GUI |
| 01 | Trivia 01) Initialize | Start join period |
| 02 | Trivia 02) Join | Players join the game |
| 03 | Trivia 03) Start Game | Lock players and begin |
| 04 | Trivia 04) Ask Question | Display question in chat |
| 05 | Trivia 05) Open Answers | Show answer options and start timer |
| 06 | Trivia 06) Collect Answers | Process player responses |
| 07 | Trivia 07) End Question | Reveal answer and eliminate players |
| 08 | Trivia 08) End Game | Display results and update leaderboards |
| 09 | Trivia 09) Discord Leaderboards | Post to Discord (optional) |

**Commands Imported:**

| Command | Trigger | Permission |
|---------|---------|------------|
| `!trivia` | Chat Message | Everyone |
| `!trivia-setup` | Chat Message | Mod/Broadcaster |
| `!trivia-init` | Chat Message | Mod/Broadcaster |
| `!trivia-start` | Chat Message | Mod/Broadcaster |
| `!trivia-ask` | Chat Message | Mod/Broadcaster |
| `!trivia-end` | Chat Message | Mod/Broadcaster |

### Step 3: Choose Your Setup Path

**Windows users:** Continue to [Windows Quick Start](#windows-quick-start)

**Linux users:** Skip to [Linux Manual Setup](#linux-manual-setup)

---
## Windows Quick Start

Windows users can use the graphical Setup UI for easy configuration.

### Step 1: Create Your Trivia Folder

Create a folder to store your trivia data:

**Example:** `C:\Trivia`

You can use any location - just remember the path for the next step.

### Step 2: Create Your Questions File

In your trivia folder, create a file named **`questions.csv`**

**Format:**

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"Capital of France?",Berlin,London,Paris,C
"Largest ocean?",Atlantic,Pacific,Indian,B

**Quick Start Option:**

Use the included `questions-sample.csv` file:
1. Copy `questions-sample.csv` to your trivia folder
2. Rename it to `questions.csv`

### Step 3: Run the Setup UI

In your Twitch chat (as broadcaster or moderator):

!trivia-setup

A configuration window will open with three tabs.

### Step 4: Configure Basic Settings

**Tab 1: Basic Setup**

1. **Data Folder Path:** Paste your folder path (e.g., `C:\Trivia`)
2. Click **"Validate CSV"** to check your questions file
   - Green message = Success
   - Red message = Error details shown
3. **Questions per Game:** Set how many questions (1-30, default: 10)
4. **Question Delay:** Seconds before answers appear (0-30, default: 5)
5. **Answer Window:** Seconds players have to answer (5-60, default: 15)

**Tab 2: Discord Integration (Optional)**

1. **Discord Webhook URL:** Paste your webhook URL (or leave blank to skip)
2. **Winners Display:** Choose how many to show (Top 5/10/25/50/All)
3. **Sole Survivors Display:** Choose how many to show (Top 5/10/25/50/All)
4. **Enable Auto-Posting:** Check the box and follow instructions to enable Action 09

**Tab 3: Leaderboards**

- View current Winners and Sole Survivors
- **Export Backup:** Save leaderboards to timestamped JSON file
- **Import Backup:** Restore from a previous backup
- **Reset All:** Clear all leaderboard data (requires confirmation)

### Step 5: Save Settings

Click **"SAVE SETTINGS"** at the bottom of the window.

All global variables will be created/updated automatically.

### Step 6: Verify Installation

See [Verification & Testing](#verification--testing) section below.

---

## Linux Manual Setup

Linux users need to manually configure global variables since the Setup UI uses Windows Forms.

**Note:** Once configured, all game features work identically on Linux and Windows.

### Step 1: Create Your Data Folder

mkdir -p ~/trivia
cd ~/trivia

Or use any folder path you prefer - just use the **absolute path** in configuration.

### Step 2: Create Your Questions File

Create `~/trivia/questions.csv` with this format:

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"Capital of France?",Berlin,London,Paris,C
"Largest ocean?",Atlantic,Pacific,Indian,B

**Quick Start Option:**

cp /path/to/questions-sample.csv ~/trivia/questions.csv

**Important CSV Rules:**

- First row must be the header: `Question,A,B,C,Correct`
- Exactly 5 columns per row
- Wrap questions/answers with commas in quotes
- Correct answer must be A, B, or C (case-insensitive)
- Need at least as many questions as your "Questions per Game" setting

### Step 3: Configure Global Variables

Open Streamer.bot and navigate to: **Settings → Variables → Global**

Click **"Add"** for each variable below.

**Important:** Make sure **"Persisted"** checkbox is checked for each variable!

#### Required Variables

| Variable Name | Type | Example Value | Description |
|--------------|------|---------------|-------------|
| `TriviaDataFolder` | String | `/home/username/trivia` | **Use absolute path!** |
| `TriviaQuestionCount` | Number | `10` | Questions per game (1-30) |
| `TriviaQuestionDelay` | Number | `5` | Delay before showing answers (0-30 seconds) |
| `TriviaQuestionDelayMs` | Number | `5000` | Same as above in milliseconds (multiply by 1000) |
| `TriviaAnswerWindow` | Number | `15` | Time to answer (5-60 seconds) |
| `TriviaAnswerWindowMs` | Number | `15000` | Same as above in milliseconds (multiply by 1000) |
| `TriviaDiscordWinnersDisplay` | String | `Top 10` | Options: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |
| `TriviaDiscordChampsDisplay` | String | `Top 10` | Options: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |

#### Optional Variables

| Variable Name | Type | Example Value | Description |
|--------------|------|---------------|-------------|
| `TriviaDiscordWebhook` | String | `https://discord.com/api/webhooks/...` | Leave empty to skip Discord |

**Path Requirements (Linux):**

❌ **Don't use:** `~/trivia` (shell expansion may not work in Streamer.bot)
✅ **Use:** `/home/username/trivia` (absolute path)
✅ **Remember:** Linux paths are case-sensitive

### Step 4: Enable Discord Auto-Post (Optional)

If you configured a Discord webhook and want automatic posting:

1. In Streamer.bot, go to the **Actions** tab
2. Find **"Trivia 08) End Game"**
3. Double-click to edit
4. Locate the **disabled** subaction: `Run Action: Trivia 09) Discord Leaderboards`
5. Right-click → **Enable**

### Step 5: Set File Permissions

Ensure proper permissions on your files:

chmod 755 ~/trivia
chmod 644 ~/trivia/questions.csv

### Step 6: Verify Installation

See [Verification & Testing](#verification--testing) section below.

---
## Verification & Testing

After completing either Windows or Linux setup, verify everything is working correctly.

### Step 1: Check CSV Loading

**Windows:**
- Run `!trivia-setup` and click "Validate CSV"
- Look for green success message

**Linux:**
- Check file exists: `ls -la ~/trivia/questions.csv`
- Verify format: `head -5 ~/trivia/questions.csv`

### Step 2: Test Game Initialization

In Twitch chat (as broadcaster or mod):

!trivia-init

**Expected Result:**

🎮 TRIVIA SURVIVAL has started! Type !trivia to join!
Players have 30-60 seconds to join before the game begins.

**Check Streamer.bot Logs:**

Look for messages like:
- `[Trivia] Loaded 20 questions from CSV`
- `[Trivia] Game initialized - join period started`

### Step 3: Test Joining

In chat (as any user):

!trivia

**Expected Result:**

✅ @YourUsername has joined! (1 player)

### Step 4: Test Game Start

After at least one player joins:

!trivia-start

**Expected Result:**

🔒 Player list locked! 1 brave contestant is competing!
🎯 10 questions | Answer with A, B, or C
Get ready... Question 1 coming up!

### Step 5: Test Question Flow

Wait for a question to appear, then answer:

A

(or B or C, depending on the question)

**Expected Result:**
- Question displays in chat
- After delay: Answer options appear with Unicode symbols (🇦, 🇧, 🇨)
- After answer window: Correct answer revealed
- Wrong answers eliminated
- Next question begins automatically

### Step 6: Verify Leaderboards

After game completes, check:

**In Chat:** Top 5 leaderboards should display automatically

**Windows:** Run `!trivia-setup` → Leaderboards tab to view full data

**Linux:** Check global variables:
- Settings → Variables → Global
- Look for `TriviaSharedWins` and `TriviaChampionWins`

**Discord (if configured):** Check your Discord channel for leaderboard post

---

## Troubleshooting

### CSV File Not Loading

**Error:** "CSV file not found" or "Failed to load CSV"

**Windows Solutions:**
- Verify the path in `!trivia-setup` → Basic Setup
- Use full path (e.g., `C:\Trivia` not relative paths)
- Check file is named exactly `questions.csv` (not `questions.csv.txt`)
- Run "Validate CSV" to see specific errors

**Linux Solutions:**

# Check file exists
ls -la ~/trivia/questions.csv

# Verify path in global variables
# Settings → Variables → Global → TriviaDataFolder
# Should be: /home/username/trivia (not ~/trivia)

# Check file permissions
chmod 644 ~/trivia/questions.csv

# Check for Windows line endings
file ~/trivia/questions.csv
# Should say: "ASCII text" or "UTF-8 Unicode text"
# If it says "CRLF line terminators":
dos2unix ~/trivia/questions.csv

### CSV Validation Errors

**Error:** "Invalid format at row X"

**Solutions:**
- Ensure first row is exactly: `Question,A,B,C,Correct`
- Every row must have exactly 5 columns
- Put quotes around any text containing commas
- Correct answer must be A, B, or C (uppercase or lowercase)

**Example of a problematic row:**

"What's the capital of Paris, France?",London,Paris,Berlin,2

**Fixed version:**

"What's the capital of Paris, France?",London,Paris,Berlin,B

### Players Can't Join

**Symptom:** `!trivia` command doesn't respond

**Solutions:**
- ✅ Make sure you ran `!trivia-init` first
- ✅ Check game state isn't already "live" (restart with `!trivia-end` then `!trivia-init`)
- ✅ Verify `!trivia` command is enabled in Streamer.bot (Commands tab)
- ✅ Check Streamer.bot logs for errors
- ✅ Ensure "Trivia 02) Join" action exists and is enabled

### Answers Not Being Collected

**Symptom:** Players type A/B/C but nothing happens

**Solutions:**
- ✅ Answers only count during the answer window (after options display)
- ✅ Players must type exactly A, B, or C (case-insensitive, no extra text)
- ✅ Each player can only answer once per question
- ✅ Check "Trivia 06) Collect Answers" has Chat Message event trigger
- ✅ Verify action is enabled (not disabled)

### Game Gets Stuck

**Symptom:** Game doesn't progress to next question

**Solutions:**

**Manually advance:**

!trivia-ask

**Force end the game:**

!trivia-end

**Restart fresh:**

!trivia-end
!trivia-init

**Check logs for errors:**
- Bottom panel in Streamer.bot
- Look for red error messages or exceptions

### Discord Not Posting

**Symptom:** Leaderboards don't appear in Discord

**Solutions:**
- ✅ Webhook URL is valid and starts with `https://discord.com/api/webhooks/`
- ✅ Webhook URL is set in global variable `TriviaDiscordWebhook`
- ✅ "Trivia 09) Discord Leaderboards" subaction is **enabled** in "Trivia 08) End Game"
- ✅ Check Streamer.bot logs for Discord API errors (403, 404, etc.)
- ✅ Verify webhook still exists in Discord server settings

### Leaderboards Not Saving (Linux)

**Symptom:** Leaderboards reset after restart

**Solutions:**

# Check folder write permissions
ls -ld ~/trivia
chmod 755 ~/trivia

# Check if JSON files exist
ls -la ~/trivia/*.json

# Verify global variables are persisted
# Settings → Variables → Global
# TriviaSharedWins and TriviaChampionWins should have "Persisted" checked

### Path Issues (Linux)

**Symptom:** "File not found" or "Access denied" errors

**Common Mistakes:**
❌ Using `~/trivia` instead of `/home/username/trivia`
❌ Incorrect case: `/Home/Username/Trivia` vs `/home/username/trivia`
❌ Relative paths: `trivia/questions.csv` instead of absolute path

**Solutions:**
✅ Always use absolute paths: `/home/username/trivia`
✅ Match case exactly (Linux is case-sensitive)
✅ Test path in terminal first: `cat /home/username/trivia/questions.csv`

### Setup UI Won't Open (Windows)

**Symptom:** `!trivia-setup` command doesn't show window

**Solutions:**
- ✅ Check Streamer.bot logs for errors
- ✅ Ensure .NET Framework 4.7.2+ is installed
- ✅ Try running Streamer.bot as administrator
- ✅ Check "Trivia 00) Setup UI" action is enabled
- ✅ Verify no antivirus blocking the window

**Alternative:** Configure manually like Linux users (see global variables table above)

---

## Next Steps

Once setup and verification are complete:

✅ **Customize your questions** - Edit your CSV file with stream-specific content

✅ **Adjust timing** - Experiment with question delay and answer window settings

✅ **Test with mods** - Run a full practice game before going live

✅ **Configure Discord** - Set up webhook for automatic leaderboard posting

✅ **Backup your data** - Export leaderboards regularly

---

## Additional Resources

- **[Commands Reference](COMMANDS.md)** - All available commands and usage
- **[Configuration Guide](CONFIGURATION.md)** - Detailed settings explanations
- **[Main README](../README.md)** - Feature overview and tips

---

## Getting Help

If you're still experiencing issues:

1. **Check Streamer.bot Logs** - Bottom panel shows detailed error messages
2. **Review this troubleshooting section** - Most issues are covered above
3. **Open a GitHub Issue** - Include:
   - Streamer.bot version
   - Platform (Windows/Linux)
   - Specific error messages from logs
   - Steps to reproduce the problem

---

**Setup complete!** You're ready to run trivia games. See the [Commands Reference](COMMANDS.md) for usage details.

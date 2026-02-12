# Configuration Guide - Trivia Survival v1.0

Complete guide to configuring and customizing Trivia Survival for your stream.

---

## Table of Contents

- [Configuration Overview](#configuration-overview)
- [Global Variables Reference](#global-variables-reference)
- [Game Timing Settings](#game-timing-settings)
- [Discord Integration](#discord-integration)
- [CSV Question File](#csv-question-file)
- [Advanced Customization](#advanced-customization)
- [Best Practices](#best-practices)

---

## Configuration Overview

Trivia Survival stores all configuration in **Streamer.bot Global Variables**. You can configure via:

### Windows: Setup UI (Recommended)

Run `!trivia-setup` in chat to open the graphical configuration window.

**Advantages:**
- Visual interface for all settings
- CSV validation with error details
- Leaderboard viewer and backup tools
- Automatic variable creation

### Linux or Manual: Global Variables

Configure directly in: **Settings → Variables → Global**

**Advantages:**
- Works on any platform
- Direct control over values
- No UI dependencies

---

## Global Variables Reference

All variables must be created in: **Settings → Variables → Global**

**Important:** Check **"Persisted"** for each variable to save between sessions!

### Core Settings

| Variable Name | Type | Required | Default | Description |
|--------------|------|----------|---------|-------------|
| `TriviaDataFolder` | String | ✅ Yes | N/A | Folder path for CSV and leaderboard files |
| `TriviaQuestionCount` | Number | ✅ Yes | `10` | Total questions per game (1-30) |
| `TriviaQuestionDelay` | Number | ✅ Yes | `5` | Seconds before showing answer options (0-30) |
| `TriviaQuestionDelayMs` | Number | ✅ Yes | `5000` | Same as above in milliseconds (delay × 1000) |
| `TriviaAnswerWindow` | Number | ✅ Yes | `15` | Seconds players have to answer (5-60) |
| `TriviaAnswerWindowMs` | Number | ✅ Yes | `15000` | Same as above in milliseconds (window × 1000) |

### Discord Integration

| Variable Name | Type | Required | Default | Description |
|--------------|------|----------|---------|-------------|
| `TriviaDiscordWebhook` | String | ⭐ Optional | Empty | Discord webhook URL for posting leaderboards |
| `TriviaDiscordWinnersDisplay` | String | ✅ Yes | `Top 10` | How many winners to show: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |
| `TriviaDiscordChampsDisplay` | String | ✅ Yes | `Top 10` | How many sole survivors to show: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |

### Leaderboard Data

| Variable Name | Type | Required | Default | Description |
|--------------|------|----------|---------|-------------|
| `TriviaSharedWins` | Dictionary | 🔄 Auto-created | `{}` | Tracks all winners (created automatically on first game) |
| `TriviaChampionWins` | Dictionary | 🔄 Auto-created | `{}` | Tracks sole survivors (created automatically on first game) |

**Note:** Leaderboard variables are created automatically - you don't need to add them manually.

### Game State Variables

**⚠️ DO NOT manually create these!** They are managed automatically by the system:

- `TriviaGameState` - Current state (idle/pregame/live/ended)
- `TriviaPlayers` - Dictionary of active players
- `TriviaQuestions` - Loaded question list
- `TriviaCurrentQuestion` - Current question index
- And many others...

---

## Game Timing Settings

Understanding how timing affects gameplay.

### Question Delay

**Variable:** `TriviaQuestionDelay` (seconds) and `TriviaQuestionDelayMs` (milliseconds)

**What it controls:** Time between displaying the question and showing answer options.

**Purpose:** Gives players time to read the question before answers appear.

**Recommendations:**

| Pace | Delay | Use Case |
|------|-------|----------|
| **Fast** | 3 seconds | Short questions, experienced players |
| **Balanced** | 5 seconds | Default, works for most streams |
| **Relaxed** | 7-10 seconds | Complex questions, casual audience |

**Example Flow with 5-second delay:**

1. `00:00` - Question appears: "What is the capital of France?"
2. `00:05` - Options appear: 🇦 London | 🇧 Paris | 🇨 Berlin
3. Players can now answer

### Answer Window

**Variable:** `TriviaAnswerWindow` (seconds) and `TriviaAnswerWindowMs` (milliseconds)

**What it controls:** Time players have to submit their answer after options appear.

**Purpose:** Creates urgency and keeps game moving.

**Recommendations:**

| Pace | Window | Use Case |
|------|--------|----------|
| **Quick Fire** | 10 seconds | Easy questions, fast gameplay |
| **Balanced** | 15 seconds | Default, good for most content |
| **Casual** | 20-25 seconds | Harder questions, relaxed pace |
| **Accessibility** | 30+ seconds | Maximum inclusivity |

**Example Flow with 15-second window:**

1. `00:00` - Options appear: 🇦 London | 🇧 Paris | 🇨 Berlin
2. `00:01-00:15` - Players type A, B, or C
3. `00:15` - Window closes, answers locked
4. Correct answer revealed, wrong answers eliminated

### Question Count

**Variable:** `TriviaQuestionCount`

**What it controls:** Total questions per game.

**Range:** 1-30 questions

**Recommendations:**

| Count | Duration | Use Case |
|-------|----------|----------|
| **5** | ~2-3 minutes | Quick stream segment, fast elimination |
| **10** | ~5-7 minutes | Default, balanced gameplay |
| **15** | ~8-10 minutes | Extended game, more competitive |
| **20+** | 12+ minutes | Marathon mode, ultimate challenge |

**Calculation:**

Total Time ≈ (Question Delay + Answer Window + 5s result) × Question Count

Example: (5 + 15 + 5) × 10 = 250 seconds ≈ 4 minutes

---
## Discord Integration

Configure Discord webhook posting for automatic leaderboard updates.

### Setting Up Discord Webhook

**Step 1: Create Webhook in Discord**

1. Open your Discord server
2. Go to **Server Settings** → **Integrations** → **Webhooks**
3. Click **"New Webhook"** or **"Create Webhook"**
4. Configure:
   - **Name:** "Trivia Leaderboards" (or your choice)
   - **Channel:** Select where to post (e.g., #trivia-results)
   - **Avatar:** Optional custom image
5. Click **"Copy Webhook URL"**

**Step 2: Add to Trivia Survival**

**Windows:**
1. Run `!trivia-setup` in chat
2. Go to **Discord** tab
3. Paste webhook URL
4. Choose display options
5. Click **"SAVE SETTINGS"**

**Linux:**
1. Settings → Variables → Global
2. Find or create `TriviaDiscordWebhook`
3. Set value to your webhook URL
4. Check **"Persisted"**

**Step 3: Enable Auto-Posting**

1. In Streamer.bot, go to **Actions** tab
2. Find **"Trivia 08) End Game"**
3. Double-click to edit
4. Locate the **disabled** subaction: `Run Action: Trivia 09) Discord Leaderboards`
5. Right-click → **Enable**
6. Save

### Discord Display Options

**Variable:** `TriviaDiscordWinnersDisplay`

Controls how many winners to show in Discord posts.

**Options:**
- `Top 5` - Shows top 5 winners only
- `Top 10` - Shows top 10 winners (default)
- `Top 25` - Shows top 25 winners
- `Top 50` - Shows top 50 winners
- `All` - Shows all winners (not recommended for large leaderboards)

**Variable:** `TriviaDiscordChampsDisplay`

Controls how many sole survivors to show in Discord posts.

**Options:** Same as above (Top 5/10/25/50/All)

### Example Discord Post

📊 TRIVIA LEADERBOARDS

🏅 Winners (Top 10):
1. PlayerOne - 12 wins
2. PlayerTwo - 8 wins
3. PlayerThree - 7 wins
4. PlayerFour - 5 wins
5. PlayerFive - 4 wins
6. PlayerSix - 3 wins
7. PlayerSeven - 3 wins
8. PlayerEight - 2 wins
9. PlayerNine - 2 wins
10. PlayerTen - 1 win

👑 Sole Survivors (Top 10):
1. PlayerOne - 5 times
2. PlayerThree - 3 times
3. PlayerFour - 2 times
4. PlayerNine - 1 time

### Manual Discord Posting

To post leaderboards on-demand without running a game:

**Option 1: Trigger Action Manually**
- Actions tab → "Trivia 09) Discord Leaderboards"
- Right-click → Test

**Option 2: Create Custom Command**
- Create new command: `!trivia-leaderboards`
- Action: Run "Trivia 09) Discord Leaderboards"
- Permission: Mod/Broadcaster

---

## CSV Question File

Configure your question bank for the trivia game.

### File Location

**Variable:** `TriviaDataFolder`

The CSV file must be named **`questions.csv`** and placed in your data folder.

**Example paths:**
- Windows: `C:\Trivia\questions.csv`
- Linux: `/home/username/trivia/questions.csv`

### CSV Format

**Required structure:**

Question,A,B,C,Correct

**Rules:**

✅ **First row must be header:** Exactly `Question,A,B,C,Correct`
✅ **5 columns per row:** Question + 3 answers + correct letter
✅ **Quote text with commas:** Wrap in double quotes
✅ **Correct answer:** Must be `A`, `B`, or `C` (case-insensitive)
✅ **Minimum questions:** At least as many as `TriviaQuestionCount` setting

### Example CSV File

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"What is the capital of France?",Berlin,London,Paris,C
"What is the largest ocean?",Atlantic,Pacific,Indian,B
"How many continents are there?",Five,Seven,Nine,B
"What is H2O?",Oxygen,Water,Hydrogen,B
"Who painted the Mona Lisa?",Michelangelo,Da Vinci,Picasso,B
"What is the fastest land animal?",Lion,Cheetah,Leopard,B
"How many planets are in our solar system?",Seven,Eight,Nine,B

### Common CSV Mistakes

❌ **Missing header:**

"What is 2+2?",Two,Four,Six,B

✅ **Correct:**

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B

---

❌ **Using numbers for correct answer:**

"What is 2+2?",Two,Four,Six,2

✅ **Correct:**

"What is 2+2?",Two,Four,Six,B

---

❌ **Unquoted commas:**

What is the capital of Paris, France?,Berlin,London,Paris,C

✅ **Correct:**

"What is the capital of Paris, France?",Berlin,London,Paris,C

---

❌ **Wrong number of columns:**

"What is 2+2?",Four,B

✅ **Correct:**

"What is 2+2?",Two,Four,Six,B

### Validating Your CSV

**Windows:**
1. Run `!trivia-setup`
2. Go to **Basic Setup** tab
3. Click **"Validate CSV"**
4. Green = Success, Red = Error details

**Linux:**
1. Run `!trivia-init` in chat
2. Check Streamer.bot logs for errors
3. Look for: `[Trivia] Loaded X questions from CSV`

### Updating Questions

You can update your CSV file anytime:

1. Edit `questions.csv` in your data folder
2. Save the file
3. Run `!trivia-init` to reload

**No need to restart Streamer.bot!** Questions load fresh each initialization.

---

## Advanced Customization

### Multiple Question Sets

Create different CSV files for themed games:

C:\Trivia\questions-movies.csv
C:\Trivia\questions-gaming.csv
C:\Trivia\questions-science.csv
C:\Trivia\questions-stream-lore.csv

**To switch question sets:**

**Windows:**
1. Run `!trivia-setup`
2. Temporarily change data folder to themed folder
3. Or rename desired CSV to `questions.csv`

**Linux:**
- Rename desired file: `mv questions-movies.csv questions.csv`
- Or update `TriviaDataFolder` variable to different folder

### Custom Game Modes

Create different timing profiles by adjusting variables:

#### Speed Run Mode

TriviaQuestionCount: 5
TriviaQuestionDelay: 3
TriviaQuestionDelayMs: 3000
TriviaAnswerWindow: 8
TriviaAnswerWindowMs: 8000

**Result:** Fast 2-minute game, high pressure

#### Marathon Mode

TriviaQuestionCount: 20
TriviaQuestionDelay: 5
TriviaQuestionDelayMs: 5000
TriviaAnswerWindow: 20
TriviaAnswerWindowMs: 20000

**Result:** 8-minute epic challenge

#### Accessibility Mode

TriviaQuestionCount: 10
TriviaQuestionDelay: 7
TriviaQuestionDelayMs: 7000
TriviaAnswerWindow: 30
TriviaAnswerWindowMs: 30000

**Result:** More time for reading and answering

### Leaderboard Management

#### Backup Leaderboards

**Windows (via Setup UI):**
1. Run `!trivia-setup`
2. Go to **Leaderboards** tab
3. Click **"Export Backup"**
4. File saved to data folder: `winners_backup_YYYYMMDD_HHMMSS.json`

**Linux (manual):**

cd ~/trivia
cp winners.json winners_backup_$(date +%Y%m%d_%H%M%S).json
cp champions.json champions_backup_$(date +%Y%m%d_%H%M%S).json

#### Restore Leaderboards

**Windows (via Setup UI):**
1. Run `!trivia-setup`
2. Go to **Leaderboards** tab
3. Click **"Import Backup"**
4. Enter filename (e.g., `winners_backup_20260212_153045.json`)

**Linux (manual):**

cd ~/trivia
cp winners_backup_20260212_153045.json winners.json
cp champions_backup_20260212_153045.json champions.json

#### Reset Leaderboards

**Windows (via Setup UI):**
1. Run `!trivia-setup`
2. Go to **Leaderboards** tab
3. Click **"Reset All Leaderboards"**
4. Confirm when prompted

**Linux (manual):**

cd ~/trivia
rm winners.json champions.json

**Or** reset via Streamer.bot:
- Settings → Variables → Global
- Delete `TriviaSharedWins` and `TriviaChampionWins`

---
## Best Practices

### Timing Recommendations by Stream Style

#### High-Energy Stream
- **Question Count:** 5-10
- **Question Delay:** 3-5 seconds
- **Answer Window:** 10-12 seconds
- **Best for:** Fast-paced, hype content, short attention spans

#### Balanced Stream
- **Question Count:** 10
- **Question Delay:** 5 seconds
- **Answer Window:** 15 seconds
- **Best for:** Most streams, default settings

#### Chill/Casual Stream
- **Question Count:** 8-12
- **Question Delay:** 5-7 seconds
- **Answer Window:** 20-25 seconds
- **Best for:** Relaxed pace, chat interaction, inclusive gameplay

#### Educational/Complex Content
- **Question Count:** 10-15
- **Question Delay:** 7-10 seconds
- **Answer Window:** 25-30 seconds
- **Best for:** Difficult questions, learning-focused content

### Question Writing Tips

✅ **Keep questions concise** - Aim for one sentence when possible

✅ **Avoid ambiguity** - One clearly correct answer

✅ **Balance difficulty** - Mix easy, medium, and hard questions

✅ **Test for readability** - Can average viewer read it in 3-5 seconds?

✅ **Vary topics** - Mix categories to keep it interesting

✅ **Stream-specific questions** - Add inside jokes, lore, community references

#### Example Question Difficulty Progression

**Easy (Questions 1-3):**
"What color is the sky?",Red,Blue,Green,B
"How many legs does a dog have?",Two,Four,Six,B

**Medium (Questions 4-7):**
"What is the capital of France?",Berlin,London,Paris,C
"Who painted the Mona Lisa?",Michelangelo,Da Vinci,Picasso,B

**Hard (Questions 8-10):**
"What year did World War II end?",1943,1945,1947,B
"What is the smallest prime number?",One,Two,Three,B

### Pre-Stream Checklist

Before going live with trivia:

- [ ] CSV file validated (no errors)
- [ ] Timing tested (feels good for your audience)
- [ ] At least 20+ questions in CSV (more variety)
- [ ] Leaderboards backed up (if you have existing data)
- [ ] Test game run completed (with mods or alt accounts)
- [ ] Discord webhook tested (if using)
- [ ] Commands verified (all working in chat)

### During Stream Best Practices

**Announcing the Game:**
- Give 1-2 minute warning before starting
- Explain rules clearly for new viewers
- Mention how many questions (sets expectations)

**Join Period:**
- Allow 30-60 seconds for joins
- Call out join count as it grows ("10 players so far!")
- Remind viewers to type `!trivia`

**During Questions:**
- Read questions aloud for accessibility
- Don't rush - let timers do their job
- Build hype between questions
- Celebrate survivors

**After Game:**
- Congratulate winners
- Hype up sole survivors (if any)
- Thank everyone who played
- Announce next trivia time

### Backup Strategy

**Recommended schedule:**

- **Before major updates:** Export backup
- **Weekly (active streams):** Export backup
- **Monthly (casual streams):** Export backup
- **Before reset/testing:** Export backup

**Naming convention:**

winners_backup_YYYYMMDD_HHMMSS.json
champions_backup_YYYYMMDD_HHMMSS.json

**Example:**
- `winners_backup_20260212_143022.json`
- `champions_backup_20260212_143022.json`

### Performance Optimization

#### CSV File Size

**Recommended:**
- 20-50 questions: Optimal for variety without lag
- 50-100 questions: Good for long-term use
- 100+ questions: May cause slight delay on load

**File size impact:**
- Small (20 questions): <2KB, instant load
- Medium (50 questions): <5KB, instant load
- Large (200 questions): ~15KB, still fast
- Very large (1000+ questions): May notice delay

**Tip:** Keep CSV under 200 questions for best performance. Create multiple themed files instead of one massive file.

#### Leaderboard Size

**Considerations:**
- 10-50 players: No impact
- 50-200 players: Minimal impact
- 200-500 players: Slight delay on leaderboard display
- 500+ players: Consider periodic resets or archiving

**Optimization:**
- Archive old leaderboards seasonally
- Reset for special events
- Keep Discord display at Top 10 or Top 25

### Seasonal/Event Configuration

**New Season Setup:**

1. **Backup current leaderboards**
2. **Archive to dated folder:**
   - `Season1_winners.json`
   - `Season1_champions.json`
3. **Reset leaderboards** for fresh start
4. **Announce season theme** (if using themed questions)

**Special Event Setup:**

Create temporary configuration:
- Separate data folder for event
- Custom question set
- Adjusted timing (faster/slower for variety)
- Restore normal config after event

---

## Troubleshooting Configuration Issues

### Variables Not Saving

**Symptom:** Settings reset after Streamer.bot restart

**Solution:**
- Ensure **"Persisted"** checkbox is checked on all variables
- Settings → Variables → Global → Edit variable → Check "Persisted"

### Millisecond Variables Don't Match

**Symptom:** Warning about delay/window mismatch

**Solution:**
- `TriviaQuestionDelayMs` must equal `TriviaQuestionDelay × 1000`
- `TriviaAnswerWindowMs` must equal `TriviaAnswerWindow × 1000`

**Examples:**
- Delay: 5 seconds → DelayMs: 5000
- Window: 15 seconds → WindowMs: 15000

### Discord Display Options Not Working

**Symptom:** Discord shows wrong number of entries

**Solution:**
- Value must match exactly (case-sensitive):
  - ✅ `Top 10`
  - ❌ `top 10`
  - ❌ `Top10`
  - ❌ `10`
- Valid options: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All`

### Path Issues

**Windows common mistakes:**
- ❌ `C:\Trivia\` (trailing slash may cause issues)
- ❌ `C:/Trivia/questions.csv` (including filename)
- ✅ `C:\Trivia` (folder only, no trailing slash)

**Linux common mistakes:**
- ❌ `~/trivia` (shell expansion may not work)
- ❌ `/home/username/trivia/` (trailing slash)
- ✅ `/home/username/trivia` (absolute path, no trailing slash)

---

## Configuration Templates

### Quick Start Template (Copy & Modify)

**Windows Example:**

TriviaDataFolder: C:\Trivia
TriviaQuestionCount: 10
TriviaQuestionDelay: 5
TriviaQuestionDelayMs: 5000
TriviaAnswerWindow: 15
TriviaAnswerWindowMs: 15000
TriviaDiscordWinnersDisplay: Top 10
TriviaDiscordChampsDisplay: Top 10
TriviaDiscordWebhook: (leave empty or add webhook URL)

**Linux Example:**

TriviaDataFolder: /home/username/trivia
TriviaQuestionCount: 10
TriviaQuestionDelay: 5
TriviaQuestionDelayMs: 5000
TriviaAnswerWindow: 15
TriviaAnswerWindowMs: 15000
TriviaDiscordWinnersDisplay: Top 10
TriviaDiscordChampsDisplay: Top 10
TriviaDiscordWebhook: (leave empty or add webhook URL)

---

## Summary

**Essential Configuration Steps:**

1. ✅ Set `TriviaDataFolder` to your data location
2. ✅ Create `questions.csv` in that folder
3. ✅ Configure timing (delay and window + milliseconds)
4. ✅ Set question count
5. ✅ Configure Discord display options
6. ✅ (Optional) Add Discord webhook
7. ✅ Validate CSV and test

**Remember:**
- Windows users: Use `!trivia-setup` for easy configuration
- Linux users: Set global variables manually
- Always check "Persisted" on variables
- Test before going live
- Backup leaderboards regularly

---

## Additional Resources

- **[Setup Guide](SETUP.md)** - Installation and first-time setup
- **[Commands Reference](COMMANDS.md)** - All available commands
- **[Main README](../README.md)** - Feature overview and tips

---

## Getting Help

Configuration questions? Check:

1. **Streamer.bot Logs** - Bottom panel shows detailed errors
2. **CSV Validation** - Use `!trivia-setup` → Validate CSV (Windows)
3. **This guide** - Most configuration covered above
4. **GitHub Issues** - Open an issue with details

---

**Configuration complete!** You're ready to customize Trivia Survival for your stream.

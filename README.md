# Trivia Survival - v1.0

A fully-featured multiple-choice trivia game for Twitch streamers using Streamer.bot.

Players compete by answering A/B/C questions. Wrong answers eliminate players. Last one(s) standing win!

---

## ✨ Features

- **Elimination-style gameplay** - Wrong answers = elimination
- **Customizable questions** - Load from CSV file
- **Dual leaderboards** - Track all winners + sole survivors
- **Discord integration** - Auto-post leaderboards (optional)
- **Configurable settings** - Question count, timing, display options
- **Backup/restore** - Export and import leaderboard data
- **Cross-platform** - Works on Windows and Linux

---

## 🖥️ Platform Compatibility

| Platform | Status | Notes |
|----------|--------|-------|
| **Windows** | ✅ Fully Supported | GUI setup available |
| **Linux** | ✅ Supported | Manual configuration required (see below) |

⚠️ **Linux Users:** The Setup UI uses Windows Forms and will not work on Linux. See the "Manual Setup for Linux" section below. Once configured, all game features work identically on both platforms.

---

## 📦 Installation (Windows & Linux)

### **Step 1: Download Files**
- `Trivia-Survival-v1.0.sb` (main export file)
- `questions-sample.csv` (example questions)
- This README

**⚠️ Note about sample questions:** The included `questions-sample.csv` is designed for **testing purposes only**. All 30 questions have "B" as the correct answer to make testing easier. For actual gameplay, create your own questions with randomized correct answers (A, B, or C) to prevent players from guessing patterns.

### **Step 2: Import into Streamer.bot**

1. Open Streamer.bot
2. Click the **"Import"** button at the top of the window
3. **Drag and drop** the `Trivia-Survival-v1.0.sb` file into the import window
4. Confirm the import

This will import:
- ✅ 10 Trivia actions (00-09)
- ✅ 6 Chat commands
- ✅ No variables (clean slate for new setup)

### **Step 3: Choose Your Setup Method**

- **Windows Users:** Jump to "Quick Start (Windows)" below
- **Linux Users:** Jump to "Manual Setup (Linux)" below

---
## 🚀 Quick Start (Windows)

### **1. Create Your Trivia Folder**

Create a folder anywhere on your computer, for example: `C:\Trivia`

### **2. Create questions.csv**

In your trivia folder, create a file named `questions.csv` with this format:

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"Capital of France?",Berlin,London,Paris,C
"Largest ocean?",Atlantic,Pacific,Indian,B

**Or** use the included `questions-sample.csv` and rename it to `questions.csv`.

### **3. Run Setup**

In Twitch chat (as broadcaster or mod): `!trivia-setup`

A configuration window will open:

**Basic Setup Tab:**
1. Paste your folder path (e.g., `C:\Trivia`)
2. Click "Validate CSV" to check your questions
3. Adjust game settings:
   - Questions per Game (default: 10)
   - Question Delay (default: 5 seconds)
   - Answer Window (default: 15 seconds)

**Discord Tab (Optional):**
1. Paste your Discord webhook URL
2. Choose how many winners/survivors to display
3. Follow on-screen instructions to enable auto-posting

**Leaderboards Tab:**
- View current leaderboards
- Export/import backups
- Reset all data

### **4. Click "SAVE SETTINGS"**

### **5. You're Ready!**

See "How to Run a Game" below.

---
## 🐧 Manual Setup (Linux & Advanced Users)

Since the Setup UI uses Windows Forms (not available on Linux), you'll need to manually configure global variables in Streamer.bot. Once configured, **all game features work identically** on Linux and Windows.

### **Step 1: Create Your Data Folder**

For example: `mkdir -p ~/trivia`

Or use any folder path you prefer. Just remember it for Step 2.

### **Step 2: Create questions.csv**

Create a file at `~/trivia/questions.csv` with this format:

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"Capital of France?",Berlin,London,Paris,C
"Largest ocean?",Atlantic,Pacific,Indian,B

**Rules:**
- First row is the header (required)
- Exactly 5 columns per row
- Wrap questions in quotes if they contain commas
- Correct answer must be A, B, or C (case-insensitive)
- Create at least as many questions as your "Questions per Game" setting

### **Step 3: Configure Global Variables in Streamer.bot**

Open Streamer.bot and navigate to: **Settings → Variables → Global**

Click **"Add"** for each variable below:

#### **Required Configuration Variables:**

| Variable Name | Type | Value | Description |
|--------------|------|-------|-------------|
| `TriviaDataFolder` | String | `/home/username/trivia` | **Replace with your actual folder path** |
| `TriviaQuestionCount` | Number | `10` | How many questions per game (1-30) |
| `TriviaQuestionDelay` | Number | `5` | Seconds before accepting answers (0-30) |
| `TriviaQuestionDelayMs` | Number | `5000` | Same as above in milliseconds (delay × 1000) |
| `TriviaAnswerWindow` | Number | `15` | Seconds players have to answer (5-60) |
| `TriviaAnswerWindowMs` | Number | `15000` | Same as above in milliseconds (window × 1000) |
| `TriviaDiscordWinnersDisplay` | String | `Top 10` | Options: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |
| `TriviaDiscordChampsDisplay` | String | `Top 10` | Options: `Top 5`, `Top 10`, `Top 25`, `Top 50`, `All` |

#### **Optional Discord Integration:**

| Variable Name | Type | Value | Description |
|--------------|------|-------|-------------|
| `TriviaDiscordWebhook` | String | `https://discord.com/api/webhooks/...` | Leave empty to skip Discord features |

**Important Notes:**
- Make sure **"Persisted"** checkbox is checked when creating each variable
- Use **absolute paths** (e.g., `/home/username/trivia`, not `~/trivia`)
- Linux paths are **case-sensitive**

### **Step 4: Initialize Leaderboards (Optional)**

The system will create leaderboard variables automatically on first game completion. You don't need to create them manually.

### **Step 5: Verify Configuration**

1. **Check your CSV file exists:** `ls -la ~/trivia/questions.csv`

2. **Test the game:**
   - `!trivia-init` (as mod/broadcaster)
   - `!trivia` (as any user - join the game)
   - `!trivia-start` (as mod/broadcaster)

3. **Watch Streamer.bot logs** (bottom panel) for:
   - `[Trivia] Loaded 20 questions from CSV`
   - `[Trivia] Game initialized - join period started`

### **Step 6: Enable Discord Auto-Post (Optional)**

If you configured a Discord webhook:

1. In Streamer.bot, open **Actions** tab
2. Find **"Trivia 08) End Game"** action
3. Double-click to edit
4. Find the **disabled** subaction: `Run Action: Trivia 09) Discord Leaderboards`
5. **Enable** that subaction (right-click → Enable)
6. Save the action

After each game, leaderboards will auto-post to Discord.

---
## 🎮 How to Run a Game

### **Step 1: Initialize (Start Join Period)**

As broadcaster or moderator in chat: `!trivia-init`

Chat will announce:

🎮 TRIVIA SURVIVAL has started! Type !trivia to join!
Players have 30-60 seconds to join before the game begins.

### **Step 2: Players Join**

Viewers type in chat: `!trivia`

They'll receive confirmation: `✅ @Username has joined! (7 players total)`

### **Step 3: Start the Game**

After 30-60 seconds, lock the player list and begin: `!trivia-start`

Chat announces:

🔒 Player list locked! 12 brave contestants are competing!
🎯 10 questions | Answer with A, B, or C
Get ready... Question 1 coming up!

### **Step 4: Game Runs Automatically**

The game will automatically:
1. Display each question
2. Wait (Question Delay)
3. Display answer options with Unicode symbols: `🇦 Two  |  🇧 Four  |  🇨 Six`
4. Collect answers
5. Eliminate wrong/missing answers
6. Announce survivors
7. Continue to next question

**Players answer by typing:** `A` or `B` or `C` (case-insensitive)

### **Step 5: Game Ends Automatically**

The game ends when:
- Final question is answered, OR
- All players are eliminated

End Game runs automatically and displays:
- Game summary
- Winners (if any)
- Top 5 leaderboards in chat
- Optionally posts to Discord

### **Manual Controls (If Needed)**

| Command | Purpose |
|---------|---------|
| `!trivia-ask` | Manually advance to next question (if stuck) |
| `!trivia-end` | Force end the game early |

---

## 📋 CSV Format Guide

Your `questions.csv` file must follow this format:

### **Format:**

Question,A,B,C,Correct

### **Example:**

Question,A,B,C,Correct
"What is 2+2?",Two,Four,Six,B
"What color is the sky?",Red,Blue,Green,B
"How many legs does a spider have?",Six,Eight,Ten,B
"Capital of France?",Berlin,London,Paris,C
"What is the largest ocean?",Atlantic,Pacific,Indian,B
"How many continents are there?",Five,Seven,Nine,B
"What is H2O?",Oxygen,Water,Hydrogen,B
"Who painted the Mona Lisa?",Michelangelo,Da Vinci,Picasso,B
"Fastest land animal?",Lion,Cheetah,Leopard,B
"How many planets in our solar system?",Seven,Eight,Nine,B

### **Rules:**

✅ **First row is header** - Must be exactly: `Question,A,B,C,Correct`  
✅ **5 columns per row** - Question, 3 answer choices, correct letter  
✅ **Use quotes around questions** - If they contain commas  
✅ **Correct answer** - Must be A, B, or C (case-insensitive)  
✅ **Minimum questions** - At least as many as "Questions per Game" setting  

❌ **Don't:**
- Skip the header row
- Use more or fewer than 5 columns
- Use numbers (1, 2, 3) for the correct answer
- Leave rows blank

### **Validation:**

**Windows users:** Use `!trivia-setup` → Basic Setup → "Validate CSV" button

**Linux users:** Check Streamer.bot logs after running `!trivia-init` for errors

---

## 💬 Commands Reference

| Command | Who Can Use | What It Does |
|---------|-------------|--------------|
| `!trivia` | Everyone | Join the game during join period |
| `!trivia-setup` | Mod/Broadcaster | Open configuration UI (Windows only) |
| `!trivia-init` | Mod/Broadcaster | Start join period |
| `!trivia-start` | Mod/Broadcaster | Lock players and begin game |
| `!trivia-ask` | Mod/Broadcaster | Manually advance to next question |
| `!trivia-end` | Mod/Broadcaster | Force end the game early |

---

## ⚙️ Configuration Options

### **Game Settings:**

| Setting | Default | Range | Description |
|---------|---------|-------|-------------|
| Questions per Game | 10 | 1-30 | Total questions each game |
| Question Delay | 5 sec | 0-30 | Delay before accepting answers |
| Answer Window | 15 sec | 5-60 | Time players have to answer |

### **Discord Display Options:**

| Setting | Default | Options | Description |
|---------|---------|---------|-------------|
| Winners Display | Top 10 | Top 5, 10, 25, 50, All | How many winners to show |
| Sole Survivors Display | Top 10 | Top 5, 10, 25, 50, All | How many survivors to show |

---
## 🏆 Leaderboards

### **Two Types of Winners:**

**🏅 Winners (All-Time):**
- Anyone who survives to the final question
- Shared wins are tracked
- Example: 3 players survive = all 3 get +1 win

**👑 Sole Survivors:**
- Players who win ALONE (no ties)
- Most prestigious achievement
- Only tracked when exactly 1 player remains

### **Viewing Leaderboards:**

**In Chat:** Shown automatically at end of each game (Top 5)

**In Setup UI (Windows):** Leaderboards tab → Shows Top 10

**In Discord:** Configurable (Top 5/10/25/50/All)

### **Backup & Restore:**

**Windows (via Setup UI):**
1. Open `!trivia-setup`
2. Go to Leaderboards tab
3. Click "Export Backup" → saves timestamped JSON file to your Data Folder
4. Click "Import Backup" → enter filename from your Data Folder

**Linux (manual):**

Backup: `cp ~/trivia/winners.json ~/trivia/backup-winners-$(date +%Y%m%d).json`

Restore: `cp ~/trivia/backup-winners-20260212.json ~/trivia/winners.json`

**Reset All Leaderboards:**
- Windows: Setup UI → Leaderboards tab → "Reset All Leaderboards"
- Linux: Delete `winners.json` and `champions.json` from your Data Folder

---

## 🔧 Troubleshooting

### **CSV File Not Loading**

**Windows:**
- Run `!trivia-setup` → Basic Setup → "Validate CSV"
- Check error details in the validation box
- Make sure file is named exactly `questions.csv`
- Verify 5 columns per row
- Ensure correct answers are A, B, or C

**Linux:**
- Check file exists: `ls -la ~/trivia/questions.csv`
- Check file permissions: `chmod 644 ~/trivia/questions.csv`
- Verify no Windows line endings: `file ~/trivia/questions.csv`
- Should say: "ASCII text" or "UTF-8 Unicode text"
- If it says "CRLF", convert: `dos2unix ~/trivia/questions.csv`

### **Players Can't Join**

✅ Make sure you ran `!trivia-init` first  
✅ Check game state is "pregame" (not "live" or "ended")  
✅ Verify `!trivia` command is enabled in Streamer.bot  
✅ Check Streamer.bot logs for errors  

### **Answers Not Registering**

✅ Players must type exactly: `A`, `B`, or `C` (case-insensitive)  
✅ Answers only count during the answer window (after options display)  
✅ Each player can only answer once per question  
✅ Check "Trivia 06) Collect Answers" has Chat Message trigger enabled  

### **Game Gets Stuck**

**Manually advance:**
- `!trivia-ask` (skip to next question)
- `!trivia-end` (force end the game)

**Check Streamer.bot logs** for errors

**Restart fresh:**
- `!trivia-end`
- `!trivia-init`

### **Discord Not Posting**

✅ Webhook URL is valid (starts with `https://discord.com/api/webhooks/`)  
✅ Discord subaction is **enabled** in "Trivia 08) End Game"  
✅ Variable `TriviaDiscordWebhook` is set correctly  
✅ Check Streamer.bot logs for Discord API errors  

### **Leaderboards Not Saving (Linux)**

Check folder write permissions: `chmod 755 ~/trivia`

Check if files exist: `ls -la ~/trivia/*.json`

Manually verify global variables exist in Settings → Variables → Global:
- `TriviaSharedWins`
- `TriviaChampionWins`

### **Path Issues (Linux)**

❌ **Don't use:** `~/trivia` (shell expansion may not work)  
✅ **Use:** `/home/username/trivia` (absolute path)  
✅ **Remember:** Linux paths are case-sensitive  

---
## 📊 Discord Integration

### **Setup:**

1. **Create Discord Webhook:**
   - Open your Discord server
   - Go to Server Settings → Integrations → Webhooks
   - Click "New Webhook"
   - Choose the channel for trivia posts
   - Copy the Webhook URL

2. **Configure in Trivia:**
   - **Windows:** `!trivia-setup` → Discord tab → Paste URL
   - **Linux:** Add `TriviaDiscordWebhook` global variable

3. **Enable Auto-Posting:**
   - Open "Trivia 08) End Game" action
   - Enable the "Discord Leaderboards" subaction
   - Save

### **Manual Post:**

You can also post leaderboards on-demand by triggering "Trivia 09) Discord Leaderboards" action manually, or set up a custom command like `!trivia-discord`

### **Example Discord Post:**

📊 TRIVIA LEADERBOARDS

🏅 Winners:
1. PlayerOne - 5 wins
2. PlayerTwo - 3 wins
3. PlayerThree - 2 wins

👑 Sole Survivors:
1. PlayerOne - 2 times
2. PlayerFour - 1 time

---

## 🎯 Tips & Best Practices

### **For Streamers:**

✅ **Announce in advance** - Let viewers know trivia is coming  
✅ **Allow 30-60 seconds** for joins - Don't rush `!trivia-start`  
✅ **Have 15-20 questions minimum** - More variety per game  
✅ **Mix difficulty levels** - Keep it fun for everyone  
✅ **Backup leaderboards regularly** - Export before major changes  
✅ **Test with mods first** - Run through a full game before going live  

### **Question Writing Tips:**

✅ **Keep questions concise** - Short and clear  
✅ **Avoid trick questions** - Make it fair and fun  
✅ **Mix topics** - Pop culture, general knowledge, stream-specific  
✅ **Balance difficulty** - Easy opener, harder mid-game, medium finale  
✅ **Test for ambiguity** - Make sure one answer is clearly correct  

### **Timing Recommendations:**

- **Fast-paced:** 5 sec delay, 10 sec window, 5 questions
- **Balanced:** 5 sec delay, 15 sec window, 10 questions (default)
- **Casual:** 3 sec delay, 20 sec window, 8 questions

---

## 🔄 Updating Questions

You can update questions anytime:

1. Edit your `questions.csv` file (add/remove/change questions)
2. Save the file
3. Run `!trivia-init` to load the new questions

**No need to restart Streamer.bot** - questions load fresh each game.

---

## 📝 Version History

### **v1.0.1 (2026-02-12)**
- Performance optimization: Collect Answers action only runs during answer windows
- Improved efficiency in busy channels
- Better state management

### **v1.0 (2026-02-12)**
- Initial release
- 10 questions per game (configurable 1-30)
- Elimination-style gameplay
- Dual leaderboards (Winners + Sole Survivors)
- Discord integration
- Windows GUI setup
- Linux manual setup support
- Backup/restore system
- Unicode answer symbols (no emoji conflicts)

---

## 📜 License & Credits

**Created for Streamer.bot**  
**Version:** 1.0  
**Release Date:** February 12, 2026  
**Compatible with:** Streamer.bot v0.2.0+ and v1.0.4+

**Platform Support:**  
✅ Windows (Full GUI)  
✅ Linux (Manual Configuration)

**This extension is provided as-is with no warranty.**  
Feel free to modify for personal use.

---

## 🆘 Support

**Check Streamer.bot Logs:**
- Bottom panel in Streamer.bot
- Look for `[Trivia]` prefixed messages
- Errors will show specific issues

**Common Issues:**
- See "Troubleshooting" section above
- Verify all configuration variables are set
- Check CSV file format and path
- Ensure commands are enabled

**For bugs or feature requests:**
- Include Streamer.bot version
- Include platform (Windows/Linux)
- Include relevant log messages
- Describe steps to reproduce

---

## 🎉 Enjoy Your Trivia Games!

Have fun engaging your community with Trivia Survival!

**Good luck to all contestants!** 🏆

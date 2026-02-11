# ⚙️ Configuration Guide

This guide covers all the settings you can customize to make the Trivia Survival Game work exactly how you want.

---

## Table of Contents

1. [Global Variables Overview](#global-variables-overview)
2. [Game Settings](#game-settings)
3. [CSV Configuration](#csv-configuration)
4. [Discord Webhooks (Optional)](#discord-webhooks-optional)
5. [Error Logging](#error-logging)
6. [Advanced Customization](#advanced-customization)

---

## Global Variables Overview

All game settings are controlled through **Global Variables** in Streamer.bot.

To access them:
1. Open Streamer.bot
2. Go to **Settings** → **Variables**
3. Find the variable you want to change
4. Edit its value
5. Click **OK**

**Important:** Some variables are managed automatically by the game - don't change these manually during a game!

---

## Game Settings

### Core Configuration

| Variable Name | Type | Default | Description | Safe to Edit? |
|---------------|------|---------|-------------|---------------|
| `TriviaCSVPath` | String | (required) | Full path to your questions CSV file | ✅ (when not running) |
| `TriviaQuestionCount` | Number | `10` | Number of questions per game | ✅ (before initialize) |
| `TriviaAnswerWindow` | Number | `15` | Seconds players have to answer each question | ✅ (before initialize) |
| `TriviaQuestionDelay` | Number | `5` | Seconds between showing question and answer options | ✅ (before initialize) |

#### Detailed Settings:

##### `TriviaCSVPath`
**What it does:** Points to your question bank file

**Examples:**
- Windows: `C:\\StreamerBot\\Trivia\\questions.csv`
- Alternative: `C:/StreamerBot/Trivia/questions.csv`

**Tips:**
- Use full paths (not relative)
- Must end in `.csv`
- File must exist before running Initialize

---

##### `TriviaQuestionCount`
**What it does:** How many questions are asked per game

**Range:** 1 - 999 (practical range: 5-20)

**Recommendations:**
- **5-7 questions:** Quick games (~3-5 minutes)
- **10 questions:** Standard games (~6-8 minutes) ⭐ Recommended
- **15-20 questions:** Long games (~10-15 minutes)

**Note:** Your CSV must have at least this many questions!

---

##### `TriviaAnswerWindow`
**What it does:** How long (in seconds) players have to type their answer

**Range:** 5 - 60 seconds

**Recommendations:**
- **10 seconds:** Fast-paced, intense ⚡
- **15 seconds:** Balanced ⭐ Recommended
- **20-30 seconds:** Relaxed, accessible

**Consider:**
- Longer windows = more inclusive for slower typers
- Shorter windows = more exciting, higher stakes

---

##### `TriviaQuestionDelay`
**What it does:** Delay between question appearing and answer options appearing

**Range:** 0 - 30 seconds

**Recommendations:**
- **3-5 seconds:** Standard ⭐ Recommended
- **0-2 seconds:** Fast-paced
- **10+ seconds:** For read-aloud questions on stream

**Why have a delay?**
- Prevents instant guessing
- Gives you time to read the question aloud
- Creates dramatic tension

---

### Auto-Managed Variables (Do Not Edit Manually)

These variables are controlled by the game - **don't change them manually**:

| Variable Name | Type | Purpose |
|---------------|------|---------|
| `TriviaGameState` | String | Tracks current game phase (idle/pregame/starting/live/ended) |
| `TriviaAcceptingAnswers` | Boolean | Controls when answers can be submitted |
| `TriviaCurrentQuestion` | Number | Tracks which question you're on |
| `TriviaPlayersJoined` | Number | Total players who joined |
| `TriviaPlayersAlive` | Number | Players still in the game |
| `TriviaPlayers` | Dictionary | Player data (answers, status, etc.) |
| `TriviaQuestions` | List | Loaded questions from CSV |

---

## CSV Configuration

### File Format

Your CSV must follow this **exact** structure:

```csv
Question,A,B,C,Correct
"What is the capital of France?",London,Paris,Berlin,B
"What is 2+2?",3,4,5,B
"Which planet is closest to the Sun?",Venus,Mercury,Mars,B
```

### CSV Rules

✅ **Required:**
- Header row: `Question,A,B,C,Correct`
- At least as many questions as `TriviaQuestionCount`
- `Correct` column must be `A`, `B`, or `C` (uppercase)

❌ **Common Mistakes:**
- Missing header row
- Wrong header names
- Lowercase correct answers (a, b, c)
- Fewer questions than configured
- Missing columns

### Formatting Tips

**Use quotes for text with commas or special characters:**
```csv
Question,A,B,C,Correct
"Which actor said ""I'll be back""?",Stallone,Schwarzenegger,Willis,B
```

**Keep questions concise for chat:**
- ✅ Good: "What color is grass?"
- ❌ Too long: "In the natural world, when considering the typical pigmentation of the most common variety of lawn grass species found in temperate climates, what color would you generally observe?"

**Balance difficulty:**
- Mix easy, medium, and hard questions
- Consider your audience's knowledge areas
- Test questions before using them live

---

## Discord Webhooks (Optional)

Post game results and leaderboards to Discord automatically!

### Setup

#### Step 1: Create a Discord Webhook

1. Go to your Discord server
2. Select a channel → Click the gear icon ⚙️ (Edit Channel)
3. Go to **Integrations** → **Webhooks**
4. Click **New Webhook**
5. Name it (e.g., "Trivia Bot")
6. **Copy Webhook URL**

#### Step 2: Configure in Streamer.bot

Create these Global Variables:

| Variable Name | Type | Value |
|---------------|------|-------|
| `TriviaDiscordWebhook` | String | Your webhook URL |
| `TriviaEnableDiscord` | Boolean | `true` |

#### Step 3: Test It

Run a full game - results should appear in your Discord channel!

### What Gets Posted?

When enabled, the following are sent to Discord:

1. **Game Start** - "Game starting with X players!"
2. **Game Results** - Winners/Sole Survivor announcement
3. **Leaderboards** - Top 5 Winners and Sole Survivors

### Customizing Messages

Messages are sent from the C# code in the `Trivia - Intermission` action. To customize:

1. Open the action in Streamer.bot
2. Find the C# subaction
3. Look for the Discord webhook sections
4. Edit the message strings

---

## Error Logging

The game includes built-in error logging for CSV validation and game issues.

### Log Location

Errors are logged to Streamer.bot's **Log** tab.

### What Gets Logged?

- **CSV Errors:**
  - File not found
  - Invalid format
  - Missing columns
  - Invalid correct answers
  - Line-by-line errors with row numbers

- **Game Errors:**
  - Players trying to join when game is running
  - Invalid game states
  - Answer processing issues

### Viewing Logs

1. In Streamer.bot, go to the **Log** tab
2. Look for entries prefixed with `[Trivia]`
3. Errors show with details about what went wrong

### Example Log Output

```
[Trivia] Initializing game...
[Trivia] CSV loaded successfully: 50 questions found
[Trivia] Game ready - pregame state activated
```

Or if there's an error:
```
[Trivia] ERROR: CSV file not found at C:\questions.csv
[Trivia] ERROR: Row 15 - Invalid correct answer 'D' (must be A, B, or C)
```

---

## Advanced Customization

### Custom Chat Messages

All chat messages are sent via Streamer.bot's "Send Message to Channel" subactions. To customize:

1. Open any trivia action (e.g., `Trivia - Initialize`)
2. Find the "Send Message to Channel" subactions
3. Edit the message text
4. Save the action

**Tip:** Use Twitch emotes for personality! 
- Example: `"Game starting! PogChamp Get ready!"`

### Leaderboard Storage

Leaderboards are stored using Streamer.bot's **User Variables**:

| User Variable | Description |
|---------------|-------------|
| `TriviaWins` | Total wins (including sole survivor) |
| `TriviaSoleSurvivors` | Sole survivor wins only |

These persist across streams automatically!

**To reset leaderboards:**
1. Go to **Users** tab in Streamer.bot
2. Right-click → **Manage User Variables**
3. Delete `TriviaWins` and `TriviaSoleSurvivors` for users

### Custom Point Rewards

Instead of `!trivia` command, use Channel Points:

1. Create a reward on Twitch (e.g., "Join Trivia" - 10 points)
2. In Streamer.bot:
   - **Platforms** → **Twitch** → **Channel Point Rewards**
   - Right-click your reward → **Assign Action**
   - Select `Trivia - Join`

**Tip:** Make it free or very cheap so everyone can join!

### Question Randomization

**Q: Are questions random?**
A: Currently, questions are asked in CSV order.

**To randomize:**
1. Open the `Trivia - Initialize` action
2. In the C# subaction (CSV reading)
3. Add code to shuffle the questions list after loading

We can add this feature if there's interest!

---

## Example Configurations

### Quick Fire Mode (Fast Games)
```
TriviaQuestionCount: 5
TriviaAnswerWindow: 10
TriviaQuestionDelay: 3
```
Game length: ~3-4 minutes

---

### Standard Mode (Balanced)
```
TriviaQuestionCount: 10
TriviaAnswerWindow: 15
TriviaQuestionDelay: 5
```
Game length: ~6-8 minutes ⭐

---

### Marathon Mode (Long Games)
```
TriviaQuestionCount: 20
TriviaAnswerWindow: 20
TriviaQuestionDelay: 5
```
Game length: ~12-15 minutes

---

### Accessible Mode (Inclusive)
```
TriviaQuestionCount: 8
TriviaAnswerWindow: 25
TriviaQuestionDelay: 10
```
More time for everyone to participate!

---

## Tips for Success

### Before Your First Stream
1. **Test everything** with a few questions
2. **Run a mock game** with mods/friends
3. **Prepare backups** - have extra question CSVs ready

### Question Bank Tips
1. **Start with 50+ questions** so you can run multiple games
2. **Categorize questions** - make themed CSVs (e.g., movies.csv, gaming.csv)
3. **Rotate question sets** to keep games fresh

### Managing Games
1. **Announce games in advance** - give viewers time to join
2. **Set a join deadline** - "Join now! Starting in 2 minutes!"
3. **Use !revive sparingly** - only for technical issues/unfairness

---

## Need Help?

- 📖 Check the [Setup Guide](SETUP.md)
- 💬 Open an [issue on GitHub](https://github.com/1moreastronaut/trivia-survival-game/issues)
- 📺 Ask [@1moreastronaut](https://twitch.tv/1moreastronaut)

---

**Next:** [Commands Reference →](COMMANDS.md)

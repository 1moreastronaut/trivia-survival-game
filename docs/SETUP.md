# 🛠️ Setup Guide

This guide will walk you through setting up the Trivia Survival Game in Streamer.bot.

---

## Prerequisites

Before you begin, make sure you have:

- ✅ [Streamer.bot](https://streamer.bot/) installed (v0.2.0 or later recommended)
- ✅ Streamer.bot connected to your Twitch account
- ✅ Basic familiarity with Streamer.bot's interface

---

## Step 1: Download the Files

1. Go to the [Releases](https://github.com/1moreastronaut/trivia-survival-game/releases) page
2. Download the latest release ZIP file
3. Extract it to a location on your computer

You should have:
- `trivia-actions.sb` - The Streamer.bot actions file
- `sample-questions.csv` - Example questions to get started
- Documentation files

---

## Step 2: Import Actions into Streamer.bot

1. Open **Streamer.bot**
2. Go to the **Actions** tab
3. Right-click in the actions list → **Import**
4. Select the `trivia-actions.sb` file you downloaded
5. Click **OK** to import all actions

You should now see a group of actions with names like:
- `Trivia - Initialize`
- `Trivia - Join`
- `Trivia - Start Game`
- `Trivia - Ask Question`
- etc.

---

## Step 3: Prepare Your Questions CSV File

### Option A: Use the Sample File (Quick Start)

1. Copy `sample-questions.csv` to a permanent location (e.g., `C:\Streamer.bot\Trivia\questions.csv`)
2. Note the **full file path** - you'll need it in the next step

### Option B: Create Your Own

Create a CSV file with this exact structure:

```csv
Question,A,B,C,Correct
"What color is the sky?",Red,Blue,Green,B
"What is 2+2?",3,4,5,B
```

**Important CSV Rules:**
- First row MUST be the header: `Question,A,B,C,Correct`
- `Correct` column must be `A`, `B`, or `C` (case-sensitive)
- Questions and answers should be in quotes if they contain commas
- You need at least 10 questions (or adjust the question count - see Configuration Guide)

---

## Step 4: Configure Global Variables

Streamer.bot uses **Global Variables** to store game settings. You need to set these up:

1. In Streamer.bot, go to **Settings** → **Variables**
2. Create the following **Global Variables**:

### Required Variables

| Variable Name | Type | Default Value | Description |
|---------------|------|---------------|-------------|
| `TriviaCSVPath` | String | `C:\path\to\questions.csv` | Full path to your CSV file |
| `TriviaQuestionCount` | Number | `10` | How many questions per game |
| `TriviaAnswerWindow` | Number | `15` | Seconds players have to answer |
| `TriviaQuestionDelay` | Number | `5` | Seconds between question and answers appearing |
| `TriviaGameState` | String | `idle` | Current game state (auto-managed) |
| `TriviaAcceptingAnswers` | Boolean | `false` | Answer window status (auto-managed) |

### Optional Variables (for Discord Webhooks)

| Variable Name | Type | Default Value | Description |
|---------------|------|---------------|-------------|
| `TriviaDiscordWebhook` | String | (empty) | Discord webhook URL for leaderboards |
| `TriviaEnableDiscord` | Boolean | `false` | Enable Discord leaderboard posting |

### How to Create a Variable:
1. Click **"Add"**
2. Enter the **Variable Name** exactly as shown above
3. Set the **Value**
4. Click **OK**

---

## Step 5: Set Up Triggers

Now you need to connect the actions to Twitch events:

### 1. Join Trigger (`!trivia` command)

**Option A: Chat Command (Recommended)**
1. Go to **Commands** tab
2. Click **Add** to create a new command
3. Configure:
   - **Command:** `!trivia`
   - **Enabled:** ✅
   - **Location:** Chat
4. Click **Actions** tab within the command
5. Add action: `Trivia - Join`
6. Click **OK**

**Option B: Channel Points Reward**
1. Create a Channel Points reward on Twitch (e.g., "Join Trivia")
2. In Streamer.bot, go to **Platforms** → **Twitch** → **Channel Reward Redemptions**
3. Right-click the reward → **Assign Action**
4. Select `Trivia - Join`

### 2. Collect Answers Trigger (Chat Message)

1. Go to **Events** tab
2. Find **Twitch** → **Chat** → **Chat Message**
3. Right-click → **Add Action**
4. Select `Trivia - Collect Answers`
5. Click **OK**

**Note:** This action will automatically filter messages - it only processes A/B/C answers during the answer window.

### 3. Broadcaster/Mod Controls (Commands)

Create these commands the same way as the `!trivia` command:

| Command | Action | Permissions |
|---------|--------|-------------|
| `!leave` | `Trivia - Leave` | Everyone |
| `!remove` | `Trivia - Mod Remove` | Moderator/Broadcaster only |
| `!revive` | `Trivia - Revive` | Moderator/Broadcaster only |

For mod-only commands:
1. In the command settings, check **"Moderators"** and **"Broadcaster"** under **Allowed**

---

## Step 6: Test Your Setup

### Initialize Test
1. In Streamer.bot, find `Trivia - Initialize` action
2. Right-click → **Test**
3. Check your **Twitch chat** - you should see a message announcing the game is ready
4. Check Streamer.bot's **Log** tab for any errors

**If you see errors:**
- Double-check your `TriviaCSVPath` variable points to a valid file
- Verify your CSV file follows the correct format
- See [Troubleshooting](#troubleshooting) below

### Full Game Test
1. Run `Trivia - Initialize` (right-click → Test)
2. Type `!trivia` in your chat to join
3. Run `Trivia - Start Game`
4. The game should begin automatically!

---

## Step 7: How to Run a Game

Once everything is set up:

1. **Before stream/when ready:**
   - Run `Trivia - Initialize` action (manual trigger or stream deck button)
   - Viewers can now join with `!trivia`

2. **When ready to start:**
   - Run `Trivia - Start Game` action
   - The game begins automatically - sit back and enjoy!

3. **During the game:**
   - Questions and eliminations happen automatically
   - Use `!remove @username` or `!revive @username` if needed

4. **After the game:**
   - Winners and leaderboards are announced automatically

---

## Troubleshooting

### "CSV file not found" error
- Check that `TriviaCSVPath` variable has the **full path** to your CSV file
- Make sure the file exists at that location
- Windows paths need double backslashes: `C:\\Streamer.bot\\questions.csv`

### Questions not appearing in chat
- Verify you have at least 10 questions in your CSV (or lower `TriviaQuestionCount`)
- Check the CSV format - make sure the header is exactly: `Question,A,B,C,Correct`

### Players can't join
- Make sure you've run `Trivia - Initialize` first
- Verify the `!trivia` command trigger is set up correctly
- Check that the command is enabled

### Answers not being collected
- Ensure the **Chat Message** event trigger is connected to `Trivia - Collect Answers`
- Check that `TriviaAcceptingAnswers` is `true` during the answer window (check Variables tab)

### Still having issues?
- Check Streamer.bot's **Log** tab for error messages
- Open an [issue on GitHub](https://github.com/1moreastronaut/trivia-survival-game/issues) with details

---

## Next Steps

✅ Setup complete! Now you can:
- [Customize your game settings](CONFIGURATION.md)
- [Learn all available commands](COMMANDS.md)
- Create your own question bank
- Set up Discord webhooks for leaderboards (optional)

---

## Optional: Stream Deck Integration

You can add buttons to your Stream Deck to control the game:

1. Add a **Streamer.bot** button
2. Configure it to run these actions:
   - `Trivia - Initialize` (game setup)
   - `Trivia - Start Game` (start the game)

---

**Need help?** Open an issue or reach out to [@1moreastronaut](https://twitch.tv/1moreastronaut)!

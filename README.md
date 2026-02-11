# 🎮 Trivia Survival Game for Streamer.bot

A **single-elimination trivia competition** that runs entirely in Twitch chat using [Streamer.bot](https://streamer.bot/).

Perfect for streamers who want to engage their community with interactive trivia games - **no OBS overlays required!**

---

## ✨ Features

- 🎯 **Single-elimination format** - Answer wrong, you're out!
- ⏱️ **Timed questions** with configurable answer windows
- 💬 **100% chat-based** - no visuals needed, pure Twitch chat gameplay
- 🏆 **Leaderboard tracking** - Winners and Sole Survivors
- 🔧 **Fully configurable** - adjust question count, timers, answer windows
- 📝 **CSV question bank** - easy to create, edit, and customize
- 🛡️ **Moderator controls** - remove players, revive eliminated contestants
- 🪝 **Discord webhook support** (optional) - post results to Discord
- 📊 **Error logging** - detailed CSV validation and game error tracking
- ⚡ **Easy setup** - import actions and start playing in minutes

---

## 🎲 How It Works

### Game Flow

1. **🎬 Pre-Game (Joining Phase)**
   - Streamer initializes the game
   - Viewers join using `!trivia` command in chat
   - Join window stays open until streamer starts the game

2. **🚀 Game Start**
   - Streamer triggers "Start Game"
   - Player count locks in - no late joins allowed
   - First question appears automatically

3. **❓ Questions (10 by default)**
   - Question appears in chat
   - 5 seconds later: Answer options (A/B/C) appear and answer window opens
   - Players have 15 seconds to type their answer (A, B, or C)
   - No answer changes allowed - first answer is locked in
   - Timer ends, correct answer revealed
   - Wrong answers = elimination
   - Eliminated players announced in chat

4. **🏆 Results**
   - **1 survivor** = **Sole Survivor** 👑 (special recognition!)
   - **2+ survivors** = **Winners** 🎉
   - Leaderboards updated and displayed in chat
   - Optional: Results posted to Discord via webhook

### Example Game in Chat

```
Bot: 🎮 Trivia Survival is starting! Type !trivia to join the game!

Viewer1: !trivia
Bot: @Viewer1 has joined! (1 player)

Viewer2: !trivia
Bot: @Viewer2 has joined! (2 players)

[...more players join...]

Bot: Game starting with 12 players! No more joins allowed. Good luck!

Bot: Question 1: What is the capital of France?
[5 seconds pass]
Bot: Answer window open! A) London  B) Paris  C) Berlin

Viewer1: B
Viewer2: A
Viewer3: B

[15 seconds pass]
Bot: Correct answer: B) Paris
Bot: Eliminated: @Viewer2 (11 players remain)

[...9 more questions...]

Bot: 🏆 GAME OVER! 🏆
Bot: Sole Survivor: @Viewer1 👑
Bot: 📊 Top Winners: @Viewer1 (5 wins), @Viewer5 (3 wins)...
```

---

## 🚀 Quick Start

### Prerequisites

- ✅ [Streamer.bot](https://streamer.bot/) installed (v0.2.0 or later)
- ✅ Streamer.bot connected to your Twitch account
- ✅ Basic familiarity with Streamer.bot

### Installation (5 minutes)

1. **Download** the latest release from [Releases](https://github.com/1moreastronaut/trivia-survival-game/releases)
2. **Import** actions into Streamer.bot
3. **Configure** your settings (CSV path, timers, etc.)
4. **Create** or use the sample questions CSV
5. **Test** and play!

👉 **[Full Setup Guide](docs/SETUP.md)** - Detailed step-by-step instructions

---

## 📚 Documentation

| Guide | Description |
|-------|-------------|
| **[📖 Setup Guide](docs/SETUP.md)** | Complete installation and setup instructions |
| **[⚙️ Configuration Guide](docs/CONFIGURATION.md)** | Customize timers, questions, Discord webhooks, and more |
| **[💬 Commands Reference](docs/COMMANDS.md)** | All available commands for players and moderators |

---

## 🎮 Commands

### Player Commands

| Command | Description | When Available |
|---------|-------------|----------------|
| `!trivia` | Join the game | Pre-game only |
| `!leave` | Leave the game voluntarily | Pre-game & Live |

### Moderator/Broadcaster Commands

| Command | Description | When Available |
|---------|-------------|----------------|
| `!remove @user` | Remove a player from the game | Anytime during game |
| `!revive @user` | Revive an eliminated player | Live game & After game |

👉 **[Full Commands Guide](docs/COMMANDS.md)**

---

## ⚙️ Configuration Options

All settings are controlled via Streamer.bot Global Variables:

| Setting | Default | Description |
|---------|---------|-------------|
| **Question Count** | 10 | Number of questions per game |
| **Answer Window** | 15 seconds | Time players have to answer |
| **Question Delay** | 5 seconds | Delay before showing answer options |
| **CSV Path** | (required) | Path to your questions file |

**Optional Features:**
- 🪝 Discord webhook integration for leaderboards
- 📊 Detailed error logging
- 🎨 Customizable chat messages

👉 **[Full Configuration Guide](docs/CONFIGURATION.md)**

---

## 📝 Creating Questions

Questions are stored in a simple CSV file:

```csv
Question,A,B,C,Correct
"What is the capital of France?",London,Paris,Berlin,B
"What is 2+2?",3,4,5,B
```

### CSV Format Rules:

- ✅ Header row: `Question,A,B,C,Correct`
- ✅ `Correct` must be `A`, `B`, or `C` (uppercase)
- ✅ Use quotes for questions/answers with commas
- ✅ Need at least as many questions as your configured question count

👉 **[Example CSV](examples/sample-questions.csv)** - 30 sample questions ready to use!

---

## 🎯 Game Modes & Ideas

### Quick Fire (Fast-paced)
- 5 questions, 10 second answer window
- Perfect for short stream segments

### Standard Mode (Balanced)
- 10 questions, 15 second answer window
- Best for most streams

### Marathon Mode
- 20 questions, 20 second answer window
- Epic challenge for dedicated viewers

### Themed Games
Create different CSV files for different themes:
- 🎬 Movies & TV
- 🎮 Gaming trivia
- 🎵 Music knowledge
- 🌍 Geography
- 📚 Literature

---

## 🔧 Technical Details

### What's Included

**Essential Actions (13 total):**
- Initialize
- Join
- Start Game
- Ask Question
- Open Answers
- Collect Answers
- End Question
- Intermission
- Update Player Counts
- End Game
- Leave
- Mod Remove
- Revive

**Features:**
- Dictionary-based player tracking
- Game state management (idle/pregame/starting/live/ended)
- CSV validation with detailed error logging
- User variable persistence for leaderboards
- Discord webhook integration
- Answer locking (no changes once submitted)

### Requirements

- **Streamer.bot v0.2.0+** (for C# Execute Code support)
- **Twitch connection** (for chat interaction)
- **CSV file** with valid questions

---

## 🐛 Troubleshooting

### Common Issues

**"CSV file not found" error**
- Check your `TriviaCSVPath` variable has the full file path
- Use `C:\path\to\file.csv` or `C:/path/to/file.csv`

**Players can't join**
- Make sure game is initialized (run Initialize action)
- Check that `!trivia` command trigger is set up

**Answers not being collected**
- Verify Chat Message event is connected to "Collect Answers" action
- Check Streamer.bot logs for errors

👉 **[Full Troubleshooting Guide](docs/SETUP.md#troubleshooting)**

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

- 🐛 **Report bugs** - Open an issue with details
- 💡 **Suggest features** - Share your ideas
- 📝 **Improve docs** - Submit corrections or clarifications
- 🎨 **Share question banks** - Create themed CSV files

### Reporting Issues

When reporting a bug, please include:
1. What you were trying to do
2. What happened vs. what you expected
3. Streamer.bot version
4. Relevant error messages from logs

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

**TL;DR:** Free to use, modify, and distribute. Just keep the license notice.

---

## 💡 Credits & Thanks

**Created by:** [@1moreastronaut](https://github.com/1moreastronaut)

**Powered by:** [Streamer.bot](https://streamer.bot/) by Nate (nate1280)

### Using This on Your Stream?

I'd love to hear about it! 

- 📺 Twitch: [twitch.tv/1moreastronaut](https://twitch.tv/1moreastronaut)
- 🐦 Tag me or share your experiences!
- ⭐ Star this repo if you find it useful!

---

## ⭐ Support This Project

If you find this useful:

- ⭐ **Star this repository** on GitHub
- 📢 **Share it** with other streamers
- 🐛 **Report bugs** to help improve it
- 💬 **Provide feedback** on what works and what doesn't

---

## 🔮 Roadmap & Future Ideas

Potential features being considered:

- 🎲 Question randomization option
- 📊 More detailed statistics tracking
- 🎨 Multiple question formats (True/False, Fill-in-blank)
- ⏰ Configurable pre-game join timer
- 🏅 Achievement system
- 📈 Per-game analytics

Have an idea? [Open an issue](https://github.com/1moreastronaut/trivia-survival-game/issues) and let's discuss!

---

## 📞 Support & Community

- 💬 **Questions?** Open a [GitHub Issue](https://github.com/1moreastronaut/trivia-survival-game/issues)
- 📖 **Docs:** Check the [/docs](docs/) folder
- 🎮 **Live help:** Stop by [my stream](https://twitch.tv/1moreastronaut)

---

**Ready to get started?** → [Setup Guide](docs/SETUP.md)

**Need to customize?** → [Configuration Guide](docs/CONFIGURATION.md)

**Questions about commands?** → [Commands Reference](docs/COMMANDS.md)

---

Made with ❤️ for the Twitch community
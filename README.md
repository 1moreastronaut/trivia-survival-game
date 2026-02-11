# Trivia Survival Game for Streamer.bot

A single-elimination, chat-based trivia game for Twitch streamers using Streamer.bot. Viewers compete to be the last one standing by correctly answering multiple-choice questions!

## 🎮 Features

- **Chat-Based Gameplay** - All game interactions happen in Twitch chat
- **Single-Elimination Format** - Wrong answers eliminate players immediately
- **Configurable Settings** - Customize question count, timers, and join methods
- **Leaderboard System** - Track Winners and Sole Survivors (optional Discord webhook support)
- **Moderation Tools** - Remove or revive players as needed
- **Error Logging** - Built-in CSV validation and error tracking
- **Fair Play** - No answer changes allowed, locked-in join periods

## 🎯 How It Works

1. **Pre-Game Join Period** - Viewers use `!trivia` to join
2. **Streamer Starts Game** - Locks in contestants
3. **Questions Begin** - Question appears in chat
4. **Answer Window Opens** - 5 seconds later, answer options (A/B/C) appear
5. **Contestants Answer** - 15-second window to type A, B, or C
6. **Elimination Round** - Wrong answers eliminate players
7. **Winner(s) Crowned** - Last player(s) standing win!

### Special Win Conditions
- **Winners** - Multiple players survive all questions
- **Sole Survivor** - Only ONE player survives all questions (harder achievement)

## 📋 Requirements

- [Streamer.bot](https://streamer.bot/) (v0.2.0 or higher recommended)
- Twitch account connected to Streamer.bot
- Questions CSV file (template provided)

## 🚀 Quick Start

### 1. Download Files
Download or clone this repository to your computer.

### 2. Import Actions
See [SETUP.md](SETUP.md) for detailed installation instructions.

### 3. Configure Settings
Edit the Global Variables in Streamer.bot (see [CONFIGURATION.md](CONFIGURATION.md))

### 4. Add Questions
Use the provided [questions-template.csv](questions-template.csv) to create your trivia questions.

### 5. Start Playing!
Run the **Initialize** action and you're ready to go!

## 📚 Documentation

- **[Setup Guide](SETUP.md)** - Step-by-step installation
- **[Configuration Guide](CONFIGURATION.md)** - Customize your game
- **[Commands Reference](COMMANDS.md)** - All available commands
- **[CSV Format Guide](CSV-FORMAT.md)** - How to create questions

## 🎪 Commands

| Command | Who Can Use | Description |
|---------|-------------|-------------|
| `!trivia` | Everyone | Join the game during pre-game period |
| `!leave` | Contestants | Leave the game voluntarily |
| `!revive <username>` | Mods/Broadcaster | Revive an eliminated player |
| Remove player | Mods/Broadcaster | Remove a player from the game |

## 🏆 Leaderboard (Optional)

The game tracks two types of wins:
- **Winners** - All wins (including Sole Survivor wins)
- **Sole Survivors** - Only games won alone

Optional Discord webhook integration to post results!

## 🤝 Contributing

Found a bug or have a suggestion? Feel free to open an issue or submit a pull request!

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Credits

Created by [@1moreastronaut](https://github.com/1moreastronaut)

Built for the Streamer.bot community

---

**Ready to test your viewers' trivia knowledge? Let the games begin!** 🎉
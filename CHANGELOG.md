# Changelog

All notable changes to Trivia Survival will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---
## [1.0.1] - 2026-02-12

### 🔧 Performance Improvements

#### Fixed

- **Collect Answers action optimization** - The "Trivia 06) Collect Answers" action now only enables during answer windows, significantly reducing overhead in busy channels
  - Action enables when answer options appear
  - Action disables after answer window closes
  - Safety disables added to Initialize and End Game actions
  - Prevents unnecessary message processing when game is idle/inactive
  - Improves performance in high-traffic channels (100+ messages/minute)

#### Technical Details

**Actions Modified:**
- `Trivia 01) Initialize` - Added disable subaction at start
- `Trivia 05) Open Answers` - Added enable subaction when answer window opens
- `Trivia 07) End Question` - Added disable subaction when answer window closes
- `Trivia 08) End Game` - Added disable subaction at game end

**Default State:**
- `Trivia 06) Collect Answers` now ships in disabled state by default

#### Benefits

- ✅ Reduced CPU/memory usage in busy channels
- ✅ No interference with other chat-based features
- ✅ Cleaner state management
- ✅ More explicit action lifecycle

#### Migration from v1.0.0

If updating from v1.0.0:
1. Import the new `Trivia-Survival-v1.0.1.sb` file (overwrite existing actions)
2. No configuration changes needed
3. Test that answers are collected correctly during games

---

## [1.0.0] - 2026-02-12

### 🎉 Initial Release

First stable release of Trivia Survival for Streamer.bot!

### Added

#### Core Gameplay
- **Elimination-style trivia game** - Wrong answers eliminate players
- **Multiple-choice questions** - A/B/C answer format
- **Automatic game flow** - Questions advance automatically with timers
- **10 configurable actions** - Complete game system (00-09)
- **6 chat commands** - Full player and moderator control
- **CSV question loading** - Easy question bank management
- **Question randomization** from loaded CSV file

#### Leaderboard System
- **Dual leaderboard tracking**:
  - **Winners leaderboard** - Tracks all players who survive to the end
  - **Sole Survivors leaderboard** - Special tracking for solo winners
- **Persistent leaderboards** - Data saved between games and Streamer.bot restarts
- **Top 5 display in chat** - Automatic leaderboard announcements after each game
- **JSON file backup** - Leaderboards stored in data folder

#### Configuration
- **Windows Setup UI** - Graphical configuration interface (`!trivia-setup`)
  - Data folder configuration
  - CSV validation with error details
  - Game timing settings (question delay, answer window)
  - Discord webhook configuration
  - Leaderboard viewer
  - Backup/restore functionality
  - Reset leaderboards
- **Linux manual configuration** - Full support via global variables
- **Configurable game settings**:
  - Questions per game (1-30, default: 10)
  - Question delay (0-30 seconds, default: 5)
  - Answer window (5-60 seconds, default: 15)

#### Discord Integration
- **Discord webhook support** - Post leaderboards to Discord automatically
- **Configurable display options**:
  - Top 5, 10, 25, 50, or All winners
  - Top 5, 10, 25, 50, or All sole survivors
- **Manual posting** - Trigger leaderboard posts on-demand
- **Rich formatting** - Emoji and structured Discord messages

#### Platform Support
- **Windows (Full Support)**:
  - Graphical Setup UI
  - All features available
  - Windows Forms-based configuration
- **Linux (Full Support)**:
  - Manual configuration via global variables
  - All game features functional
  - Command-line backup/restore tools documented

#### User Experience
- **Unicode answer symbols** - Uses 🇦 🇧 🇨 regional indicators (no emoji conflicts)
- **Clear chat announcements** - Game state, eliminations, survivors
- **Real-time player counts** - Shows remaining players after each question
- **Join period management** - Flexible 30-60 second join window
- **Player lock** - No late joins after game starts

#### Data Management
- **CSV validation** - Comprehensive error checking and reporting
- **Backup system** - Export leaderboards to timestamped JSON files
- **Restore system** - Import leaderboards from backup files
- **Reset functionality** - Clear leaderboards for new seasons/events

#### Documentation
- **Comprehensive README** - Feature overview, quick start, tips
- **Setup Guide** - Detailed installation for Windows and Linux
- **Configuration Guide** - All settings explained with examples
- **Commands Reference** - Complete command documentation with examples
- **Sample questions CSV** - 30 example questions included

#### Commands
- `!trivia` - Players join the game
- `!trivia-setup` - Open configuration UI (Windows only, Mod/Broadcaster)
- `!trivia-init` - Start join period (Mod/Broadcaster)
- `!trivia-start` - Lock players and begin game (Mod/Broadcaster)
- `!trivia-ask` - Manually advance to next question (Mod/Broadcaster)
- `!trivia-end` - Force end game early (Mod/Broadcaster)

### Technical Details

- **Streamer.bot v0.2.0+ compatible** (also v1.0.4+)
- **C# Execute Code** actions for game logic
- **Global variable** system for configuration and state
- **Dictionary-based** player tracking
- **File I/O** for CSV questions and JSON leaderboards
- **Twitch chat** integration for all interactions

### Files Included

- `Trivia-Survival-v1.0.sb` - Main import file (10 actions, 6 commands)
- `README.md` - Primary documentation
- `LICENSE` - MIT License
- `docs/SETUP.md` - Installation guide
- `docs/CONFIGURATION.md` - Configuration reference
- `docs/COMMANDS.md` - Command documentation
- `examples/sample-questions.csv` - 30 sample questions

---
## [Unreleased]

### Planned Features

Ideas and features being considered for future releases:

- Question randomization toggle (enable/disable)
- True/False question format support
- Fill-in-the-blank questions
- Configurable join timer (instead of manual start)
- Per-game statistics and analytics
- Season/tournament mode with brackets
- Hint system for difficult questions
- Streak tracking (consecutive correct answers)
- Achievement system
- OBS overlay integration (optional visual layer)
- Multi-language support
- Question categories/tags
- Difficulty ratings per question
- Timed speed bonuses
- Question of the Day feature

### Under Consideration

- macOS support for Setup UI
- Question editor GUI
- Cloud question bank sharing
- API for external integrations
- Mobile companion app
- Voice command support
- Integration with channel points

---

## Version History Summary

| Version | Release Date | Highlights |
|---------|--------------|------------|
| 1.0.1 | 2026-02-12 | Performance optimization - Collect Answers action lifecycle management |
| 1.0.0 | 2026-02-12 | Initial release - Full game system with dual leaderboards, Discord integration, cross-platform support |

---

## Migration Guide

### Migrating from Pre-Release/Beta

If you were using any pre-release or development versions:

1. **Backup your current leaderboards**
   - Export via Setup UI (Windows) or copy JSON files (Linux)

2. **Remove old actions**
   - Delete all previous Trivia actions from Streamer.bot

3. **Import v1.0**
   - Import `Trivia-Survival-v1.0.sb`

4. **Reconfigure settings**
   - Use `!trivia-setup` (Windows) or manually set global variables (Linux)

5. **Restore leaderboards (optional)**
   - Import backup via Setup UI or copy JSON files back

---

## Known Issues

### v1.0.0

**Minor Issues:**
- Setup UI (Windows Forms) not available on Linux/macOS - Use manual configuration as documented

**Limitations:**
- Maximum 30 questions per game (configurable limit)
- CSV file must be named exactly `questions.csv`
- All questions must be multiple choice (A/B/C format)
- No question randomization in v1.0 (loads in file order)

**Workarounds Documented:**
- See [Troubleshooting](docs/SETUP.md#troubleshooting) section in Setup Guide

---

## Support & Contributions

### Reporting Issues

Found a bug? Please report it:

1. Check [existing issues](https://github.com/1moreastronaut/trivia-survival-game/issues)
2. Open a new issue with:
   - Streamer.bot version
   - Platform (Windows/Linux)
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant log messages

### Suggesting Features

Have an idea? We'd love to hear it:

1. Check [existing issues](https://github.com/1moreastronaut/trivia-survival-game/issues) for duplicates
2. Open a feature request with:
   - Clear description of the feature
   - Use case / why it's valuable
   - Any relevant examples or mockups

### Contributing

Contributions welcome! See the main [README](README.md#contributing) for guidelines.

---

## Links & Resources

- **Repository:** [github.com/1moreastronaut/trivia-survival-game](https://github.com/1moreastronaut/trivia-survival-game)
- **Issues:** [github.com/1moreastronaut/trivia-survival-game/issues](https://github.com/1moreastronaut/trivia-survival-game/issues)
- **Releases:** [github.com/1moreastronaut/trivia-survival-game/releases](https://github.com/1moreastronaut/trivia-survival-game/releases)
- **Streamer.bot:** [streamer.bot](https://streamer.bot/)
- **Creator:** [@1moreastronaut](https://github.com/1moreastronaut)

---

## Acknowledgments

- **Streamer.bot** by Nate (nate1280) - Amazing platform that makes this possible
- **Community testers** - Thank you for feedback and bug reports
- **Early adopters** - Appreciate your patience and support

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**[⬆ back to top](#changelog)**

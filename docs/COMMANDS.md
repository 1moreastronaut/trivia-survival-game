## Troubleshooting Commands

Common command issues and solutions.

### !trivia Not Responding

**Symptom:** Players type `!trivia` but nothing happens

**Possible Causes:**

1. **Game not initialized**
   - Solution: Mod must run `!trivia-init` first

2. **Command disabled in Streamer.bot**
   - Solution: Check Commands tab → Ensure `!trivia` is enabled

3. **Game already started**
   - Solution: Wait for next game - late joins not allowed

4. **Typo in command**
   - Solution: Type exactly `!trivia` (no spaces, correct spelling)

**Verification Steps:**

1. Check Streamer.bot logs for errors
2. Verify "Trivia 02) Join" action exists and is enabled
3. Check game state (should be "pregame")
4. Test with mod account

---

### !trivia-init Fails

**Symptom:** Command doesn't start join period

**Possible Causes:**

1. **CSV file not found**
   - Error: "CSV file not found at path..."
   - Solution: Check `TriviaDataFolder` path is correct

2. **CSV validation fails**
   - Error: "Invalid CSV format at row X"
   - Solution: Run `!trivia-setup` → Validate CSV for details (Windows)
   - Solution: Check Streamer.bot logs for specific error (Linux)

3. **Game already active**
   - Error: "A game is already in progress!"
   - Solution: Use `!trivia-end` first, then retry

4. **Missing global variables**
   - Error: "Configuration incomplete"
   - Solution: Verify all required global variables exist

**Debug Steps:**

Check Streamer.bot logs for:
- `[Trivia] Loaded X questions from CSV` (success)
- `[Trivia] Error loading CSV: ...` (failure)

Verify variables exist:
- Settings → Variables → Global
- Look for all `Trivia...` variables

Test CSV file:
- Windows: Open in Excel/Notepad
- Linux: `cat ~/trivia/questions.csv`

---

### !trivia-start Shows "No Players"

**Symptom:** Game won't start, says no players joined

**Possible Causes:**

1. **Players actually didn't join**
   - Solution: Have players use `!trivia` during join period

2. **Join command failed silently**
   - Solution: Check Streamer.bot logs for join errors
   - Solution: Verify "Trivia 02) Join" action is working

3. **Player dictionary corrupted**
   - Solution: Run `!trivia-end` then `!trivia-init` to reset

**Verification:**

Check global variables:
- Settings → Variables → Global
- Look for `TriviaPlayers` variable
- Should show dictionary with player names

---

### !trivia-setup Window Doesn't Open

**Symptom:** Command in chat but no window appears

**Possible Causes:**

1. **Linux/Mac platform (Windows Forms not available)**
   - Solution: Configure manually via global variables

2. **Window opened off-screen**
   - Solution: Check taskbar for window
   - Solution: Alt+Tab to find window

3. **Action disabled**
   - Solution: Check "Trivia 00) Setup UI" is enabled

4. **Missing .NET Framework**
   - Solution: Install .NET Framework 4.7.2 or higher

**Workaround:**

Configure manually:
- Settings → Variables → Global
- Create/edit all `Trivia...` variables manually
- See [Configuration Guide](CONFIGURATION.md) for values

---

### !trivia-ask Doesn't Advance

**Symptom:** Command used but next question doesn't appear

**Possible Causes:**

1. **Game not in live state**
   - Solution: Can only use during active game

2. **Game already ended**
   - Solution: Start new game with `!trivia-init`

3. **All questions completed**
   - Solution: Game naturally ended - start new game

**Debug:**

Check current question count:
- Settings → Variables → Global
- Look for `TriviaCurrentQuestion`
- Compare to `TriviaQuestionCount`

---

### Game Stuck After !trivia-start

**Symptom:** Game starts but first question never appears

**Solution 1: Manual Advance**

!trivia-ask

(Forces first question to appear)

**Solution 2: Restart**

!trivia-end
!trivia-init
(Players rejoin)
!trivia-start

**Solution 3: Check Logs**

Look for errors in Streamer.bot logs:
- Action execution failures
- Delay/timer errors
- Chat message send failures

---

### Leaderboards Not Updating

**Symptom:** Game completes but leaderboards stay the same

**Possible Causes:**

1. **All players eliminated (no winners)**
   - Expected: Leaderboards only update if someone survives

2. **Variables not persisted**
   - Solution: Check `TriviaSharedWins` and `TriviaChampionWins` have "Persisted" checked

3. **File write permissions (Linux)**
   - Solution: `chmod 755 ~/trivia`

4. **Action failed silently**
   - Solution: Check Streamer.bot logs during "Trivia 08) End Game"

**Verification:**

Check variables after game:
- Settings → Variables → Global
- `TriviaSharedWins` should show player names with counts
- `TriviaChampionWins` should show sole survivors with counts

---

### Discord Not Posting

**Symptom:** Leaderboards don't appear in Discord

**Checklist:**

✅ **Webhook URL configured**
- Settings → Variables → Global → `TriviaDiscordWebhook`
- Should start with `https://discord.com/api/webhooks/`

✅ **Subaction enabled**
- Actions → "Trivia 08) End Game" → Edit
- Find "Run Action: Trivia 09) Discord Leaderboards"
- Must be **enabled** (not disabled)

✅ **Webhook still valid**
- Check Discord → Server Settings → Integrations → Webhooks
- Verify webhook exists and is active

✅ **Display options set**
- `TriviaDiscordWinnersDisplay` = `Top 5`, `Top 10`, `Top 25`, `Top 50`, or `All`
- `TriviaDiscordChampsDisplay` = same options
- Case-sensitive, must match exactly

**Test Manually:**

Actions → "Trivia 09) Discord Leaderboards" → Test

Check logs for:
- `[Discord] Posted leaderboards successfully` (success)
- `[Discord] Error: 403` (webhook invalid/deleted)
- `[Discord] Error: 404` (webhook URL wrong)

---

## Command Permissions

How to modify command permissions in Streamer.bot.

### Default Permissions

**!trivia:**
- Everyone (all viewers can join)

**!trivia-setup, !trivia-init, !trivia-start, !trivia-ask, !trivia-end:**
- Moderators and Broadcaster only

### Changing Permissions

**To make !trivia mod-only (subscriber games, etc.):**

1. Open Streamer.bot
2. Go to **Commands** tab
3. Find `!trivia` command
4. Double-click to edit
5. Change **Permissions** from "Everyone" to "Moderators"
6. Save

**To allow VIPs to control games:**

1. Commands tab → Find mod commands
2. Edit each command
3. Add **VIP** to permissions
4. Save

**To create broadcaster-only commands:**

1. Commands tab → Find command
2. Edit
3. Set permissions to "Broadcaster"
4. Save

---

## Command Aliases

Create alternate command names for convenience.

### Example: Short Commands

Create these additional commands with same actions:

| Original | Alias | Action |
|----------|-------|--------|
| `!trivia-init` | `!tstart` | Trivia 01) Initialize |
| `!trivia-start` | `!tgo` | Trivia 03) Start Game |
| `!trivia-end` | `!tstop` | (Manual end logic) |

**To create alias:**

1. Commands tab → Add
2. Command: `!tstart`
3. Action: "Trivia 01) Initialize"
4. Permissions: Moderators
5. Save

---

## Quick Reference Card

Print or save this for quick access during streams:

**PRE-GAME:**
- `!trivia-init` → Start join period
- Wait 30-60 seconds
- `!trivia-start` → Begin game

**DURING GAME:**
- Let it run automatically
- `!trivia-ask` → Skip to next question (if stuck)
- `!trivia-end` → Abort game

**BETWEEN GAMES:**
- `!trivia-init` → Start next game

**MAINTENANCE:**
- `!trivia-setup` → Configure settings (Windows)
- Edit CSV → Update questions
- `!trivia-init` → Reload questions

**PLAYERS:**
- `!trivia` → Join during join period
- Type A, B, or C → Answer questions

---

## Command Cooldowns

Preventing command spam.

### Recommended Cooldowns

**!trivia:**
- User cooldown: 5 seconds (prevents rejoin spam)
- Global cooldown: 0 seconds (allow simultaneous joins)

**!trivia-init:**
- User cooldown: 10 seconds
- Global cooldown: 10 seconds (prevent double-initialization)

**!trivia-start:**
- User cooldown: 10 seconds
- Global cooldown: 10 seconds (prevent double-start)

**!trivia-ask:**
- User cooldown: 3 seconds
- Global cooldown: 3 seconds (prevent rapid skipping)

**!trivia-end:**
- User cooldown: 5 seconds
- Global cooldown: 5 seconds

**To set cooldowns:**

1. Commands tab → Edit command
2. Set "User Cooldown" and "Global Cooldown"
3. Save

---

## Additional Resources

- **[Setup Guide](SETUP.md)** - Installation and configuration
- **[Configuration Guide](CONFIGURATION.md)** - Detailed settings reference
- **[Main README](../README.md)** - Feature overview and tips

---

## Getting Help

Command not working as expected?

1. **Check Streamer.bot logs** - Bottom panel shows detailed errors
2. **Verify command enabled** - Commands tab → Find command → Check enabled
3. **Check permissions** - Ensure user has permission to run command
4. **Review this guide** - Most issues covered above
5. **Test with mod account** - Rule out permission issues
6. **Open GitHub issue** - Include logs and steps to reproduce

---

**Commands mastered!** You're ready to run smooth trivia games on your stream. 🎮

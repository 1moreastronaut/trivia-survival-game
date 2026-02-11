# 📖 Commands Reference

Complete guide to all commands available in the Trivia Survival Game.

---

## Command Overview

| Command | Who Can Use | When Available | Description |
|---------|-------------|----------------|-------------|
| `!trivia` | Everyone | Pre-game only | Join the game |
| `!leave` | Players | Pre-game & Live | Leave the game voluntarily |
| `!remove @username` | Mods/Broadcaster | Anytime during game | Remove a player from the game |
| `!revive @username` | Mods/Broadcaster | Live game & Ended | Revive an eliminated player |

---

## Player Commands

### `!trivia` - Join the Game

**Who can use:** Everyone

**When to use:** During the pre-game joining phase only

**What it does:**
- Adds you to the current game
- You'll be announced in chat as a participant
- You cannot join once the game has started

**Examples:**
```
Viewer: !trivia
Bot: @Viewer has joined the game! (5 players joined)
```

**Error cases:**
```
Viewer: !trivia
Bot: @Viewer - You've already joined the game!
```

```
Viewer: !trivia
Bot: @Viewer - Sorry, the game has already started. No late joins allowed!
```

**Tips:**
- Join early! Once the game starts, you can't join
- You only need to type it once - multiple messages won't help
- Watch chat for the game announcement

---

### `!leave` - Leave the Game

**Who can use:** Players who have joined

**When to use:** 
- During pre-game (before game starts)
- During live game (while playing)

**What it does:**
- Removes you from the game
- Frees up your spot for others (pre-game)
- Counts as elimination (during live game)

**Examples:**
```
Viewer: !leave
Bot: @Viewer has left the game. (4 players remaining)
```

**Error cases:**
```
Viewer: !leave
Bot: @Viewer - You're not in the current game.
```

**Tips:**
- Use this if you need to step away
- Leaving during a live game counts as elimination
- You cannot rejoin the same game after leaving

---

## Moderator Commands

These commands are restricted to **Moderators** and the **Broadcaster** only.

### `!remove @username` - Remove Player

**Who can use:** Moderators, Broadcaster

**When to use:** Anytime during an active game (pre-game or live)

**What it does:**
- Forcibly removes a player from the game
- Useful for rule violations or trolling
- Player is immediately eliminated

**Syntax:**
```
!remove @username
!remove username
```

**Examples:**
```
Moderator: !remove @TrollUser
Bot: @TrollUser has been removed from the game by @Moderator
```

```
Moderator: !remove @Viewer
Bot: @Viewer has been removed from the game. (7 players remaining)
```

**Error cases:**
```
Moderator: !remove @NotInGame
Bot: @NotInGame is not in the current game.
```

**Use cases:**
- Player is being disruptive
- Player is using multiple accounts
- Player asks to be removed but can't type !leave

**Tips:**
- This is immediate and cannot be undone (use `!revive` to undo)
- Player can still chat, just can't participate
- Use sparingly to maintain fairness

---

### `!revive @username` - Revive Eliminated Player

**Who can use:** Moderators, Broadcaster

**When to use:**
- During live game (to revive eliminated players)
- After game ends (before next game starts)

**What it does:**
- Brings an eliminated player back into the game
- Resets their status to "alive"
- Useful for fixing unfair eliminations or technical issues

**Syntax:**
```
!revive @username
!revive username
```

**Examples:**
```
Moderator: !revive @Viewer
Bot: @Viewer has been revived by @Moderator! Welcome back to the game!
```

```
Broadcaster: !revive @UnfairElimination
Bot: @UnfairElimination is back in the game! (8 players alive)
```

**Error cases:**
```
Moderator: !revive @StillAlive
Bot: @StillAlive is still in the game and doesn't need revival.
```

```
Moderator: !revive @NeverJoined
Bot: @NeverJoined was never in this game.
```

```
Regular Viewer: !revive @Friend
Bot: (No response - command ignored for non-mods)
```

**Use cases:**
- Player answered correctly but was marked wrong (bug/glitch)
- Chat lag caused a late answer submission
- Accidental `!remove` by another mod
- Technical issue prevented player from answering
- Broadcaster discretion for fairness

**Important Notes:**
- ⚠️ **Use responsibly** - this can impact game fairness
- Players will know someone was revived (announced in chat)
- Cannot revive players who left voluntarily with `!leave`
- Only works during game states: `live` and `ended`

**Tips:**
- Explain why you're reviving someone to maintain transparency
- Consider game balance - reviving too many reduces stakes
- Use for technical issues, not for "gimmes"

---

## Broadcaster-Only Actions

These are manual action triggers in Streamer.bot (not chat commands):

### Initialize Game

**Action name:** `Trivia - Initialize`

**How to trigger:**
- Right-click the action → **Test**
- Stream Deck button (if configured)
- Custom hotkey (if configured)

**What it does:**
- Loads questions from CSV file
- Sets game state to "pre-game"
- Announces joining phase in chat
- Validates CSV and logs any errors

**When to use:** Before each game, when you're ready for people to join

**Chat output:**
```
Bot: 🎮 Trivia Survival is starting! Type !trivia to join the game!
```

---

### Start Game

**Action name:** `Trivia - Start Game`

**How to trigger:**
- Right-click the action → **Test**
- Stream Deck button (if configured)
- Custom hotkey (if configured)

**What it does:**
- Locks in all joined players
- Changes game state to "starting"
- Begins asking questions automatically

**When to use:** When you're ready to start (after enough players have joined)

**Chat output:**
```
Bot: Game is starting with 12 players! No more joins allowed. Good luck!
```

**Automatic sequence:**
1. Game starts
2. First question appears (5 second timer starts)
3. Answer options appear + answer window opens
4. 15 seconds for players to answer
5. Question ends, eliminations announced
6. Repeat for all questions
7. Winners announced

---

## Game Flow & Commands

Here's how commands are used during a typical game:

### Phase 1: Pre-Game (Joining)
```
Broadcaster: (Triggers "Initialize" action)
Bot: 🎮 Trivia Survival is starting! Type !trivia to join!

Viewer1: !trivia
Bot: @Viewer1 has joined! (1 player)

Viewer2: !trivia
Bot: @Viewer2 has joined! (2 players)

Viewer3: !trivia
Viewer3: !leave (oops, gotta go)
Bot: @Viewer3 has left the game. (1 player remaining)

Moderator: !remove @TrollAccount
Bot: @TrollAccount has been removed from the game.
```

### Phase 2: Starting
```
Broadcaster: (Triggers "Start Game" action)
Bot: Game starting with 10 players! No more joins!

Viewer4: !trivia
Bot: (No response - game already started)
```

### Phase 3: Live Game
```
Bot: Question 1: What is the capital of France?
(5 seconds pass)
Bot: Answer window open! A) London  B) Paris  C) Berlin

Viewer1: B
Viewer2: A
Viewer3: C

(15 seconds pass)
Bot: Correct answer: B) Paris
Bot: Eliminated: @Viewer2, @Viewer3 (8 players remain)

Moderator: !revive @Viewer2
Bot: @Viewer2 has been revived! (9 players alive)

(Game continues...)
```

### Phase 4: End Game
```
Bot: 🏆 GAME OVER! 🏆
Bot: Sole Survivor: @Viewer1 👑
Bot: Congratulations! You answered all 10 questions correctly!

Bot: 📊 LEADERBOARDS 📊
Bot: Top Winners: @Viewer1 (5), @Viewer5 (3), @Viewer6 (2)...
Bot: Top Sole Survivors: @Viewer1 (2), @OtherPlayer (1)...
```

---

## Command Permissions Setup

To restrict commands to mods/broadcaster in Streamer.bot:

### For `!remove` and `!revive`:

1. Go to **Commands** tab
2. Find/create the command
3. In command settings, under **Allowed:**
   - ✅ Check **Broadcaster**
   - ✅ Check **Moderators**
   - ❌ Uncheck everything else (VIPs, Subscribers, Everyone)
4. Click **OK**

### For `!trivia` and `!leave`:

1. Under **Allowed:**
   - ✅ Check **Everyone**
   - (Also check Broadcaster, Mods, VIPs, Subs automatically)

---

## Advanced: Custom Messages

You can customize the bot's responses by editing the C# code in each action:

1. Open the action in Streamer.bot
2. Find the C# Execute Code subaction
3. Look for lines like:
   ```csharp
   CPH.SendMessage($"{user} has joined the game!");
   ```
4. Change the message text
5. Save the action

**Example customization:**
```csharp
// Before:
CPH.SendMessage($"{user} has joined the game!");

// After (with emotes):
CPH.SendMessage($"PogChamp {user} is ready to play! glhfGG");
```

---

## FAQ

**Q: Can I change the join command from `!trivia` to something else?**
A: Yes! Just edit the command in Streamer.bot's Commands tab and change the trigger.

**Q: Can players answer multiple times?**
A: No - once you submit an answer (A, B, or C), it's locked in. No changes allowed.

**Q: What happens if I don't answer in time?**
A: You're eliminated. Silence = wrong answer.

**Q: Can I revive someone who used !leave?**
A: Technically yes, but it's not recommended. The !leave command is voluntary.

**Q: How do leaderboards work?**
A: Win counts are stored per-user automatically. Sole Survivor wins also count toward total wins.

**Q: Can I reset leaderboards?**
A: Yes - see the [Configuration Guide](CONFIGURATION.md) under "Leaderboard Storage"

---

## Troubleshooting

### Commands Not Working

**`!trivia` not responding:**
- ✅ Check that game is initialized (pre-game state)
- ✅ Verify command is enabled in Streamer.bot
- ✅ Check that trigger is set up correctly

**`!remove` / `!revive` not working:**
- ✅ Verify you're a mod or broadcaster
- ✅ Check command permissions in Streamer.bot
- ✅ Make sure you're @mentioning the username

**Player says they answered but weren't counted:**
- Chat lag might have delayed their message
- They might have answered outside the window
- Use `!revive` if it's unfair

---

## Quick Reference Card

Print this for your mods:

```
=== TRIVIA SURVIVAL COMMANDS ===

PLAYERS:
!trivia          - Join game (pre-game only)
!leave           - Leave game

MODS/BROADCASTER:
!remove @user    - Remove player
!revive @user    - Revive eliminated player

BROADCASTER ACTIONS (Streamer.bot):
- Initialize     - Start joining phase
- Start Game     - Begin questions
```

---

**Previous:** [← Configuration Guide](CONFIGURATION.md)

**Next:** Ready to create your questions? Check out the example CSV in `/examples/`!

---

**Need help?** Open an [issue on GitHub](https://github.com/1moreastronaut/trivia-survival-game/issues) or ask [@1moreastronaut](https://twitch.tv/1moreastronaut)!

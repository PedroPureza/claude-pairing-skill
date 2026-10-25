# pairing — a Claude Code skill

You type. Claude navigates.

`pairing` turns Claude Code into a pair-programming navigator for when you want to **learn by implementing** instead of watching an agent write the code. Claude gives one small, verifiable step per turn, waits for you to run it and paste the output, reads that output back, and only then moves on. It never edits your repository unless you hand it a step explicitly.

## Why

Agents are fast at writing code, and that's exactly the problem when the goal is learning: the answers are already on the screen before you've thought about the question. This skill keeps the red → green loop in your hands.

## What a turn looks like

Every turn has the same shape:

1. **Read-back.** What your last output says, and whether it's the expected red or green.
2. **Where.** One file and the spot in it.
3. **What.** The exact code, complete. No `...`.
4. **Why.** The one to three decisions you couldn't infer from reading the code.
5. **Run.** One command, the expected output, and what a wrong result would mean.
6. Stop and wait for your paste.

## Features

- **Keyboard rule.** Claude only reads files. You write and run everything, unless you say "you do this one".
- **Hint mode.** Say "let me try first" and Claude gives the goal and the run command but no code. It escalates hints when you fail, and reveals the code only when you ask or after two failed attempts.
- **Plan-aware.** If you already have an implementation plan, Claude follows its order and splits bundled tasks into one edit per turn.
- **Pressure-proof.** "We're short on time" makes steps smaller, never bigger.
- **Pause and resume.** "Let's stop here" ends at a green checkpoint with a Done / State / Next recap. Paste it into a new session to pick up where you left off.

## Install

### As a plugin (recommended)

```
/plugin marketplace add PedroPureza/claude-pairing-skill
/plugin install pairing@pedropureza-pairing
```

### As a personal skill

```bash
git clone https://github.com/PedroPureza/claude-pairing-skill.git
mkdir -p ~/.claude/skills
cp -r claude-pairing-skill/skills/pairing ~/.claude/skills/
```

## Usage

Start with `/pairing` (or `/pairing:pairing` when installed as a plugin), or just say something like:

- "Let's work on this together, I'll drive"
- "Tell me which file to go to and what to write"
- "I want to implement this myself, teach me as we go"

## License

MIT

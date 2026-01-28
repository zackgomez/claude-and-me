# claude-and-me

Tips and scripts for working with [Claude Code](https://claude.ai/code).

## Spawning Claude instances from Claude

If you run Claude Code in a terminal, you can have it spawn new Claude instances in separate windows. Handy for delegating subtasks or starting fresh contexts for different projects.

### The script

Save this as `~/.local/bin/newclaude` and `chmod +x` it:

```bash
#!/bin/bash
# Usage: newclaude [directory] [prompt]
#        newclaude [directory] -f <prompt-file>

dir="${1:-$HOME}"
shift 2>/dev/null

if [[ "$1" == "-f" && -n "$2" ]]; then
    prompt="$(cat "$2")"
elif [[ -n "$1" ]]; then
    prompt="$1"
else
    prompt=""
fi

if [[ -n "$prompt" ]]; then
    alacritty --working-directory "$dir" -e claude "$prompt" &
else
    alacritty --working-directory "$dir" -e claude &
fi
```

Swap `alacritty` for your terminal (e.g., `gnome-terminal --`, `kitty`, `wezterm start --`).

### Teaching Claude about it

Document it in your `~/CLAUDE.md` so Claude knows it exists:

```markdown
## Spawning New Claude Instances

Use `newclaude` to open a new terminal running Claude Code.

\`\`\`bash
newclaude                                    # home dir
newclaude ~/code/myproject                   # specific dir
newclaude ~/code/myproject "fix the tests"   # with prompt
newclaude ~/code/myproject -f task.txt       # prompt from file
\`\`\`
```

Now you can say "newclaude in the api folder and have it review auth" and Claude will do it.

## Push notifications via ntfy

[ntfy](https://ntfy.sh) lets Claude send push notifications to your phone. Useful for alerting you when long tasks complete, sharing links, or getting your attention.

### Setup

1. Install the ntfy app ([iOS](https://apps.apple.com/app/ntfy/id1625396347) / [Android](https://play.google.com/store/apps/details?id=io.heckel.ntfy))
2. Subscribe to a private topic (e.g., `yourname-claude-derpXXYY` - make it hard to guess)
3. Document it in your `~/CLAUDE.md`:

```markdown
### Push Notifications (ntfy)
Send notifications to my phone via ntfy.sh:
\`\`\`bash
# Simple message
curl -d "message" ntfy.sh/yourname-claude-derpXXYY

# With title and clickable URL
curl -d "message" -H "Title: Headline" -H "Click: https://url" ntfy.sh/yourname-claude-derpXXYY

# Urgent priority
curl -d "message" -H "Priority: 5" ntfy.sh/yourname-claude-derpXXYY
\`\`\`
Use for: task completion alerts, links to review, anything needing my attention.
```

### Usage examples

Ask Claude to:
- "Upload this to my server and send me the link"
- "Run the full test suite and ping me when it's done"
- "Notify me if the build fails"

### Rate limits

ntfy.sh allows 60 messages burst, refilling at 1 per 5 seconds. More than enough for normal use.


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

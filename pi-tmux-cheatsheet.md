# Pi + tmux cheatsheet

A quick reference for getting comfortable with this learning setup.

## 1. Interactive controls

### Pi controls

| Control | What it does |
|---|---|
| `Enter` | Send the current message to Pi. |
| `Shift+Enter` or `Ctrl+j` | Insert a new line without sending. |
| `Esc` | Interrupt/cancel the current generation or action. |
| `Ctrl+c` | Clear the editor; pressing again can exit/cancel depending on context. |
| `Ctrl+d` | Exit Pi when the editor is empty. |
| `Ctrl+o` | Expand/collapse tool output. Useful when a tool result is hidden. |
| `Ctrl+l` | Open model selector. |
| `Ctrl+p` | Cycle to next model / toggle path display in some selectors. |
| `Shift+Tab` | Cycle thinking level. |
| `Ctrl+t` | Collapse/expand thinking blocks. |
| `Ctrl+g` | Open current prompt in external editor. |
| `PageUp` / `PageDown` | Scroll transcript by page. |
| `Home` / `End` | Jump to top/bottom of transcript. |
| `Ctrl+f` on WSL | Search transcript. |

Inside Pi, run:

```text
/hotkeys
```

to see the active shortcuts for your current setup.

### tmux controls

`tmux` uses a prefix key first:

```text
Ctrl+b
```

Then press another key.

| Control | What it does |
|---|---|
| `Ctrl+b`, then `d` | Detach from tmux without stopping Pi. |
| `Ctrl+b`, then `x` | Kill/close the current pane, after confirmation. |
| `Ctrl+b`, then arrow key | Move between panes. |
| `Ctrl+b`, then `%` | Split pane vertically. |
| `Ctrl+b`, then `"` | Split pane horizontally. |
| `Ctrl+b`, then `z` | Zoom/unzoom current pane. |
| `Ctrl+b`, then `[` | Enter copy/scroll mode. Use arrows/PageUp/PageDown, then `q` to quit. |
| Mouse wheel, if mouse mode is enabled | Scroll tmux history / select panes. Enable with `set -g mouse on`. |
| `Ctrl+b`, then `c` | Create a new tmux window. |
| `Ctrl+b`, then `n` | Next tmux window. |
| `Ctrl+b`, then `p` | Previous tmux window. |
| `Ctrl+b`, then `,` | Rename current tmux window. |

### Common mental model

- **Pi session** = the actual AI conversation.
- **tmux session** = the terminal container keeping Pi alive.
- **pane** = a split terminal inside tmux.
- **subagent pane** = a background Pi process launched for a task.

If you detach from tmux, Pi keeps running. If you kill a pane, whatever was running inside that pane stops.

### Scrolling note

It is normal that the terminal scrollbar/mouse wheel may not scroll Pi history directly. Pi is a full-screen terminal UI running inside `tmux`; the normal terminal scrollback is not the source of truth. By default, tmux scrollback is entered with:

```text
Ctrl+b, then [
```

Then use `PageUp`, `PageDown`, arrows, or mouse wheel if mouse mode is enabled. Press `q` to leave scroll mode.

To make mouse scrolling work more naturally in tmux, add this to `~/.tmux.conf`:

```tmux
set -g mouse on
```

Then reload tmux config:

```bash
tmux source-file ~/.tmux.conf
```

---

## 2. Commands

### Start or reconnect to Pi

From the repo:

```bash
cd ~/learn
tmux new -A -s pi 'pi'
```

In this repo on Windows/WSL path, if needed:

```bash
cd "/mnt/c/Users/FabienSartre/OneDrive - digitalvalue/Documents/Administratif/Personnel/learn"
tmux new -A -s pi 'pi'
```

Detach without stopping Pi:

```text
Ctrl+b, then d
```

Reconnect later:

```bash
tmux attach -t pi
```

or:

```bash
tmux new -A -s pi 'pi'
```

### Useful Pi slash commands

Run these inside Pi, not in bash.

| Command | What it does |
|---|---|
| `/login` | Log in to a model provider, e.g. OpenAI, Anthropic, OpenRouter. |
| `/model` | Choose the active model. |
| `/thinking` | Choose thinking level. |
| `/hotkeys` | Show active keyboard shortcuts. |
| `/md-log path/to/file.md` | Start logging the lesson to a Markdown file. |
| `/md-unlog` | Stop Markdown logging. |
| `/new` | Start a new session. |
| `/resume` | Resume a previous session. |
| `/tree` | Open the session tree. |
| `/reload` | Reload config after editing Pi config files. |

Example for next finance lesson:

```text
/md-log finance/lesson-01-what-is-a-company-financially.md
```

Then say:

```text
Let's start finance lesson 1.
```

### Inspect tmux panes

List panes:

```bash
tmux list-panes -a
```

More detailed view:

```bash
tmux list-panes -a -F '#{pane_id} #{session_name}:#{window_index}.#{pane_index} #{pane_current_command}'
```

Close a specific pane:

```bash
tmux kill-pane -t %1
```

Replace `%1` with the pane id from `tmux list-panes`.

### Git save workflow

After a learning session:

```bash
git status
git add -A
git commit -m "lesson notes"
git push
```

Before starting from another PC:

```bash
git pull
```

### Check current Pi model from a shell tool

Inside a bash command run by Pi:

```bash
printf '%s/%s\n' "$PI_PROVIDER" "$PI_MODEL"
printf 'reasoning=%s session=%s\n' "$PI_REASONING_LEVEL" "$PI_SESSION_ID"
```

### API keys through environment variables

If not using `/login`, set keys before starting Pi:

```bash
export OPENAI_API_KEY="..."
export ANTHROPIC_API_KEY="..."
export OPENROUTER_API_KEY="..."
pi
```

Important: environment variables must exist before Pi starts. Subagents inherit from the Pi process that launched them.

### Subagent troubleshooting

If a subagent pane is stale or stuck:

1. List panes:

```bash
tmux list-panes -a -F '#{pane_id} #{pane_tty} #{pane_current_command}'
```

2. Capture the pane output:

```bash
tmux capture-pane -t %1 -p -S -120
```

3. Kill only that pane if safe:

```bash
tmux kill-pane -t %1
```

Do not kill the main Pi pane unless you want to stop the main session.

---
title: 'tmux'
date: '2022-08-02 23:24:00'
public: true
tags:
  - linux
  - terminal
  - tmux
---

# tmux

## Installing

**macOS**

```bash
brew install tmux
```

## Commands

### tmux ls

```
tmux ls
```

- returns list of active tmux sessions


## Default key bindings

### Default prefix key

```
ctrl(control) + b
```

### Panes

*default key bindings*

**Split the window horizontally** (Horizontal pane)

```
ctrl(control) + b + "
```

**Split the window vertically** (Vertical pane)

```
ctrl(control) + b + %
```

**Delete (kill) current (focused) pane**

(with confirmation prompt)

```
ctrl(control) + b + x
```

**Switch to the right pane**

```
ctrl(control) + b + (right arrow)
```

**Switch to the left pane**

```
ctrl(control) + b + (left arrow)
```

**Switch to the top pane**

```
ctrl(control) + b + (up arrow)
```

**Switch to the bottom pane**

```
ctrl(control) + b + (down arrow)
```

### Windows

**Create and switch to new window**

```
ctrl(control) + b + c
```

**Switch to specific existing window**

```
ctrl(control) + b + <window_number>
```

**Switch to the next window**

```
ctrl(control) + b + n
```

**Switch to the previous window**

```
ctrl(control) + b + p
```

**Delete (kill) the current window**

(with confirmation prompt)

```
ctrl(control) + b + &
```

**Rename the current window**

```
ctrl(control) + b + ,
```

## Sessions

**Create and attach to a new tmux session**

(outside of tmux)

```bash
tmux
```

**Create and attach to a new session with specific name**

```bash
tmux new -s "session_name"
```

**Disconnect the current tmux session**

```
ctrl(control) + b + d
```

**Back to last disconnected session**

```bash
tmux attach
```

**Show list of existing tmux sessions**

```bash
tmux list-sessions
```

or

```bash
tmux ls
```

**Attach to a specific tmux session**

```bash
tmux attach -t <session_number|session_name>
```

or

```bash
tmux a -t <session_number|session_name>
```

**Rename the current session**

```
ctrl(control) + b + $
```

**Show the list of tmux sessions, attach to or manage the session**

Show the list

```
ctrl(control) + b + s
```

- attach to selected session: Press the Enter
- delete the selected session (with prompt): Press x


## Configuring

**Create configuration file**

```bash
vim ~/.tmux.conf
```

**Reload tmux to apply changes in config file**

```bash
tmux source-file ~/.tmux.conf
```

> In some cases you need to kill tmux process to apply changes in config file. Notice it will kill all tmux running sessions.

```bash
tmux kill-server
```

## Links

- https://tmuxcheatsheet.com/



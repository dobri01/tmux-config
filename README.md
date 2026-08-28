# tmux config

Personal tmux configuration built around a `C-a` prefix, Vim-style pane and
copy-mode controls, cross-platform clipboard integration, persistent sessions,
and convenient handling of nested tmux instances.

In the tables below, `prefix` means press `Ctrl-a`, release it, and then press
the listed key. Keys are case-sensitive: `P` means `Shift-p`, while `M-p` means
`Alt-p` (or `Esc`, then `p` in terminals that do not pass Alt directly).

## Installation

This configuration needs tmux 3.1 or newer and Git. It also needs Ruby 2.3 or
newer for `tmux-jump`; Bash is used by several other plugins.

```sh
# Debian/Ubuntu
sudo apt-get install tmux git ruby-full
# Wayland (the Ubuntu desktop default)
sudo apt-get install wl-clipboard
# For an X11 session, install xsel instead
# sudo apt-get install xsel

# macOS (after installing Homebrew)
brew install tmux ruby
```

Clone this repository, link its config into the location tmux reads, and
install [TPM](https://github.com/tmux-plugins/tpm):

```sh
git clone https://github.com/dobri01/tmux-config.git ~/tmux-config
ln -sf ~/tmux-config/.tmux.conf ~/.tmux.conf
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Start tmux and press `prefix I` to install and load all plugins. For an already
running server, reload the config first:

```sh
tmux source-file ~/.tmux.conf
```

## Bindings

### Prefix, panes, and sessions

| Binding | Action |
| --- | --- |
| `prefix C-a` | Send a literal `C-a` to the program running in the pane |
| `prefix \|` or `prefix \` | Split to the right, preserving the current pane's directory |
| `prefix -` | Split below, preserving the current pane's directory |
| `prefix h/j/k/l` | Select the pane to the left/down/up/right |
| `prefix H/J/K/L` | Move the current pane to the left/bottom/top/right edge, spanning the full window dimension |
| `prefix z` | Power-zoom the pane into a temporary window; press again to restore it |
| `prefix s` | Start `tmux-jump`; enter the first character of a word and then its displayed hint |
| `prefix d` | Detach the current client |

`prefix z` comes from `tmux-power-zoom`, so it differs from tmux's normal pane
zoom: the pane is moved into a temporary window, where more panes can be
created, and later moved back to its original position.

### Copy mode and clipboard

Copy mode uses Vim keys. Enter it with the standard `prefix [` binding.

| Binding | Action |
| --- | --- |
| `v` | Begin a character-wise selection |
| `V` | Select the current line |
| `y` | Copy the selection to the system clipboard and leave copy mode |
| `Y` | Copy the selection, leave copy mode, and paste it at the command line |
| `Enter` | Copy the selection to the system clipboard and leave copy mode |
| Mouse drag | Copy the selection to the system clipboard, remain in copy mode, and keep the viewport in place |
| Mouse wheel | Scroll two lines per event instead of tmux's default five |
| `prefix y` | Copy the current shell command line to the system clipboard |
| `prefix Y` | Copy the current pane's working directory to the system clipboard |

The copy bindings automatically use `pbcopy` on macOS, `wl-copy` on Wayland,
or `xsel`/`xclip` on X11. `tmux-yank` supplies the `y`, `Y`, `prefix y`, and
`prefix Y` bindings and uses the same platform clipboard tools.

### Plugins and persistence

| Binding | Action |
| --- | --- |
| `prefix I` | Install missing plugins and refresh the tmux environment |
| `prefix U` | Update installed plugins |
| `prefix M-u` | Remove plugins no longer listed in `.tmux.conf` |
| `prefix C-s` | Manually save sessions, windows, panes, layouts, and working directories |
| `prefix C-r` | Restore the last saved tmux environment |
| `prefix P` | Toggle logging for the current pane |
| `prefix M-p` | Save the currently visible pane text to a file |
| `prefix M-P` | Save all available pane history to a file |
| `prefix M-c` | Clear the current pane's history |

Logging and captures are written to the home directory by default. Complete
history is limited to the 50,000 lines retained by this configuration.

`tmux-continuum` automatically saves the environment every 15 minutes. This
configuration does **not** enable Continuum's automatic restore option, so use
`prefix C-r` after starting a fresh tmux server when restoration is required.

### Nested tmux sessions

These bindings intentionally do **not** use the prefix, allowing them to pass
control between nested local or remote tmux instances:

| Binding | Action |
| --- | --- |
| `F1` | Disable the outermost active tmux instance and pass keys inward |
| `F2` | Re-enable the innermost inactive tmux instance |
| `F3` | Re-enable all tmux instances; also resets a broken nested state |

An inactive outer instance uses a dimmed status style to make the current level
visible.

## Configuration particularities

- Windows and panes are numbered from 1. Window indexes are renumbered after a
  window is removed.
- New splits inherit the active pane's working directory.
- Mouse selection and scrolling are enabled; copy mode uses Vim keys.
- The scrollback limit is 50,000 lines.
- `aggressive-resize` lets separate clients viewing the same session use their
  available terminal size more independently.
- Window names update automatically, focus events are forwarded, xterm-style
  modified arrow keys are enabled, and the escape delay is set to zero for
  responsive Vim mode switching.
- Visual and audible activity/bell handling is disabled.
- The bottom status bar shows the session, windows, date/time, CPU, RAM, and
  current tmux mode. It refreshes every five seconds.
- `$TERM` inside tmux is forced to `screen-256color`.

## Useful tmux commands

```sh
tmux attach-session -t <NAME>  # attach to a named session
tmux list-sessions             # list sessions
tmux source-file ~/.tmux.conf # reload this configuration
tmux kill-server               # stop the tmux server and all its sessions
```

For tmux's standard bindings and commands, see the
[tmux cheat sheet](https://tmuxcheatsheet.com/).

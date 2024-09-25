# Installation notes

## Prerequisites
```
sudo apt-get install tmux
sudo apt-get install python
sudo apt-get install ruby-full
```

## Using this config
```
git clone https://github.com/dobri01/tmux-config.git ~/example/tmux-config
ln -sf $(echo ~/example/tmux-config/.tmux.conf) $(echo ~/.tmux.conf)

git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Inside tmux:
```
prefix + I	- for reloading the tmux env
prefix + U	- for updating all the plugins
```

# Tmux commands
These are some commands that I often forget but are useful.

For more general info, check https://tmuxcheatsheet.com/

## Logging
- `prefix + shift + p` - toggle logging in the current pane
- `prefix + alt + p` - save visible text on the current pane
- `prefix + alt + shift + p` - retroactively save complete

## Basic
- `prefix + alt + c` - clear pane history
- `prefix + d` - detach from session
- `tmux attach-session -t <NAME>` - attach to a session
- `tmux kill-server` - kill tmux server

## Nested sessions
- `prefix + F1` - disable the outer-most active tmux instance
- `prefix + F2` - enable the inner-most inactive tmux instance
- `prefix + F3` - recursively enable all tmux instances

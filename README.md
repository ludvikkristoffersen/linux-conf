### Dependencies
For the .ZSHRC file to work without problems you need to install a few things.
```sh
# If you do not have ZSH installed
sudo apt install zsh
```
```sh
sudo apt install zsh-syntax-highlighting
```
```sh
sudo apt install zsh-autosuggestions
```
```sh
# We install this to get a more colorful "ls" command
sudo dpkg -i <latest release https://github.com/lsd-rs/lsd/releases>
```

### Font and colors
- **Font**: [JetBrains Mono](https://www.jetbrains.com/lp/mono/) or [JetBrains Mono Nerd Font (in order to have icons)](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/JetBrainsMono.zip)
- **Colors**: [Nightfly color scheme](https://github.com/bluz71/vim-nightfly-colors/tree/master?tab=readme-ov-file#terminal-colors)

### .ZSHRC file
```sh
# Autoloads and enables color support in zsh
autoload -U colors && colors

# Autoload vcs_info to have git branch in prompt
autoload -Uz vcs_info
precmd() {
        vcs_info
        echo # Adding space before prompt
}

# Setting the prompt including the git branch
GITBRANCH_ICON="-"
zstyle ':vcs_info:git:*' formats " on %{$fg[yellow]%}$GITBRANCH_ICON%b%{$reset_color%}(%u)"
setopt PROMPT_SUBST
PROMPT='┌─[$(date +%T)]─%B%F{green}%n%b%f in %B%F{blue}%~%b%f${vcs_info_msg_0_}
└> '

# Removing "/" from the list of word characters simplifies working with files and paths.
WORDCHARS=${WORDCHARS//\/}

# Key bindings for ease-of-use
bindkey -e
bindkey " " magic-space
bindkey "^U" backward-kill-line
bindkey "^[[3;5~" kill-word
bindkey "^[[3~" delete-char
bindkey "^[[1;5C" forward-word
bindkey "^[[1;5D" backward-word
bindkey "^[[5~" beginning-of-buffer-or-history
bindkey "^[[6~" end-of-buffer-or-history
bindkey "^[[H" beginning-of-line
bindkey "^[[F" end-of-line
bindkey "^[[Z" undo

# Zsh history file settings
HISTFILE=~/.zsh_history
HISTSIZE=1000
setopt hist_expire_dups_first
setopt hist_ignore_dups
setopt hist_ignore_space
setopt hist_verify

# Setting some zsh plugins
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
#source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Enables completion features in zsh
autoload -Uz compinit
compinit -d ~/.cache/zcompdump
zstyle ":completion:*:*:*:*:*" menu select
zstyle ":completion:*" auto-description "specify: %d"
zstyle ":completion:*" completer _expand _complete
zstyle ":completion:*" format "Completing %d"
zstyle ":completion:*" group-name ""
zstyle ":completion:*" list-colors ""
zstyle ":completion:*" list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
zstyle ":completion:*" matcher-list "m:{a-zA-Z}={A-Za-z}"
zstyle ":completion:*" rehash true
zstyle ":completion:*" select-prompt %SScrolling active: current selection at %p%s
zstyle ":completion:*" use-compctl false
zstyle ":completion:*" verbose true
zstyle ":completion:*:kill:*" command "ps -u $USER -o pid,%cpu,tty,cputime,cmd"

# Setting color output for certain commands
alias ls="lsd"
alias ll="lsd -alh"
alias grep="grep --color=auto"
alias dir="dir --color=auto"
alias ip="ip --color=auto"
alias cls="clear"

# Set directories where commands can be run from without specifying their full paths.
export PATH=$PATH/bin
export PATH=$HOME/.local/bin:$PATH
```

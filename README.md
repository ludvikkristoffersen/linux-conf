### Font and colors
- **Font**: [JetBrains Mono](https://www.jetbrains.com/lp/mono/)

### .ZSHRC file
```
# Autoloads and enables color support in zsh
autoload -U colors && colors

# Autoload vcs_info to have git branch in prompt
autoload -Uz vcs_info
precmd() {
        vcs_info
        echo # Adding space before prompt
}

# Setting the prompt including the git branch
zstyle ':vcs_info:git:*' formats " on %{$fg[yellow]%}%b%{$reset_color%}(%u)"
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

# Set directories where commands can be run from without specifying their full paths.
export PATH=$PATH/bin
export PATH=$HOME/.local/bin:$PATH
export PATH=$PATH:/snap/bin
```

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

# Functions
# Git help command to print out a more detailed git --help command
gitHelp() {
    echo "\e[1;36mGit Command Helper\e[0m\n"

    echo "\e[1;34mSetting Up Git in Your Directory:\e[0m"
    echo "  \e[0;32m- git config --global user.name \"Your Name\"\e[0m"
    echo "  \e[0;32m- git config --global user.email \"your.email@example.com\"\e[0m\n"

    echo "\e[1;34mCreating a New Git Repository:\e[0m"
    echo "  \e[0;32m- git init\e[0m\n"

    echo "\e[1;34mStaging and Committing Changes:\e[0m"
    echo "  \e[0;32m- git add <file.txt>          # Stage a specific file\e[0m"
    echo "  \e[0;32m- git add .                  # Stage all modified files\e[0m"
    echo "  \e[0;32m- git commit -m \"Message\"  # Commit with a message\e[0m"
    echo "  \e[0;32m- git commit                  # Commit with editor\e[0m\n"

    echo "\e[1;34mChecking Repository Status:\e[0m"
    echo "  \e[0;32m- git status                  # Show current repo status\e[0m\n"

    echo "\e[1;34mWorking with Branches:\e[0m"
    echo "  \e[0;32m- git branch                  # List branches\e[0m"
    echo "  \e[0;32m- git branch <branch-name>    # Create a new branch\e[0m"
    echo "  \e[0;32m- git checkout <branch-name>  # Switch to a branch\e[0m"
    echo "  \e[0;32m- git checkout -b <branch-name> # Create and switch to a new branch\e[0m\n"

    echo "\e[1;34mSyncing with Remote Repository:\e[0m"
    echo "  \e[0;32m- git remote add origin <url>  # Link local repo to remote\e[0m"
    echo "  \e[0;32m- git push origin <branch>    # Push commits to remote\e[0m"
    echo "  \e[0;32m- git pull origin <branch>    # Pull changes from remote\e[0m\n"

    echo "\e[1;34mViewing Commit History:\e[0m"
    echo "  \e[0;32m- git log                     # Show commit history\e[0m"
    echo "  \e[0;32m- git log --oneline           # Compact commit history\e[0m\n"

    echo "\e[1;34mTemporary Storage with Stash:\e[0m"
    echo "  \e[0;32m- git stash                   # Save uncommitted changes\e[0m"
    echo "  \e[0;32m- git stash pop               # Reapply last stashed changes\e[0m"
    echo "  \e[0;32m- git stash list              # List all stashes\e[0m\n"

    echo "\e[1;34mUndoing and Discarding Changes:\e[0m"
    echo "  \e[0;32m- git reset --soft <commit>   # Undo commits but keep changes staged\e[0m"
    echo "  \e[0;32m- git reset --hard <commit>   # Undo commits and discard changes\e[0m"
    echo "  \e[0;32m- git revert <commit>         # Revert a specific commit\e[0m"
    echo "  \e[0;32m- git checkout -- <file>      # Discard changes in a specific file\e[0m"
    echo "  \e[0;32m- git restore <file>          # Restore file to last committed state\e[0m"
    echo "  \e[0;32m- git restore --staged <file> # Unstage a file without discarding changes\e[0m\n"

    echo "\e[1;34mViewing Differences:\e[0m"
    echo "  \e[0;32m- git diff                    # Show changes not staged\e[0m"
    echo "  \e[0;32m- git diff --staged           # Show staged changes\e[0m\n"

    echo "\e[1;34mTagging Important Points:\e[0m"
    echo "  \e[0;32m- git tag <tag-name>          # Create a new tag\e[0m"
    echo "  \e[0;32m- git tag                     # List all tags\e[0m\n"

    echo "\e[1;36mHappy Coding!\e[0m 🎉"
}

# Setting color output for certain commands
alias ls="lsd"
alias ll="lsd -alh"
alias grep="grep --color=auto"
alias dir="dir --color=auto"
alias ip="ip --color=auto"
alias cls="clear"
alias gith="gitHelp"

# Set directories where commands can be run from without specifying their full paths.
export PATH=$PATH/bin
export PATH=$HOME/.local/bin:$PATH
```

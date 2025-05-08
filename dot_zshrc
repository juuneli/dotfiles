# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=1000
SAVEHIST=1000
unsetopt autocd
bindkey -e
# End of lines configured by zsh-newuser-install
# The following lines were added by compinstall
zstyle :compinstall filename '/home/wukong/.zshrc'

autoload -Uz compinit
compinit
# End of lines added by compinstall

#!/bin/zsh

# Nord Colors - using proper 256-color codes
nord_fg="%F{189}"        # Base text (light periwinkle)
nord_username="%F{75}"   # Username (medium blue)
nord_hostname="%F{110}"  # Hostname (steel blue)
nord_folder="%F{116}"    # Folder (aqua)
nord_git_branch="%F{147}" # Branch name (lavender blue)
nord_git_clean="%F{79}"  # Clean (seafoam green)
nord_git_dirty="%F{138}" # Dirty (dusty lavender)
nord_git_remote="%F{80}" # Remote status (teal)
nord_prompt="%F{153}"    # Prompt character (powder blue)
nord_reset="%f"          # Reset color

# Load color support
autoload -U colors && colors

# Git prompt function with branch + status
git_prompt_info() {
  if git rev-parse --is-inside-work-tree &>/dev/null; then
    local dirty=""
    local remote=""
    local branch=""
    local status_symbol=""
    
    # Branch name (or tag fallback)
    branch=$(git symbolic-ref --short HEAD 2>/dev/null || git describe --tags --exact-match 2>/dev/null || echo "detached")
    
    # Dirty status
    if [[ -n $(git status --porcelain 2>/dev/null) ]]; then
      status_symbol="${nord_git_dirty}✗${nord_reset}"
    else
      status_symbol="${nord_git_clean}✓${nord_reset}"
    fi
    
    # Remote tracking
    if git rev-parse --abbrev-ref @{u} &>/dev/null; then
      local ahead behind
      ahead=$(git rev-list --count @{u}..HEAD 2>/dev/null || echo 0)
      behind=$(git rev-list --count HEAD..@{u} 2>/dev/null || echo 0)
      
      if [[ $ahead -gt 0 && $behind -gt 0 ]]; then
        remote="${nord_git_remote}⇅${nord_reset}"
      elif [[ $ahead -gt 0 ]]; then
        remote="${nord_git_remote}↑${nord_reset}"
      elif [[ $behind -gt 0 ]]; then
        remote="${nord_git_remote}↓${nord_reset}"
      fi
    fi
    
    echo "${nord_git_branch}(${branch})${nord_reset} ${status_symbol}${remote}"
  fi
}

# Final prompt (folder only, not full path)
precmd() {
  local folder="%1~"  # current folder name
  local git_info=$(git_prompt_info)
  
  # Set prompt with optional git info
  PROMPT="${nord_username}%n${nord_reset}@${nord_hostname}%m ${nord_folder}${folder}${nord_reset}"
  
  if [[ -n "$git_info" ]]; then
    PROMPT+=" ${git_info}"
  fi
  
  # Add final prompt indicator
  PROMPT+=$'\n'  # Add newline for better readability
  PROMPT+="${nord_fg}❯${nord_reset} "
}

# Initialize prompt
setopt PROMPT_SUBST

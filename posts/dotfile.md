---
title: "Customise coding environment for better productivity"
date: 2022-06-27
layout: default
---

## There are lots of repetitive actions required while coding. 
## By minimising the number of chance working on those, we can reduce the amount of time we spend on trivial detour and therefore, less stress. 
## Here are some changes to share.

## VS Code extension
- [Open In GitHub](https://marketplace.visualstudio.com/items?itemName=sysoev.vscode-open-in-github): Open and view the current file on GitHub
  - This is useful when you are editing code locally on VS Code, and want to check the original file or different branch on remote repo.
  - There are several options what to open: File, Blame, History, or copy the URL for those files. 
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens): Especially gitblame
  - This package is really rich in great functions.
  - Especially the blame annotation at the end of the current line, and authorship to the top of the file / code blocks are useful. 

## Customise Zsh
Details in my [.zshrc](https://github.com/snkzt/dotfiles/blob/main/.zshrc) file
- [Oh My Zsh](https://ohmyz.sh/)
  - Oh My Zsh is an open-source framework for zsh with rich [themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes), [plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins), and helpers.
  - For example, you can update your shell prompt to show simple but informative current location or get many useful plugins such as web search from a terminal.
- Alias / Function
  - Set your own aliases / function for commands long and frequently used.
    - One character alias for frequently used command; in this case, ```fcd``` let you navigate to specific directory set for the alias.
      ```
      alias -g(global) fcd(alias name) = cd(original command) + directory(where you repeatedly navigate to)
      ```
    - Short function for frequently used command
      ```
      k_pods() {
        kubectl get pods
      }
      ```

## Extra: Useful VS Code shortcut key for mac
There are many tips on the internet about shortcut keys but here are some I often use.
- Reload: ```shift+cmd+p```
- Go to files: ```cmd+p```
- Find commands: ```cmd+shift+p```
- Pop up terminal panel: ```cmd+j```
- Set cursors
  - Above or below the current position: ```opt+cmd+↑/↓```
  - Below occurrences of the currenct selection: ```cmd+d```
  - All the occurrences of the current selection: ```shift+cmd+l```

### Simplify your input on screen, get easier accessibility between tools. Removing stress is the key to productive work environment. 



<[Home](https://snkzt.github.io/)>
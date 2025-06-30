---
title: Cable Channels
sidebar_position: 2
---

# Cable Channels

> _Tired of broadcast television? Want to watch your favorite shows on demand? television has you covered with cable channels. Cable channels are channels that are not built-in to television but are instead provided by the community._

Installing cable channels is as simple as creating provider files in your configuration folder.

A provider file is a `*channels.toml` file that contains cable channel prototypes defined as follows:

**my-custom-channels.toml**

```toml
[[cable_channel]]
name = "git-log"
source_command = 'git log --oneline --date=short --pretty="format:%h %s %an %cd" "$@"'
preview_command = 'git show -p --stat --pretty=fuller --color=always {0}'

[[cable_channel]]
name = "dotfiles"
source_command = 'fd -t f . $HOME/.config'
preview_command = ':files:'
```

This would add two new cable channels to `television`:

- a `git-log` channel, using the specified `source_command` to get its entries and using the `preview_command` to generate previews
- a `dotfiles` channel, using the specified `source_command` to get its entries and using tv's builtin `files` previewer

These cable channels are accessible:

- through the cli `tv my-dotfiles` or `tv git-log`
- using the remote control mode as shown below

<img width="1465" alt="Cable channels" src="https://github.com/user-attachments/assets/0f30b3c2-140d-4865-8d2e-0175d481073c"></img>

# 📘 Cable channel prototypes spec

```[[cable_channel]]
name = String
source_command = String
preview_command = Option<String>
preview_delimiter = Option<String>
```

> [!TIP] > **`preview_command` may be:**
>
> - a shell command that produces an output given the selected entry (or a segment of it)
> - a reference to a builtin previewer formatted as `:previewer:` where `previewer` is one of the following:
>   - `files` (syntax highlighted content)
>   - `env` (environment variable values)
>   - `basic` (the full entry string)

> [!TIP] > **Deciding which part of the source command output to pass to the previewer:**
>
> By default, each line of the source command can be passed to the previewer using `{}`.
>
> If you wish to pass only a part of the output to the previewer, you may do so by specifying the `preview_delimiter` to use as a separator and refering to the desired part using the corresponding index.
>
> **Example:**
>
> ```toml
> [[cable_channel]]
> name = "Disney channel"
> source_command = 'echo "one:two:three:four" && echo "five:six:seven:eight"'
> preview_command = 'echo {2}'
> preview_delimiter = ':'
> # which will pass "three" and "seven" to the preview command
> ```

# 📺 Cable Channels Guide

What follows is a list of cable channels you may pick and choose from for your own use.

> [!NOTE] > **This list is maintained by the community, so feel free to contribute your own ideas too! 😊**

## ⚙️​ Git

### 🍿 `logs`

```toml
[[cable_channel]]
name = "git-log"
source_command = 'git log --oneline --date=short --pretty="format:%h %s %an %cd" "$@"'
preview_command = 'git show -p --stat --pretty=fuller --color=always {0}'
```

<img width="1465" alt="Cable channels" src="https://github.com/user-attachments/assets/0f30b3c2-140d-4865-8d2e-0175d481073c"></img>

### 🍿 `branches`

```toml
[[cable_channel]]
name = "git-branch"
source_command = 'git --no-pager branch --all --format="%(refname:short)"'
preview_command = 'git show -p --stat --pretty=fuller --color=always {0}'
```

<img width="1465" alt="branches" src="https://github.com/user-attachments/assets/75519e04-11c8-46b8-9bc8-0e3c1e1c9784"></img>

### 🍿 `diff`

```toml
[[cable_channel]]
name = "git-diff"
source_command = "git diff --name-only"
preview_command = "git diff --color=always {0}"
```

### 🍿 `files`

```toml
[[cable_channel]]
name = "git-files"
source_command = 'git ls-files'
preview_command = 'bat -n --color=always {}'
```

### 🍿 `reflog`

```toml
[[cable_channel]]
name = "git-reflog"
source_command = 'git reflog'
preview_command = 'git show -p --stat --pretty=fuller --color=always {0}'
```

## ⚙️​ Shell history

### 🍿 `zsh`

```toml
[[cable_channel]]
name = "zsh-history"
source_command = "tail -r $HOME/.zsh_history | cut -d\";\" -f 2-"
```

### 🍿 `bash`

```toml
[[cable_channel]]
name = "bash-history"
source_command = "tail -r $HOME/.bash_history"
```

### 🍿 `fish`

```toml
[[cable_channel]]
name = "fish-history"
source_command = "fish -c 'history'"
```

## ⚙️​ files

### 🍿 `dotfiles`

```toml
[[cable_channel]]
name = "dotfiles"
source_command = "fd -t f . $HOME/.config"
preview_command = ":files:"
```

### 🍿 `hidden-files`

```toml
[[cable_channel]]
name = "files"
source_command = "fd -I -t f"
preview_command = ":files:"
```

## ⚙️​ Docker

### 🍿 `images`

```toml
[[cable_channel]]
name = "docker-images"
source_command = 'docker image list --format "{{.ID}} {{.Repository}}\t{{.Tag}}\t{{.Size}}"'
preview_command = 'docker image inspect {0} | jq -C'
```

## ⚙️​ S3

### 🍿 `buckets`

```toml
[[cable_channel]]
name = "s3-buckets"
source_command = 'aws s3 ls | cut -d " " -f 3'
preview_command = 'aws s3 ls s3://{0}'
```

## ⚙️​ Homebrew

### 🍿 `brew list`

```toml
[[cable_channel]]
name = "Brew list"
source_command = 'brew list --versions'
preview_command = 'brew info {0}'
```

<img width="2557" alt="Screenshot 2024-12-07 at 14 20 25" src="https://github.com/user-attachments/assets/2e55ddb9-c77e-4e1a-b322-03861497db9a"></img>

## ⚙️​ Tmux

### 🍿 `sessions`

```toml
[[cable_channel]]
name = "tmux list-sessions"
source_command = 'tmux list-sessions'
preview_command = 'tmux list-windows -t {0}'
```

To select a session to attach to `tmux attach -t $(tmux list-sessions | tv --preview 'tmux list-windows -t {0}' | cut -d':' -f1)`

## ⚙️ Nix

### 🍿 `nixpkgs`

See https://github.com/3timeslazy/nix-search-tv

```toml
[[cable_channel]]
name = "nixpkgs"
source_command = "nix-search-tv print"
preview_command = "nix-search-tv preview {}"
```

[![asciicast](https://asciinema.org/a/AUt4rfSukwSWsrlis7ZNsBP4N.svg)](https://asciinema.org/a/AUt4rfSukwSWsrlis7ZNsBP4N)

## ⚙️ Guix

### 🍿 `guix-packages`

```toml
[[cable_channel]]
name = "guix-packages"
source_command = "guix package --list-available=.* | cut -f 1 | tr -d [:blank:]"
preview_command = "guix package --show={0}"
```

![Screenshot displaying the search results for Emacs. The main Emacs package is selected, showing its description in the preview.](https://github.com/user-attachments/assets/1135dfab-ffbc-4003-8dec-45aef1c0d52e)

## ⚙️ Pacman

### 🍿 `pacman-pkgs`

```toml
[[cable_channel]]
name = "pacman-pkgs"
source_command = "pacman -Qe"
preview_command = "pacman -Qi {0}"
```

![tv-pacman](https://github.com/user-attachments/assets/2b5ea887-0189-48f3-88c2-59376fb5c966)

## ⚙️ Task Warrior

### 🍿 `task`

This will allow you to search tasks. Note that it doesn't handle tasks with quotes in them. And annotations are not quite right.

```toml
[[cable_channel]]
name = "task"
source_command = "task rc.defaultwidth:500 rc.defaultheight:1000"
preview_command = "task $(echo {} | awk '/([0-9])/{ print $1 }')"

[[cable_channel]]
name = "task_complete"
source_command = "task rc.defaultwidth:500 rc.defaultheight:1000 complete"
preview_command = "task $(echo {} | awk '/([0-9a-f])/{ print $2 }')"
```

These aliases will allow you to edit the resulting task:

```bash
alias tfz='B=$(tv task | awk '"'"'/([0-9])/{ print $1 }'"'"' ); task edit $B'
alias tfzc='B=$(tv task_complete | awk '"'"'/([1-9])/{ print $2 }'"'"' ); task edit $B'
```

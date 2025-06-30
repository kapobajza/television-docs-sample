---
title: Tips and Tricks
sidebar_position: 9
---

## Overriding builtin channels
You may override builtin channels by defining a cable channel with the same name.

### Showing hidden files on the `files` channel

This might be useful in cases where you'd like a slightly different behavior for some builtin channels, such as showing hidden files for the `files` channel, which would be achieved as follows:

```toml
[[cable_channel]]
name = "files"
source_command = "fd -I -t f"
preview_command = ":files:"
```
<img width="1010" alt="Screenshot 2025-03-23 at 13 39 26" src="https://github.com/user-attachments/assets/32d526d6-28e9-4080-91ed-dfe8ea7dc78c" />

### Searching for `git-repos` inside hidden directories

Depending on your needs, you might need to include hidden directories when crawling for git repositories on your filesystem.

This can be done by overwriting the builtin `git-repos` channel with the following cable channel:

```toml
[[cable_channel]]
name = "git-repos"
source_command = "fd '\\.git$' ~/ --type d --no-ignore --hidden --format '{//}'"
preview_command = "cd {} && git log -n 200 --pretty=medium --all --graph --color"
```

## Dynamically Update Theme (light/dark mode)

Television has both UI and syntax themes that need to be updated. One way to update those themes via scripting is to reference an "adaptive" theme in the television config and change the symlink to those theme files.

`~/.config/television/config.toml`

```toml
[ui]
theme = "adaptive"

[previewers.file]
theme = "adaptive"
```

Add a script that can change the adaptive themes to dark themes...

```sh
ln -s -f ~/.config/bat/themes/CatppuccinMocha.tmTheme ~/.config/bat/themes/adaptive.tmTheme;
ln -s -f ~/.config/television/themes/catppuccin-mocha.toml ~/.config/television/themes/adaptive.toml;
bat cache --build
```

...and the light would just do the opposite...

```sh
ln -s -f ~/.config/bat/themes/CatppuccinMocha.tmTheme ~/.config/bat/themes/adaptive.tmTheme;
ln -s -f ~/.config/television/themes/catppuccin-mocha.toml ~/.config/television/themes/adaptive.toml;
bat cache --build
```

In the video below I've got a script named `toggle_theme` that, among other things, updates these symlinks and changes my terminal's (Wezterm) theme. I typically kick this script off from within an editor so I don't see that logging.

https://github.com/user-attachments/assets/c884b329-05cb-4749-a264-65235fa7e0b9

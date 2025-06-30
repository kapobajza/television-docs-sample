---
title: Configuration file
sidebar_position: 3
---
**Default configuration: [config.toml](https://github.com/alexpasmantier/television/blob/main/.config/config.toml)**

Locations where `television` expects the configuration files to be located for each platform:

|Platform|Value|
|--------|:-----:|
|Linux|`$HOME/.config/television/config.toml`|
|macOS|`$HOME/.config/television/config.toml`|
|Windows|`%LocalAppData%\television\config`|

Or, if you'd rather use the XDG Base Directory Specification, tv will look for the configuration file in
`$XDG_CONFIG_HOME/television/config.toml` if the environment variable is set.
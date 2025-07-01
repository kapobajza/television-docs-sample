---
sidebar_position: 1
title: Installation
---

<img width="200" alt="tv-version" src="https://github.com/user-attachments/assets/b31e7495-89c8-4227-83bd-53919c5222e4" />

## Installing Television

Television is available on multiple platforms and can be installed in various ways. Below are the installation methods for different systems. Choose the one that suits you best.

import Tabs from '@site/src/components/InstallationTabs';
import TabItem from '@theme/TabItem';

<Tabs queryString="installation">
  <TabItem value="nix" label="nix" default>
    Television is [available on `nixpkgs`](https://github.com/NixOS/nixpkgs/blob/master/pkgs/by-name/te/television/package.nix)

    ```bash
    nix run nixpkgs#television
    ```
  </TabItem>
  <TabItem value="homebrew" label="Homebrew">
    ```bash
    brew install television
    ```
  </TabItem>
  <TabItem value="scoop" label="Scoop">
    ```bash
    scoop bucket add extras
    scoop install television
    ```
  </TabItem>
  <TabItem value="winget" label="WinGet">
    ```pwsh
    winget install --exact --id alexpasmantier.television
    ```
  </TabItem>
  <TabItem value="arch" label="Arch Linux">
    ```bash
    pacman -S television
    ```
  </TabItem>
  <TabItem value="chimera" label="Chimera Linux">
    ```bash
    apk add chimera-repo-user
    apk add television
    ```
  </TabItem>
  <TabItem value="debian" label="Debian-based">
    ```bash
    VER=`curl -s "https://api.github.com/repos/alexpasmantier/television/releases/latest" | grep '"tag_name":' | sed -E 's/.*"tag_name": "([^"]+)".*/\1/'`
    curl -LO https://github.com/alexpasmantier/television/releases/download/$VER/tv-$VER-x86_64-unknown-linux-musl.deb
    echo $VER
    sudo dpkg -i tv-$VER-x86_64-unknown-linux-musl.deb
    ```
  </TabItem>
  <TabItem value="conda" label="Conda-forge">
    ```bash
    pixi global install television
    ```
  </TabItem>
  <TabItem value="binary" label="Pre-compiled Binary">
    From the [latest release](https://github.com/alexpasmantier/television/releases/latest) page:
    - Download the latest release asset for your platform (e.g. `tv-vX.X.X-linux-x86_64.tar.gz` if you're on a linux x86 machine)
    - Unpack and copy to the relevant location on your system (e.g. `/usr/local/bin` on macos and linux for a global installation)
  </TabItem>
  <TabItem value="crates" label="Crates.io">
    Setup the latest stable Rust toolchain via rustup:
    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    rustup update
    ```
    Install `television`:
    ```bash
    cargo install --locked television
    ```
  </TabItem>
  <TabItem value="source" label="Building from source">
    If you want to benefit from the latest updates on main, clone the repo and build from source by running:
    ```bash
    git clone git@github.com:alexpasmantier/television.git && cd television
    just build release
    ```

    You can then alias `tv` to the produced binary:
    ```bash
    alias tv=$TELEVISION_DIR/target/release/tv
    ```
  </TabItem>
</Tabs>

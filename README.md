# Auto Synchronize forked repositories

This repository automates the synchronization of forked repositories for the GitHub namespace `Paebbels`. The
algorithm lives in the reusable action
[pyTooling/SynchronizeForks](https://github.com/pyTooling/SynchronizeForks); this repository contributes only the
[workflow](https://github.com/Paebbels/SynchronizeForks/blob/main/.github/workflows/Synchronize.yml) calling it and
the configuration files (`*.repos`) listing the forks.

This code is licensed under [MIT License](LICENSE.md).


## Example Pipeline Log

![GitHub Action log](Pipeline.png)

*(Screenshot of the previous, inline implementation. The action's log groups the repositories per organisation and
ends with a summary.)*

## Steps to Setup

1. Create a repository like `SynchronizeForks` in your namespace.
2. Add a workflow calling the action:
   ```yaml
   jobs:
     Synchronize:
       runs-on: ubuntu-latest
       steps:
         - name: ⏬ Checkout
           uses: actions/checkout@v6

         - name: 🔄 Synchronize Repositories
           uses: pyTooling/SynchronizeForks@v1
           with:
             github-token: ${{ secrets.GH_TOKEN }}
   ```
   `GH_TOKEN` is a repository secret holding a token with write access to the contents of every listed fork. A
   workflow's automatic `GITHUB_TOKEN` isn't sufficient. `@v1` is the action's major-version branch, moved to each
   release.
3. Add an `.ALL.repos` file and the matching `*.repos` files.
4. Push the commit and check the Action for success.


## Configuration File Formats

These are the files this repository owns; the action reads them. See the
[action's README](https://github.com/pyTooling/SynchronizeForks#configuration-file-formats) for the authoritative
description, including the optional tag patterns.

The `.ALL.repos` file is the entry point for the script listing all GitHub organisations that should be synchronized.
It lists one organisation per line or alternatively a comment starting with `#`. In case many single repositories from
various organisations or private accounts should be synchronized, it's recommended to add an `_Others` or `_Misc` entry.

**Example:**
```
ghdl
OSVVM
_Others
```

Each organisation has a matching `*.repos` file contains one repository per line. Alternatively, a comment starting
with `#`. A repository line has the following format:  
`<upstream>=<localRepository>:<branches>`

* `<upstream>` is formatted like `<organisation>/<repository>` or `<privateAccount>/<repository>`.
* `<localRepository>` is formatted like `<repository>`.  
  An account or organisation is not required, because it's inferred from the repository this script runs in.
* `<branches>` is a comma separated list of branch names like `<branch>,<branch>,<branch>`.
	
**Example:**
```
OSVVM/OSVVM=OSVVM:main,dev
OSVVM/AXI4=OSVVM-AXI4:main,dev
#OSVVM/AvalonST=OSVVM-AvalonST:main
```


## Synchronized repositories

* ghdl
  * `ghdl` ⇐ [ghdl/ghdl](https://github.com/ghdl/ghdl) — `master`
* OSVVM
  * `OSVVM-Libraries` ⇐ [OSVVM/OSVVMLibraries](https://github.com/OSVVM/OSVVMLibraries) — `main`, `dev`
  * `OSVVM-Scripts` ⇐ [OSVVM/OSVVM-Scripts](https://github.com/OSVVM/OSVVM-Scripts) — `main`, `dev`
  * `OSVVM` ⇐ [OSVVM/OSVVM](https://github.com/OSVVM/OSVVM) — `main`, `dev`
  * `OSVVM-Common` ⇐ [OSVVM/OSVVM-Common](https://github.com/OSVVM/OSVVM-Common) — `main`, `dev`
  * `OSVVM-AvalonMM` ⇐ [OSVVM/AvalonMM](https://github.com/OSVVM/AvalonMM) — `main` *(disabled)*
  * `OSVVM-AvalonST` ⇐ [OSVVM/AvalonST](https://github.com/OSVVM/AvalonST) — `main` *(disabled)*
  * `OSVVM-AXI4` ⇐ [OSVVM/AXI4](https://github.com/OSVVM/AXI4) — `main`, `dev`
  * `OSVVM-CoSim` ⇐ [OSVVM/CoSim](https://github.com/OSVVM/CoSim) — `main`, `dev`
  * `OSVVM-CoSimPCIe` ⇐ [OSVVM/CoSimPCIe](https://github.com/OSVVM/CoSimPCIe) — `main`, `dev`
  * `OSVVM-DPRAM` ⇐ [OSVVM/DpRam](https://github.com/OSVVM/DpRam) — `main`, `dev`
  * `OSVVM-Ethernet` ⇐ [OSVVM/Ethernet](https://github.com/OSVVM/Ethernet) — `main`
  * `OSVVM-UART` ⇐ [OSVVM/UART](https://github.com/OSVVM/UART) — `main`, `dev`
  * `OSVVM-Wishbone` ⇐ [OSVVM/Wishbone](https://github.com/OSVVM/Wishbone) — `main`
  * `OSVVM-Documentation` ⇐ [OSVVM/Documentation](https://github.com/OSVVM/Documentation) — `main`
  * `OSVVM-SPI` ⇐ [OSVVM/SPI_GuyEschemann](https://github.com/OSVVM/SPI_GuyEschemann) — `main`
  * `OSVVM-VideoBus` ⇐ [OSVVM/VideoBus_LouisAdriaens](https://github.com/OSVVM/VideoBus_LouisAdriaens) — `main`
* VHDL
  * `PoC` ⇐ [VHDL/PoC](https://github.com/VHDL/PoC) — `main`, `dev`, `master`
* *others*
  * `progit2` ⇐ [progit/progit2](https://github.com/progit/progit2) — `main`
  * `MINGW-packages` ⇐ [msys2/MINGW-packages](https://github.com/msys2/MINGW-packages) — `master`
  * `microwatt` ⇐ [antonblanchard/microwatt](https://github.com/antonblanchard/microwatt) — `master`
  * `itertree` ⇐ [BR1py/itertree](https://github.com/BR1py/itertree) — `main`


## GitHub CLI Documentation

See https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork


# License

This GitHub Action Example (source code) is licensed under [MIT License](LICENSE.md).

-------------------------
SPDX-License-Identifier: MIT

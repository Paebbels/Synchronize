# Auto Synchronize forked repositories

This repository automates the synchronization of forked repositories for the GitHub namespace `Paebbels`. The
algorithm lives in the reusable action
[pyTooling/SynchronizeForks](https://github.com/pyTooling/SynchronizeForks); this repository contributes only the
[workflow](https://github.com/Paebbels/SynchronizeForks/blob/main/.github/workflows/Synchronize.yml) calling it and
the configuration files (`*.repos`) listing the forks as well as the branches and tags to synchronize.

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
3. Write your own configuration: an `.ALL.repos` listing the upstream organisations you fork from, and one
   `<organisation>.repos` per entry naming **your** forks, their branches and optionally their tags. The files in this
   repository describe the `Paebbels` namespace and are an example, not a starting point — see
   [Configuration File Formats](#configuration-file-formats) for the syntax.
4. Add a `README.md` saying which forks the repository keeps in sync. Nothing requires it, but the repository is
   otherwise a directory of `*.repos` files with no explanation of what it is for. A `LICENSE.md` is no longer needed:
   the algorithm lives in the action, and what remains here is configuration.
5. Push the commit and check the Action for success.


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
`<upstream>=<localRepository>:<branches>[:<tagPatterns>]`

* `<upstream>` is formatted like `<organisation>/<repository>` or `<privateAccount>/<repository>`.
* `<localRepository>` is formatted like `<repository>`.  
  An account or organisation is not required, because it's inferred from the repository this script runs in.
* `<branches>` is a comma separated list of branch names like `<branch>,<branch>,<branch>`.
* `<tagPatterns>` is optional: a comma separated list of tag names or regular expressions.
	
**Example:**
```
OSVVM/OSVVM=OSVVM:main,dev
OSVVM/AXI4=OSVVM-AXI4:main,dev
#OSVVM/AvalonST=OSVVM-AvalonST:main
```


## Synchronized repositories

Which branches and tags of each fork are synchronized is in the `*.repos` files; repeating it here would only go out
of date.

* ghdl
  * `ghdl` ⇐ [ghdl/ghdl](https://github.com/ghdl/ghdl)
* OSVVM
  * `OSVVM-Libraries` ⇐ [OSVVM/OSVVMLibraries](https://github.com/OSVVM/OSVVMLibraries)
  * `OSVVM-Scripts` ⇐ [OSVVM/OSVVM-Scripts](https://github.com/OSVVM/OSVVM-Scripts)
  * `OSVVM` ⇐ [OSVVM/OSVVM](https://github.com/OSVVM/OSVVM)
  * `OSVVM-Common` ⇐ [OSVVM/OSVVM-Common](https://github.com/OSVVM/OSVVM-Common)
  * `OSVVM-AvalonMM` ⇐ [OSVVM/AvalonMM](https://github.com/OSVVM/AvalonMM) *(disabled)*
  * `OSVVM-AvalonST` ⇐ [OSVVM/AvalonST](https://github.com/OSVVM/AvalonST) *(disabled)*
  * `OSVVM-AXI4` ⇐ [OSVVM/AXI4](https://github.com/OSVVM/AXI4)
  * `OSVVM-CoSim` ⇐ [OSVVM/CoSim](https://github.com/OSVVM/CoSim)
  * `OSVVM-CoSimPCIe` ⇐ [OSVVM/CoSimPCIe](https://github.com/OSVVM/CoSimPCIe)
  * `OSVVM-DPRAM` ⇐ [OSVVM/DpRam](https://github.com/OSVVM/DpRam)
  * `OSVVM-Ethernet` ⇐ [OSVVM/Ethernet](https://github.com/OSVVM/Ethernet)
  * `OSVVM-UART` ⇐ [OSVVM/UART](https://github.com/OSVVM/UART)
  * `OSVVM-Wishbone` ⇐ [OSVVM/Wishbone](https://github.com/OSVVM/Wishbone)
  * `OSVVM-Documentation` ⇐ [OSVVM/Documentation](https://github.com/OSVVM/Documentation)
  * `OSVVM-SPI` ⇐ [OSVVM/SPI_GuyEschemann](https://github.com/OSVVM/SPI_GuyEschemann)
  * `OSVVM-VideoBus` ⇐ [OSVVM/VideoBus_LouisAdriaens](https://github.com/OSVVM/VideoBus_LouisAdriaens)
* VHDL
  * `PoC` ⇐ [VHDL/PoC](https://github.com/VHDL/PoC)
* *others*
  * `progit2` ⇐ [progit/progit2](https://github.com/progit/progit2)
  * `MINGW-packages` ⇐ [msys2/MINGW-packages](https://github.com/msys2/MINGW-packages)
  * `microwatt` ⇐ [antonblanchard/microwatt](https://github.com/antonblanchard/microwatt)
  * `itertree` ⇐ [BR1py/itertree](https://github.com/BR1py/itertree)


## Documentation

See [pyTooling/SynchronizeForks](https://github.com/pyTooling/SynchronizeForks) for the action: its parameters, the
configuration file format, what it reports as an error, and how branches and tags are synchronized.


# License

This GitHub Action Example (source code) is licensed under [MIT License](LICENSE.md).

-------------------------
SPDX-License-Identifier: MIT

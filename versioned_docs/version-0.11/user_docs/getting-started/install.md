---
sidebar_position: 2
---

# Installation

## 1. Install KCL

### From the Binary Releases

Each release of KCL includes various OSes and architectures. These binary versions can be manually downloaded and installed from [Github](https://github.com/kcl-lang/cli/releases/) and add `{install-location}` to the environment PATH.

#### MacOS & Linux

```bash
export PATH=$PATH:{install-location}
```

#### Windows

```powershell
$env:PATH += ";{install-location};"
```

### Using script to install the latest release

#### MacOS

Install or upgrade the latest darwin KCL to /usr/local/bin

```bash
curl -fsSL https://kcl-lang.io/script/install-cli.sh | /bin/bash
```

Uninstall

```bash
curl -fsSL https://kcl-lang.io/script/uninstall-cli.sh | /bin/bash
```

#### Linux

Install or upgrade the latest linux KCL to /usr/local/bin

```bash
wget -q https://kcl-lang.io/script/install-cli.sh -O - | /bin/bash
```

Uninstall

```bash
wget -q https://kcl-lang.io/script/uninstall-cli.sh -O - | /bin/bash
```

#### Windows

Install or upgrade the latest windows KCL to $Env:SystemDrive\kclvm\bin and add this directory to User PATH environment variable.

```bash
powershell -Command "iwr -useb https://www.kcl-lang.io/script/install-cli.ps1 | iex"
```

Uninstall

```shell
powershell -Command "iwr -useb https://www.kcl-lang.io/script/uninstall-cli.ps1 | iex"
```

### Homebrew (MacOS)

- Install

```bash
# Install the latest version
brew install kcl-lang/tap/kcl

# Specify a version
brew install kcl-lang/tap/kcl@x.y.z
```

- Upgrade

```bash
brew upgrade kcl-lang/tap/kcl
```

- Uninstall

```bash
brew uninstall kcl-lang/tap/kcl
```

### Scoop (Windows)

Install [Scoop](https://scoop.sh/) first, then add this bucket and install `kcl` by running:

```bash
scoop bucket add kcl-lang https://github.com/kcl-lang/scoop-bucket.git
scoop install kcl-lang/kcl
```

### From Go

Install `kcl` through the `Go` command (`Go` requires 1.22+).

```bash
go install kcl-lang.io/cli/cmd/kcl@latest
```

### From Docker

- Command

```bash
docker run --rm -it kcllang/kcl
```

- Update image

```bash
docker pull kcllang/kcl
```

### From Nix Packages

See [here](https://search.nixos.org/packages?channel=unstable&show=kcl&from=0&size=50&sort=relevance&type=packages&query=kcl)

### Note

We can execute the following command to ensure that KCL has been installed correctly.

```bash
kcl --help
```

If you are unable to successfully install and run KCL, you can refer to [here](/docs/user_docs/support/faq-install)

## 2. Install KCL IDE Extension

### Install Language Server

Before we enable the IDE extension, first we install the KCL Language Server binary and add it to the PATH.

#### MacOS

Install or upgrade the latest darwin KCL language server to /usr/local/bin

```bash
curl -fsSL https://kcl-lang.io/script/install-kcl-lsp.sh | /bin/bash
```

#### Linux

Install or upgrade the latest linux KCL language server to /usr/local/bin

```bash
wget -q https://kcl-lang.io/script/install-kcl-lsp.sh -O - | /bin/bash
```

#### Windows

Install or upgrade the latest windows KCL language server to $Env:SystemDrive\kclvm\bin and add this directory to User PATH environment variable.

```bash
powershell -Command "iwr -useb https://www.kcl-lang.io/script/install-kcl-lsp.ps1 | iex"
```

#### Homebrew (MacOS)

- Install

```bash
# Install the latest version
brew install kcl-lang/tap/kcl-lsp

# Specify a version
brew install kcl-lang/tap/kcl-lsp@x.y.z
```

- Upgrade

```bash
brew upgrade kcl-lang/tap/kcl-lsp
```

- Uninstall

```bash
brew uninstall kcl-lang/tap/kcl-lsp
```

#### Scoop (Windows)

Install [Scoop](https://scoop.sh/) first, then add this bucket and install `kcl-language-server` by running:

```bash
scoop bucket add kcl-lang https://github.com/kcl-lang/scoop-bucket.git
scoop install kcl-lang/kcl-lsp
```

### Install KCL Extensions for IDE

#### VS Code

The KCL Extension provides comprehensive coding assistance, offering features such as highlighting, go-to definition, completion, hover, outline, and diagnostics. For more information about the installation, please visit [here](/docs/tools/Ide/vs-code)

![Completion](/img/docs/tools/Ide/vs-code/Completion.gif)

#### NeoVim

To configure the KCL language server and enable it, please refer to [here](/docs/tools/Ide/neovim)

![kcl.nvim](/img/docs/tools/Ide/neovim/overview.png)

#### IntelliJ IDEA

We offer IntelliJ KCL plugins. For detailed information about the installation, please visit [here](/docs/tools/Ide/intellij)

![intellij](/img/docs/tools/Ide/intellij/overview.png)

# 发布 KCL 包到 ghcr.io

本文将指导您如何使用 kcl 包管理将您的 kcl 包推送到发布到 OCI Registry 中。kcl 包管理默认使用 [ghcr.io](https://ghcr.io) 作为 OCI Registry, 您可以通过修改 kcl 包管理配置文件来更改默认的 OCI Registry。关于如何修改 kcl 包管理配置文件的信息，请参阅 [kcl oci registry](https://github.com/kcl-lang/kpm/blob/main/docs/kpm_oci-zh.md#kpm-registry)

下面是一个简单的步骤，指导您如何使用 kcl 包管理将您的 kcl 包推送到 ghcr.io。

## 步骤 1：安装 KCL CLI

首先，您需要在您的计算机上安装 KCL CLI。您可以按照 [KCL CLI 安装文档](https://kcl-lang.io/zh-CN/docs/user_docs/getting-started/install)中的说明进行操作。

## 步骤 2：创建一个 ghcr.io 令牌

如果您使用默认的 OCI Registry, 要将 kcl 包推送到 ghcr.io，您需要创建一个用于身份验证的令牌。您可以参考以下文档。

- [创建 ghcr.io token](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)

## 步骤 3：登录 ghcr.io

在安装了 kcl 包管理并创建了 ghcr.io 令牌后，您需要使用 kcl 包管理登录 ghcr.io。您可以使用以下命令进行操作：

```shell
kcl registry login -u <USERNAME> -p <TOKEN> ghcr.io
```

其中 `<USERNAME>` 是您的 GitHub 用户名，`<TOKEN>` 是您在步骤 2 中创建的令牌。

关于如何使用 kcl 包管理登录 ghcr.io 的更多信息，请参阅 [kcl registry login](https://www.kcl-lang.io/zh-CN/docs/tools/cli/package-management/command-reference/login)。

## 步骤 4：推送您的 kcl 包

现在，您可以使用 kcl 包管理将您的 kcl 包推送到 ghcr.io。

### 1. 一个合法的 kcl 包

首先，您需要确保您推送的内容是符合一个 kcl 包的规范，即必须包含合法的 kcl.mod 和 kcl.mod.lock 文件。

如果您不知道如何得到一个合法的 `kcl.mod` 和 `kcl.mod.lock`。您可以使用 `kcl mod init` 命令。

```shell
# 创建一个名为 my_package 的 kcl 包
kcl mod init my_package
```

`kcl mod init my_package` 命令将会为您创建一个新的 kcl 包 `my_package`, 并为这个包创建 `kcl.mod` 和 `kcl.mod.lock` 文件。

如果您已经有了一个包含 kcl 文件的目录 `exist_kcl_package`，您可以使用以下命令将其转换为一个 kcl 包，并为其创建合法的 `kcl.mod` 和 `kcl.mod.lock`。

```shell
# 在 exist_kcl_package 目录下
pwd
/home/user/exist_kcl_package

# 执行 kcl 包管理init 命令来创建 kcl.mod 和 kcl.mod.lock
kcl mod init
```

关于如何使用 kcl 包管理init 的更多信息，请参阅 [kcl mod init](https://kcl-lang.io/zh-CN/docs/tools/cli/package-management/command-reference/init)。

### 2. 推送 kcl 包

您可以在 `kcl` 包的根目录下使用以下命令进行操作：

```shell
# 在 exist_kcl_package 包的根目录下
pwd
/home/user/exist_kcl_package

# 推送 kcl 包到默认的 OCI Registry
kcl mod push
```

完成上述步骤后，您就成功地将您的 kcl 包推送到了默认的 OCI Registry 中。
关于如何使用 kcl mod push 的更多信息，请参阅 [kcl mod push](https://kcl-lang.io/zh-CN/docs/tools/cli/package-management/command-reference/push)。

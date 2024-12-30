# 使用 OCI Registries

KCL 包管理工具支持通过 OCI Registries 保存和分享 KCL 包。

## 默认 OCI Registry

KCL 包管理工具默认使用 ghcr.io 保存 KCL 包。

默认 registry - [https://github.com/orgs/kcl-lang/packages](https://github.com/orgs/kcl-lang/packages)

## 自定义 OCI Registry

有几个支持 OCI 的托管容器 Registry，您可以将其用于存储 KCL 模块。

- [Docker Hub](https://docs.docker.com/docker-hub/oci-artifacts/)
- [Harbor](https://goharbor.io/docs/main/administration/user-defined-oci-artifact/)
- [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/push-oci-artifact.html)
- [Azure Container Registry](https://learn.microsoft.com/azure/container-registry/container-registry-oci-artifacts)
- [Google Artifact Registry](https://cloud.google.com/artifact-registry/docs/helm/manage-charts)
- [Alibaba Cloud Container Registry](https://help.aliyun.com/acr/)
- [IBM Cloud Container Registry](https://cloud.ibm.com/docs/Registry)
- [JFrog Artifactory](https://jfrog.com/help/r/jfrog-artifactory-documentation/docker-registry)

你可以通过以下方法调整 OCI Registry 的地址和仓库名称。

### 通过环境变量

你可以通过设置三个环境变量 KPM_REG、KPM_REGO 来调整配置。

```shell
# 设置默认仓库地址
export KPM_REG="ghcr.io"
# 设置默认仓库
export KPM_REPO="kcl-lang"
```

### 通过配置文件

KCL 包管理工具的配置文件位于 `$KCL_PKG_PATH/.kpm/config/kpm.json`，如果环境变量 `KCL_PKG_PATH` 没有设置，它默认保存在 `$HOME/.kcl/kpm/.kpm/config/kpm.json`。

配置文件的默认内容如下：

```json
{
  "DefaultOciRegistry": "ghcr.io",
  "DefaultOciRepo": "kcl-lang"
}
```

## 快速开始

在接下来的内容中，我们将使用 `localhost:5001` 作为示例 OCI Registry，并且为这个 OCI Registry 添加了一个账户 `test`，密码是 `1234`, 上传一个名称为 `MyPkg` 的 `v0.1.0` 的包。

### kcl registry login

你可以通过以下四种方式使用 `kcl registry login`。

#### 1. 使用账户和密码登陆 OCI Registry

```shell
kcl registry login -u <account_name> -p <password> <oci_registry>
Login succeeded
```

对我们的示例来说，命令如下：

```shell
kcl registry login -u test -p 1234 localhost:5001
```

#### 2. 使用账户登陆 OCI Registry，并且交互式输入密码

```shell
kcl registry login -u <account_name> <oci_registry>
Password:
Login succeeded
```

对我们的示例来说，命令如下：

```shell
kcl registry login -u test localhost:5001
Password: 1234
Login succeeded
```

#### 3. 交互式输入账户和密码登陆 OCI Registry

```shell
kcl registry login <oci_registry>
Username: <account_name>
Password:
Login succeeded
```

对我们的示例来说，命令如下：

```shell
kcl registry login localhost:5001
Username: test
Password: 1234
Login succeeded
```

### kcl registry logout

你可以使用 `kcl registry logout` 退出一个 OCI Registry。

```shell
kcl registry logout <registry>
```

对我们的示例来说，命令如下：

```shell
kcl registry logout localhost:5001
```

### kcl mod push

你可以在 kcl 包的根目录下使用 `kcl mod push` 命令将 kcl 包上传到一个 OCI Registry。

```shell
# 创建一个新的 kcl 包。
kcl mod init <package_name>
# 进入 kcl 包的根目录
cd <package_name>
# 将 kcl 包上传到一个 oci registry
kcl mod push
```

对于示例来说，命令如下：

```shell
kcl mod init MyPkg
cd MyPkg
kcl mod push
```

你也可以在 `kcl mod push` 命令中指定 OCI registry 的 url。

```shell
# 创建一个新的 kcl 包。
kcl mod init <package_name>
# 进入 kcl 包的根目录
cd <package_name>
# 将 kcl 包上传到一个 oci registry
kcl mod push <oci_url>
```

对于示例来说，您可以通过命令来 push kcl 包到 localhost:5001 中

```shell
kcl mod init MyPkg
cd MyPkg
kcl mod push oci://localhost:5001/test/MyPkg --tag v0.1.0
```

### kcl mod pull

你可以使用 `kcl mod pull` 从默认的 OCI registry 中下载一个 kcl 包。kpm 会自动从 `kpm.json` 中的 OCI registry 中寻找 kcl 包。

```shell
kcl mod pull <package_name>:<package_version>
```

对于示例来说，命令如下：

```shell
kcl mod pull MyPkg:v0.1.0
```

或者，你也可以从指定的 OCI registry url 中下载一个 kcl 包。

```shell
kcl mod pull <oci_url>
```

对于示例来说，命令如下：

```shell
kcl mod pull oci://localhost:5001/test/MyPkg --tag v0.1.0
```

### kcl mod add

你可以使用 `kcl mod add` 从默认的 OCI registry 中添加一个 kcl 包作为当前 kcl 包的三方依赖。kpm 会自动从 `kpm.json` 中的 OCI registry 中寻找 kcl 包。

```shell
kcl mod add <package_name>:<package_version>
```

对于示例来说，命令如下：

```shell
kcl mod add MyPkg:v0.1.0
```

或者，你也可以从指定的 OCI registry url 中下载一个 kcl 包。

```shell
kcl mod add <oci_url>
```

对于示例来说，命令如下：

```shell
kcl mod add oci://localhost:5001/test/MyPkg --tag v0.1.0
```

### kcl run

KCL 可以直接通过 OCI 的 url 编译 kcl 包。

```shell
kcl run <oci_url>
```

对于示例来说，命令如下：

```shell
kcl run oci://localhost:5001/test/MyPkg --tag v0.1.0
```

另外，你也可以通过 OCI 引用来编译 kcl 包。

```shell
kcl run <package_name>:<package_version>
```

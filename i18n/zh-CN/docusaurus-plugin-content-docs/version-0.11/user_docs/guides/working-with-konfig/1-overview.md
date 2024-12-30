---
id: overview
sidebar_label: 概述
---

import DocsCard from '@site/src/components/global/DocsCard';
import DocsCards from '@site/src/components/global/DocsCards';

# Konfig 概述

在 KCL 中推荐通过**配置库**的方式统一管理所有的配置清单和模型库，即不仅存放抽象模型本身的 KCL 定义，还存放各种类型的配置清单，比如应用的运维配置、策略配置等。配置大库推荐托管在各类 VCS 系统中，以方便做配置的回滚和漂移检查。配置大库的最佳实践代号为 Konfig，仓库托管在 [Github](https://github.com/kcl-lang/konfig)。

⚡️ 配置大库主要包括：

- KCL 模块声明文件（kcl.mod）
- KCL 领域模型库 (Kubernetes, Prometheus 等)
- 各类配置清单目录 (应用运维配置等)
- 配置构建和测试脚本 (Makefile，Github CI 文件等)

之所以用一个统一的仓库管理全部的 KCL 配置代码，是由于不同代码包的研发主体不同，会引发出包管理和版本管理的问题。将业务配置代码、基础配置代码在一个统一仓库中，代码间的版本依赖管理会比较简单，通过定位唯一代码库的目录及文件即可，可以将配置代码统一管理，便于查找、修改、维护。

下面是配置大库（Konfig）的架构图：

![](/img/docs/user_docs/guides/konfig/konfig-arch.png)

Konfig 提供给用户开箱即用、高度抽象的配置界面，模型库最初朴素的出发点就是改善 YAML 用户的效率和体验，我们希望通过将代码更繁杂的模型抽象封装到统一的模型中，从而简化用户侧配置代码的编写。Konfig 由以下部分组成：

- **核心模型**：
  - **前端模型**：前端模型即「用户界面」，包含平台侧暴露给用户的所有可配置属性，其中省略了一些重复的、可推导的配置，抽象出必要属性暴露给用户，具有用户友好的特性，比如 server.k。
  - **后端模型**：后端模型是「模型实现」，是让前端模型属性生效的模型，主要包含前端模型实例的渲染逻辑，后端模型中可借助 KCL 编写校验和逻辑判断等以提高配置代码复用性和健壮性，对用户不感知，比如 server_backend.k
- **领域模型**：是不包含任何实现逻辑和抽象的模型，往往由工具转换生成，无需修改，和真正生效的 YAML 属性一一对应，底层模型需要经过进一步抽象，一般不直接被用户使用。比如，k8s 是 Kubernetes 场景的底层模型库。

此外，核心模型内部通过前端模型和后端模型两层抽象简化前端用户的配置代码，底层模型则是通过 KCL Import 工具自动生成。

## 文档

<DocsCards>
  <DocsCard header="结构" href="structure">
    <p>Konfig 仓库目录和代码结构的说明。</p>
  </DocsCard>
  <DocsCard header="快速开始" href="guide">
    <p>使用 Konfig 的快速指南。</p>
  </DocsCard>
  <DocsCard header="最佳实践" href="practice">
    <p>将新模型集成到 Konfig 以及 KCL 代码编写的最佳实践指南。</p>
  </DocsCard>
</DocsCards>

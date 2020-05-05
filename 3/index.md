# 雨果模块


<!--more-->

# 雨果模块

**雨果模块**是**雨果**的核心构建模块。一个*模块*可以是你的主要项目或更小的模块，提供雨果定义的7种成分中的一种或多种：**静态**，**内容**，**布局**，**数据**，**资产**，**国际化**，和**原型**。

您可以按自己喜欢的任何方式组合模块，甚至可以从非Hugo项目中挂载目录，从而形成一个大型的虚拟联合文件系统。

Hugo Modules由Go Modules驱动。有关Go模块的更多信息，请参见：

- https://github.com/golang/go/wiki/Modules
- https://blog.golang.org/using-go-modules

这都是全新的，周围只有几个示例项目：

- https://github.com/bep/docuapi是在测试此功能时已移植到Hugo Modules的主题。这是将非Hugo项目装入Hugo文件夹结构的一个很好的例子。它甚至显示了常规Go模板中的JS Bundler实现。
- https://github.com/bep/my-modular-site是用于测试的非常简单的站点。





# 配置模块

本页介绍模块的配置选项。

## 模块配置：顶级

配置。

yaml托姆json

```toml
[module]
  noProxy = "none"
  private = "*.*"
  proxy = "direct"
```



- 代理

  定义用于下载远程模块的代理服务器。默认值为`direct`，表示“ git clone”和类似名称。

- noProxy

  逗号分隔的全局列表匹配路径，不应使用上面配置的代理。

- 私人的

  逗号分隔的全局列表匹配路径应视为私有。

请注意，以上术语直接映射到Go模块中的相应术语。这些设置中的某些设置很自然地设置为OS环境变量。例如，要设置要使用的代理服务器：

```
env HUGO_MODULE_PROXY=https://proxy.example.org hugo
```

**Hugo Modules的**大多数命令都需要安装更高版本的Go（请参阅https://golang.org/dl/）和相关的VCS客户端（例如Git，请参阅[https://git-scm.com/downloads）](https://git-scm.com/downloads))。如果您在Netlify上运行一个“较旧”的站点，则可能必须在“环境”设置中将GO_VERSION设置为1.12。

有关Go模块的更多信息，请参见：

- https://github.com/golang/go/wiki/Modules
- https://blog.golang.org/using-go-modules

## 模块配置：hugoVersion

如果您的模块要求使用特定版本的Hugo，则可以在该`module`部分中进行说明，如果使用的版本太旧/太新，则会警告用户。

配置。

yaml托姆json

```toml
[module]
  [module.hugoVersion]
    extended = false
    max = ""
    min = ""
```



以上任何一项都可以省略。

- 分

  支持的最低Hugo版本，例如 `0.55.0`

- 最大值

  支持的最高Hugo版本，例如 `0.55.0`

- 扩展的

  是否需要扩展版本的Hugo。

## 模块配置：导入

配置。

yaml托姆json

```toml
[module]

  [[module.imports]]
    disable = false
    ignoreConfig = false
    path = "github.com/gohugoio/hugoTestModules1_linux/modh1_2_1v"

  [[module.imports]]
    path = "my-shortcodes"
```



- 路径

  可以是有效的Go Module模块路径（例如）`github.com/gohugoio/myShortcodes`，也可以是主题文件夹中存储的模块目录名称。

- ignoreConfig

  如果启用，`config.toml`则不会加载任何模块配置文件，例如。请注意，这还将停止加载任何可传递模块依赖项。

- 禁用

  设置为`true`禁用模块，同时保留`go.*`文件中的任何版本信息。

**Hugo Modules的**大多数命令都需要安装更高版本的Go（请参阅https://golang.org/dl/）和相关的VCS客户端（例如Git，请参阅[https://git-scm.com/downloads）](https://git-scm.com/downloads))。如果您在Netlify上运行一个“较旧”的站点，则可能必须在“环境”设置中将GO_VERSION设置为1.12。

有关Go模块的更多信息，请参见：

- https://github.com/golang/go/wiki/Modules
- https://blog.golang.org/using-go-modules

## 模块配置：安装

当`mounts`配置在Hugo 0.56.0中引入时，我们非常小心地保留了现有的`staticDir`和类似的配置，以确保所有现有站点都能继续工作。

但是，您不应同时拥有两者。因此，如果添加`mounts`部分，则应使其完整并删除旧的`staticDir`等设置。

配置。

yaml托姆json

```toml
[module]

  [[module.mounts]]
    source = "content"
    target = "content"

  [[module.mounts]]
    source = "static"
    target = "static"

  [[module.mounts]]
    source = "layouts"
    target = "layouts"

  [[module.mounts]]
    source = "data"
    target = "data"

  [[module.mounts]]
    source = "assets"
    target = "assets"

  [[module.mounts]]
    source = "i18n"
    target = "i18n"

  [[module.mounts]]
    source = "archetypes"
    target = "archetypes"
```



- 资源

  挂载的源目录。对于主项目，这可以是项目相对的或绝对的，甚至是符号链接。对于其他模块，它必须相对于项目。

- 目标

  应该将其安装到Hugo的虚拟文件系统中的位置。它必须以雨果的组件文件夹中的一个：`static`，`content`，`layouts`，`data`，`assets`，`i18n`，或`archetypes`。例如`content/blog`。

- 郎

  语言代码，例如“ en”。仅与`content`安装有关，并且`static`在多主机模式下安装。

## 使用Hugo模块

如何使用Hugo Modules构建和管理您的网站。

## 先决条件

**Hugo Modules的**大多数命令都需要安装更高版本的Go（请参阅https://golang.org/dl/）和相关的VCS客户端（例如Git，请参阅[https://git-scm.com/downloads）](https://git-scm.com/downloads))。如果您在Netlify上运行一个“较旧”的站点，则可能必须在“环境”设置中将GO_VERSION设置为1.12。

有关Go模块的更多信息，请参见：

- https://github.com/golang/go/wiki/Modules
- https://blog.golang.org/using-go-modules

## 初始化新模块

使用`hugo mod init`初始化一个新的雨果模块。如果无法猜测模块路径，则必须将其作为参数提供，例如：

```bash
hugo mod init github.com/gohugoio/myShortcodes
```

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_init/)。

## 更新模块

将模块作为导入添加到配置中时，将下载并添加[模块](https://gohugo.io/hugo-modules/configuration/#module-config-imports)，请参阅[模块导入](https://gohugo.io/hugo-modules/configuration/#module-config-imports)。

要更新或管理版本，可以使用`hugo mod get`。

一些例子：

### 更新所有模块

```bash
hugo mod get -u
```

### 递归更新所有模块

[v0.65.0的新功能](https://gohugo.io/news/0.65.0-relnotes/)

```bash
hugo mod get -u ./...
```

### 更新一个模块

```bash
hugo mod get -u github.com/gohugoio/myShortcodes
```

### 获取特定版本

```bash
hugo mod get github.com/gohugoio/myShortcodes@v1.0.7
```

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_get/)。

## 在模块中进行和测试更改

对项目中导入的模块进行本地开发的一种方法是，将替换指令添加到本地目录中，其源位于`go.mod`：

```bash
replace github.com/bep/hugotestmods/mypartials => /Users/bep/hugotestmods/mypartials
```

如果`hugo server`正在运行，将重新加载配置并将其`/Users/bep/hugotestmods/mypartials`放在监视列表中。

## 打印依赖图

使用`hugo mod graph`从相关模块的目录，它会打印依赖关系图，包括vendoring，更换模块或禁用状态。

例如：

```
hugo mod graph

github.com/bep/my-modular-site github.com/bep/hugotestmods/mymounts@v1.2.0
github.com/bep/my-modular-site github.com/bep/hugotestmods/mypartials@v1.0.7
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myassets@v1.0.4
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myv2@v1.0.0
DISABLED github.com/bep/my-modular-site github.com/spf13/hyde@v0.0.0-20190427180251-e36f5799b396
github.com/bep/my-modular-site github.com/bep/hugo-fresh@v1.0.1
github.com/bep/my-modular-site in-themesdir
```

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_graph/)。

## 供应商您的模块

`hugo mod vendor`将所有模块依赖关系写入一个`_vendor`文件夹，该文件夹随后将用于所有后续构建。

注意：

- 您可以`hugo mod vendor`在模块树中的任何级别上运行。
- 供应商不会存储您`themes`文件夹中存储的模块。
- 大多数命令接受一个`--ignoreVendor`标志，该标志将`_vendor`在模块树中不存在任何文件夹的情况下运行。

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_vendor/)。

## 整理go.mod，go.sum

运行`hugo mod tidy`以删除`go.mod`和中未使用的条目`go.sum`。

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_clean/)。

## 清理模块缓存

运行`hugo mod clean`以删除整个模块缓存。

请注意，您还可以使用来配置`modules`缓存`maxAge`，请参阅[文件缓存](https://gohugo.io/hugo-modules/configuration/#configure-file-caches)。

另请参阅[CLI Doc](https://gohugo.io/commands/hugo_mod_clean/)。



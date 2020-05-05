# 雨果内容管理


<!--more-->

# 内容组织

Hugo假定使用用于组织源内容的相同结构来组织呈现的网站。

## 页面捆绑[ ](https://gohugo.io/content-management/organization/#page-bundles)

雨果`0.32`宣布将与页面相关的图片和其他资源打包到中`Page Bundles`。

这些术语是相关的，您还需要阅读有关“ [页面资源”](https://gohugo.io/content-management/page-resources/)和“ [图像处理”](https://gohugo.io/content-management/image-processing/)的全部内容。

该图显示了3个捆绑包。请注意，主页捆绑包不能包含其他内容页面，但是其他文件（图像等）也可以。

```
</small>
</figcaption>
```

捆绑包文档正在开发**中**。我们将很快发布有关此内容的更全面的文档。

# 内容来源的组织

在Hugo中，您的内容应以反映所呈现网站的方式进行组织。

尽管Hugo支持在任何级别嵌套的内容，但顶层（即`content/`）在Hugo中是特殊的，被认为是用于确定布局等的内容类型。要了解有关部分的更多信息，包括如何嵌套它们，请参见[部分](https://gohugo.io/content-management/sections/)。

无需任何其他配置，以下各项将起作用：

```
.
└── content
    └── about
    |   └── index.md  // <- https://example.com/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

## 雨果的路径分解[ ](https://gohugo.io/content-management/organization/#path-breakdown-in-hugo)

以下内容演示了您的内容组织与呈现后的Hugo网站的输出URL结构之间的关系。这些示例假设您[使用的是漂亮的URL](https://gohugo.io/content-management/urls/#pretty-urls)，这是Hugo的默认行为。这些示例还假设`baseURL = "https://example.com"`您[网站的配置文件中](https://gohugo.io/getting-started/configuration/)的键值为。

### 索引页： `_index.md`[ ](https://gohugo.io/content-management/organization/#index-pages-_indexmd)

`_index.md`在雨果中扮演特殊角色。它使您可以将主题和内容添加到[列表模板中](https://gohugo.io/templates/lists/)。这些模板包括[部分模板](https://gohugo.io/templates/section-templates/)，[分类模板](https://gohugo.io/templates/taxonomy-templates/)，[分类术语模板](https://gohugo.io/templates/taxonomy-templates/)和您的[主页模板的模板](https://gohugo.io/templates/homepage/)。

**提示：**`_index.md`使用[`.Site.GetPage`函数](https://gohugo.io/functions/getpage/)可以获取对内容和元数据的引用。

您可以`_index.md`在首页中保留一个，在每个内容部分，分类法和分类术语中保留一个。下面显示了一个通常的位置，该位置`_index.md`将包含`posts`Hugo网站上部分列表页面的内容和前端内容：

```
.         url
.       ⊢--^-⊣
.        path    slug
.       ⊢--^-⊣⊢---^---⊣
.           filepath
.       ⊢------^------⊣
content/posts/_index.md
```

在构建时，它将与相关值一起输出到以下目标：

```
                     url ("/posts/")
                    ⊢-^-⊣
       baseurl      section ("posts")
⊢--------^---------⊣⊢-^-⊣
        permalink
⊢----------^-------------⊣
https://example.com/posts/index.html
```

这些[部分](https://gohugo.io/content-management/sections/)可以根据需要深度嵌套。要理解的重要部分是，要使部分树完全导航，至少最下面的部分需要一个内容文件。（即`_index.md`）。

### 章节中的单页[ ](https://gohugo.io/content-management/organization/#single-pages-in-sections)

您每个部分中的单个内容文件都将呈现为[单页模板](https://gohugo.io/templates/single-page-templates/)。这里是一个单一的示例`post`内`posts`：

```
                   path ("posts/my-first-hugo-post.md")
.       ⊢-----------^------------⊣
.      section        slug
.       ⊢-^-⊣⊢--------^----------⊣
content/posts/my-first-hugo-post.md
```

在Hugo建立您的网站时，内容将输出到以下目的地：

```
                               url ("/posts/my-first-hugo-post/")
                   ⊢------------^----------⊣
       baseurl     section     slug
⊢--------^--------⊣⊢-^--⊣⊢-------^---------⊣
                 permalink
⊢--------------------^---------------------⊣
https://example.com/posts/my-first-hugo-post/index.html
```

## 路径说明[ ](https://gohugo.io/content-management/organization/#paths-explained)

以下概念将在构建输出网站时更深入地了解您项目的组织与Hugo的默认行为之间的关系。

### `section`[ ](https://gohugo.io/content-management/organization/#section)

默认内容类型由一条内容的部分确定。`section`由项目`content`目录中的位置确定。`section` *不能*在前面指定或覆盖。

### `slug`[ ](https://gohugo.io/content-management/organization/#slug)

内容`slug`是`name.extension`或`name/`。的值`slug`取决于

- 内容文件的名称（例如`lollapalooza.md`）或
- 前沿物质优先

### `path`[ ](https://gohugo.io/content-management/organization/#path)

内容的内容`path`由文件的节路径决定。文件`path`

- 基于到内容位置的路径，并且
- 不包括the

### `url`[ ](https://gohugo.io/content-management/organization/#url)

该`url`是对该条内容的相对URL。的`url`

- 基于目录结构中内容的位置或
- 在最前面定义，并*覆盖以上所有内容*

## 通过前项覆盖目标路径[ ](https://gohugo.io/content-management/organization/#override-destination-paths-via-front-matter)

雨果（Hugo）相信您有目的地组织内容。用于组织您的源内容的相同结构用于组织呈现的网站。如上所示，源内容的组织将在目标中进行镜像。

有时您可能需要对内容进行更多控制。在这些情况下，可以在最前面的内容中指定字段以确定特定内容的目的地。

出于特定原因，按以下顺序定义了以下项目：列表中后面解释的项目将覆盖较早的项目，并且并非所有这些项目都可以在前面定义：

### `filename`[ ](https://gohugo.io/content-management/organization/#filename)

这不是最重要的问题，而是文件的实际名称减去扩展名。这将是目标中文件的名称（例如，`content/posts/my-post.md`变为`example.com/posts/my-post/`）。

### `slug`[ ](https://gohugo.io/content-management/organization/#slug-1)

在最前面定义时，`slug`可以代替目标的文件名。

内容/帖子/old-post.md



```md
---
title: A new post with the filename old-post.md
slug: "new-post"
---
```

这将根据Hugo的默认行为呈现到以下目标：

```
example.com/posts/new-post/
```

### `section`[ ](https://gohugo.io/content-management/organization/#section-1)

`section`是由内容在磁盘上的位置确定的，*不能*在前面的内容中指定。请参阅[各节](https://gohugo.io/content-management/sections/)以获取更多信息。

### `type`[ ](https://gohugo.io/content-management/organization/#type)

内容的`type`位置也取决于其在磁盘上的位置，但是与不同`section`，它*可以*在最前面指定。查看[类型](https://gohugo.io/content-management/types/)。当您希望使用不同的布局呈现内容时，这会特别方便。在以下示例中，`layouts/new/mylayout.html`即使在许多其他帖子中，您也可以在Hugo 处创建一个布局来呈现此内容。

内容/帖子/my-post.md



```md
---
title: My Post
type: new
layout: mylayout
---
```

### `url`[ ](https://gohugo.io/content-management/organization/#url-1)

可以提供完整的URL。这将覆盖以上所有与最终目的地有关的内容。这必须是baseURL的路径（以开头`/`）。`url`将完全按照前面的说明使用，并且将忽略`--uglyURLs`站点配置中的设置：

内容/帖子/old-url.md



```md
---
title: Old URL
url: /blog/new-url/
---
```

假设你`baseURL`被[配置](https://gohugo.io/getting-started/configuration/)来`https://example.com`，加入`url`到前面的问题将`old-url.md`呈现以下目标：

```
https://example.com/blog/new-url/
```

您可以在[URL Management中](https://gohugo.io/content-management/urls/)看到有关如何控制输出路径的更多信息。

## 页面捆绑

使用页面捆绑的内容组织

页面捆绑包是对[页面资源](https://gohugo.io/content-management/page-resources/)进行分组的一种方法。

页面捆绑包可以是以下之一：

- 叶子包（叶子意味着没有孩子）
- 分支捆绑包（主页，部分，分类术语，分类列表）

|                           | 叶捆                                                         | 分支捆绑                                                     |
| :------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 用法                      | 单页内容和附件的收集                                         | 章节页面（首页，章节，分类术语，分类列表）的附件集合         |
| 索引文件名                | `index.md` [1个](https://gohugo.io/content-management/page-bundles/#fn:1) | `_index.md` [1个](https://gohugo.io/content-management/page-bundles/#fn:1) |
| 允许的资源                | 页面和非页面（例如图像，pdf等）类型                          | 仅非页面（如图像，pdf等）类型                                |
| 资源可以住在哪里？        | 在叶捆绑目录中的任何目录级别。                               | 仅在分支捆绑目录的目录级别**，**即包含`_index.md`（[ref](https://discourse.gohugo.io/t/question-about-content-folder-structure/11822/4?u=kaushalmodi)）的目录。 |
| 布局类型                  | `single`                                                     | `list`                                                       |
| 套料                      | 不允许在其下嵌套更多束                                       | 允许在其下嵌套叶或分支束                                     |
| 例                        | `content/posts/my-post/index.md`                             | `content/posts/_index.md`                                    |
| 来自非索引页面文件的内容… | 仅作为页面资源访问                                           | 仅以常规页面访问                                             |

## 叶捆[ ](https://gohugo.io/content-management/page-bundles/#leaf-bundles)

一个*叶包*是在中的任何层次的目录`content/` 目录，包含一个**`index.md`**文件。

### Leaf Bundle组织的示例[ ](https://gohugo.io/content-management/page-bundles/#examples-of-leaf-bundle-organization)

```text
content/
├── about
│   ├── index.md
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── content2.md
│   │   ├── image1.jpg
│   │   ├── image2.png
│   │   └── index.md
│   └── my-other-post
│       └── index.md
│
└── another-section
    ├── ..
    └── not-a-leaf-bundle
        ├── ..
        └── another-leaf-bundle
            └── index.md
```

在上面的示例`content/`目录中，有四个叶束：

- 关于

  此叶捆绑包位于根级别（直接在`content`目录下 ），并且只有`index.md`。

- 我的帖子

  此叶包具有`index.md`，两个其他内容Markdown文件和两个图像文件。

- 我的另一个职位

  此叶丛只有`index.md`。

- 另一叶捆

  此叶束嵌套在几个目录下。此捆绑包也只有`index.md`。

只要**叶**束不在另一个**叶**束之内，创建叶束的层次深度就无关紧要。

### 无头束[ ](https://gohugo.io/content-management/page-bundles/#headless-bundle)

无头包是配置为无法在任何地方发布的包：

- 在中将没有`Permalink`任何HTML `public/`。
- 它不会成为的一部分`.Site.RegularPages`，等等。

但是你可以得到它`.Site.GetPage`。这是一个例子：

```go-html-template
{{ $headless := .Site.GetPage "/some-headless-bundle" }}
{{ $reusablePages := $headless.Resources.Match "author*" }}
<h2>Authors</h2>
{{ range $reusablePages }}
    <h3>{{ .Title }}</h3>
    {{ .Content }}
{{ end }}
```

*在此示例中，我们假设`some-headless-bundle`头是一个无头文件包，其中包含一个或多个匹配的 **页面**资源。`.Name``"author\*"`*

以上示例的说明：

1. 获取`some-headless-bundle`页面“对象”。
2. 收集*切片*的资源在这一*页包*是匹配 `"author*"`使用`.Resources.Match`。
3. 循环浏览该嵌套页面*切片*，并输出它们的`.Title`和 `.Content`。

------

可以在下面的“前部事项”（中的`index.md`）中添加无叶束的束头：

```toml
headless = true
```

只能使叶束无头。

这种无头页面捆绑有很多用例：

- 共享媒体库
- 可重复使用的页面内容“代码段”

## 分支捆绑[ ](https://gohugo.io/content-management/page-bundles/#branch-bundles)

一个*分支束*处于内的任何层级的任何目录 `content/`的目录，包含至少一个**`_index.md`**文件。

这`_index.md`也可以直接下`content/`目录。

这里`md`（markdown）仅用作示例。您可以使用任何文件类型作为内容资源，只要它是Hugo可以识别的内容类型即可。

### 分支捆绑组织的示例[ ](https://gohugo.io/content-management/page-bundles/#examples-of-branch-bundle-organization)

```text
content/
├── branch-bundle-1
│   ├── branch-content1.md
│   ├── branch-content2.md
│   ├── image1.jpg
│   ├── image2.png
│   └── _index.md
└── branch-bundle-2
    ├── _index.md
    └── a-leaf-bundle
        └── index.md
```

在上面的示例`content/`目录中，有两个分支捆绑包（和叶子捆绑包）：

- `branch-bundle-1`

  此分支捆绑包包含`_index.md`，两个其他内容Markdown文件和两个图像文件。

- `branch-bundle-2`

  该分支束具有`_index.md`和嵌套叶束。

创建分支束的层次深度无关紧要。

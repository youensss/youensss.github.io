<!--
.. title: WSL2 使用 Nikola 搭建个人博客
.. slug: wsl2-shi-yong-nikola-da-jian-ge-ren-bo-ke
.. date: 2022-04-29 13:28:09 UTC+08:00
.. tags: WSL2, Nikola, blog, github, Jupyter-notebook
.. category: Nikola
.. link: 
.. description: 
.. type: text
-->

[TOC]

一直希望搭建一个博客，一方面是希望能有一个地方记录整理自己学过的知识，另一方面也希望能交到一些志同道合的朋友（有人可以讨论真是太棒了！）在各种鼓捣之后，我选择了 [Nikola](https://getnikola.com/)，因为它原生支持 Jupyter notebook。至于为什么选择静态博客，Nikola 官网这篇[文章](https://getnikola.com/handbook.html#why-static)写的很好。

关于如何使用 Nikola 搭建博客和个人网站，相关的中文资源比较少，这篇文章是我个人，来自一个非科班背景的踩坑经历。博客搭建环境如下：

```
WLS2 Ubuntu 20.04.3 LTS
Nikola Version: 8.2.1
Python 3.8.10
pip 20.0.2
```

# 1 Nikola 安装与博客搭建

这部分内容是对官方文档 [getting started](https://getnikola.com/getting-started.html) 的一个梳理。其实官方文档写的非常清晰，强烈建议大家去瞅一瞅。

## 1.1 Nikola 安装

官方建议使用虚拟环境来安装 Nikola，我们可以使用 Python 自带的 venv 模块来创建虚拟环境。

```shell
# 新建目录
mkdir nikola

# 进入到 nikola 目录下，创建虚拟环境
cd nikola
python3 -m venv blog

# 进入到虚拟环境目录，激活虚拟环境
cd blog
source ./bin/activate

# 使用 pip 安装 Nikola
pip install -U pip setuptools wheel
pip install -U "Nikola[extras]"
```

官方建议在安装时选择 Nikola[extras] 版本（可以体验一些额外的功能），但是你也可以选择安装 Nikola 版本。

## 1.2 初始化博客

在安装完 Nikola 之后，我们就可以使用 Nikola 提供的命令初始化博客了。

```shell
nikola init --demo my_blog
```

这条命令会在当前目录下创建一个新的目录 `my_blog`，`--demo my_blog` 参数表示在初始化目录的时候创建一些 demo 文件（当然你也可以选择不创建 demo 文件，使用 `--quiet` 参数）。你会看到如下输出和提问：

```shell
Creating Nikola Site
====================

This is Nikola v8.2.1.  We will now ask you a few easy questions about your new site.
If you do not want to answer and want to go with the defaults instead, simply restart with the `-q` parameter.
--- Questions about the site ---
Site title [My Nikola Site]:
```

回答命令行中的提问：

- Site title：网站名
- Site author：网站作者
- Site author's e-mail：联系邮箱
- Site description：网站描述，生成网站的 meta description
- Site URL：网页 URL
- Enable pretty URLs (/page/ instead of /page.html) that don't need web server configuration? [Y/n]：是否开启 [pretty URLs](https://en.wikipedia.org/wiki/Clean_URL)
- Language(s) to use [en]：选择网站显示语言
- Time zone [Asia/Shanghai]：选择时区
- Comment system：选择评论系统

当你看到以下提示时，就说明博客初始化成功了。

```shell
That's it, Nikola is now configured.  Make sure to edit conf.py to your liking.
If you are looking for themes and addons, check out https://themes.getnikola.com/ and https://plugins.getnikola.com/.
Have fun!
[2022-04-29 16:09:00] INFO: init: A new site with example data has been created at my_blog.
[2022-04-29 16:09:00] INFO: init: See README.txt in that folder for more information.
```

我们可以看一下 `my_blog` 目录，这是我们的博客根目录。

```shell
.
├── files
│   └── images
├── galleries
│   └── demo
├── images
├── listings
│   └── __pycache__
├── pages
├── posts
└── templates
```

- files：
- galleries：
- images：
- listings：
- pages：
- posts：
- templates：

## 1.3 新建博客

博客初始化完成之后，我们就可以添加第一篇博客了。

### 1.3.1 添加第一篇博客

使用命令 `nikola new_post -f markdown` 添加第一篇博客：

```shell
Creating New Post
-----------------

Title: First post!
Scanning posts........done!
[2022-04-29 20:50:04] INFO: new_post: Your post's text is at: posts/first-post.md
```

`-f` 参数指定文章格式，这里我们指定为 markdown 格式，该命令会在 posts 文件夹下新建一个markdown 文件，你可以使用自己喜欢的编辑器打开编辑。Nikola 支持如下格式：

> - reStructuredText (default and pre-configured)
> - [Markdown](https://getnikola.com/handbook.html#markdown) (pre-configured since v7.8.7)
> - [Jupyter Notebook](https://getnikola.com/handbook.html#jupyter-notebook)
> - [HTML](https://getnikola.com/handbook.html#html)
> - [PHP](https://getnikola.com/handbook.html#php)
> - anything [Pandoc](https://getnikola.com/handbook.html#pandoc) supports (including Textile, DocBook, LaTeX, MediaWiki, TWiki, OPML, Emacs Org-Mode, txt2tags, Microsoft Word .docx, EPUB, Haddock markup)

### 1.3.2 生成静态文件

使用命令 `nikola build` 会在 output 目录下生成博客静态文件。output 目录是博客资源的根目录，你的博客中所显示的所有内容都来自于 output 目录。

### 1.3.3 本地调试

使用命令 `nikola serve -b` 会打开默认浏览器，方便我们进行本地调试。但是，由于我是在 WSL2 中搭建博客，键入 `nikola serve -b` 命令之后，`http://0.0.0.0:8000/ ` 显示拒绝访问。

解决方法是在 windows `C:\Users\Me` 目录下新建 `.wslconfig` 配置文件，并输入以下内容：

```shell
[wsl2]
localhostForwarding=true
```

## 1.4 让 Nikola 支持 Jupyter Notebook

让 Nikola 支持 Jupyter Notebook 需要满足两点：

- `ipynb` 在 `COMPILERS` 中（Nikola V8.2.1 中 `ipynb` 已在 `COMPILERS` 中被定义），打开站点配置文件 `conf.py`：

```
COMPILERS = {                                                                                                 "rest": ['.rst', '.txt'],
    "markdown": ['.md', '.mdown', '.markdown'],
    "textile": ['.textile'],
    "txt2tags": ['.t2t'],
    "bbcode": ['.bb'],
    "wiki": ['.wiki'],
    "ipynb": ['.ipynb'],
    "html": ['.html', '.htm'],
    "php": ['.php'],
}
```

- `.ipynb` 插件在 `POSTS` 中被定义，编辑站点配置文件 `conf.py`，修改第 238 行 `POSTS` 元组如下：

```python
 POSTS = (                                                                                                            ("posts/*.rst", "posts", "post.tmpl"),
         ("posts/*.md", "posts", "post.tmpl"),
         ("posts/*.txt", "posts", "post.tmpl"),
         ("posts/*.html", "posts", "post.tmpl"),
         ("posts/*.ipynb", "posts", "post.tmpl"), 
 )
```

我们可以使用字典向 Jupyter Notebook 中添加博客文章必需的元信息，方法为打开 Jupyter Notebook，菜单栏选择 *Edit -> Edit Notebook Metadata*，添加如下格式信息：

```json
{
    "nikola": {
        "title": "Example for Jupyter Notebook metadata configuration",
        "slug": "example-for-jupyter-notebook-metadata-configuration",
        "date": "2022-04-29 19:52:05 UTC"
    },
}
```

## 1.5 markdown 支持

Nikola 默认使用 [python-markdown](https://python-markdown.github.io/reference/) 解析 markdown，你也可以选择使用 pandoc，这篇[博客](https://www.brainsorting.com/posts/create-a-blog-with-nikola/#using-markdown-for-your-post) 讲的非常清楚。打开站点配置文件，编辑 markdown 插件配置选项：

```python
 MARKDOWN_EXTENSIONS = [
     'markdown.extensions.meta',
     'markdown.extensions.fenced_code', 
     'markdown.extensions.codehilite', 
     'markdown.extensions.extra',
     'markdown.extensions.toc',  # 自动解析文章目录
 ]
```

# 2 博客美化

## 2.1 导航栏配置

## 2.2 字体配置

## 2.3 代码块配置

## 2.4 目录美化

# 3 部署到 Github 




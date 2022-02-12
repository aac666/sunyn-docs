# Sphinx + Github + ReadTheDocs

## 第1章 安装python3环境

1.执行命令：cd /usr/local切换到目录 /usr/local
2.下载python3 tar包：wget http://cdn.npm.taobao.org/dist/python/3.6.5/Python-3.6.5.tgz
3.执行命令：tar -zxvf Python-3.6.5.tgz  解压包
4.执行命令：cd Python-3.6.5 进入到解压出的python目录内
5.执行命令：./configure
6.执行命令：make clean
7.执行命令：make all
8.执行命令：make install
9.执行命令：ln -s /usr/local/bin/python3 /usr/bin/python3 设置python3软连接
10.执行命令：python3 --version验证python版本

## 第2章 安装git环境

### 1.最新git源码下载地址

https://github.com/git/git/releases
https://www.kernel.org/pub/software/scm/git/
安装git
yum install git
查看yum源仓库Git信息
yum info git

### 2.安装依赖库

[root@wugenqiang ~]# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel 
[root@wugenqiang ~]# yum install gcc-c++ perl-ExtUtils-MakeMaker

### 3.如果原有的git版本过低，移除默认安装的旧版git

[root@wugenqiang ~]# git --version    ## 查看自带的版本git version 1.8.3.1
[root@wugenqiang ~]# yum remove git   ## 移除原来的版本

### 4.下载&安装

[root@wugenqiang ~]# cd /usr/src
[root@wugenqiang src]#wget https://www.kernel.org/pub/software/scm/git/git-2.18.0.tar.gz
  
### 5.解压

[root@wugenqiang ~]# tar xf git-2.18.0.tar.gz

### 6.配置编译安装

[root@wugenqiang ~]# cd /usr/src
[root@wugenqiang src]# ls
debug  git-2.18.0  kernels
[root@wugenqiang src]# cd git-2.18.0/
[root@wugenqiang git-2.18.0]# 
[root@wugenqiang git-2.18.0]# make configure
[root@wugenqiang git-2.18.0]# ./configure --prefix=/usr/git ##配置目录
[root@wugenqiang git-2.18.0]# make profix=/usr/git
[root@wugenqiang git-2.18.0]# make install

### 7.加入环境变量

[root@wugenqiang ~]# echo "export PATH=$PATH:/usr/git/bin" >> /etc/profile
[root@wugenqiang ~]# source /etc/profile

### 8.检查版本

[root@wugenqiang ~]# git --version
git version 2.18.0

## 第3章 安装Sphinx

先把远程项目克隆到本地：
git clone git@github.com:aac666/sunyn-docs.git 
cd sunyn-docs
创建 python 虚拟环境

$ python3 -m venv v-env# 激活 python 虚拟环境环境
$ source v-env/bin/activate# 停用 python 虚拟环境使用以下命令
$ deactivate
安装Sphinx同时会安装很多python依赖，请耐心等待...
$ pip install sphinx sphinx-autobuild sphinx_rtd_theme
安装完成后，执行命令
$ sphinx-quickstart

翻译成中文如下
欢迎使用 Sphinx 4.3.2 快速配置工具。

请输入接下来各项设置的值（如果方括号中指定了默认值，直接
按回车即可使用默认值）。

已选择根路径：.

有两种方式来设置 Sphinx 输出的创建目录：
一是在根路径下创建“_build”目录，二是在根路径下创建“source”
和“build”两个独立的目录。
> 独立的源文件和构建目录（y/n） [n]: y

项目名称将会出现在文档的许多地方。
> 项目名称: Leeks-Docs
> 作者名称: Leeks
> 项目发行版本 []: 0.0.1

如果用英语以外的语言编写文档，
你可以在此按语言代码选择语种。
Sphinx 会把内置文本翻译成相应语言的版本。

支持的语言代码列表见：
http://sphinx-doc.org/config.html#confval-language。
> 项目语种 [en]: zh_CN

创建文件 /Users/yushuai/workspace/tmp/read-the-docs/source/conf.py。
创建文件 /Users/yushuai/workspace/tmp/read-the-docs/source/index.rst。
创建文件 /Users/yushuai/workspace/tmp/read-the-docs/Makefile。
创建文件 /Users/yushuai/workspace/tmp/read-the-docs/make.bat。

完成：已创建初始目录结构。

你现在可以填写主文档文件 /Users/yushuai/workspace/tmp/read-the-docs/source/index.rst 并创建其他文档源文件了。 用 Makefile 构建文档，例如：
 make builder
此处的“builder”是支持的构建器名，比如 html、latex 或 linkcheck。



更改主题 sphinx_rtd_theme
安装主体插件：pip3 install sphinx_rtd_theme
$ vim source/conf.py# 在文件中加入一下信息
import sphinx_rtd_theme
html_theme = "sphinx_rtd_theme"
html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

然后回到docs目录编译文件，注意此时java的虚拟环境一定要启动着
执行后，变得HTML 页面保存在 build/html 目录。
配置一个nginx，访问即可，我的nginx 文件在  /usr/local/nginx/conf/vhosts

## 第4章 配置github托管

### 4.1 本地生成SSH密钥

$ ssh-keygen -t rsa -C “your email address”
[root@wugenqiang ~]# ssh-keygen -t rsa -C "2422676183@qq.com"
results：

### 4.2 添加密钥到GitHub

打开 Github，登录自己的账号后
点击自己的头像->settings->SSH And GPG Keys->New SSH key
将本地 id_rsa.pub 中的内容粘贴到 Key 文本框中，随意输入一个 title(不要有中文)，点击 Add Key 即可

### 4.3 本地测试验证

[root@wugenqiang ~]# ssh git@github.com
表明验证成功
4.4 git常用命令参考
git remote -v/--verbose：显示出详细的url地址名和对应的别名.
git clone <address>：复制代码库到本地；
git add <file> ...：添加文件到代码库中；
git rm <file> ...：删除代码库的文件；
git commit -m <message>：提交更改，在修改了文件以后，使用这个命令提交修改。
git pull：从远程同步代码库到本地。
git push：推送代码到远程代码库。
git branch：查看当前分支。带*是当前分支。
git branch <branch-name>：新建一个分支。
git branch -d <branch-name>：删除一个分支。
git checkout <branch-name>：切换到指定分支。
git log：查看提交记录（即历史的 commit 记录）。
git status：当前修改的状态，是否修改了还没提交，或者那些文件未使用。
git reset <log>：恢复到历史版本。
  
### 4.5 我的操作Git实例

1、远程仓库README.git为空，把本地代码上传到远程仓库
echo "# Test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:aac666/sunyn-docs.git
git push -u origin main
2、更新本地代码到远程仓库
git add README.md
git commit -m "first commit"
git push -u origin main
3、获取远程仓库中的代码到本地
git clone git@github.com:aac666/sunyn-docs.git
4、从远程仓库同步代码更新本地代码
git pull origin main

## 第5章 支持markdown

安装插件：pip install recommonmark
编辑 source/conf.py,我的文件内容如下
$ vim source/conf.py

import sys
import os
import shlex
import sphinx_rtd_theme
import recommonmark
from recommonmark.parser import CommonMarkParser
from recommonmark.transform import AutoStructify
如果扩展（或使用 autodoc 记录的模块）在另一个目录中，请在此处将这些目录添加到 sys.path。 如果目录相对于文档根目录，请使用 os.path.abspath 使其成为绝对路径，如此处所示。
sys.path.insert(0, os.path.abspath('..'))source_suffix = ['.rst', '.md']
project = 'Documentation'copyright = '2022, TenderLeeks'author = 'TenderLeeks'release = '0.0.1'
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',
    'sphinx.ext.mathjax',
    'recommonmark',]
master_doc = 'index'templates_path = ['_templates']version = recommonmark.__version__language = 'zh_CN'exclude_patterns = ['_build']default_role = Nonepygments_style = 'sphinx'todo_include_todos = Falsehtml_theme = "sphinx_rtd_theme"html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]html_static_path = ['_static']htmlhelp_basename = 'Recommonmarkdoc'
latex_elements = {}
latex_documents = [
  (master_doc, 'Recommonmark.tex', u'Recommonmark Documentation',
   u'Lu Zero, Eric Holscher, and contributors', 'manual'),]
man_pages = [
    (master_doc, 'recommonmark', u'Recommonmark Documentation',
     [author], 1)]
texinfo_documents = [
  (master_doc, 'Recommonmark', u'Recommonmark Documentation',
   author, 'Recommonmark', 'One line description of project.',
   'Miscellaneous'),]

def setup(app):
    app.add_config_value('recommonmark_config', {
        'auto_toc_tree_section': 'Contents',
        'enable_math': False,
        'enable_inline_math': False,
        'enable_eval_rst': True,
        'enable_auto_doc_ref': True,
    }, True)
    app.add_transform(AutoStructify)


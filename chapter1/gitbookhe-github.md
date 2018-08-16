# GitBook关联Github {#articleTitle}

* 下载安装GitBook Editor

链接：[http://downloads.editor.gitbook.com/download/windows](http://downloads.editor.gitbook.com/download/windows)

安装后打开，客户端提示登录GitBook帐号。由于我值需要用GitBook Editor做编辑工具，不需要把文章存在GitBook上（根本原因是登录不了，原因你懂）。选Do that later,修改菜单栏GitBook Editor - Change Library Path,改书存储地址

* 在本地创建图书
* 关联GitHub

在GitBook打开新创建的图书，点击`Add an article`随便输入点东西。

注意右上角有两个按钮：`Save`和`Publish`。当点击`Save`的时候，GitBook Editor会把编辑的内容保存在`Library Path`。而当点击`Publish`的时候，就会把编辑的内容保存到Git仓库（可以是任意的Git仓库：GitHub、码云、oschina...）。如果当前这本存储在本地的图书没有关联Git仓库，GitBook Editor会弹出提示：

![](/assets/2360203761-59d26466c2e7b_articlex.png)

那么这时候就需要创建一个Git仓库了。到GitHub创建一个空白的仓库，并复制`https`的git仓库地址。**注意必须使用https的因为GitBook Editor暂时不支持SSH**




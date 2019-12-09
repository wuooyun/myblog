---
title: git中文乱码
date: 2019-12-09 11:38:04
tags:
 - git
categories:
 - git
    
---

### git处理中文名称时候显示为编码形式（已解决）
[转载](https://www.cnblogs.com/yqmcu/p/9920481.html)

#### 问题描述：

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            static/README.md
            "\350\207\252\346\265\213\350\257\225\345\244\247\347\272\262.md"

git status之后，显示为乱码。

#### 解决方法：

这个问题是由于git默认会被utf-8文件名进行转码，需要设置

    **git config --global core.quotepath false**

再次git status 查看文档时候，文件名就正常了。

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            static/README.md
            自测试大纲.md

 还有如果现实文件utf-8文件的中文乱码，需要设置

    **git config --global i18n.commitencoding utf-8  # commit 编码
    
    git config --global i18n.logoutputencoding utf-8 # log输出显示编码**

另外，如果是Linux系统，需要设置环境变量

    **export LESSCHARSET=utf-8**

如果是Windows系统，需要在图形界面设置环境变量：

即，右击**我的电脑**->属性->高级系统设置->环境变量->新建一个变量名为 LESSCHARSET 值为 utf-8 即可。

如果你的中文文件是GBK编码的，上述 utf-8 改成 gbk 即可。

![](https://img2018.cnblogs.com/blog/722295/201811/722295-20181108215041556-1220014142.png)

 

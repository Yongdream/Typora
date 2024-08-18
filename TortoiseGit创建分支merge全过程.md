# 用TortoiseGit高效管理Git分支：多人协作的秘诀



[TOC]

---

## 前言

> 在软件开发的世界里，版本控制是团队协作的基石。Git，作为一种强大的分布式版本控制系统，为多人协作提供了极大的便利。然而，随着团队规模的扩大和项目复杂性的增加，如何高效地管理多个分支成为了一个重要议题。想象一下，你的团队里有好多小伙伴，每个人都在忙活自己的那一块。有的在忙着把新功能加到产品里，有的在测试即将上线的版本，还有的在修复bug。这时候，如果大家的代码都混在一起，那不就乱套了吗？
>
> 本文将探讨在企业环境中，如何通过Git分支策略来管理以及TortoiseGit的可视化操作，确保团队协作的流畅性和项目的稳定性。	

## Git 分支管理概述

在一个典型的企业软件开发项目中，我们通常会遇到以下几种版本：常用的分支包括`master`、`develop`、`feature`、`release`和`hotfix`。这些分支各自扮演着不同的角色，并通过特定的流程相互关联，以确保代码的稳定性和开发效率：

**分支管理流程图示大致如下**：

![分支管理流程](C:\Users\10147\Desktop\Typora\typora_photo\TortoiseGit创建分支merge全过程\f9d68e1a38bc0b3abfb459513bba875e.png)

### 分支功能和检出合并关系

- **`master`**

  - 经过严格测试并发布给最终用户的版本。它代表着项目的稳定状态，所有的正式发布代码都会最终合并到`master`分支中，以确保生产环境的稳定性；

  - 代码只有通过`release`分支发布或通过`hotfix`分支紧急修复后，才能合并到`master`分支，确保进入生产环境的代码已经过充分的验证。

    ```bash
    # 从远程仓库检出 master 分支到本地
    git checkout master
    ```

- **`develop`**

  - 进行日常开发工作的主分支。所有的`feature`分支都会从`develop`分支创建，并在开发完成后合并回`develop`。它汇集了所有未发布的功能和修复，作为下一版本的基础；

  - `develop`分支在功能开发完成后会创建`release`分支，`release`分支中的代码发布后再合并回`develop`，以保持开发主分支与生产环境的同步。

    ```bash
    # 从远程仓库检出 develop 分支到本地
    git checkout develop
    ```

- **`feature`**

  - 开发新功能或修复特定任务的分支。每个`feature`分支都会从`develop`分支创建，独立开发，以确保多个功能可以并行开发而互不干扰；

  - 开发完成后，`feature`分支会合并回`develop`分支，所有功能在集成后进行测试和进一步开发，准备进入发布阶段。

    ```bash
    # 从 develop 分支创建 feature 分支
    git checkout -b feature_<FunctionName> develop
    
    # 完成后合并回 develop
    git checkout develop
    git merge --no-ff feature_<FunctionName>
    
    # 删除 feature 分支
    git branch -d feature_<FunctionName>
    ```

- **`release`**

  - 即将发布到生产环境的版本分支，通常用于进行最后的测试和验证。在这个阶段，只有重要的Bug修复和版本号更新等操作，确保代码的稳定性和可发布性；

  - `release`分支从`develop`分支创建，完成发布准备工作后，代码合并到`master`分支进行正式发布，同时合并回`develop`分支以保持一致。

    ```bash
    # 从 develop 分支创建 release 分支
    git checkout -b release_<versionNumber> develop
    
    # 完成后合并到 master 分支，并打上标签
    git checkout master
    git merge --no-ff release_<versionNumber>
    git tag -a <versionNumber> -m "版本说明"
    
    # 合并到 develop 分支
    git checkout develop
    git merge --no-ff release_<versionNumber>
    
    # 删除 release 分支
    git branch -d release_<versionNumber>
    ```

- **`hotfix`**

  - 修复生产环境中的紧急问题的分支。`hotfix`分支允许开发人员在不影响正在进行的开发工作的情况下，快速修复关键的Bug；

  - `hotfix`分支从`master`分支创建，修复完成后，代码会合并回`master`以确保生产环境的稳定性，并同步合并回`develop`分支以保持开发工作的一致性。

    ```bash
    # 从 master 分支创建 hotfix 分支
    git checkout -b hotfix_<Repair name> master
    
    # 完成后合并到 master 分支，并打上标签
    git checkout master
    git merge --no-ff hotfix_<Repair name>
    git tag -a 修复名称 -m "修复说明"
    
    # 合并到 develop 分支
    git checkout develop
    git merge --no-ff hotfix_<Repair name>
    
    # 删除 hotfix 分支
    git branch -d hotfix_<Repair name>
    ```

了解了Git中各个分支的功能和相互关系后，掌握如何高效管理这些分支对于确保项目的稳定性和开发效率至关重要。虽然命令行工具是操作Git的基本方式，但对于那些更喜欢图形化界面的开发者来说，TortoiseGit是一个非常有用的工具。

## TortoiseGit：简化Git操作的利器

**TortoiseGit**（即小乌龟）可以为您提供一种将Git的强大功能与图形用户界面相结合的便捷工具。作为Windows文件资源管理器的插件，TortoiseGit使得即使是不熟悉命令行的开发者也能够轻松地进行版本控制。它提供了一种直观的方式来处理Git仓库，包括但不限于创建分支、合并代码、解决冲突等操作。使用TortoiseGit，您可以更加高效地管理和维护您的项目代码。

安装教程：ref 

## TortoiseGit 操作

### 创建分支

分支管理是Git中最强大的特性之一，它允许开发人员在隔离的环境中工作，而不会影响主分支的稳定性。

### 切换分支



### 提交内容

![image-20240818022020457](C:\Users\10147\Desktop\Typora\typora_photo\TortoiseGit创建分支merge全过程\image-20240818022020457.png)

### 推送内容

### 平台 pull request & merge

## 其他问题

[[TortoiseGit图标不显示 及 文件夹中.git文件夹不显示]](https://blog.csdn.net/Aquamarine__/article/details/134268050?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-134268050-blog-84639445.235%5Ev43%5Epc_blog_bottom_relevance_base2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-134268050-blog-84639445.235%5Ev43%5Epc_blog_bottom_relevance_base2&utm_relevant_index=5)

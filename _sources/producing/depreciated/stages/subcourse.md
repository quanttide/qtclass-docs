# 子课程

## 方案选择

这里的管理难点是，虽然可以使用敏捷的概念规范，通过频繁发小版本和预发布版本，最大限度刺激课程研发进程。
但上线必须要整个课程全部完善，而我们的每个课程体系都比较庞大，这样会严重影响上线进程。

目前除了拆分子课程上线以外，暂无其他理想的方案。Git拆分子项目有两种方案，submodule和subtree，其中：

- submodule是引用子仓库，引用另外一个仓库作为这个项目的子仓库。
- [subtree](https://www.atlassian.com/git/tutorials/git-subtree)是拷贝子仓库，子项目的提交直接在父仓库，看起来像直接在操作父仓库，不需要额外的配置。

对于submodule和subtree的对比，详见：https://gb.yekai.net/concepts/subtree-vs-submodule。

由于我们常见的流程是大课程在研发过程中逐步拆分成小课程，因此subtree的模式更适合这种情况，且Git官方也更推荐使用subtree代替submodule。

综上所述，我们使用**subtree**解决此问题。

## 方案设计

为了方便集中管理，我们不希望像其他使用subtree的情况一样为子项目创建新仓库。因此我们只需要在主分支和子项目分支之间操作，需要用到：

- `git subtree split`可以把父项目拆分为子项目，创建或更新都可以。
- `git subtree merge`可以把子项目合并到父项目。不过实际上有Bug，用`git merge -s subtree`代替。

我们这里的实践需求不同于大部分使用subtree的场景，需要集中维护仓库、但分别发布每个部分，和正常的实践正好相反。正常的实践通常是开发子项目以后集成到父项目。我们这里的情况更加类似于在GitHub仓库的`master`分支上拆分`gh-pages`分支以部署网页到GitHub Pages。

我们使用[GitLab Flow的release分支管理策略](https://docs.gitlab.com/ee/topics/gitlab_flow.html#release-branches-with-gitlab-flow)，把要发布的子课程定义为subtree子项目、并且定义子项目分支为为release类分支，使用`release/<child_project>`的方式命名以方便触发CI。基于此思路，我们的研发流程定义为：

1. 在feature分支上更新并提交到`master`分支。
2. `master`分支根据需要（更新时或发版本时，目前是前者）触发CI拆分子项目到`release/<child_project>`分支。
3. 根据需要（子项目发版本或更新，目前计划前者）触发CI发布子项目到课程平台。

CI构建计划则定义了拆分方式，具体表现为`<folder>`拆分到`<branch>`。由课程研发负责人（代码仓库的维护者）维护和制定规则，根据课程实际结构决定，一般根据chapter或者part拆分，优先使用chapter拆分。

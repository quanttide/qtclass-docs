由于本地组织教程使用GitBook，可以使用GitBook为工具搭建DevOps流程。

# GitBook的原始流程

1. 创建项目。可以用`gitbook init`初始化项目。GitBook项目里包含一个SUMMARY.md文件为做书的目录，很多关键逻辑都围绕它展开。
2. 编译。运行`gitbook build`命令，可以生成静态站点。
3. 运行GitBook服务器。运行`gitbook serve`命令，可以运行静态站点。

# GitBook的DevOps流程

写作过程管理使用Git即可，编译用CI流程进一步自动化，输出用CD流程进一步自动化。

## Git版本管理

以GitHub Flow管理。其中，master版本用以发布到内部平台，release版本发布到课堂App。release则必须要根据以课程为单位组织，则拆分大项目为小项目很有必要。此外，大项目也不利于安全管理（i.e. 不支持在仓库级别设置白名单模式），容易被一个人拖库泄露。

计划以演进式方案拆分：
1. 已经有明确研发方案的，从父仓库里逐步拆分为独立项目管理，并且构建专用DevOps模板用以发布。
2. 考虑做一个索引项目。

备注：
- Git子项目主要管理多个父仓库使用同一个子库的情况，适合处理公共依赖，而不是本场景，还是考虑手动拆分。

## CI流程

gitbook是一个Node.js仓库，因此制作CI流程需要：
1. 在CI环境中下载Node.js。`node -v`检测。
2. 下载gitbook：`npm install gitbook -g`下载，`gitbook -V`检测。
3. 编译：`gitbook build`。

对于CI流程的疑惑主要是：
- 如何在CI环境里下载Node.js环境。详见参考资料。
- 目前使用一个大项目管理所有GitBook项目，是否需要为每个项目单独做CICD流程、还是使用Git子模块拆分管理、还是两者结合，还需要进一步明确方案。

## CD流程

需要一个静态网站托管或者云托管服务。从教程看，是可以直接以静态网站的形式部署的。
1. 把编译好的_book文件夹上传到对应目录；
2. 启动或重启静态网站服务。

可能考虑编译成PDF。步骤如下：
1. 下载gitbook-pdf依赖。
2. 运行`gitbook pdf`打包。
3. 把PDF文件上传到制品库。
4. 从制品库传到其他需要的地方。

## 访问权限控制

参考此方案：https://cloud.tencent.com/developer/article/1748227。

把静态网站数据从后端加载以后，修改.innerHTML属性加载，详见 https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML。

权限控制通过前端代码实现即可。

TODO：尚需确定是否可以用Flutter for Web实现此类操作。
 

# 部署方案

部署方案有如下选择：
1. Serverless静态网站托管。优点是直接在Coding中可以做，缺点是不方便整合到同一个云开发环境。
2. 云开发静态网站托管。优点是在同一个环境、最容易让后期管理简单，缺点是框架本身有缺点不好管理。
3. 云开发云托管。优点是好做CICD流程，缺点是没有特别为静态网站优化。

部署方案需要的新知识：
- 云开发：需要使用CloudBase Framework，同样是一个Node.js库；
- Serverless：Follow Coding官方方案。

## 云开发静态网站

### 部署原理

cloudbase命令行工具可以实现CICD流程。

### 运行原理

在根目录和每个子目录下检索index.html。

GitBook每个文件夹都有index.html文件，对于每本书没有问题。

难点在于生成项目里的GitBook书的目录并用一个index.html索引。考虑用README转首页，用SUMMARY转目录。

# 参考资料

- GitBook使用教程：https://tonydeng.github.io/gitbook-zh/gitbook-howtouse/。
- node.js在Jenkins中使用：https://www.jenkins.io/zh/doc/tutorials/build-a-node-js-and-react-app-with-npm/

# 敏捷事项

参考[Coding官方文档](https://help.coding.net/docs/collaboration/pattern/scrum/intro.html)进一步细化。

## 史诗

定义为某个教程的某一章或者某一节，通常需要数个迭代逐步成型。比如，“云计算教程-函数计算入门”，对应云计算教程的9.1节。考虑到节的编号经常变化，因此不加入史诗中，通过史诗排序体现教程中的排序即可。

建立在清晰的教程目录的基础上，教程目录的相关事宜放在史诗“<xx>教程-整体结构”。通常排列在本教程相关史诗的最前面。

完整史诗列表详见[史诗列表](https://quanttide.coding.net/p/qtclass-courses/epics/issues)。

## 迭代

建议以某个项目的实现目标为迭代目标，围绕迭代目标组织多个教程、多个章节的不同讲的写作。

比如，[腾讯云SDK的云函数处理大文件用例项目](https://quanttide.coding.net/p/qtopen-python/d/qcloud-sdk-py/git/tree/master/examples/scf_large_file)有一段获取存储桶视频并选取最大的一批视频测试下载速度的代码。

```python
selected_tasks = sorted(tasks, key=lambda item: item['size'], reverse=True)[0:20]
```

以解决此问题为迭代目标规划迭代，我们可以得到：

- Python教程 > 数据结构章 > 字典节，增加一讲或者半讲，使用sorted的key参数、指定文件大小做排序。
- Python教程 > 文件和文件系统章 > 文件，文件属性相关讲增加内容，讲解文件大小的定义，特别是字节bytes等。
- Python教程 > 网络应用章 > HTTP协议，增加关于content-length的说明。
- 云计算教程 > 存储服务 > 对象存储，增加GET Bucket（List Object）API的说明。

## 需求和任务

需求和任务的主要区别是：

- 需求比较模糊，需要经过评审（从未评审区到已评审区），完成以后需要验证和再评审（“测试中”状态）。
- 任务不需要协同，结果比较明确，可以单人完成。

子工作项由事项负责人自己维护，不纳入团队管理范畴。

暂不使用“用户故事”事项类型，仅在产品研发项目中使用。

主要使用事项评论区收集意见，通过事项描述整理汇总形成已评审结论，以可执行、有明确价值为完成评审的标准。

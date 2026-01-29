参考资料：

- mod本体： https://steamcommunity.com/sharedfiles/filedetails/?id=3551203752
- 这个帖子挖掘了talk的代码实现部分：https://tieba.baidu.com/p/10385060258#
- 泰南官方世界观翻译： https://tieba.baidu.com/p/7363308046#
- talk使用了Scriban语法的模板引擎，这是Scriban语法中文翻译： https://www.cnblogs.com/mondol/p/18059560/scriban
- 贴吧里scriban在talk中实现参考代码挖掘： https://tieba.baidu.com/p/10408667645?pid=153105380480&cid=#153105380480

# rimtalk是什么？

rimtalk是一个rimworld（边缘世界）的mod，，他可以让小人使用ai生成的内容来对话让你的小人看起来栩栩如生？（看起来罢了）

# 前言

当我回顾我整个玩ai和酒馆的历程发现酒馆和ai在我眼中已经没有了当初的神秘迷雾笼罩，因此距离不再能产生美，这期间我已经很少打开酒馆，大部分时间都是在瓜区水聊天，哪怕是前一段时间我打开酒馆游玩发现gemini-3pro的八股文依旧没有任何改观。
近期talk更新了所谓的高级预设功能，我认为是时候该写一个talk的预设玩玩了，不然我非常无聊。

> 吐槽：在贴吧看这位雨曦凌杳的文档简直就是痛苦，特别是没有md渲染还要看ai的一堆加粗\*符号，分享内容是放百度网盘，文件保存的编码具有gbk，如果能放github就更好了，不过还是要感谢他，不然c#的我看不懂一点

# 我需要什么？

- 专注于对话本身的预设
- 不需要考虑外星人
- 目前暂时不考虑用围绕talk的各种附属mod
- 尽可能简单？


暂时先这样吧

> 不过我认为这个：[RimTalk新版预设，简洁省Token版 ](https://tieba.baidu.com/p/10410868161#) 比我的预设优秀多了，一堆代码判断

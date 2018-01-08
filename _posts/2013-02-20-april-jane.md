---
title: April Jane上线
date: 2013-02-20 15:59
author: Ley
category: 小学作文
slug: april-jane
tags: pelican, 前端
---
![screen shot of april jane, the theme](/images/april-jane.png)前段时间默默地把博客的主题上线了，这个主题是我给自己布置的寒假作业之一，也是我唯一勉强算完成了的寒假作业。

这个主题是一个简单的二栏的主题，其实这是一种很没有特点的页面布局方式，一开始我也没有定下来会用什么样的布局，写着写着就这样子了，我也不确定以后会不会修改布局，或许哪天我心血来潮，整个布局就变了。

写主题的时候参考了[not my idea][not-my-idea]的布局，然后动手写的时候又学习了一下html5的知识，算是把这个主题html5化了。因为写这个主题的时候其实我上网很不方便，于是就按自己的理解写了一个tagcloud，感觉很是简陋，以后估计还会改的。

最佳的显示效果应该是在OSX下的Chrome，因为挺喜欢Helvetica这个字体，OSX下默认的Helvetica英文字体看起来很舒服，黑体也比微软雅黑看起来舒服。回头有时间的话，找找有没有什么开源的英文字体，可以至少统一了网站的英文字体风格。中文字体我就没有办法了。

还有一些bug，比如说是line-height设置了以后的行高还是不符合预期，因为我想用left-border做一下标题左边的提示符，如果显示有误会很难看。这个也就在Mac下的Chrome达到了我的预期，在不同的Win7上问题还表现得不一样。其它一些bug是我忘了一些css的hack~回头这些bug都得修复一下。

做的时候是从无CSS一步步改过来的，因为缺少必要的工具，也不好查色彩搭配的资料，整体的视觉风格以灰色为主，做完匆忙上线以后，看了几次觉得页面显得太紧凑了。这些都是后面要改的非bug问题，我希望能最后的成品是一个黄绿配色的，看起来没这么紧凑的主题。

另外，我在banner上放了一张图片，准备是放一些自己拍的照片切换的，但是整体视觉风格，感觉不是灰常搭调。艺术细胞还是比较欠缺啊。不过如果没有图片整体又太单调。这个不知道如何取舍。

本主题使用到的其他的开源项目有：

* [normalize.css v2.1.0][normalize-css]: 统一各浏览器的视觉风格
* [autumn.css][autumn-css]: pygment的代码高亮主题


***最后***，这个主题为什么要叫 *April Jane* 呢，因为这个主题是我在**某个四月**下决心要开始做的——那时我还在用WordPress，还挺熟悉前端的一些东东。而Jane呢，是我的那个**她**。

[not-my-idea]: http://coding.smashingmagazine.com/2009/08/04/designing-a-html-5-layout-from-scratch/ "notmyidea参考文章"
[normalize-css]: http://necolas.github.com/normalize.css/ "normalize.css"
[autumn-css]: https://github.com/richleland/pygments-css "css files created from pygment's built-in styles"

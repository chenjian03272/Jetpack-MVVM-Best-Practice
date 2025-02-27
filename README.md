![](https://images.xiaozhuanlan.com/photo/2021/b106fd65d34a4a724244e7c5b42a2372.jpg)

[《重学安卓》](https://xiaozhuanlan.com/kunminx)付费读者加微信进群：myatejx

> [免费试读](https://mp.weixin.qq.com/s/h2IvuIRWe0OLx3hgbTABYA)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)

&nbsp;

# 版权声明

我们就本项目 "被卖课" 一事，在掘金发表了一期专访 [《开源项目被人拿去做课程卖了 1000 多万是什么体验》](https://juejin.im/post/5ecb4950518825431a669897)

本项目系我为了方便开发者们 **无痛理解 Google 开源的 Jetpack MVVM 中每个架构组件的 存在缘由、职责边界**，而 **精心设计的一个又一个高频应用场景**，

与此同时，本项目是作为 [《重学安卓》](https://xiaozhuanlan.com/topic/6017825943)专栏 Jetpack MVVM 系列文章的配套项目而存在，**文章内容和项目中的代码设计均涉及本人对 Jetpack MVVM 的独家理解，本人对此享有著作权**。

任何组织或个人，未经与作者本人沟通，不得将本项目的代码设计和本人对 Jetpack MVVM 的独家理解用于 "**打包贩卖、引流、出书 和 卖课**" 等商业用途。

&nbsp;

# 架构图一览

![](https://images.xiaozhuanlan.com/photo/2021/1fc3f3cefb1d8e6755dea55ac828874d.png)

&nbsp;

# 前言

上周我在各大 “技术社区” 发表了一篇 [《Jetpack MVVM 精讲》](https://juejin.im/post/5dafc49b6fb9a04e17209922)，原以为在 “知识网红” 唱衰 Android 的 2019 会无人问津，没想到文章一经发布，从 “国内知名公司” 的架构师、技术经理，到 “世界级公司” 的 Android 开发都在看。

并且从读者的反馈来看，近期大部分 Android 开发已跳出舒适圈，开始尝试认识和应用 Jetpack MVVM 到实际项目中。

只可惜，关于 Jetpack MVVM，网上多是 **东拼西凑、人云亦云、通篇贴代码** 的文章，这不仅不能提供完整的视角 来帮助读者 首先明确背景状况，更是给还没入门 Jetpack 的读者 **徒添困扰**、起到 **劝退** 的作用。

好消息是，这一期，我们带着 **精心打磨的 Jetpack MVVM 最佳实践案例** 来了！

&nbsp;
&nbsp;


|                   是让人爱不释手的交互设计                   |                       是连贯的用户体验                       |                     唯一可信源的统一分发                     |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![1231111323.gif](https://upload-images.jianshu.io/upload_images/57036-0a5cdc68f003211a.gif) | ![222.gif](https://upload-images.jianshu.io/upload_images/57036-2b21db531e51ff03.gif) | ![333.gif](https://upload-images.jianshu.io/upload_images/57036-9a541148ce5bed2e.gif) |



|                  横竖屏布局的无缝切换                  |
| :----------------------------------------------------: |
| ![](https://i.loli.net/2021/08/25/X9rado7AfnCEgv3.gif) |

&nbsp;
&nbsp;

# 项目简介

本人拥有 3 年的 “移动端业务架构” 践行和设计经验，领导或参与团队重构的 中大型项目 多达十数个，对 Jetpack MVVM 架构在 确立规范化、标准化 开发模式 以 **减少不可预期的错误** 所作的努力，有着深入的理解。



在这个案例中，我将为你展示，Jetpack MVVM 是如何 **以简驭繁** 地 将原本十分容易出错、一出错就会耽搁半天时间的开发工作，通过 寥寥的几行代码 轻而易举地完成。

> 👆👆👆 划重点！

&nbsp;

在这个项目中，

> 我们为 **横、竖屏** 的情况 分别安排了两套 **截然不同的布局**，并且在 [生命周期](https://xiaozhuanlan.com/topic/0213584967)、[重建机制](https://xiaozhuanlan.com/topic/7692814530)、[状态管理](https://xiaozhuanlan.com/topic/7692814530)、[DataBinding](https://xiaozhuanlan.com/topic/9816742350)、[ViewModel](https://xiaozhuanlan.com/topic/6257931840)、[LiveData](https://xiaozhuanlan.com/topic/0168753249) 、[Navigation](https://xiaozhuanlan.com/topic/5860149732) 等知识点的帮助下，通过寥寥几行代码，轻松做到 **在横竖屏两种布局间 无缝地切换，并且不产生任何 预期外的错误**。


> 我们在多个 Fragment 页面 分别安排了 **播放状态 指示器**（包括 播放暂停按钮状态、播放列表当前索引指示 等），并向你展示了 如何 以及为何 通过 [LiveData](https://xiaozhuanlan.com/topic/0168753249) **配合** 作为唯一可信源 的 [ViewModel](https://xiaozhuanlan.com/topic/6257931840) 或单例，来实现 **全应用范围内 可追溯事件 的统一分发**。


> 我们在 Fragment 和 Activity 之间分别安排了 跨页面通信，从而向你展示 如何基于 **迪米特原则**（也称 最少知道原则）、通过 UnPeekLiveData 和 应用级 SharedViewModel 来实现 **生命周期安全的、确保消息同步一致性和可靠性的 页面通信**（事件回调）。


> 我们在 `ui.page` 、`data.repository`、`domain.request` 等目录下，分别安排了 视图控制器、[ViewModel](https://xiaozhuanlan.com/topic/6257931840) 、DataRepository 等 内容，从而向你展示，**单向依赖** 的架构设计，是如何通过分层的 数据请求和响应，来 **规避 内存泄漏** 等问题。


> 本项目的代码一律采用 经过 ISO 认证的 标准化工业级语言 Java 来编写。并且，在上述目录 所包含的 类中，我们大都 **提供了丰富的注释**，来帮助你理解 骨架代码 为何要如此设计、如此设计能够 **在软件工程的背景下** 避免哪些不可预期的错误。

&nbsp;
&nbsp;

除了 **在 "以简驭繁" 的代码中 掌握 MVVM 最佳实践**，你还可以 从这个开源项目中 获得的内容 包括：

1. 整洁的代码风格 和 标准的资源命名规范。
2. 对 视图控制器 知识点的 深入理解 和 正确使用。
3. AndroidX 和 Material Design 2 的全面使用。
4. ConstraintLayout 约束布局的最佳实践。
5. **优秀的 用户体验 和 交互设计**。
6. 绝不使用 Dagger，绝不使用奇技淫巧、编写艰深晦涩的代码。
7. The one more thing is：

即日起，可在 "应用商店" 下载体验！

[![google-play1.png](https://upload-images.jianshu.io/upload_images/57036-f9dbd7810d38ae95.png)](https://www.coolapk.com/apk/247826) [![coolapk1.png](https://upload-images.jianshu.io/upload_images/57036-6cf24d0c9efe8362.png)](https://www.coolapk.com/apk/247826)


&nbsp;
&nbsp;

# Thanks to

[AndroidX](https://developer.android.google.cn/jetpack/androidx)

[Jetpack](https://developer.android.google.cn/jetpack/)

[material-components-android](https://github.com/material-components/material-components-android)

[轻听](https://play.google.com/store/apps/details?id=com.tencent.qqmusiclocalplayer)

[AndroidSlidingUpPanel](https://github.com/umano/AndroidSlidingUpPanel)

项目中使用的 图片素材 来自 [UnSplash](https://unsplash.com/) 提供的 **免费授权图片**。

项目中使用的 音频素材 来自 [BenSound](https://www.bensound.com/) 提供的 **免费授权音乐**。

&nbsp;
&nbsp;

# Who is using

感谢小伙伴们对 “开源库使用情况” 匿名调查问卷的参与，截至 2021年4月25日，通过小伙伴的私下反馈和调查问卷，我们了解到

包括 “腾讯音乐、BMW、TCL” 在内的诸多知名厂商的软件，都参考过我们开源的此架构模式，以及正在使用我们维护的 [UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData) 等框架。

目前我们已将具体的统计数据更新到 相关的开源库 ReadMe 中，错过本次问卷调查的小伙伴也不用担心，我们继续对此保持开放，不定期将小伙伴们登记的公司和产品更新到表格，

以便吸引到更多的小伙伴 参与到对这些架构组件的 使用、反馈，集众人之所长，让架构组件得以不断演化和升级。

https://wj.qq.com/s2/8362688/124a/

| 集团 / 公司 / 品牌 / 团队                             | 产品               |
| ----------------------------------------------------- | ------------------ |
| 腾讯音乐                                              | QQ 音乐            |
| TCL                                                   | 内置应用，暂时保密 |
| 贵州广电网络                                          | 乐播播             |
| 福建树叶网络科技有限公司<br/>福建天奖网络科技有限公司 | 天奖谱林           |
|                                                       | 小辣椒             |
| BMW                                                   | Speech             |
| 上海互教信息有限公司                                  | 知心慧学教师       |
| 美术宝                                                | 弹唱宝             |
|                                                       | 网安               |
| 字节跳动直播                                          | 直播 SDK           |
| 一加手机                                              | OPNote             |

&nbsp;
&nbsp;


# My Pages

Email：[kunminx@gmail.com](mailto:kunminx@gmail.com)

Home：[KunMinX 的个人博客](https://www.kunminx.com/)

Juejin：[KunMinX 在掘金](https://juejin.im/user/58ab0de9ac502e006975d757/posts)

[《重学安卓》 专栏](https://xiaozhuanlan.com/kunminx)

付费读者加微信进群：myatejx

[![重学安卓小专栏](https://images.xiaozhuanlan.com/photo/2021/d493a54a32e38e7fbcfa68d424ebfd1e.png)](https://xiaozhuanlan.com/kunminx)

&nbsp;
&nbsp;

# License

```
Copyright 2019-present KunMinX

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

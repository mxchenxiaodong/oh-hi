---
layout: center
download: true
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# colorSchema: 'dark'
# some information about the slides, markdown enabled
info: |
  ## Ruby 培训资料

---

## <openmoji-zany-face style='display: inline' />这次讲什么呢？
这次来分享一个不一样的监控系统？

叫做 Grafana

---

## <openmoji-face-with-monocle style='display: inline' />先来看一看整体的效果？

看图说话，一看就懂。

<v-clicks>

* phone-cloud的 pod 情况
* redis
* sidekiq / puma

</v-clicks>

---

## <openmoji-person-cartwheeling-medium-light-skin-tone style='display: inline' /> 看到这些图表，大家觉得怎么样？

<br><br>

<div v-click>

这就体现了 Grafana 的一大特点：
* 有丰富的图表供展示。

</div>

---

## <openmoji-face-with-head-bandage style='display: inline' />这些仪表盘的数据是怎么来的？

<v-clicks>

我们这里的数据源，是从 Promethues 来的。

</v-clicks>

<br><br>

<v-clicks>

是不是有小伙伴还没听过 Promethues ？ 那 Promethues 是什么？
- 是一个监控系统，带有一个时序数据库，集成了报警的功能。

</v-clicks>

<br><br>

<v-clicks>

你觉得时序数据库是啥？

- 存一些带有时间戳的数据。传统的关系型数据库功能过剩。
- 针对这种时间类型的数据，用更适合的技术来实现。

</v-clicks>

---

这就又体现 Grafana 的另一大特点：
* 可以支持 Promethues。
* 但不单单只有 可以支持 Promethues，还支持其他许许多多的数据源。

---


## <openmoji-hundred-points style='display: inline' /> 那么看到这里，Grafana 是什么？

<v-clicks>

Grafana 从【数据源】读取数据 + 仪表盘展示。

</v-clicks>

---

## <openmoji-incredulous-face style='display: inline' /> 那真正的数据从哪里来？

<v-clicks>

<img src="/pull.png" class="m-26 h-40 rounded shadow" />

- 通过拉取 pull 的方式。
- 从服务的 api 中获取指标。

</v-clicks>

---

## <openmoji-person-climbing-light-skin-tone style='display: inline' /> 继续来看几个例子:

<v-clicks>

- puma 、sidekiq （采集业务指标的）
- redis （采集服务指标的）

而这个提供指标的东西，就叫做 Exporter 。

</v-clicks>

<br><br><br>

<v-clicks>

互动：推、拉模型的区别?

比如：消息的推拉、时间线的推拉等等。

</v-clicks>

---

## <openmoji-man-biking style='display: inline' /> 再总结一下

整体的流程是：
Exporter <— Promethues <— Grafana 


其中 Grafana 主要做的，就是：
* 自定义仪表盘
* 支持多种数据源

---

## <openmoji-man-juggling-light-skin-tone style='display: inline' /> 一些更加具体的可以看一下

- [概述 - Prometheus 入门到实战](https://p8s.io/docs/basic/overview/)
- [Prometheus简介 - prometheus-book](https://yunlzheng.gitbook.io/prometheus-book/parti-prometheus-ji-chu/quickstart/why-monitor)

<br><br>

> 这2本书都不错，可以相互结合起来看。
>
> 还有一本 《Prometheus监控实战》，在微信读书上有，也是挺不错的，特别是前面的关于监控的理论部分。

---

## <openmoji-person-with-dog style='display: inline' /> 鸡汤一下

最近新东方的直播，不知道大家有没有看过，其中有一个叫董宇辉的主播讲过这么一句话。

“听所有的意见，然后有自己的判断”。

拓展一下思路。说不定以后就用上了呢？

---

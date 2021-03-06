---
layout: article
title: 性能监控 - Datadog
---

[原文](https://tech.glowing.com/cn/performance-monitoring-with-datadog/)


这是一篇解释使用datadog的文章，摘录非常赞同的最后一部分文字

___


生产系统的性能监控并不仅仅是为了保证系统的稳定性，也是为了可以持续地改进整体的系统架构。性能调优是任何一个对技术有热情的团队都愿意做的事情。


***但很多时候一些技术人员虽热衷于尝试新的技术与想法，但却缺少一种“一切以数据说话”的态度。***

***我的观点是，没有明确的量化指标，性能优化就无从谈起。***

***也不要轻易地去拿一些网上的benchmark来说话，因为真实的生产环境复杂得多。***


这篇文章虽然是对Datadog这个工具的介绍，但我想传达是一种对生产环境的真实性能做全方位量化的态度。从每个API call所用的时间，到各个层级缓存的使用频率与命中率，再到数据库每张的读写频率与响应时间，这些数据都需要收集，不但要收集，更需要有效地可视化，让技术团队方便实时地分析这些数据，并基于数据来决定未来架构的发展。

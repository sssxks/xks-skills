---
name: build-dashboard
description: Apply when building an dashboard UI
---

use heroui library or other applicable UI library to build dashboard. 

尽量不要自作聪明地自定义样式！尽量不要手写组件！use existing token！用组件的时候文档查清楚！

no filler copy or caption. minimize the amount of non-interactive components.

font size should be readable. no tiny font.

卡片不是用来展示数据行的，老实用表格！卡片也不是用来做页面布局的！

自己参考卡片的示例看一下卡片什么时候用是正确的！https://heroui.com/en/docs/react/components/card

prefer icon buttons(text in tooltip), or both icon and text over pure text buttons.

present or visualize data in the same way we model it in the backend. 

do not hardcode visualization, analysis, or even conclusion & limitation.

骨架屏！整个加载的过程，布局都得是一致的！骨架屏必须和实际布局匹配！cache上一次每行的高度！

h1、h2都不能用！分组用h3，或者切surface
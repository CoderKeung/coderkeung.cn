+++
title = "CSS 布局学习"
date = 2021-10-28T20:08:20+08:00
draft = false
indent = false
initial = false
toc = false
+++

## flexBox「弹性盒子」

弹性盒子有且仅有两层，外层父元素叫做 `flex 容器`，内层子元素叫做 `flex 项` 。

弹性盒子有且仅有两条轴线：`主轴`  和 `交叉轴` 。

[弹性盒子 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Flexbox)

`主轴`  为基础轴线，即排列方向（横、纵）；`交叉轴` 依主轴而定，垂直于主轴。

- **flex 容器** 所支持的 css 参数

    `display: flex;`（主要参数，设置弹性盒子）

    `flex-direction: column;`  （确定主轴和交叉轴）

    `flex-flow: row wrap;` （上面两个参数的合写）

    `flex-wrap: wrap;`  （俗称「换行」）

    `align-items: center;` （以主轴为基准进行排列）

    `justify-content: center;` （以交叉轴为基准进行排列）

- **flex 项** 所支持的 css 参数

    `flex: 1;` （确定子元素空间分配方案，除去元素本身空间外剩余空间的分配）

    `order: 1;` （确定子元素排列顺序，值越小越靠前，默认 0 ，可为负数）


## gireBox「网格盒子」

网格布局外层叫做 `grid 容器` ，内层叫做 `grid 项` 。与 `flex 盒子` 有异曲同工之妙。

网格布局分成两种 —— `显式网格` 和 `隐式网格` 。

`显示网格` 是指用 `grid-template-columns` 或者 `grid-template-rows` 两个属性定义的网格内容。`隐式网格` 是指 `显式网格` 没有定义的。「显示如果使用列式（ `grid-template-columns` ）那么相应的 `隐式网格` 就是 `row` 相关的设置，反之亦然。」

[网格 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Grids)

- **grid 容器** 所支持的 css 参数

    `display: grid;` （主要参数，使用网格布局）

    `grid-template-columns: 1fr 1rf;` `grid-template-rows: 1fr 1fr;` （显式网格 设置）

    `grid-auto-rows: 100px;` `grid-auto-columns: 100px;` （隐式网格 设置）

    `grid-gap: 10px;` `gap: 10px;` （网格之间间隙「排水槽」）

    `grid-template-areas: "one one" "two three""four four";` （网格区域设置）

    `minmax(minValue, maxValue);` （固定最小尺寸和最大尺寸的函数）

    `repeat();` （重复构建网格行列函数）

- **grid 项** 所支持的 css 参数
    - `grid-column: 1 / 3;` `grid-row: 1 / 3;` （确定子元素开始线和结束线条）

        在这里，开始线和结束线总数比网格数多 1 。


    `grid-area: one;` （给每个子元素命名，用于 `grid-template-areas` 的使用）


## float 「浮动」

[浮动 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Floats)

浮动针对的是单个元素，某个元素设置为浮动其他元素将围绕其做浮动。

- **浮动** 所支持的 css 参数

    `float: left;` （为某个元素设置为 浮动）

    `clear: both;` （为其他元素设置为清除 浮动）


## position 「定位」

- `position: relative;` （相对定位，以原文档流为基础）
- `position: absolute;` （绝对定位，以 `<html>` 元素或者最近父元素定位为基础）
- `position: fixed;` （固定定位，以浏览器窗口为基础）
- `position: sticky;` （粘性定位，当某元素达到定位要求开始定位）

[定位 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Positioning)

`z-index: -1;` （对元素 z 轴的确定，值越大越上）

## table & multicol「表格与多列」

[多列布局 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Multiple-column_Layout)

设置为多列布局的元素被叫做 `multicol 容器` 。

- **multicol 容器** 所支持的 css 属性

    `column-count: 12;` （设置所要拆分的列数）

    `column-width: 200px;` （设置所要拆分的列的宽度）

    `column-gap: 10px;`（设置每列之间的间隙）

    `column-rule: 4px dotted red;` （设置每列之间的分割线）

- **multicol 容器** 下层子元素所支持的 css 属性

    `break-inside: avoid;` `page-break-inside: avoid;` （不破坏子元素盒子）


> 表格布局现在用的不多，问题还挺大，就不学了……下次有机会在搞 🤪  。

---
title: flex
categories: web
tags: [css, flex]
---

## 容器与项目（容器成员）

### flex 容器

设置为flex容器： `display:flex;`

**属性**

1. flex-direction 主轴方向

	* row
	* row-reverse
	* column
	* column-reverse


2. flex-warp 发生换行时的堆叠方向

	* nowarp 不换行，会挤压item主轴方向
	* warp 换行，不会挤压item主辆方向，而是挤压交叉轴。
	* warp-reverse,同上，从下往上排。
	
3. flex-flow flex-direction及flex-warp的缩写。

	<flex-direction> <flex-warp>|initial|inherit

4. justify-content 主轴对齐方向

	* flex-start
	* flex-end
	* center
	* space-between 平散对齐
	* space-around 每侧相隔相等

	
5. align-items 交叉轴对齐方式

	* flex-start
	* flex-end
	* center
	* baseline
	* stretch

6. align-content
	

### flex容器内的元素则为flex项目。


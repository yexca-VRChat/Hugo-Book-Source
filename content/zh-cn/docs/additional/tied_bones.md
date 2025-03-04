---
title: 物体绑骨
weight: 1
slug: /additional/tied_bones
---

{{% hint info %}}
**物体绑骨**  
让物体随着我移动
{{% /hint %}}

## 简述

物品绑骨可以让物品随骨骼运动，例如眼镜绑定头部骨骼，剑绑定手部骨骼

## 一、放在对应骨骼下方

在左方找到模型相应的骨骼，然后将物品拖进去即可，可调整位置

例如将剑拖入手部骨骼下方，然后更改位置是剑柄放在手上

**注意**：如果物品有骨骼，需要将物品骨骼放入模型骨骼(想要邦骨的位置，如剑的骨骼放手上)，其他的放入模型子集，不然模型骨骼会出错

## 二、使用约束

点击要邦骨的物体，然后在“检查器”里“添加组件”，搜索“Parent Constraint”并添加

可以先将物品移动到对应的位置(例如将剑柄移动至手上)

在下方“源”，点击加号，然后将想绑定的骨骼拖入(例如手部骨骼)

然后在“检查器”里点击激活即可

---
title: Animator Layers
weight: 11
slug: /Summary/Layers
---

{{% hint info %}}
**Animator Layers**  
Unity 图层的机制，模型是如何被修改的呢？
{{% /hint %}}

不同图层的状态可以同时发生，同一图层同时只能执行一个状态

## 图层

点击 `+` 可以添加一个图层，点击图层旁边的齿轮图标，会弹出一个小窗口，可以设置该图层对应的参数

| 属性         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| Weight(权重) | 这一层的权重，0代表该层权重是0（该层不生效），1代表该层权重为1（该层中的动画能完全表现） |
| Mask(遮罩)   | 此层上使用的遮罩，可以限制动画的播放部位，例如只要播放上半身的动画，可以使用一个遮罩定义在上半身播放动画。设置后该图层上面会显示一个`M`的小图标 |
| Blending     | 和其他层混合的模式                                           |
| *Override*   | 覆盖上面的层中对应Mask的部位                                 |
| *Additive*   | 会加在之前图层的动画之上                                     |
| Sync         | 复用其他层中的状态机                                         |
| IK Pass      | 反向动力学                                                   |

## 状态

状态是指在图层中的`小方框`，当拖入动画时会自动变成的“东西”

新建一个图层，可以看到三个状态

| 状态名称  | 简述                                           |
| :-------- | :--------------------------------------------- |
| Entry     | 入口，根据过渡进入默认状态                     |
| Any State | 任意状态，只要条件满足，可任意跳转到过渡的状态 |
| Exit      | 退出，返回到”Entry“                            |

### 添加状态

可以在空白处右键添加`Empty`(空状态)，也可以将动画文件拖到`Animator`窗口中添加一个状态

如果当前在`Project`窗口选中了一个动画文件，可以通过右击右击的`From Selected Clip`创建一个状态，不过还是直接拖入更方便

*第一个创建的状态默认是橙色的，代表默认状态，`Animator`会在一开始就播放，如果状态中有动画，也会被播放*

### 颜色

橙色：默认状态，即进入就执行的动画

灰色：可通过过渡达到条件执行的动画

### 状态设置

每个状态都可以包含一个动画，处于该状态时，`Animator`组件所在的物体会播放动画，选中一个状态时，会有如下选项

| 名称                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Motion                 | 状态绑定的动画（Animation）                                  |
| 速度(Speed)            | 动画播放的速度                                               |
| 乘数(Multiplier)       | Speed * 指定参数（float）作为运行时的播放速度                |
| Motion Time            | 单位化时间，指定参数（float，范围是0~1）作为动画播放时间(可使用此制作轮盘开关) |
| 镜像(Mirror)           | 镜像播放，若打开”参数(Parameter)“：指定参数作（bool）为镜像的开关 |
| 周期偏移(Cycle Offset) | 循环偏移量，可以用来同步循环的动画，使用单位化时间（范围是0~1），也可使用参数控制 |
| Foot IK                | 只用于人形动画，角色的脚是否使用IK（反向动力学）             |
| Write Default          | 进入下一个状态时重置此状态所作的更改（初始化该状态没有用到的参数为默认值） |

### Transition

显示当前状态的过渡，选中后出现内容与在图层中选中一致

| 名称 | 描述                               |
| ---- | ---------------------------------- |
| Solo | 选中则只生效当前过渡，其他将被禁用 |
| Mute | 选中则禁用当前过渡                 |

*如果同时选中`Solo`和`Mute`，`Mute`会优先生效*

## 过渡

表示状态之间的切换条件，一般会有一个或多个条件，用于从一个状态切换到另一个状态

右击一个状态，`Make Transition`后选择另一个状态即可创建过渡，在过渡中可以添加相应条件

| 名称 | 描述                               |
| ---- | ---------------------------------- |
| Solo | 选中则只生效当前过渡，其他将被禁用 |
| Mute | 选中则禁用当前过渡                 |

*如果同时选中`Solo`和`Mute`，`Mute`会优先生效*

多个出过渡时可调整位置(上下)以实现优先级(上方优先级高于下方)

| 名称          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| Name Field    | 名称框可以为过渡命名，用于区分俩个状态之间的多条过渡时很有用 |
| Has Exit Time | 是否有退出时间为条件                                         |

退出时间是一种特殊的过渡条件，它没有依赖参数，而是根据设置的退出时间点作为条件进行状态转换。

### Settings

过渡的一些参数设置

| 名称                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| 退出时间(Exit Time)               | 如果勾选了`Has Exit Time`，该参数可以设置，设置动画退出的单位化时间(单位：上一状态动画长度，如为物体打开动画/空动画可当成以秒为单位)。<br />例如设置为0.75，代表动画播放到75%时为`true`，如果没有其他条件，会直接切换到下一个状态。<br />如果exit time小于1，那么状态每次循环到对应位置的时候（不管动画是否设置为循环，状态总是循环的），该条件都会为true。比如第一次播放到75%，第二次播放到75%……时退出时间条件都会为true。<br />如果exit time大于1，该条件只会检测一次。比如exit time为3.5，state的动画会在循环3次后，在播放到第4次的50%时为true。 |
| 固定持续时间(Fixed Duration)      | 勾选，下一项”过渡持续时间(Transition Duration)“以秒为单位<br />取消勾选，下一项”过渡持续时间(Transition Duration)“以（前一个状态的）百分数计数，1为100% |
| 过渡持续时间(Transition Duration) | 过渡的过渡时间。两个状态在转换时，一般不会瞬间从一个状态转换到另一个状态，而是会经过平滑混合，这个属性就是设置了平滑混合的时间 |
| 过度偏移(Transition Offset)       | 状态开始播放时的时间偏移。即后一个状态从何处开始，值为后一个状态的百分数，1为100%<br />例如设置为0.5，则转换到下一个状态时，会从50%的位置开始播放 |

#### 过渡图

上面的参数不仅可以手动修改，也可以通过过渡图预览和修改

![img](https://jsd.cdn.zzko.cn/gh/yexca/picx-images-hosting@master/2022-VRChat/07-Layers/image.d1zgl4fkjnc.webp)

#### 中断源(Interruption Source)

可以打断状态的过渡，但尽量避免使用，比较复杂

例如以下图层

![img](https://jsd.cdn.zzko.cn/gh/yexca/picx-images-hosting@master/2022-VRChat/07-Layers/image.3j6e8ox86fg0.webp)

##### Current State(当前状态)

默认情况下，当A到C的切换触发后，状态机开始切换到C，在切换到C之前无法被改变。但是，如果将A->C的过渡的`中断源(Interruption Source)`改为`Current State`，A->C的过渡就可以被A上的**一些**过渡中断

在状态A上的过渡如下：

![img](https://jsd.cdn.zzko.cn/gh/yexca/picx-images-hosting@master/2022-VRChat/07-Layers/image.3te260424lu0.webp)

`一些`的原因：若勾选`Ordered Interruption`则只有优先级比A->B高的才能打断

过渡的优先级从上到下依次降低，即优先级`A->B`>`A->C`>`A->D`

* 如果勾选`Ordered Interruption`

  如果此时A->B的切换触发，则会中断A->C，触发A->B

  但如果此时A->D的切换触发，不会中断A->C，还是触发A->C

* 如果不勾选`Ordered Interruption`

  此时A->B或A->D的切换触发都会中断A->C

  如果A->B和A->D同时触发，则会优先激活A->B，因为优先级较高

##### Next State(下一状态)

如果将A->B的过渡的`中断源(Interruption Source)`改为`Next State`，A->B和A->D就不能中断A->C了

如果我们激活A->B后，又激活B->D，那么A->C的过渡会被中断，转而切换到D

状态B上的过渡顺序也会有影响，但此时`Ordered Interruption`无法勾选(因为A->B是在状态A上，不在状态B上，不参与B的排序)。状态B上的过渡顺序会决定同时触发时会使用哪一个过渡，例如下图过渡

![](https://jsd.cdn.zzko.cn/gh/yexca/picx-images-hosting@master/2022-VRChat/07-Layers/image.1xdggp8q1bq8.webp)

如果B->C和B->D同时触发，则优先执行B->C

##### Current State Then Next State(当前状态然后下一状态)

如果将A->B的过渡的`中断源(Interruption Source)`改为`Current State Then Next State`，则状态A与状态B的过渡都会被考虑在内

如果A->B切换时，同时激活的A->C, A->D, B->C和B->D（不勾选`Ordered Interruption`）

因`Current State Then Next State(当前状态然后下一状态)`，先看状态A的过渡，由于A->C的优先级比A->D高，则将执行A->C，因结果已出，不用再考虑下一状态B

##### Current State Then Next State(当前状态然后下一状态)

与上一个相反

##### 总结

| 名称                          | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| Ordered Interruption          | 优先级高于当前过渡才可以打断                                 |
| 无(None)                      | 不可打断                                                     |
| Current State                 | 前一个状态开始的状态过渡可以打断正在进行的状态过渡           |
| Next State                    | 从后一个状态开始的状态过渡可以打断正在进行的状态             |
| Current State Then Next State | 决定哪个动画State上的过渡节点优先权更高，先看前一个State的优先级如果没有被触发，再看后一个State的优先级，来达到不同的融合与打断效果 |
| Next State Then Current State | 决定哪个动画State上的过渡节点优先权更高，与上一个相反        |

### Condition

 过渡条件，一个过渡可以有一个条件，也可以有多个条件，甚至没有条件

* 如果没有条件，但是勾选了`Has Exit Time`，那么`Exit Time`会被作为状态退出的条件，到达`Exit Time`时，会切换到下一个状态

* 如果有一个或多个条件，需要同时满足这些条件才能切换到下一个状态

条件可以通过参数设置，具体如下

| 参数类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| Int型    | 可选择Greater（>）、Less（<）、Equals（==）、NotEqual（!=）指定的数作为过渡条件 |
| Float型  | 可选择Greater（>）、Less（<）指定的数作为过渡条件            |
| Bool型   | 可选择指定值（true or false）作为过渡条件                    |
| Trigger  | 条件为true时触发状态过渡                                     |

如果`Has Exit Time`勾选了，并且过渡还有一个或多个条件，那么过渡需要同时满足到达`Exit Time`，同时条件全为true，才会切换到下一个state

> 一个过渡至少要有一个条件（Has Exit Time可以作为一个条件），否则过渡将会被忽略(不会被执行进入下一个状态)

注：*Trigger貌似在VRChat中没有作用，至少我不知道*

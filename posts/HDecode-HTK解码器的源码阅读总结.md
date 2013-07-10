---
title: HDecode-HTK解码器的源码阅读总结
date: '2013-07-10'
description: HTK
categories: HTK
---

这段时间学习语音识别，看了下HTK解码器HDecode的源码，写个总结记录下，以方便日后回顾
</br>
作者把解码的信息全放在DecoderInst这个实体里了，然后用数据对该实体的节点(一般有很多节点，作者按层的形式存储着这些节点,每层有个链表，用来维护该层的节点)所包含的相邻state之间的跳转更新分数，记录最大的分数。
</br>1.入口函数main在HDecode.c里面, main里做了一系列初始化的工作（没管），最后定位到DoRecognition,它是主要的识别函数
</br>2.在函数DoRecognition里面，作者一个outpBlocksize一个outpBlocksize的读取帧（outpBlocksize=1），然后调用ProcessFrame函数（在文件HLVRec-propagate.c里面）。
</br>3.在函数ProcessFrame里，作者用该帧对DecoderInst安bfs的形式一层一层的更新节点，更新节点的操作在函数PropagateInternal里。
</br>4.在函数PropagateInternal，作者根据当前帧的信息，更新该节点所包含的state，更新相邻state之间的跳转分数。（减枝部分还没看）
</br>5.每处理一帧更新一次(就是没读一调帧就调用一次ProcessFrame，并把该帧作为参数传给ProcessFrame)，到处理所有数据帧完为止。此时HDecode维护的信息就是最优解。
</br>

最后吐槽下作者的剪枝写的太乱了，如果把减枝的代码全删了，代码就能简洁很多

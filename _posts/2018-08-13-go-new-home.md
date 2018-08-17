---
layout:     post
title:      "继续去新家洗澡"
subtitle:   "2018/08/13 洗啊洗"
date:       2018-08-13
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180813.jpg?raw=true"
tags:
    - 日记
    - 强化学习
---


```
    今天周一，上午休息，下午继续去新家洗澡！顺便带上了姥姥，我们一起在新家好好走了走，也顺便让姥姥
运动运动。以后等新家装修好了，让姥姥住进去就能够生活的更加舒适啦！
```

## 强化学习之SARAS

下面对比两种强化学习方法

### Q-learning步骤

Q_leaming 算法每次重新Q 函数的步骤为: 
- (1)使用$$\epsilon-greedy$$或真他方法选出一个action; 
- (2)智能体采取动作action ，得到奖励reward 和新的状态new_state; 
- (3)用reward + GAMMA* Q[new_state, :].max()来更新Q[state,action］。
更新时会设定一个学习率ALPHA ，即使用(1 - ALPHA) * Q[state,action]+ 
ALPHA* (reward+ GAMMA* Q[new_state, : ].max())来进行迭代。

### SARSA步骤

SARSA 算法的更新步骤为：
- (1)记录当前的state; 
- (2)执行上一步选定好的action，得到奖励reward 和新的状态new_state; 
- (3)在new_state 下，根据当前的Q函数，选定要执行的步骤new_action; 
- (4)用值reward +GAMMA* Q[new_ state , new_action］来更新Q[state, action］。和Q_learning算
法一样，在这里同样也存在一个学习率ALPHA。真正替换掉Q[state, action]
的值为(1-ALPHA) * Q[state, action] + ALPHA * (reward + GAMMA *Q[ new_ state, new_ action]); 
- (5)action=new_action 。这个动作将会在下一个循环中被执行。

回顾一下SARSA 算法的过程，智能体从一个状态（state ，用S 表示）
出发， 执行一个动作（action ，用A表示），得到奖励（reward ，用R表示）
和新的状态（s），在新的状态下又会选择一个新的动作（A），通过新的状
态和新的动作来更新Q 函数。S-A-R-S-A ，这是SARSA 算法名字的由来。







# Knapsack - 背包问题

在一次抢珠宝店的过程中，抢劫犯只能抢走以下三种珠宝，其重量和价值如下表所述。

| Item(jewellery) | Weight | Value |
| -- | -- | -- |
| 1 | 6 | 23 |
| 2 | 3 | 13 |
| 3 | 4 | 11 |

抢劫犯这次过来光顾珠宝店只带了一个最多只能承重17 kg的粉红色小包，于是问题来了，怎样搭配这些不同重量不同价值的珠宝才能不虚此行呢？哎，这年头抢劫也不容易啊...

用数学语言来描述这个问题就是：
背包最多只能承重 $$W$$ kg, 有 $$n$$ 种珠宝可供选择，这 $$n$$ 种珠宝的重量分别为 $$\omega_1,\cdots,\omega_n$$, 相应的价值为 $$v_1,\cdots,v_n$$. 问如何选择这些珠宝使得放进包里的珠宝价值最大化？

## Knapsack with repetition - 物品重复可用的背包问题

由于这类背包问题中，同一物品可以被多次选择，因此称为Knapsack with repetition, 又称Unbounded knapsack problem(无界背包问题).

动态规划是解决背包问题的有力武器，而在动态规划中，主要的问题之一就是——状态(子问题)是什么？在本题中我们可以从两个方面对原始问题进行化大为小：要么是是更小的背包容量 $$\omega \leq W$$, 要么尝试更少的珠宝数目(如珠宝 $$1, 2, \cdots , j, ~for~ j \leq n$$). 这两个状态(子问题)究竟哪个对于解题更为方便，还需进一步论证——能否根据状态(子问题)很方便地写出状态转移方程。

先来看看第一种状态：**在背包容量为 $$\omega$$ 时抢劫犯所能获得的最优值为 $$K(\omega)$$.** 对应此状态的状态转移方程并不是那么直观，先从 $$K(\omega)$$ 所包含的信息出发，$$K(\omega) > 0$$ 时，背包中必然含有某件值钱的珠宝，不妨假设最优值 $$K(\omega)$$ 包含某珠宝 $$i$$, 那么将珠宝 $$i$$ 从背包中移除后，背包中剩余珠宝的价值加上珠宝 $$i$$ 的价值即为 $$K(\omega)$$. 哪尼？这不就是个天然的状态转移方程么？抢劫犯灵机一动，立马想出了如下状态转移方程：
$$K(\omega) = F(\omega - \omega_i) + v_i ~(\omega_i \in \Omega)$$

其中 $$F(\omega - \omega_i)$$ 为拿出珠宝 $$i$$ 后的价值映射函数(用人话来说就是把粉红色小包里剩下的珠宝价值加起来)，取出来的珠宝重量 $$\omega_i < \omega$$(总不能取出大于背包重量的珠宝吧...), $$\Omega$$ 即为 $$K(\omega)$$ 中 $$\omega_i$$ 的所有可能取值。想了想好像哪里不对劲，$$K(\omega)$$ 的转移关系没鼓捣出来，反而新添了个 $$F(\omega - \omega_i)$$, 真是旧爱未了又添新欢... 别急，再仔细瞅瞅以上等式两端，拿出珠宝 $$i$$ 后，其价值 $$v_i$$ 就可以认为是一个定值了，故要想 $$K(\omega)$$ 为最大值，$$F(\omega - \omega_i)$$ 也理应是背包容量为 $$\omega - \omega_i$$ 时的包内珠宝的最大价值，如若不是，则必然存在 $$F(\omega - \omega_i) < K(\omega - \omega_i)$$, 即有
$$K(\omega) = F(\omega - \omega_i) + v_i < K(\omega - \omega_i) + v_i = K^{\prime}(\omega)$$
与 $$K(\omega)$$ 为在背包容量为 $$\omega$$ 时的最大值的定义不符，故假设不成立，$$F(\omega - \omega_i) = K(\omega - \omega_i)$$. 千斤顶终于成功上位——变成了备胎... 新的状态转移方程可改写为：
$$K(\omega) = K(\omega - \omega_i) + v_i$$

嗯，好像还是有哪里不对劲，千斤顶虽然已晋级为备胎，可备胎这个身份实在是不怎么好听，这不还有下标 $$i$$ 这个标记嘛，我们给抢劫犯想想法子，怎么才能让备胎尽快转正呢？！仔细分析发现我们刚才取出d的价值 $$v_i$$ 是从已知背包容量为 $$\omega$$ 时取出来的珠宝 $$i$$, 重量为 $$\omega_i$$. 那么到底那几个珠宝才是可能被取出来的呢？答案不得而知，只知道肯定是小于背包容量 $$\omega$$ 中的某一个。既然是这样，我们把所有小于背包容量 $$\omega$$ 的珠宝挨个拿出来比一比不就完了么？但这样一来又有了新的问题：取出来的珠宝 $$\omega_i$$ 不一定是最大值 $$K(\omega)$$中所包含的珠宝，那假如我们一定要拿出来比一比呢？得到的结果自然是不大于最大值 $$K(\omega)$$(如果不是，反证法证之), 用数学语言表示就是：
$$K(\omega) \geq K(\omega - \omega_j) + v_j ~(\omega_j \notin \Omega)$$

整理一下思路，用优雅的数学语言来表示就是：
$$K(\omega) = \max_{i:~\omega_i \leq \omega} \{K(\omega - \omega_i) + v_i\}$$

备胎终于得以登堂入室，警察叔叔，就是她了... 状态转移方程终于完整的找到了，千斤顶窃喜道：皇天不负有心人，我也有转正的一天，蛤蛤蛤...

## Knapsack without repetition - 01背包问题


上节讲述的是最原始的背包问题，这节我们探讨条件受限情况下的背包问题。若一件珠宝最多只能带走一件，请问现在抢劫犯该如何做才能使得背包中的珠宝价值总价最大？

显然，无界背包中的状态及状态方程已经不适用于01背包问题，那么我们来比较这两个问题的不同之处，无界背包问题中同一物品可以使用多次，而01背包问题中一个背包仅可使用一次，区别就在这里。我们将 $$K(\omega)$$ 改为 $$K(i,\omega)$$ 即可，**新的状态表示前 $$i$$ 件物品放入一个容量为 $$\omega$$ 的背包可以获得的最大价值。**

现在从以上状态定义出发寻找相应的状态转移方程。$$K(i-1, \omega)$$为 $$K(i, \omega)$$ 的子问题，如果不放第 $$i$$ 件物品，那么问题即转化为「前 $$i-1$$ 件物品放入容量为 $$\omega$$ 的背包」，此时背包内获得的总价值为 $$K(i-1, \omega)$$；如果放入第 $$i$$ 件物品，那么问题即转化为「前 $$i-1$$ 件物品放入容量为 $$\omega - \omega_i$$ 的背包」，此时背包内获得的总价值为 $$K(i-1, \omega - \omega_i) + v_i$$. 新的状态转移方程用数学语言来表述即为：
$$K(i,\omega) = \max \{K(i-1, \omega), K(i-1, \omega - \omega_i) + v_i\}$$

## Reference

- Chapter 6.4 Knapsack *Algorithm - S. Dasgupta*
- [0019算法笔记——【动态规划】0-1背包问题 - liufeng_king的专栏](http://blog.csdn.net/liufeng_king/article/details/8683136)
- [崔添翼 § 翼若垂天之云 › 《背包问题九讲》2.0 alpha1](http://cuitianyi.com/blog/%E3%80%8A%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E4%B9%9D%E8%AE%B2%E3%80%8B2-0-alpha1/)


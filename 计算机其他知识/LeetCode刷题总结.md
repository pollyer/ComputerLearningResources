# LeetCode刷题总结

## 一、双指针

### 1、有序数组的 Tow Sum（167）

- 注意审题，返回的不完全是下标 

  > Return the indices of the two numbers (**1-indexed**) as an integer array answer of size 2, where **1** <= answer[0] < answer[1] <= numbers.length

- 养成先判断**是否为空**的习惯

  ```java
  public int[] twoSum(int[] numbers, int target) {
          if (numbers == null) {
              return null;
          }
          /**...**/
      }
  ```

***

### 2、两数平方和（633）

- 思路：可以看成是在元素为 0~target 的**有序数组**中查找两个数，使得这两个数的平方和为 target

  > 能不能想到**利用数组**这回事

- 右指针可以**初始化为*sqrt(target)***

  > (int)向下取整

- 注意循环结束条件

  ```java
  while(i <= j) {
      /**...**/
  }
  ```

- 平方自己乘自己就行了，别用Math.pow()了

  ```java
  int powSum = i * i + j * j;
  ```

- 还是注意先判断边界

  ```java
  if(c < 0) return false;
  ```

***

### 3、反转字符串中的元音字符（345）

- 使用HashSet保存元音字母，利用contains方法判断是否在HashSet中

  ```java
  private final static HashSet<Character> vowels = new HashSet<>(
          Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));
  ```

- 可以提前创建好一个result字符数组，不是元音就直接加，是元音就等着

- 一定要考虑循环的边界条件；可以利用外层循环让条件分支简单一些

  ```java
  while(i <= j) { 
      char ci = s.charAt(i);
      char cj = s.charAt(j);
      if(!vowels.contains(ci)) {
          result[i++] = ci;
      } else if (!vowels.contains(cj)) {
          result[j--] = cj;
      } else {
          result[i++] = cj;
          result[j--] = ci;
      }
  }
  ```

***

### 4、回文字符串——可删除（680）

- 为体现“可删除一个”，当检测到不匹配时，要立即切换到一个“新平台”
- 分治思想，当出现一个不匹配时，只去判断子字符串即可

```java
for(int i = 0, j = s.length() - 1; i < j; i++, j--) {
    if(s.charAt(i) != s.charAt(j)) {
        return isPalindrome(s, i + 1, j) || isPalindrome(s, i, j - 1);//使用return跳转到“新平台”
    }   
}
```

***

### 5. 归并两个有序数组（88）

- 灵活选择遍历方向，可以从前向后填，也可以**从后向前**填

***

### 6. 判断链表是否存在环（141）

- 使用双指针，一个指针**每次移动一个**节点，一个指针**每次移动两个**节点，如果存在环，那么这两个指针一定会相遇

- 这两个指针初始位置在哪都可以

  ```java
  /*...*/
  ListNode l1 = head, l2 = head.next;
  while (l1 != null && l2 != null && l2.next != null) {
      if (l1 == l2) return true;
      l1 = l1.next;
      l2 = l2.next.next;
  }
  /*...*/
  ```

***

### 7. 最长子序列（524）

- 可以使用String类的compareTo比较字典顺序

- 提取出一个判断是否为子序列的方法

  > 双指针法，指向主串的指针一直向前，指向次串的指针相等时才向前；
  >
  > 如果结束的时候次串指针前进到了末尾，则是子序列

***

## 二. 动态规划

***

### 1. 斐波那契数列

***

#### (1). 爬楼梯(70)

- 最好使用非递归以降低空间复杂度

  ```java
  if(n <= 2) return n;
  int pre1 = 2, pre2 = 1;
  for(int i = 2; i < n; i++) {
      int temp = pre1 + pre2;
      pre2 = pre1;
      pre1 = temp;
  }
  ```

***

#### (2). 强盗抢劫(198)

- 重要的是找到递推式

  ![递推式](https://camo.githubusercontent.com/bd68cec63de3602c4d1506b6984e579c136eecdcac43e9e8c45db4bcdbee7aee/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f32646537393463612d616137622d343866332d613535362d6130653237303863623937362e6a7067)

- 套路：先算第一个和第二个，准备好pre1和pre2，然后从i=2开始循环

***

#### (3). 强盗在环形街区抢劫(213)

- 通过分类讨论，将环化为直线

- 提取出一个私有方法

- pre1和pre2从0开始也可以

  ```java
  private int rob(int[] nums, int first, int last) {
      int pre2 = 0, pre1 = 0;
      for (int i = first; i <= last; i++) {
          int cur = Math.max(pre1, pre2 + nums[i]);
          pre2 = pre1;
          pre1 = cur;
      }
      return pre1;
  }
  ```

***

### 2. 矩阵路径

***

#### (1). 矩阵的最小路径和(64)

- 一行一行算，用一个一维数组记录走到这行的这个格子的最短距离

- 只能从左侧来或上侧来，选择最小的那个，再加上这个格子的值

- 注意处理边界

  ```java
  if (j == 0) {
      dp[j] = dp[j];        // 只能从上侧走到该位置
  } else if (i == 0) {
      dp[j] = dp[j - 1];    // 只能从左侧走到该位置
  } else {
      dp[j] = Math.min(dp[j - 1], dp[j]);
  }
  dp[j] += grid[i][j];
  ```

***

#### (2). 矩阵的总路径数(62)

- 还是用一个一维数组递推
- 记得处理好起始条件/边界条件

***

### 3. 数组区间

***

#### (1). 数组区间和(303)

- 用***private*封装类**的属性
- 数列求和问题可以考虑动态规划弄一个**和数组**

***

#### (2).  数组中等差子区间的个数(413)

- 用一个一维数组存储以下标为i元素**结尾**的等差子区间有多少个，最后再加起来
- 紧挨着的三个，和**与前面更长的拼接的**

```java
if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
    dp[i] = dp[i - 1] + 1;
}
```

```java
int total = 0;
for (int cnt : dp) {
    total += cnt;
}
```

***

### 4. 分割整数

***

#### (1).  分割整数的最大乘积(343)

- 为了处理方便，可以让数组下标与数据对应起来（放弃下标为0的元素）

- 最大值保持方法

  ```java
  dp[i] = Math.max(dp[i], Math.max(j * dp[i - j], j * (i - j)));
  ```

- 找到“新增”与“之前”的关系；这题是利用了**之前所有**的情况，而不只是上一个

- 注意特殊情况，数值小的可能不分会更大

***

#### (2). 按平方数来分割整数(279)

- 思路与之前差不多
- 取最小的别忘了处理初始情况
- 这题还有个开挂方法，就是运用“四平方定理”
- 之后BFS也会遇到这题

***

#### (3). 分割整数构成字母字符串(91)

- 关于substring方法
  - 都是小写的
  - 单参就是从beginIndex开始到结尾
  - 双参是从beginIndex到endIndex-1
- 注意处理'0'的情况就好
- 边界条件有s == null || s.length() == 0

***

### 5. 最长递增子序列

***

#### (1). 最长递增子序列(300)

- for循环依次以每个元素为子序列结尾即可（这是$O(n^2)$的简单方法）

- 一种$O(nlogn)$的方法

  - 贪心算法

    > 怎么贪心？让每次接在序列后面的元素都尽可能的小

  - 二分查找

    > 最终left会大于right，形成一个区间(righ)

***

#### (2). 一组整数对能够构成的最长链(646)

- $O(n^2)$的方法：

  > 先<u>按第一个元素</u>**排序**，再依次动态规划

- $O(nlogn)$的方法

  > 贪心：按**第二个元素排序**

***

#### (3). 最长摆动子序列(376)

- 理解1：
  - 最后一个**摆动趋势**是比较重要的
  - 用up记录前i个元素能构成的最长递增子序列长度，down记录前i个元素能构成的最长递减子序列长度
  - 当nums[i]>nums[i-1]时，最长递增一定能在最长递减的基础上增加一个，反之也成立
  
- 理解2：

  - 这个摆动子序列一定可以从头开始计算

    > 因为趋势相同的可以直接看做**延长**

  - 相等的忽略就好

  - 趋势不同的，摆动子序列长度可以直接增加

  - 趋势相同的，也没关系，直接替换子序列的最后一个元素，这会**更有利于之后的趋势改变**

***

### 6. 最长公共子序列(1143)

- 两个序列就需要用**二维数组**了

- 想一想$s_i==s_j$时和$s_i\not= s_j$时如何进行状态转移

  > 其实就是**两个序列**同时添加了$s_i$/$s_j$
  
- 也可以使用两个一维数组，记录外层循环的**上一层情况**和**当前层更新情况**即可

***

### 7. 0-1背包

***

#### (0). 简单背包问题的空间优化

- 只需要一个一维数组，记录**可以加入上一个物品后**，能得到的**最大价值**
- 这样的话每次假设重量就要**从高向低**进行了，因为计算高的要利用到**上一个物品的**低的
- 重量不够的时候也就没必要更新一维数组了

***

#### (1). 划分数组为和相等的两部分(416)

- 可以看成一个**背包大小**为$\frac{sum}{2}$的 0-1 背包问题
- dp[i]表示加入上一个数之后，是否能**刚好填满**容量i
- 中途如果已经达到目标了，可以提前结束

***

#### (2). 改变一组数的正负号使得它们的和为一给定数(494)

- 通过推导使某个**子集**等于**给出的输入**的函数
  $$
  sum(P) - sum(N) = target
  \\sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
  \\2 * sum(P) = target + sum(nums)
  $$
  
- 本题的重点在于有多少种方法，**状态转移方程也要随之作出调整**

- DFS解法：

  - 准备从一个元素开始搜索，正式开始前先检查**是否已经到了数组末尾**

  - 没到则开始搜索，两种情况：**加**这个元素、**减**这个元素

    ```java
    public int findTargetSumWays(int[] nums, int S) {
        return findTargetSumWays(nums, 0, S);
    }
    private int findTargetSumWays(int[] nums, int start, int S) {
        if (start == nums.length) {
            return S == 0 ? 1 : 0;
        }
        return findTargetSumWays(nums, start + 1, S + nums[start])
                + findTargetSumWays(nums, start + 1, S - nums[start]);
    }
    ```

***

#### (3). 01 字符构成最多的字符串(474)

- **多维费用**的 0-1 背包问题，有两个背包大小：0 的数量和 1 的数量

- 可以用一个**二重for循环**限制重量

  ```java
  for(int i = m; i >= zeroes; i--) {
      for(int j = n; j >= ones; j--) {
          dp[i][j] = Math.max(dp[i][j], dp[i - zeroes][j - ones] + 1);
      }
  }
  ```

  > 认真读题，看好题目给的**参数的意义**

***

#### (4). 找零钱的最少硬币数(322)

- 这是一个完全背包问题

  > 完全背包只需要将 0-1 背包的**逆序**遍历 dp 数组改为**正序**遍历即可

  > 之前必须从高向低是因为，不想在算高重量时利用了在**低重量使用新物品**的结果

  > 而这里刚好需要在**计算高重量时利用低重量**的结果，以保证满足**完全**背包的要求.

- 直接用0代表无法满足了，所以要处理很多“0”的情况

  ```java
  for(int i = coin; i <= amount; i++) {
      if(i == coin) dp[i] = 1;
      else if(dp[i] == 0 && dp[i - coin] != 0) dp[i] = dp[i - coin] + 1;
      else if(dp[i] != 0 && dp[i - coin] != 0) dp[i] = Math.min(dp[i], dp[i - coin] + 1); 
  }
  ```

***

#### (5). 找零钱的硬币数组合(518)

- 注意边界的意义和限制

  > 这题里coin可能比amount还大

- 可以直接让**dp[0]=1**的

- 为什么不将背包重量放到外层循环？

  ```java
  for(int i = 1; i <= amount; i++) {
      for(int coin : coins) {
          if(i >= coin) dp[i] += dp[i - coin];
          //物品不同的加入顺序又多算了一遍
      }
  }
  ```

  > 这种**计数类型**的背包，是不可以这样的
  >
  > 当然不计数、只计true\false类型的是可以的

***

#### (6). 字符串按单词列表分割(139)

- 不仅要把物品放入背包中，还要保证**不影响其他**物品的摆放

  > 这就需要使用**带顺序**的完全背包

- 背包的重量不能来回变化了，一定要一次从小到大（放在**外层循环**），才能保证物品**不重叠**地摆放

- 不计较顺序时内外均可，但推荐把物品放在外层，因为这样内层的重量循环可以直接从物品的重量开始，减少一定的次数

***

#### (7). 组合总和(377)

- 有**顺序**的**完全**背包

- for循环**一次不满足**条件就直接**结束**了

  > 可以改成for循环中的 ***if + continue***

### 8. 股票交易

***

#### (1). 需要冷却期的股票交易(309)

- 使用动态规划的方法，**维护**在股市中每一天结束后可以获得的「累计最大收益」

- 分类的多元动态规划：分类依据是“**对下一个状态有什么样的影响**”

  ```java
  int dp1 = -prices[0];//第i天结束之后有股票（无论是怎么有的股票，对下一天都无影响，所以不再分类）
  int dp2 = 0;//第i天结束之后没股票，是因为卖了（给了下一天冷冻期）
  int dp3 = 0;//第i天结束之后没股票，是因为本来就没有，也没买（没给了下一天冷冻期）
  ```

- 注意空间优化后，不要让变量**在迭代过程中互相影响**

  ```java
  for(int i = 1; i < n; i++){
      int newdp1 = Math.max(dp1, dp3 - prices[i]);
      int newdp2 = dp1 + prices[i];
      int newdp3 = Math.max(dp2, dp3);
      dp1 = newdp1;
      dp2 = newdp2;
      dp3 = newdp3;
  }
  ```

***

#### (2). 需要交易费用的股票交易(714)

- 整体思路类似，没有冷却期，动态规划少了一元
- 把交易费用看成买股票时**多花的钱**即可，因为最后不可能持有股票不卖出

***

#### (3). 只能进行两次的股票交易(123)

- 重点还是**一天结束后的几种状态**

  > 这个状态一定包括**继承之前**的情况，即这次什么也没干

  ```java
  //int none = 0;
  //从没买过，利润一直是0，不用记录
  int buy1 = -prices[0];//只是买了一次
  int sell1 = 0;//交易了一次
  int buy2 = -prices[0];//交易了一次之后又买了一次
  int sell2 = 0;//交易了两次
  ```

  ```java
  int newBuy1 = Math.max(buy1, -prices[i]);
              int newSell1 = Math.max(sell1, buy1 + prices[i]);
              int newBuy2 = Math.max(buy2, sell1 - prices[i]);
              int newSell2 = Math.max(sell2, buy2 + prices[i]);
  ```

  > 不过是**状态**数的变化而已，没什么难的

***

#### (4). 只能进行 k 次的股票交易(188)

- 这次状态变成了一个数组
- 还有一种更好的解法，但我实在是看不懂

***

### 7. 字符串编辑

***

#### (1). 删除两个字符串的字符使它们相等(583)

- 可以转换成求两个字符串**最长公共子序列**的问题

- 直接对题目进行动态规划（实际效果更好）

  > 还是分类讨论
  >
  > 新增的两个字符是否相同

  > 这里的初始化比较特殊

  ```java
  for (int i = 1; i <= m; i++) {
      dp[i][0] = i;
  }
  for (int j = 1; j <= n; j++) {
      dp[0][j] = j;
  }
  ```

***

#### (2). 编辑距离(72)

- 理解操作的等价性

  > 本质不同的操作实际上只有三种：
  >
  > - 在单词 `A` 中插入一个字符；
  > - 在单词 `B` 中插入一个字符；
  > - 修改单词 `A` 的一个字符。

- 操作的顺序并不影响，所有都在最后操作是没问题的

- 别再把charAt的下标搞错了

#### (3). 复制粘贴字符(650)

- "因子"是个关键

- 动态规划的方法：

  - 对于一对因子来说，只需要增长到$\sqrt n$即可，因为一定有一个是小于等于$\sqrt n$的

    ```java
    for (int j = 1; j * j <= i; ++j) {
        if (i % j == 0) {
            f[i] = Math.min(f[i], f[j] + i / j);
            f[i] = Math.min(f[i], f[i / j] + j);
        }
    }
    ```

- 分解质因数的方法

  - 逆向思维

  - 理解这个式子

    $f[i]={\underset{j|i}{min}}\{f[\frac ij]+j\}$

    > 即一个**最小质因子**代表了一个代价

  - 把所有最小质因子（<u>可重复</u>）相加即可

    ```java
    for (int i = 2; i * i <= n; ++i) {
        while (n % i == 0) {
            n /= i;//把数变小。转化成新的问题
            //这个新的数不可能有小于等于i的因子了（反证）
            ans += i;//代价
        }
    }
    if (n > 1) {//有可能还没分解彻底，留下了一个质因子
        ans += n;
    }
    ```

---

### 8、k站中转最便宜的航班(787)

- 通过这个**常数k**其实可以联想到动态规划

- 状态转移方程，怎么根据上一个状态到下一个状态，就是要靠最后一个航班

  > 状态转移的一个重点：==靠什么从上一个状态转移到下一个状态==

***

## 三. 分治

***

### 1. 给表达式加括号(241)

- Integer.parseInt和Integer.valueOf都不能直接转化char

- StringBuffer/StringBuilder与String不一样，别总省略toString()

- 注意多个**判断条件**的**先后**问题

- append可以直接**子串**

- 判断是否为一个数（注意**负数**也是一个数）

- 其实就不该用String一直存储操作数和运算符，还是用***List***好

- 怎么保证运算顺序不重复？

  - 先算运算符1，先算运算符2...？

    > 这样分类还是会有重复的，不如**逆向思维**

  - **最后**算运算符1，最后算运算符2

    > 正好一个运算符会把一个表达式分成两份

- switch的赋值用法：把冒号改成箭头

  > 同时别忘了default

- 递归结束条件别忘了

***

### 2. 不同的二叉搜索树(95)

- 每个值依次当根，先生成左子树，再生成右子树

- 每次都保证左子树的所有元素值小于根小于右子树

  > 用for循环就能解决了

***

## 四、贪心思想

### 1. 分配饼干(455)

- 排序 + 两个贪心

  - 先用最小的角饼干

  - 先满足需求小的孩子

    > 贪心，能满足一个是一个，当然先从小的来

  > 注意：这只是一种贪心思路，还有一种贪心方法是先满足大需求的，道理相同，不作记录了

- **同时处理两个数组**的技巧

  > 回顾一下双指针

  ```java
  int gi = 0, si = 0;
  while(gi < g.length && si < s.length) {
      if(g[gi] <= s[si]) gi++;
      si++;
  }
  ```

***

### 2. 不重叠的区间个数(435)

- 似曾相识啊
- 找最小右边界，不断更新并记录即可

***

### 3. 投飞镖刺破气球(452)

- 同样是计算重叠区间个数

  > 但是，**不同的题目要求不同**，这道题中，边界相同也算重叠

- 注意防止大数溢出

  ```java
  Arrays.sort(points, (point1, point2) -> (point1[1] > point2[1])? 1: -1);
  ```

***

### 4. 根据身高和序号重组队列(406)

- 这个排序看起来**相互影响**比较大，想个办法让先“插入队列”的人对后“插入队列”的人影响变小

  > 这道题目如果使用“<u>插入</u空”的方法，是先排高的，再排矮的

  > 因为数组中记录的是**比自己身高高的人数**，也就是说**矮的不会有影响**

- 这种“**插入*空”的问题要能想到**链表**

  > 这题还有不“插空”的方法，但理解起来较麻烦

- 二维排序的一种方法：借助三目运算符

  ```java
  Arrays.sort(people, (p1, p2) -> p1[0] == p2[0]? p1[1] - p2[1]: p2[0] - p1[0]);
  ```

- 借助ArrayList的add方法实现任意位置插入，再借助toArray方法转换成一般的数组

  > 注意：toArray方法不传入参数就会返回Object类型的数组

  ```java
  return res.toArray(new int[people.length][]);
  ```

***

### 5. 买卖股票最大的收益(121)

- 认真读题，这题只让买一次
- 动态规划的方法不是最佳的了
- 贪心：只记录**当前**买的最小价格和**当前**卖出的最大收益

***

### 6. 买卖股票的最大收益 II(12)

- 需要**证明一个猜想**：把相邻的为正收益的都加到一起就是答案了

  > 对于 [a, b, c, d]，如果有 a <= b <= c <= d ，那么最大收益为 d - a。而 d - a = (d - c) + (c - b) + (b - a) ，因此当访问到一个 prices[i] 且 prices[i] - prices[i-1] > 0，那么就把 prices[i] - prices[i-1] 添加到收益中

- 贪心算法只能用于计算最大利润，**计算的过程并不是实际的交易过程**

***

### 7. 种植花朵(605)

- 认真对待**边界条件**的判断

- 别忘了**更新数组**

- 注意**什么时候结束**

- 不要把很简单的判断搞得很复杂，能合并的就合并

  ```java
  if (flowerbed[i] == 1) {
      i++;
      continue;
  }
  int pre = i == 0 ? 0 : flowerbed[i - 1];
  int next = i == len - 1 ? 0 : flowerbed[i + 1];
  if (pre == 0 && next == 0) {
      n--;
      flowerbed[i] = 1;
  }
  ```

- 其实这个贪心的思路很简单，就是**局部最优**：**当前**能种就给它种下

***

### 8. 判断是否为子序列(392)

- 记得**看constraints**，处理好空串的情况

- 不会有脑瘫用==判断字符串是否相等吧

  ```java
  if(s.equals("")) return true;
  ```

- 可以直接用

  ```java
  public int indexOf●(int ch, int fromIndex)
  ```

  这个方法找到**单个字符在主串中的位置**；

  这样的话最好先**将字符串转换成字符数组**

  ```java
  int index = -1;
  for (char c : s.toCharArray()) {
      index = t.indexOf(c, index + 1);
      if (index == -1) {
          return false;
      }
  }
  return true;
  ```

  > 遍历的是次串，次串中每一个字符都能找到，就代表是子序列

  > 对于空串，toCharArray方法会返回一个<u>长度为0的char数组</u>

***

### 9. 修改一个数成为非递减数组(665)

- 贪心算法体现在，当两个数不满足非递减的条件时，**优先把大的变小**

  > 当然，也有另外一种情况，只能把小的变大

- 学会分情况，不满足非递减条件还可以分为两种情况

  > <u>小的那个比再前面的一个还小</u>，这时只能把小的变大

  > <u>小的那个不比再前面的还小了</u>，这时优先把大的变小（在处理这个题目时，就**不用进行任何操作**了）

  ```java
  for(int i = 1; i < n; i++) {
      if(nums[i] < nums[i - 1]) {
          if(++count > 1) return false;
          if(i - 2 >= 0 && nums[i] < nums[i - 2]) nums[i] = nums[i - 1];
      }
  }
  ```

***

### 10. 子数组最大的和(53)

- 贪心算法

  - 一直加，**每次遇到小于0**的，就**记录**一下此时的值，然后再<u>潜水</u>
  - 如果加到小于0了，就重新开始
  - 多考虑一些特殊情况

  ```java
  for (int i = 1; i < nums.length; i++) {
      preSum = preSum > 0 ? preSum + nums[i] : nums[i];
      maxSum = Math.max(maxSum, preSum);
  }
  ```

  > 不管这次的数是大于还是小于，先加着，记录最大值，**下一次再考虑**

- 动态规划

  - **状态转移方程**

    ```java
    pre = Math.max(pre + x, x);
    maxAns = Math.max(maxAns, pre);
    ```

  - 和贪心有点像

- 分治算法

  - 官方给的题解大概能看懂，这里就先不记了
  - 涉及到“线段树”这种数据结构

***

### 11. 分隔字符串使同种字符出现在一起(763)

- 使用贪心的思想寻找每个片段可能的**最小结束下标**，因此可以保证每个片段的长度一定是符合要求的最短长度；如果取更短的片段，则一定会出现同一个字母出现在多个片段中的情况

  > 在贪心算法中，一定要找到一个**“当前的最”**

- 自己多考虑一下边界情况，不要等着测试用例帮自己考虑！！！

***

## 五. 搜索

***

### 1. BFS

#### (1). 二进制矩阵中最短路径(1091)

- **最短路径**，可以想到使用**BFS**

  > 通过**队列**实现BFS

- 利用**数组**来表示/记录各种抽象的量

  > 数组表示方向

  ```java
  private static int[][] directions = {{1, 1}, {1, 0}, {0, 1}, {1, -1}, {-1, 1}, {0, -1}, {-1, 0}, {-1, -1}};
  ```

  > 数组表示坐标

  ```java
  Queue<int[]> pos = new LinkedList<>();
  ```

  ```java
  pos.add(new int[]{newX, newY});
  ```

***

#### (2). 组成整数的最小平方数数量(279)

- 可以将每个整数看成图中的一个节点，如果**两个整数之差为一个平方数**，那么这两个整数所在的节点就**有一条边**

  > 要求解最小的平方数数量，就是求解从节点 n 到节点 0 的**最短路径**

- 生成平方数的一种方法

  ```java
  private List<Integer> generateSquares(int n) {
      List<Integer> squares = new ArrayList<>();
      int square = 1;
      int diff = 3;
      while (square <= n) {
          squares.add(square);
          square += diff;
          diff += 2;
      }
      return squares;
  }
  ```

- 更能体现“优先**在同一层中拓广**，再深入下一层”的BFS

  ```java
  while (!queue.isEmpty()) {
      int size = queue.size();//得到这一层的“长度”
      level++;//一层一层加
      while (size-- > 0) {//把这一层都检查完
          int cur = queue.poll();
          for (int s : squares) {
              int next = cur - s;
              if (next < 0) {
                  break;
              }
              if (next == 0) {
                  return level;
              }
              if (marked[next]) {
                  continue;
              }
              marked[next] = true;
              queue.add(next);
          }
      }
  }
  ```

***

#### (3). 最短单词路径/单词接龙(127)

- 本题要求的是**最短**转换序列的长度，看到最短首先想到的就是**广度优先搜索**。想到广度优先搜索自然而然的就能想到图，但是本题并没有直截了当的给出图的模型，因此我们需要把它抽象成图的模型

- 可以用前两题那种最粗暴的BFS，但要注意**相同单词的删除**问题

  ```java
  if(!queue.contains(word)) queue.add(word);
  ```

- :star:更好的BFS（外加双向BFS）

  - 建图

    - 首先把单词映射成图**结点的id**

      ```java
      Map<String, Integer> wordId = new HashMap<String, Integer>();
      ```

      ```java
      if (!wordId.containsKey(word)) {
      	wordId.put(word, nodeNum++);//防止重复
          edge.add(new ArrayList<Integer>());//edge其实是顶点集，记录了与某顶点相连的其他顶点，故称其为"edge"
      }
      ```

    - 添加边时使用**虚拟结点**思想

      ```java
      addWord(word);
      int id1 = wordId.get(word);
      char[] array = word.toCharArray();
      int length = array.length;
      for (int i = 0; i < length; ++i) {
          char tmp = array[i];
          array[i] = '*';
          String newWord = new String(array);
          addWord(newWord);
          int id2 = wordId.get(newWord);
          edge.get(id1).add(id2);
          edge.get(id2).add(id1);
          array[i] = tmp;
      }
      ```

    - BFS时**双向**奔赴

      ```java
      int queBeginSize = queBegin.size();
      for (int i = 0; i < queBeginSize; ++i) {
          int nodeBegin = queBegin.poll();
          if (disEnd[nodeBegin] != Integer.MAX_VALUE) {
              return (disBegin[nodeBegin] + disEnd[nodeBegin]) / 2 + 1;
          }
          for (int it : edge.get(nodeBegin)) {
              if (disBegin[it] == Integer.MAX_VALUE) {
                  disBegin[it] = disBegin[nodeBegin] + 1;
                  queBegin.offer(it);
              }
          }
      }
      ```

      ```java
      int queEndSize = queEnd.size();
      for (int i = 0; i < queEndSize; ++i) {
          int nodeEnd = queEnd.poll();
          if (disBegin[nodeEnd] != Integer.MAX_VALUE) {
              return (disBegin[nodeEnd] + disEnd[nodeEnd]) / 2 + 1;
          }
          for (int it : edge.get(nodeEnd)) {
              if (disEnd[it] == Integer.MAX_VALUE) {
                  disEnd[it] = disEnd[nodeEnd] + 1;
                  queEnd.offer(it);
              }
          }
      }
      ```

      > 先**出队列**，看看是不是**结束点**
      >
      > 不是的话，就将其**相邻点入队列**，并设置相邻点的**距离**

***

### 2. DFS

***

#### (1). 查找最大的连通面积(695)

- [x] 提取出方法
- 四个方向走的思路不错，但别忘了初始条件

***

#### (2). 矩阵中的连通分量数目(200)

- BFS和DFS均可
- 和上一道题思路一样

***

#### (3). 无向图中连通分量数目(547)

- DFS的思路很直观
- 没啥意思，就是最简单的dfs

***

#### (4). 填充封闭区域(130)

- 可以换一换dfs思路了

  > 之前是先改这个点，再去判断四面八方是否可以

  > 这次，先去判断这个点是否符合要求（**没出界**，**应该修改**）

```java
public void dfs(char[][] board, int x, int y) {
    if (x < 0 || x >= n || y < 0 || y >= m || board[x][y] != 'O') {
        return;
    }
    board[x][y] = 'A';
    dfs(board, x + 1, y);
    dfs(board, x - 1, y);
    dfs(board, x, y + 1);
    dfs(board, x, y - 1);
}
```

***

#### (5). 能到达的太平洋和大西洋的区域(417)

- 要防止栈溢出

  > 将目前访问的暂时置为“无效”

  > 更好的方法：先判断是否已经访问过，访问过了直接return

- 正向遇到困难时，要考虑**逆向思维**啊

- 学会设置方向变量，对方向变量进行循环

  ```java
  private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
  ```

  ```java
  for (int[] d : direction) {
      int nextR = d[0] + r;
      int nextC = d[1] + c;
      if (nextR < 0 || nextR >= m || nextC < 0 || nextC >= n
          || matrix[r][c] > matrix[nextR][nextC]) continue;
      dfs(nextR, nextC, canReach);
  }
  ```

***

### 3. Backtracking

---

#### (0). Backtracking与DFS

- $Backtracking \in DFS$
- 普通 DFS 主要用在 **可达性问题** ，这种问题只需要执行到特点的位置然后返回即可
- 而 Backtracking 主要用于求解 **排列组合** 问题，这种问题在执行到特定的位置返回之后还会**继续执行求解过程**
  - 因此在程序实现时，需要注意对元素的标记问题
  - 在访问一个新元素进入新的递归调用时，需要**将新元素标记为已经访问**，这样才能在继续递归调用时不用重复访问该元素
  - 但是在**递归返回**时，需要将元素标记为未访问，因为只需要保证在一个递归链中不同时访问一个元素，可以访问已经访问过**但是不在当前递归链中**的元素

***

#### (1). 数字键盘组合(17)

- 回溯的思路

  - 从哪里**开始**
  - 回溯的**过程量**如何存储
  - 什么时候算**结束**

- 实现方法：**添加**这个元素，以此为基础进行**回溯**，这次回溯结束后再**删除**这个元素

- Java方法

  - 学会利用***toCharArray***方法
  - ***Integer.parseInt***里面只能放*String*

- 为了共用一个StringBuffer，直接在顶级调用时new一个出来

  ```java
  backtracking(res, digits, 0, new StringBuffer());
  ```

***

#### (2). IP 地址划分(93)

- 还是那个思路

  - 哪里**开始**
  - 哪里**结束**
  - **过程量**如何存储

- 得到答案之后别忘了**处理后事**

  > 为了继续回溯啊
  >
  > 别得到一个答案就直接走人

  ```java
  if(count == 4) {
      if(ip.length() == s.length() + 4) {
          ip.deleteCharAt(ip.length() - 1);
          res.add(ip.toString());
          ip.append('.');//为了能继续递归
      }
      return;
  }
  ```

- 边界啊！！

  > 每次向数组里/字符串里放下标时都要注意这个事

  ```java
  if(start == s.length()) return;
  if(s.charAt(start) == '0') {
      ip.append('0').append('.');
      backtracking(res, s, start + 1, ip, count + 1);
      ip.delete(ip.length() - 2, ip.length());
      return;
  }
  ```

***

#### (3). 在矩阵中寻找字符串(79)

- 为了**不重复**利用之前走过的字母
  - 要么使用一个*visited*数组标记
  - 要么把访问过的修改
- 为了能回溯，改过之后要再改回来

***

#### (4). 输出二叉树中所有从根到叶子的路径(257)

- **处理后事**，想想是不是要删点儿东西

- **数字**转换为**字符串**后的**长度**问题

  > 解决方法：记录起点

- 可以尝试先把**最基本的出口判断**放在最前面

  > 不然的话直接传个**空**树进来，就异常了

  ```java
  if (root != null) {
      int index = path.length();
      path.append(root.val);
      //当前节点是叶子节点
      if (root.left == null && root.right == null)
          paths.add(path.toString());
      //当前节点不是叶子节点
      else {
          path.append("->");
          constructPaths(root.left, path, paths);
          constructPaths(root.right, path, paths);
      }
      path.delete(index, path.length());
  }
  ```

***

#### (5). 全排列(46)

- toArray方法中传入的泛型数组如果**够大**，则会直接<u>把列表装到这个数组中</u>

  > 所以一般情况下，还是传一个"***new T[0]***"更好

- **整数型**列表***remove***时必须注意，如果想以“<u>对象</u>”的方式移除元素，一定要**<u>手动包装</u>**，不然会被当成***index***

- 存储过程量的问题

  - 存放引用的数组/列表/...会随着**引用**的更改而更改
  - 所以每次将过程量存放时，最好都是<u>**"new"一个新的出来**</u>

- 基本类型数组、包装类型数组、泛型列表之间的转换问题

  ```java
  //数组: int[]
  int[] arrayInt = new int[]{1,2,3,4,5,6};
  /*
  	原生->包装->列表->包装->原生
  */
  //原生数组转包装类数组: int[] to Integer[]
  Integer[] arrayInter = Arrays.stream(arrayInt).boxed().toArray(Integer[]::new);
  //包装类数组转List/ArrayList: Integer[] to List<Integer>
  List<Integer> listInter = Arrays.asList(arrayInter);
  //List/ArrayList转包装类数组: List<Integer> to Integer[]
  Integer[] arrayInter2 = listInter.toArray(new Integer[listInter.size()]);
  //包装类数组转原生数组: Integer[] to int[]
  int[] arrayInt2 = Arrays.stream(arrayInter2).mapToInt(Integer::valueOf).toArray();
  /*
  	原生->列表->原生
  */
  //原生数组转List/ArrayList: int[] to List<Integer>
  List<Integer> listInter2 = Arrays.stream(arrayInt2).boxed().collect(Collectors.toList());
  //List/ArrayList转原生数组： List<Integer> to int[]
  int[] arrayInt3 = listInter2.stream().mapToInt(Integer::valueOf).toArray();
  ```

- 已访问元素的删除问题

  - 不用真的在数组中删除吧（这样速度会超级慢），可以使用一个**visited数组**
  - 记得在回溯之后**重置visited**即可

***

#### (6). 大礼包(638)

- 哪里开始：单买所有
- 哪里结束：不能购买大礼包时
- 过程量如何存储：nxtNeeds
- 记忆化搜索：memory
- 封装的思想是对的，但如果发现重复代码，就想办法合并






















































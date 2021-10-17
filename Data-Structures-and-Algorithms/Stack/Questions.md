# 栈的习题

## 力扣-682. 棒球比赛

此题的核心特点是：每个操作符只操作它前面的一个或两个数字，也就是操作符只关心离它最近的数字。我们考虑分数和操作符分开放，因为随时需要操作最新加入的 1～2 个分数，所以分数用栈来存储。那么操作符用什么存储呢？这里很容易联想到栈在中缀表达式求值中的应用，很容易不加思考地误认为操作符也需要用栈存储，但实际上，操作符的使用顺序就与原本 ops[] 数组提供的顺序是一样的，即先出现先使用，所以操作符可以用队列存储，甚至其实不需要专门去存储操作符，直接在遍历 ops[i] 的过程中，遇到数字就入栈，遇到操作符就一个一个按顺序处理即可。

注：Java 中字符串转数字的方法为

```java
Integer.parseInt(String s) //返回值为int型
```

下面是题解代码

```java
class Solution {
    public int calPoints(String[] ops) {
        int len = ops.length;
        if (len == 0) return 0;
        Stack<Integer> score = new Stack<>();
        for (String s : ops) {
            if (s.equals("+")) {
                int cur_num = score.pop();
                int pre_num = score.pop();
                score.push(pre_num);
                score.push(cur_num);
                score.push(pre_num + cur_num);
            } else if (s.equals("C")) {
                score.pop();
            } else if (s.equals("D")) {
                score.push(score.peek() * 2);
            } else {
                score.push(Integer.parseInt(s));
            }
        }
        int sum = 0;
        for (Integer i : score) {
            sum += i;
        }
        return sum;
    }
}
```

## 力扣 155. 最小栈

https://leetcode-cn.com/problems/min-stack

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中

pop() —— 删除栈顶的元素

top() —— 获取栈顶元素

getMin() —— 检索栈中的最小元素

**思路**

最先想到的思路是：底层用数组实现，然后设置一个 $min$ 标记指向当前最小元素的下标，需要的时候就能 $O(1)$ 访问，但是问题很明显：如果某次操作把 $min$ 标记的元素 $pop()$ 掉，栈中剩余元素的最小值就无法追踪。

解决方法是：使用一个与原栈同步的辅助栈 $help$，原栈 $data$ 每 $push$ 一个数（假设该数进入原栈后，是从栈底数起第 $i$ 个元素），辅助栈也 $push$ 一个数 $tmp\_min$，这个 $tmp\_min$ 是原栈中前 $i$ 个元素当中的最小值，与此同时，辅助栈的所有操作也与原栈同步进行，包括 $pop()$

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017101019.png)

**优化**

不需要开两个栈，只开一个栈，但栈的存储对象是 $pair<int,int>$，$first$ 存当前数，$second$ 存在当前数之前的序列（包括当前数）中的最小值，速度会快不少。

```cpp
class MinStack {
private:
    stack< pair<int, int> > stk; //first存值，second存min
public:
    MinStack() {}
    
    void push(int val) {
        if (stk.empty()) {
            stk.push({val,val});
        } else {
            int tmp_min = min(val, stk.top().second);
            stk.push({val, tmp_min});
        }
    }
    
    void pop() {
        stk.pop();
    }
    
    int top() {
        return stk.top().first;
    }
    
    int getMin() {
        return stk.top().second;
    }

};
```

## 剑指 Offer 09. 用两个栈实现队列

https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/

此题要求比较特殊，题目仅仅是通过函数 $delete\_head()$ 的返回值来判断你是否成功实现了这个数据结构，只需要保证该函数的返回值符合队列的特征.

**基本思路：**

用两个栈，主栈 $stk$ 和辅助栈 $hlp$，入栈（入队）时直接入主栈即可，当需要队头出队时，将 $stk$ 中所有数据”倒入“辅助栈 $hlp$ 中，然后辅助栈出栈并返回，就模拟了”出队“.

注意！出队之后若辅助栈还有剩余元素，不需要将它们”倒回“主栈中！因为即使此时还有新的元素入队，以后仍然是辅助栈中残留的这几个元素优先出队，相对优先级是保持不变的.

只有当调用 $delete\_head()$ 且 $hlp$ 为空时，才需要再次将 $stk$ 中的元素”倒入“ $hlp$，然后出队.

**稍微总结一下思路，就是：**

入队操作永远在主栈进行，辅助栈相当于一个具有一定长度的”出队缓存“，充当一个 $Cache$ 的角色，可以在一定请求次数内用 $O(1)$ 复杂度应付出队请求，只有当 $Cache$ 空了、需要更新的时候，才会产生一次 $O(n)$ 的操作.

**低效率例子：**

如果每次请求出队一个元素之后，都将辅助栈中的所有元素又倒回主栈，就会浪费了 $Cache$ 能够提供的便利，导致每次出队都需要 $O(n)$ 的时间复杂度.

```cpp
class CQueue {
public:
    stack<int> hlp;
    stack<int> stk;
    CQueue() {

    }
    
    void appendTail(int value) { 
        stk.push(value);
    }
    
    int deleteHead() {
        if (hlp.empty() && stk.empty()) return -1;
        if (hlp.empty()) {
            while (!stk.empty()) {
                hlp.push(stk.top());
                stk.pop();
            }
        }
        int res = hlp.top();
        hlp.pop();
        return res;
    }
};
```
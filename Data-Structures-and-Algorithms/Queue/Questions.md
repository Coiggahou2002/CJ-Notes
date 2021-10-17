## 力扣-933. 最近的请求次数

https://leetcode-cn.com/problems/number-of-recent-calls/

题目要求每 `ping(t)` 一次，都在时刻 `t` 添加一个新请求，并返回 `[t-3000,t]` 时间线上的总请求数。这里有一点非常重要，就是题目保证输入的 `t` 值严格递增，所以我们只需要关注最新的 `3000 ms` 内的请求就可以了，不需要存储整个请求序列，所以就用队列，队列的元素值记录每次请求的时刻，每请求一次，就将不在最近 `3000 ms` 内的请求全部出队，队列中剩下的元素就都符合要求，此时队列长度就是最新的请求总数。

```java
class RecentCounter {
    Queue<Integer> q;

    public RecentCounter() {
        q = new LinkedList<>(); //初始化请求队列
    }
    
    public int ping(int t) {
        q.add(t);
        int tmp = q.peek();
        while (tmp < t-3000) {
            q.poll();
            tmp = q.peek();
        }
        return q.size();
    }
}
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int len = nums.length;
        if (len == 0) return new int[0];

        int[] res = new int[len-k+1];
        Queue<Integer> q = new LinkedList<>();
        int l_cur = 0;  //窗口的左侧位置对应的数组索引
        
        //初始化队列，先加前k个数到队列中
        for (int i = 0; i < k; i++) {
            q.add(nums[i]);
        }

        while (l_cur + k <= len - 1) {
            int max = Integer.MIN_VALUE;
            for (Integer i : q) {
                max = Math.max(max, i);
            }
            res[l_cur] = max;
            q.poll();
            q.add(nums[l_cur + k]);
            l_cur++;
        }

        //最后一个窗口补判
        int max = Integer.MIN_VALUE;
        for (Integer i : q) {
            max = Math.max(max, i);
        }
        res[l_cur] = max;
        return res;
    }
}
```
# 矩阵题选

## 力扣 48. 旋转图像

https://leetcode-cn.com/problems/rotate-image

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017102249.png)

**思路：**

以圈为单位，由外至内，一圈一圈进行顺时针旋转。

对于一个圈，顺时针旋转 90° 的本质就是——让每个元素“向前走若干步”，走的步数等于圈的边长减一。

对于每个圈，我们使用四根指针 $p1,p2,p3,p4$，他们之间刚好间隔“**边长减一**”个单位，初始化时安置在四个角，每次对指针所指的四个元素进行”**滚轮式赋值**“，赋值完成之后，相当于这四个指针所指的元素已经顺时针旋转了一圈，这时我们再让四大指针分别前进一步，重复上述操作，直到每根指针都走到了当前所在边的终点。

同时，四根指针都有一个特点，那就是在移动过程中，它的横坐标或纵坐标有一个是不变的.

注意，这里存储指针采用了 $pair<int,int>$的形式，便于存坐标.

**图解：**

![image-20211017102305270](C:/Users/Techn/AppData/Roaming/Typora/typora-user-images/image-20211017102305270.png)

**代码：**

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int size = matrix.size();
        if (size == 1) return;
				//由外至内，旋转每一圈
        for (int i = size, begin = 0; i >= 1; i -= 2, begin += 1) {
            rotate_cycle(matrix, begin, i);
        }
    }

    //子矩阵的起点坐标(x,x)，子矩阵边长size
    void rotate_cycle(vector<vector<int>>& matrix, int x, int size) {
				//如果已经到了最内层的单位矩阵，就不需要再旋转了
        if (size == 1) return;

				//四个指针的初始化
        pair<int, int> p1, p2, p3, p4;
        p1 = {x, x};
        p2 = {x, x + size - 1};
        p3 = {x + size - 1, x + size - 1};
        p4 = {x + size - 1, x};

        while (p1.second < x + size - 1) {
            //四指针轮转赋值
						//此处为了代码表达更清晰，直接开4个tmp变量，存完再重新赋值，而不是使用覆盖式赋值
            int tmp1, tmp2, tmp3, tmp4;
            tmp2 = matrix[p1.first][p1.second];
            tmp3 = matrix[p2.first][p2.second];
            tmp4 = matrix[p3.first][p3.second];
            tmp1 = matrix[p4.first][p4.second];
            matrix[p1.first][p1.second] = tmp1;
            matrix[p2.first][p2.second] = tmp2;
            matrix[p3.first][p3.second] = tmp3;
            matrix[p4.first][p4.second] = tmp4;

            //指针前进
						//p1和p3横坐标一直不变，p2和p4纵坐标一直不变，不修改
            p1.second += 1;
            p2.first += 1;
            p3.second -= 1;
            p4.first -= 1;
        }
    }
};
```
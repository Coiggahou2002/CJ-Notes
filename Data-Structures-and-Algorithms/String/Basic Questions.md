# 字符串基础习题

## 力扣 151. 翻转字符串里的单词

https://leetcode-cn.com/problems/reverse-words-in-a-string/

扫描字符串，双指针跳空格抓子串，注意 Corner Cases

每抓一个单词就入栈，最后全部倒出来，拼成结果串

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stack<string> rls; //reversed_list_stack
        const int len = s.length();
        int i = 0,  j = 0;
        //跳空格，抓子串，入栈
        while (i < len && j < len) {
            while (i < len && s[i] == ' ') i ++;   j = i;
            while (j < len && s[j] != ' ') j ++;
            if (i >= len) break;
            string word = s.substr(i, j - i);
            rls.push(word);
            i = j;
        }
        string ans;
        while (!rls.empty()) {
            ans.append(rls.top());
            rls.pop();
            ans.push_back(' ');
        }
        ans.pop_back(); //尾部空格去掉
        return ans;
    }
};
```

## 力扣 8. 字符串转整数

https://leetcode-cn.com/problems/string-to-integer-atoi/

```cpp
class Solution {
public:
    int myAtoi(string s) {
        int i = 0;
        const int len = s.length();
        
        //如果有，跳过前导空格
        while (i < len && s[i] == ' ') i ++;
        
        //不是正负号也不是数字，直接截断
        if (s[i] != '+' && s[i] != '-' && (s[i] > '9' || s[i] < '0')) {
            return 0;
        }

        //正负号识别，用bool变量记录，注意类似“+-+--+33389”这种情况
        bool positive = true; 
        if (s[i] == '+') positive = true, i ++;
        else if (s[i] == '-') positive = false, i ++;

        //去掉前导0
        while (i < len && s[i] == '0') i ++;

        //截取数字子串
        int j = i;
        while (j < len && s[j] >= '0' && s[j] <= '9')  j ++;
        string num_s = s.substr(i, j - i);

        //先用 long long 存结果
        long long num = 0;
        const int num_len = num_s.length();

        //INT_MAX 长度为 10 位，检测到超过11位就直接截断（这种方法成立的前提是去掉前导0）
        if (num_len <= 11) {
            for (int i = 0; i < num_len; i ++ ) {
                int tmp = num_s[i] - '0';
                num *= 10;
                num += tmp;
            }
            if (positive) {
                if (num >= 2147483647) num = 2147483647;
            } else {
                if (num >= 2147483648) num = -2147483648;
                else num = -num;
            }
        } else {
            if (positive) num = 2147483647;
            else num = -2147483648;
        }
        
        //转类型
        int res = num;
        return res;

    }
};
```
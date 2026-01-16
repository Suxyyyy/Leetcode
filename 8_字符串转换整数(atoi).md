[[8.字符串转换整数（atoi)]](https://leetcode.cn/problems/string-to-integer-atoi/description/)

```cpp
class Solution {
public:
    int myAtoi(string s) {
        int n=s.size();
        // 跳过前导空格
        int i=0;
        while(i<n && s[i]==' ') {
            ++i;
        }
        // 处理正负号
        int sign=1; 
        if(i<n && (s[i]=='+' || s[i]=='-')) {
            sign=(s[i]=='+'?1:-1);
            ++i;
        }
        // 处理数字
        int num=0;
        for(;i<n && s[i]>='0' && s[i]<= '9';++i) {
            int d=s[i]-'0';
            // 如果num*10+d>INT_MAX 则答案已确定
            if(num>INT_MAX/10 || num*10>INT_MAX-d){ //避免溢出
                return sign>0?INT_MAX:INT_MIN;
            }
            num=num*10+d;
        }
        return sign*num;
    }
};
```

## 一、跳过前导空格

从 `i = 0` 开始循环，如果 `i < n` 且 `s[i]` 是空格，那么把 `i` 加一，表示跳过前导空格。

------

## 二、处理正负号

如果 `i < n` 且 `s[i]` 是正号或者负号，那么记录 `sign = 1` 或者 `sign = -1`，然后把 `i` 加一。

------

## 三、处理数字

以把字符串 `123` 转换成数字 `123` 为例：

1. 初始化 `num = 0`。
2. 读取字符 `1`，把 `num` 更新成 `num * 10 + 1 = 1`。
3. 读取字符 `2`，把 `num` 更新成 `num * 10 + 2 = 10 + 2 = 12`。
4. 读取字符 `3`，把 `num` 更新成 `num * 10 + 3 = 120 + 3 = 123`。

------

## 四、处理截断

设 `mx = 2^31 - 1`，`d` 是 `s[i]` 对应的数字。

在处理数字的过程中，如果发现

num⋅10+d>mx

则可以提前返回答案：

- 如果 `sign = 1`，返回 `2^31 - 1`
- 如果 `sign = -1`，返回 `-2^31`

> 注意：如果 `sign = -1`，且 `num` 更新后是 `2^31`，即使没有发生截断，最终答案也固定为 `-2^31`，因此也可以提前返回。

为避免乘法和加法溢出，可将判断不等式拆分为两个等价条件：

1. `num * 10 > mx`，等价于 `num > ⌊mx / 10⌋`
2. `num * 10 + d > mx`，等价于 `num * 10 > mx - d`

如果正常循环结束，最终返回 `sign * num`。
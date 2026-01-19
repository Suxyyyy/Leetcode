[[984.不含AAA或BBB的字符串]]（https://leetcode.cn/problems/string-without-aaa-or-bbb/）
```cpp
class Solution {
public:
    string strWithout3a3b(int a, int b) {
        string ans;
        // 第一阶段：当两个字母都还有剩余时，根据数量动态调整
        while(a>0 && b>0) {
            if(a>b) {
                ans+="aab";
                a-=2;
                --b;
            }
            else if(a==b) {
                ans+="ab";
                --a,--b;
            }
            else if(a<b) {
                ans+="bba";
                b-=2;
                --a;
            }
        }
        // 第二阶段：补齐剩余的字母（此时必有一种字母已耗尽，且另一种剩余不超过 2 个）
        while(a>0) {
            ans+="a";
            --a;
        }
        while(b>0) {
            ans+="b";
            --b;
        }
        return ans;
    }
};
```

**谁多先排谁，通过“多排 2 个，少排 1 个”来快速消耗差值。**

|**情况比较**|**拼接动作**|**消耗比例**|**说明**|
|---|---|---|---|
|**$a > b$**|`ans += "aab"`|$a-2, b-1$|`a` 多，用 `aab` 快速消耗 `a`|
|**$a < b$**|`ans += "bba"`|$b-2, a-1$|`b` 多，用 `bba` 快速消耗 `b`|
|**$a = b$**|`ans += "ab"`|$a-1, b-1$|数量相等，交替排列最稳|

[[712.两个字符串的最小ASCII删除和]](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings)

```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int n=s1.size(),m=s2.size();
        int total=reduce(s1.begin(),s1.end(),0)+reduce(s2.begin(),s2.end(),0);
        vector<vector<int>>f(n+1,vector<int>(m+1));
        for(int i=0;i<n;++i) {
            for(int j=0;j<m;++j) {
                if(s1[i]==s2[j])
                    f[i+1][j+1]=f[i][j]+s1[i];
                else 
                    f[i+1][j+1]=max(f[i][j+1],f[i+1][j]);
            }
        }
        return total-f[n][m]*2;
    }
};
```

用 *s*1 和 *s*2 的 ASCII 值之和，减去保留的 ASCII 之和的最大值，就是删除字符的 ASCII 值之和的最小值。

*s*1[*i*]=*s*2[*j*] 时，都保留，保留的 ASCII 之和增加 ASCII(*s*1[*i*])⋅2。
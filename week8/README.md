# 第八周作业

# 438. 找到字符串中所有字母异位词
```
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

异位词 指字母相同，但排列不同的字符串。

 

示例 1:

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词

```
CODE
```
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        //思路是滑动窗口统计，比较滑动创建的字符串统计，如果相等就把初始滑动窗口的左边放入
        //滑动窗口的左边是--，右边是++；
    vector<int> ans;
    vector<int> cmps(26,0),cmpp(26,0);
    int n = s.size();
    int m = p.size();
    if (n < m) return {};
    for (int i = 0; i < m; i++) {
        cmps[s[i] - 'a']++;
        cmpp[p[i] - 'a']++; 
    }
    if (cmpp == cmps) ans.push_back(0);
    for (int i = m; i < n ; i++) { //初始值是m开始 滑动窗口
        cmps[s[i] - 'a'] ++; //
        cmps[s[i - m] - 'a'] --;
        if (cmps == cmpp) ans.push_back(i - m + 1);
    }
    return ans;
    }



};
```
# 44. 通配符匹配
```
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。

'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

CODE
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        s = " " + s;
        p = " " + p;
        vector<vector<bool>> f(n + 1,vector<bool>(m + 1,false));
        for (int i = 1; i <= m; i++) { //遍历*好的位置都是true
            if (p[i] == '*') f[0][i] = true;
            else break;
        }
        f[0][0] = true;
        for (int i = 1; i <= n; i++ ) {
            for (int j = 1; j <= m; j++) {
                if (p[j] == '?') { //如果是？那么当前位置不要
                    f[i][j] = f[i - 1][j - 1];
                } else if (p[j] == '*') {
                    f[i][j] = f[i][j - 1] || f[i - 1][j]; //前一个或者后一个
                } else { //如果都不是就是判断当前位置的匹配和前一个的&&
                    f[i][j] = f[i - 1][j - 1] && (s[i] == p[j]);
                }
            }
        }
        return f[n][m];
    }
};

```



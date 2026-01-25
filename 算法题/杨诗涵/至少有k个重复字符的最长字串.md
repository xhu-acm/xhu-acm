[最少有k个重复字符的最长字串]https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/description/?envType=problem-list-v2&envId=aTTpriiJ

做题时的思考：题目要求返回满足一定条件的子区间，想到滑动窗口。但本题的约束很难让左边界移动，若要依旧使用滑动窗口，只能自己加约束，然后将所有约束后得到的答案整合。

题解：根据上述思路，由于字符串只包含小写字母，最多有 26 种字符，我们可以枚举子串中字符的种类数 t，然后使用滑动窗口维护一个恰好包含 t 种字符的窗口，并保证窗口内每种字符的出现次数都不小于 k。

最后的代码：
class Solution {
public:
    int longestSubstring(string s, int k) {
        int ans = 0;
        int n = s.size();
        // 枚举子串中字符的种类数
        for (int t = 1; t <= 26; t++) {
            vector<int> cnt(26, 0);
            int left = 0, right = 0;
            int tot = 0;        // 窗口内字符种类数
            int less = 0;       // 出现次数小于 k 的字符种类数
            while (right < n) {
                int c = s[right] - 'a';
                cnt[c]++;
                if (cnt[c] == 1) {
                    tot++;
                    less++;
                }
                if (cnt[c] == k) {
                    less--;
                }
                // 如果字符种类数超过 t，收缩左边界
                while (tot > t) {
                    int d = s[left] - 'a';
                    cnt[d]--;
                    if (cnt[d] == k - 1) {
                        less++;
                    }
                    if (cnt[d] == 0) {
                        tot--;
                        less--;
                    }
                    left++;
                }
                // 当窗口内字符种类数恰好为 t，且没有出现次数小于 k 的字符
                if (less == 0) {
                    ans = max(ans, right - left + 1);
                }
                right++;
            }
        }
        return ans;
    }
};



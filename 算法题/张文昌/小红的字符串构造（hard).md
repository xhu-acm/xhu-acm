[小红的字符串构造（hard）](https://ac.nowcoder.com/acm/contest/126636/G)

自己做题时的思考：
    s',比它短的字符串可能是它的前缀，比它长的一定不是，于是我按字符串的长度从小到大的给字符串数组排序，然后遍历字符串数组，统计以当前字符串为s'的前缀个数是否满足K个。由于题目中说的是刚好K个，可能出现相同字符串，所以得统计相同字符串的个数，避免出现大于k的情况。
  

题解：


最后的代码
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
#include<map>
using namespace std;


bool compare(pair<string, int>& p1,pair<string, int>& p2) {
    return p1.first.size() < p2.first.size();
}

int main() {
    int n, k;
    cin >> n >> k;
    map<string, int> cut; 
    for (int i = 0; i < n; i++) {
        string str;
        cin >> str;
        cut[str]++;
    }
    vector<pair<string, int>> a(cut.begin(), cut.end());
    sort(a.begin(), a.end(), compare);
    int len = a.size();
    for (int i = 0; i < len; i++)
    {
        int sum = a[i].second;
        for (int j = 0; j < i; j++)
        {
            if (a[i].first.find(a[j].first) == 0)
            {
                sum += a[j].second;
            }
        }
        if (sum == k)
        {
            cout << a[i].first;
            return 0;
        }
    }
    cout << "-1" << endl;
    return 0;
}


代码内容


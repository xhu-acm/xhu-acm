[Kato_Shoko](https://ac.nowcoder.com/acm/problem/301618)
自己做题时的思考：
通过遍历和ASCII码来访问对比并存储字符，所以创建两个大小为 128 的整数向量（数组）freq_s 和 freq_t，第一个循环遍历输入字符串 s，统计 s 中每个字符出现的次数，存入 freq_s，第二个循环遍历目标字符串 a ("Kato_Shoko")，统计该名字中每个字符出现的次数，存入 freq_t，如果 s 中缺少任何一个字符（或者数量不够），输出 "NO" 并终止，否则说明 n >= 10 且 s 包含了组成 "Kato_Shoko" 所需的所有字符，输出 "YES"，后面跟着一个空格和数字 n - 10即可。

题解:
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    string s;
    cin >> s;
    string a = "Kato_Shoko";
    
    if (n < 10) {
        cout << "NO" << endl;
        return 0;
    }
    
    vector<int> freq_s(128, 0);
    vector<int> freq_t(128, 0);
    
    for (char c : s) {
        freq_s[c]++;
    }
    
    for (char c : a) {
        freq_t[c]++;
    }
    
    for (char c : a) {
        if (freq_s[c] < freq_t[c]) {
            cout << "NO" << endl;
            return 0;
        }
    }
    
    cout << "YES " << n - 10 << endl;
    return 0;
}

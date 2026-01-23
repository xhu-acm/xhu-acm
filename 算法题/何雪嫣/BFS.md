[BFS](https://www.nowcoder.com/questionTerminal/22da7730da5d4a129a188ba24d22f2bf)

自己做题时的思考：
先将输入的字符串全部转化为小写字母，再用substr函数与bob比较。相等时输出i，并将标识用的变量c赋值为0
若循环结束后c仍不为零，说明没有子字符串和bob相等，输出-1.

题解：

最后的代码

```

代码内容

```
#include <iostream>
#include<string>
#include<cstring>
using namespace std;
int main() 
{
    string a;
    cin>>a;
    for(int i=0;i<a.length();i++)
    {
        if(a[i]<97)
        {
            a[i]+=32;
        }
    }
    int c=1;
    if(a.length()<3)
    {
        cout<<-1;
        return 0;
    }
    for(int i=0;i<(a.length()-2);i++)
    {
        if(a.substr(i,3)=="bob")
        {
            cout<<i<<endl;
            c=0;
            break;
        }
    }
    if(c)
    {
        cout<<-1;
    }
}
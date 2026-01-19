[P5026 Lycanthropy](https://www.luogu.com.cn/problem/P5026)

# 自己做题时的思考：

1、看到其中增加和降低都以1为单位，而且都在一条线上处理，可以想到等差数列差分。
2、知道增加和降低都有一定的区间，那就可以分区间来等差数列差分。
3、为了避免边界的考虑，就让它们整体移动一定范围。

# 题解：
## 这是等差数列差分的公式
区间：`[l, r]` 
首项：s
尾项：e
差值：d
```c++
arr[l] += s;
arr[l + 1] += d - s;
arr[r + 1] -= e + d;
arr[r + 2] += e;
```
## 分区间
### 总的划分
`[x-3*v+1 ... x-2*v-1][x-2*v ... x-v-1](x-v)[x-v+1 ... x][x+1 ... x+v-1](x+v)[x+v+1 ... x+2*v][x+2*v+1 ... x+3*v-1] //其中（）的点不用处理，因为题意是此点不变深度`

#### 详细分段划分（其中 d 是差值）

这是样例：`[x-3*v+1 ... x-2*v-1] -> [+1 ... +(v-1)] d = 1`

解释：

`[x-3*v+1 ... x-2*v-1]` -> 区间;

`[+1 ... +(v-1)]` -> 值的变化;

`d = 1` -> 差值。

1、`[x-3*v+1 ... x-2*v-1] -> [+1 ... +(v-1)] d = 1`

2、`[x-2*v ... x-v-1] -> [+v ... +1] d = -1`

3、`[x-v+1 ... x] -> [-1 ... -v] d = -1`

4、 `[x+1 ... x+v-1] -> [-(v - 1) ... -1] d = 1`

5、`[x+v+1 ... x+2*v] -> [+1 ... +v] d = 1`

6、`[x+2*v+1 ... x+3*v-1] -> [+(v - 1) ... +1] d = -1`

最后的代码

```c++
#include <iostream>
using namespace std;
long long arr[1060010];
const int LeftSet = 30001;  /* 整体偏移的长度，因为v的最大值为10000，则LeftSet > 3*v
*/

/* set函数是等差数列差分的公式。
参数可以表示为：[l ... r] -> [s ... e] d
这对应上面的个区间划分。
*/
void set(int l, int r, int s, int e, int d)
{
    arr[l + LeftSet] += s;
    arr[l + 1 + LeftSet] += d - s;
    arr[r + 1 + LeftSet] -= e + d;
    arr[r + 2 + LeftSet] += e;
}

/* Func函数是将上面每个区间的等差数列差分代码实现*/
void Func(int v, int x)
{
    set(x - 3 * v + 1, x - 2 * v - 1, 1, v - 1, 1);
    set(x - 2 * v, x - v - 1, v, 1, -1);
    set(x - v + 1, x, -1, -v, -1);
    set(x + 1, x + v - 1, -v + 1, -1, 1);
    set(x + v + 1, x + 2 * v, 1, v, 1);
    set(x + 2 * v + 1, x + 3 * v - 1, v - 1, 1, -1);
}

int main()
{
    int n, m, v, x;
    cin >> n >> m;
    for(int i = 0; i < n; i++)
    {
        cin >> v >> x;
        Func(v, x);
    }

    /*处理数据*/
    for(int i = 1; i <= m + LeftSet + 1; i++)
    {
        arr[i] += arr[i - 1];
        arr[i - 1] += arr[i - 2];

        /*打印实际区间
        i - 1> LeftSet是因为实际区间整体偏移30001
        */
        if(i - 1 > LeftSet)cout << arr[i - 1] << " ";
    }
    return 0;
}
```
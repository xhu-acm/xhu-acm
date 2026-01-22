[地图制作（medium）](https://www.matiji.net/exam/brushquestion/2/4592/5FBEEA06D09D64B8CED3631AA553FF68)

自己做题时的思考：
因为每次操作后，需要判断当前地图是否在历史中出现过，一条边可以有多个相连，无法直接用map,所以想到用set存储每个边对应的邻居。这样我们还可以利用set的性质，避免重复添加同一条边。

存储之后，为了减少搜索时间，想到把图的结构转换成一个唯一的字符串并存储所有历史字符串，每次操作后生成新字符串并查询集合。
做法：遍历每个节点的邻接表，按节点编号从小到大，边的目标节点从小到大，拼接成类似 1-2,1-3,2-1,2-4,... 的字符串，确保相同的图结构生成相同的字符串。

操作处理：
add u v：在 adj[u] 和 adj[v] 中互相插入对方。
del u v：在 adj[u] 和 adj[v] 中互相删除对方。
pop u：先保存 u 的所有邻接节点，再遍历这些节点删除与 u 的边，最后清空 u 的邻接表。

题解：
数据结构选择：
邻接表用 vector<set<int>>，保证边的有序和唯一性，避免重边。
历史记录用 unordered_set<string>，实现 O (1) 时间复杂度的存在性查询。
唯一字符串生成：
遍历每个节点（1~n），将其邻接节点排序后，拼接成 u-v, 的形式，确保相同图结构生成的字符串完全一致。
操作流程：
初始化：读取初始图，生成初始字符串并加入历史集合。
处理每个操作：根据操作类型修改邻接表，生成当前图的字符串，检查是否在历史集合中，输出 new 或 old，并更新历史集合。

最后的代码

#include <iostream>
#include <vector>
#include <set>
#include <unordered_set>
#include <sstream>
#include <algorithm>
#include <string>
using namespace std;

string tostr(const vector<set<int>>& adj, int n) {
    stringstream ss;
    // 遍历每个节点（1~n）
    for (int node = 1; node <= n; ++node) {
        // 将set转换为vector以便排序
        vector<int> neighbors(adj[node].begin(), adj[node].end());
        sort(neighbors.begin(), neighbors.end());
        // 拼接当前节点的所有边（格式：u-v,）
        for (int v : neighbors) {
            ss << node << "-" << v << ",";
        }
    }
    return ss.str();
}
int main() {
    // n: 节点数, m: 初始边数
    int n, m;
    cin >> n >> m;
    // 邻接表存储图结构，set自动去重且有序，节点编号从1开始
    vector<set<int>> adj(n + 1);
    // 读取初始m条边，构建初始图
    for (int i = 0; i < m; ++i) {
        int u, v;
        cin >> u >> v;
        adj[u].insert(v); // 无向图，双向添加边
        adj[v].insert(u);
    }
    // 存储所有出现过的图结构的唯一标识，用于判断是否重复
    unordered_set<string> history;
    string map1 = tostr(adj, n);
    history.insert(map1); // 记录初始图结构
    // q: 操作次数
    int q;
    cin >> q;
    for (int i = 0; i < q; ++i) {
        string op;
        cin >> op;
        // 添加边操作
        if (op == "add") {
            int u, v;
            cin >> u >> v;
            adj[u].insert(v);
            adj[v].insert(u);
        }
        // 删除边操作
        else if (op == "del") {
            int u, v;
            cin >> u >> v;
            adj[u].erase(v);
            adj[v].erase(u);
        }
        // 移除节点所有边操作（pop节点）
        else if (op == "pop") {
            int u;
            cin >> u;
            // 先保存当前节点的所有邻接节点，避免遍历时修改容器
            vector<int> neighbors(adj[u].begin(), adj[u].end());
            // 双向删除当前节点与所有邻接节点的边
            for (int v : neighbors) {
                adj[v].erase(u);
            }
            adj[u].clear(); // 清空当前节点的所有边
        }
        // 生成当前图的唯一标识，判断是否出现过
        string currentMap = tostr(adj, n);
        if (history.find(currentMap) == history.end()) {
            cout << "new" << endl; // 新图结构
            history.insert(currentMap);
        } else {
            cout << "old" << endl; // 重复的图结构
        }
    }
    return 0;
}

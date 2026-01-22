# 题解：[New Year's Gifts](https://codeforces.com/contest/2182/problem/E)

## 1. 题目大意与核心转换
**问题描述**：
Monocarp 要让 $n$ 个朋友开心。每个人开心有两个条件，满足**任意一个**即可：

1. **盒子条件**：送他的礼物装在一个美丽值至少为 $x_i$ 的盒子里（共有 $m$ 个盒子，美丽值为 $a_j$）。
2. **金钱条件**：送他的礼物价值至少为 $z_i$（基础花费 $y_i$ 必须支付，若通过金钱让他开心，需额外支付 $z_i - y_i$）。
总预算为 $k$。必须给每个人买礼物（即 $\sum y_i$ 是必选支出）。求最多能让多少人开心。

**预处理与转换**：
首先，每个人必须收到礼物，所以我们先从总预算 $k$ 中扣除所有人的基础花费 $\sum y_i$。
此时，让第 $i$ 个人开心的代价变为：

- **方案 A（用盒子）**：消耗一个美丽值 $\ge x_i$ 的盒子，额外金钱花费 $0$。
- **方案 B（用钱）**：消耗额外金钱 $w_i = z_i - y_i$，不消耗盒子。

我们的目标是在资源（有限的盒子 + 剩余的 $k$）限制下，最大化开心人数。

---

## 2. 贪心策略

我们有两种资源：

1. **特殊的盒子**：只能给 $x_i \le a_j$ 的人，且不用花钱。
2. **通用的钱**：可以给任何人，但每个人代价不同。

**核心思路**：
由于“钱”是通用的，而“盒子”是有门槛的，且盒子能帮我们省下具体的金额。为了让最后剩下的钱能通过“方案 B”买更多人的开心，我们应该**尽可能用盒子去满足那些“如果用钱解决会非常贵”的人**。

**具体步骤**：
1. **排序**：将盒子按美丽值从小到大排序；将朋友按对盒子的要求 $x_i$ 从小到大排序。
2. **匹配盒子（省钱阶段）**：
   - 遍历每一个盒子（从小到大）。
   - 对于当前盒子 $a_j$，所有满足 $x_i \le a_j$ 的朋友都是**潜在候选人**。
   - 在所有候选人中，我们应该把这个盒子给谁？
   - **贪心决策**：给那个 $w_i$（即 $z_i - y_i$）**最大**的人。
   - **原因**：因为盒子是免费的，把盒子给 $w_i$ 最大的人，意味着我们省下了最多的钱。这个被满足的人移出后续考虑列表。
3. **补救（花钱阶段）**：
   - 盒子用完后（或所有人都没法用盒子后），我们手头省下了一笔钱 $k$。
   - 此时剩下一些没开心的人，按他们的 $w_i$ 从小到大排序。
   - 用剩余的 $k$ 依次满足他们，直到钱不够为止。

---

## 3. 代码实现

为了实现上述贪心策略的第2步（在动态增加的候选人集合中，每次取出 $w_i$ 最大的），我们可以使用不同的数据结构。这也正是你两份代码的区别所在。

### 做法一：优先队列

**逻辑解析**：

1.  **排序**：盒子 $a$ 升序，朋友 `node` 按 $x$ 升序。
2.  **双指针 + 大根堆**：
    -   枚举每个盒子 `a[i]`。
    -   使用下标 `j`，将所有满足 `node[j].x <= a[i]` 的朋友放入优先队列（大根堆）。
    -   由于盒子也是从小到大遍历的，之前加入堆的朋友对于当前更大的盒子依然满足条件，无需移除。
    -   **取最大**：如果堆不为空，取出堆顶（$w_i$ 最大的那个），标记为用盒子满足（`ans++`，将其 $w_i$ 设为0）。
3.  **收尾**：重新排序剩下的人，用余额 $k$ 贪心购买。

**AC代码**：

```cpp
#include <bits/stdc++.h>
using namespace std;

long long n, m, k;

struct Node {
	int x, w, id;
};

bool cmp1(Node &a, Node &b) {
	return a.x < b.x;
}

//按照w从小到大排序，w为0的排到最后面
bool cmp3(Node &a, Node &b) {
	if (a.w && b.w) {
		return a.w < b.w;
	} else if (a.w) {
		return 1;
	} else if (b.w) {
		return 0;
	} else {
		return a.x < b.x;
	}
}

//自定义排序结构体，用于给优先队列排序
struct cmpNode {
	bool operator()(Node &a, Node &b) {
		return a.w < b.w;
	}
};

int main() {
	std::ios::sync_with_stdio(false);
	std::cin.tie(0), std::cout.tie(0);
	int T;
	cin >> T;
	while (T--) {
		cin >> n >> m >> k;
		vector<int> a(m + 1);
		for (int i = 1; i <= m; i++) {
			cin >> a[i];
		}

		//先将盒子按美丽值从小到大排序
		sort(a.begin() + 1, a.end());
		vector<Node> node(n + 1);

		//预处理，先把w算出来，并用k减去y
		for (int i = 1; i <= n; i++) {
			int y, z;
			cin >> node[i].x >> y >> z;
			node[i].w = z - y;
			k -= y;
		}

		//将朋友按照x（所需盒子美丽值）从小到大排序
		sort(node.begin() + 1, node.end(), cmp1);
		for (int i = 1; i <= n; i++) {
			node[i].id = i;
		}

		//优先队列，堆顶为w最大的朋友
		priority_queue<Node, vector<Node>, cmpNode> pq;
		int ans = 0;
		for (int i = 1, j = 1; i <= m; i++) {

			//将x小于a[i]的朋友加入到优先队列
			while (j <= n && a[i] >= node[j].x) {
				pq.push(node[j]);
				j++;
			}

			//如果队列不空，则取出堆顶元素
			if (!pq.empty()) {
				Node temp = pq.top();
				ans++;

				//将w设为0代表这个朋友的礼物已经用盒子包好了
				node[temp.id].w = 0;
				pq.pop();
			}
		}

		//将剩下的朋友按照w从小到大排序，w为0表示已经有盒子了，排到最后面
		sort(node.begin() + 1, node.end(), cmp3);
		for (int i = 1; i <= n && k >= node[i].w && node[i].w; i++) {
			ans++;
			k -= node[i].w;
		}
		cout << ans << '\n';
	}

	return 0;
}
```

### 做法二：线段树 + 二分

题目要求能查询区间最大值并能实时更新，考虑用线段树，二分查找一下区间右端点就行了，太好了，又能打板子了！

**逻辑解析**：

1.  **建树**：将所有朋友按 $x$ 排序后，建立一棵线段树。线段树维护区间内 $w_i$ ($z-y$) 的最大值及其对应的 ID。
2.  **查找与更新**：
    -   遍历每个盒子 `a[i]`。
    -   **二分查找**：在有序的朋友列表中，找到满足 $x \le a[i]$ 的右边界 `l`。即区间 `[1, l]` 内的朋友都能用这个盒子。
    -   **线段树查询**：查询区间 `[1, l]` 内 $w_i$ 最大的朋友的 ID。
    -   **线段树更新**：将该朋友的 $w_i$ 更新为 0（代表已满足，不参与后续比较），并统计答案。
3.  **收尾**：同上。

**AC代码**：

```cpp
#include <bits/stdc++.h>
using namespace std;

long long n, m, k;

struct Node {
	int x, w, id;
};

bool cmp1(Node &a, Node &b) {
	return a.x < b.x;
}

//按照w从小到大排序，w为0的排到最后面
bool cmp3(Node &a, Node &b) {
	if (a.w && b.w) {
		return a.w < b.w;
	} else if (a.w) {
		return 1;
	} else if (b.w) {
		return 0;
	} else {
		return a.x < b.x;
	}
}

//建树的函数
void buil(vector<Node> &tree, vector<Node> &node, int i, int l, int r) {
	if (l == r) {
		tree[i] = node[l];
		return;
	}
	int mid = (l + r) >> 1;

	//建左孩子
	buil(tree, node, i << 1, l, mid);

	//建右孩子
	buil(tree, node, i << 1 | 1, mid + 1, r);

	//存的值为w值最大的朋友
	if (tree[i << 1].w > tree[i << 1 | 1].w) {
		tree[i] = tree[i << 1];
	} else {
		tree[i] = tree[i << 1 | 1];
	}
}

//查找函数
int find(vector<Node> &tree, vector<Node> &node, int i, int l, int r, int ql, int qr) {
	if (l == ql && r == qr) {
		return tree[i].id;
	}
	int mid = (l + r) >> 1;
	if (qr <= mid) {

		//查找区间完全在左孩子的区间内
		return find(tree, node, i << 1, l, mid, ql, qr);
	} else if (ql > mid) {

		//查找区间完全在右孩子的区间内
		return find(tree, node, i << 1 | 1, mid + 1, r, ql, qr);
	} else {
		int i1 = find(tree, node, i << 1, l, mid, ql, mid);
		int i2 = find(tree, node, i << 1 | 1, mid + 1, r, mid + 1, qr);
		if (node[i1].w > node[i2].w) {
			return i1;
		} else {
			return i2;
		}
	}
}

//更新函数，将node[qi].w设置为0
void update(vector<Node> &tree, int i, int l, int r, int qi) {
	if (l == r) {
		tree[i].w = 0;
		return;
	}
	int mid = (l + r) >> 1;
	if (qi <= mid) {
		update(tree, i << 1, l, mid, qi);
	} else {
		update(tree, i << 1 | 1, mid + 1, r, qi);
	}
	if (tree[i << 1].w > tree[i << 1 | 1].w) {
		tree[i] = tree[i << 1];
	} else {
		tree[i] = tree[i << 1 | 1];
	}
}

int main() {
	std::ios::sync_with_stdio(false);
	std::cin.tie(0), std::cout.tie(0);
	int T;
	cin >> T;
	while (T--) {
		cin >> n >> m >> k;
		vector<int> a(m + 1);
		for (int i = 1; i <= m; i++) {
			cin >> a[i];
		}

		//先将盒子按美丽值从小到大排序
		sort(a.begin() + 1, a.end());
		vector<Node> node(n + 1);

		//预处理，先把w算出来，并用k减去y
		for (int i = 1; i <= n; i++) {
			int y, z;
			cin >> node[i].x >> y >> z;
			node[i].w = z - y;
			k -= y;
		}

		//将朋友按照x（所需盒子美丽值）从小到大排序
		sort(node.begin() + 1, node.end(), cmp1);
		for (int i = 1; i <= n; i++) {
			node[i].id = i;
		}
		vector<Node> tree((n + 1) << 2);
		buil(tree, node, 1, 1, n);
		int ans = 0;
		for (int i = 1; i <= m; i++) {
			int l = 1, r = n;
			if (a[i] < node[l].x) {
				continue;
			}
			if (a[i] >= node[r].x) {
				l = r;
			}
			while (l < r) {
				int mid = (l + r) >> 1;
				if (a[i] >= node[mid].x) {
					l = mid;
				} else {
					r = mid - 1;
				}
				if (l == r - 1) {
					if (a[i] >= node[r].x) {
						l = r;
					}
					break;
				}
			}

			//经过一个二分找到x不大于a[i]的朋友的最大的下标l
			int qi = find(tree, node, 1, 1, n, 1, l);

			//如果该朋友的w不为0
			if (node[qi].w) {
				ans++;
				node[qi].w = 0;
				update(tree, 1, 1, n, qi);
			}
		}

		//将剩下的朋友按照w从小到大排序，w为0表示已经有盒子了，排到最后面
		sort(node.begin() + 1, node.end(), cmp3);
		for (int i = 1; i <= n && k >= node[i].w && node[i].w; i++) {
			ans++;
			k -= node[i].w;
		}
		cout << ans << '\n';
	}

	return 0;
}
```

**结论**：
这道题考察了贪心思想中的**决策包容性**。因为较美观的盒子能满足要求较低的朋友，而钱可以满足任何人。为了最大化利用资源，我们将“最挑剔的资源”（盒子）按能力从小到大遍历，每次贪心地解决掉“最昂贵的问题”（$z-y$ 最大的人）。
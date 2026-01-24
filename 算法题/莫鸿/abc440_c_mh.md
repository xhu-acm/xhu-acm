https://atcoder.jp/contests/abc440/tasks/abc440_c
There are N squares numbered 1 to N arranged in a line.
Initially, all squares are white, and the cost of painting square i black is C i.

Consider performing the following procedure once to paint some squares black:

Choose a positive integer x freely.
Paint square i black for all integers i satisfying 
1≤i≤N such that the remainder when 
(i+x) is divided by 
2W is less than W.
Find the minimum total cost to perform this procedure.

You are given 
T test cases; solve each of them.

思考：(i+x)%2W<w x是不确定的，没办法直接确定x；但是我们可以确定的是，
(i+x)%2w=(i%2w+x%2w)%2w;对任意x%2w属于[0,2w-1],我们总能找到一个i%2w属于[0,2w-1]，使得(i%2w+x%2w)%2w属于[0,w-1];后面会发现是在0到2w-1这个环上找连续的w个数，然后求和取最小值，就用滑动窗口就行

#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;
#include<vector>
int main() {
	int t;
	cin >> t;
	while(t--) {
		
		int n, x;
		cin >> n >> x;
		vector<long long>a(2*x + 5, 0);
		
		for (int i = 1; i <= n; i++)
		{
			
			int k; cin >> k;
			a[i % (2 * x)] += k;
			//cout << a[i % (2 * x)] << " "<< i % (2 * x)<<" ";

		}
		unsigned long long ans = 0;
		for (int i = 0; i < x; i++) {
			ans += a[i];
			
		}
		//for (int i = 0; i < 2 * x; i++)cout << a[i] << " ";
		//cout << endl;
		
		unsigned long long r = ans;;
        //滑动窗口
		for (int i = 0 ;i <2*x ; i++)
		{
			
		  ans=ans+ a[(i+x)%(2*x)] - a[i];
		//  cout << i << " " << a[i] << " " << a[(i + x) % (2 * x)] << " " << ans << " "<<endl;
		  r = min(ans, r);
		}
		//cout << endl;
		cout << r << endl;
	

	}

	

	return 0;
}
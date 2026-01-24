[Open Silver](https://vjudge.net/problem/POJ-3279)

自己做题时的思考：
第一行的数据可以随便反转，后面的的数据必须要判断前行的数据

题解：
直接使用dfs进行暴力枚举，但是需要判断是否在第一行

最后的代码

```

void dfs(int pos,int flip){
    if(pos==n*m){
        if(isZero()){
            flag = true;
            st.push_back(node{encode(),flip});
        }
        return;
    }
    int x = pos/n,y = pos%n;
    if(x==0){
        //可以反转和不反转
        dfs(pos+1,flip);
        convert(x,y);ans[x][y]++;
        dfs(pos+1,flip+1);
        convert(x,y);ans[x][y]--;
    }else{
        if(mat[x-1][y]==0){
            dfs(pos+1,flip);
        }else{
            convert(x,y);ans[x][y]++;
            dfs(pos+1,flip+1);
            convert(x,y);ans[x][y]--;
        }
    }
}

```
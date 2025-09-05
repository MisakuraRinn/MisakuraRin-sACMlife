# Codeforces Round 1044 (Div. 2)

## [A. Redstone?](https://codeforces.com/contest/2133/problem/A)

- 思路：把公式分析下就可以发现只要首尾齿数相同，就可以使结尾转速为1，找是否存在相同的即可】
  
- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  vector<int>a(n+5,0);
  for(int i=1;i<=n;i++)cin>>a[i];
  sort(a.begin()+1,a.begin()+1+n);
  for(int i=1;i<=n;i++){
    if(a[i]==a[i-1]){
      cout<<"YES"<<endl;
      return;
    }
  }
  cout<<"NO"<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [B. Villagers](https://codeforces.com/contest/2133/problem/B)

- 思路：对于一个大的，只要另一个村民脾气值比他小，那么可以越大越好，而大的肯定是要纳入计算的，所以就有贪心思路：每次找出两个最大的，然后取最大值累加进答案，直到全加完为止

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  vector<int>g(n+5,0);
  for(int i=1;i<=n;i++)cin>>g[i];
  ll ans=0;
  priority_queue<int,vector<int>,less<int>>gmx;
  for(int i=1;i<=n;i++)gmx.push(g[i]);
  for(int i=1;i<n;i++){
    int cur1=gmx.top();
    gmx.pop();
    int cur2=gmx.top();
    gmx.pop();
    ans+=max(cur1,cur2);
    gmx.push(0);
  }
  cout<<ans<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  int t;
  cin>>t;
  while(t--)sol();

  return 0;
}
```

## [C. The Nether](https://codeforces.com/contest/2133/problem/C)

- 思路：给2n次机会查询，首先先找出最长是多长，即每个点能走的最远距离，找出最长路径的起点，用n次机会然后每个点剔除后再查一次，距离不变说明是多余的，去掉，变了就说明应该保留。然后就到了最长路径包含的点，还需要按顺序排列，此时在最开始找最远距离的时候记录一下各个点对应的距离，然后可以通过反证法证明，对于最长路径上的点，从该点开始的最远距离的终点肯定也是最长路径上的终点，这样就可以直接根据最长路径上的每个点的距离来排序，得出前后顺序。

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  int mx=0,x=0;
  vector<int>dis(n+5,0);
  for(int i=1;i<=n;i++){
    cout<<"? ";
    cout<<i<<' ';
    cout<<n<<' ';
    cout<<i<<' ';
    for(int j=1;j<=n;j++){
      if(j==i)continue;
      cout<<j<<' ';
    }
    cout<<endl;
    cout.flush();
    int cur;
    cin>>cur;
    dis[i]=cur;
    if(cur>mx){
      mx=cur;
      x=i;
    }
  }
  vector<int>tmp,ans;
  for(int i=1;i<=n;i++){
    if(i!=x)tmp.push_back(i);
  }
  int cnt=tmp.size();
  for(int &i:tmp){
    cout<<"? ";
    cout<<x<<' ';
    cout<<cnt<<' ';
    cout<<x<<' ';
    for(int &j:tmp){
      if(j==i||j==0)continue;
      cout<<j<<' ';
    }
    cout<<endl;
    cout.flush();
    int cur;
    cin>>cur;
    if(cur==mx){//删了没事的
      i=0;
      cnt--;
    }else{
      // ans[cur]=i;
      ans.push_back(i);
    }
  }
  #define PII pair<int,int>
  vector<PII>res;
  // res.push_back(PII{})
  for(int &i:ans){
    res.push_back(PII{dis[i],i});
  }
  sort(res.begin(),res.end());
  cout<<"! ";
  cout<<mx<<' ';
  cout<<x<<' ';
  // for(int i=1;i<mx;i++)cout<<ans[i]<<' ';
  for(auto i=res.rbegin();i!=res.rend();i++){
    cout<<(*i).second<<' ';
  }
  // for(int i:ans)cout<<i<<' ';
  cout.flush();
}
int main(){
  // ios::sync_with_stdio(0);
  // cin.tie(0);
  // cout.tie(0);
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [D. Chicken Jockey](https://codeforces.com/contest/2133/problem/D)

- 思路：首先可以分析得知，我们要尽可能最大化收到的摔落伤害，所以从最顶上一个个清怪绝对不是最优的方案，然后对于一堆怪，清怪的方案可以由两种不同的操作组成，一种是由最底下清，一种是先清中间某个，让其掉下来形成一个新的一堆怪，然后再另作处理。而且分析可以发现，如果我们是按某种顺序依次清某个怪，先把上面的怪清掉再清下面的，必然比先清下面使高度降低再请上面的的效率更高，并且清上面的不影响下面的，也就满足无后效性，在依次设计一个能进行问题分解的状态：f(i)表示清掉i~n的怪的最少攻击数。然后推出状态转移方程`f[i]=h[i]+max(0,h[i+1]-i)+(i+2<=n?i+2-cnt[i+1]+(!gmi.empty()?gmi.front():0):0)`这里用单调队列维护`-j+cnt[j-1]+f[i]`的最小值即可
  
- 赛时反思：做dp题应该先分析题目，把玩一会，然后分析操作的组成，然后选定dp顺序设计出一种没有后效性的状态（可以拆分出状态来确保无后效性）然后再推状态转移方程进而实现。而赛时设计的倒着便利然后看取不取的是有后效性的，上面后取的受下面先取的的影响，所以不能用这个状态来dp

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  vector<int>h(n+5,0);
  for(int i=1;i<=n;i++)cin>>h[i];
  vector<ll>f(n+5,0),cnt(n+5,0);//fi定义为干掉i~n所需的最小攻击数
  deque<ll>gmi;
  for(int i=1;i<=n;i++)cnt[i]=cnt[i-1]+h[i];
  gmi.push_back(-n-1+cnt[n]);
  for(int i=n;i>=1;i--){
    f[i]=h[i]+max(0,h[i+1]-i)+(i+2<=n?i+2-cnt[i+1]+(!gmi.empty()?gmi.front():0):0);
    if(i+1<=n){
      ll cur=-i-1+cnt[i]+f[i+1];
      while(!gmi.empty()&&cur<=gmi.back())gmi.pop_back();
      gmi.push_back(cur);   
    }
  }
  cout<<f[1]<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

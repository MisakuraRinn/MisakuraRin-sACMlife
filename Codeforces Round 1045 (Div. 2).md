## [A. Painting With Two Colors](https://codeforces.com/contest/2134/problem/A)

- 思路：
  因为要对称，而且蓝色能覆盖红色，如果b>=a且n、b奇偶性一样，就可以对称，如果b< a，那么就要a、b、n奇偶性相同才能对称
- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
inline void sol(){
  int n,a,b;
  cin>>n>>a>>b;
  if((b>=a)&&(b%2==n%2)){
    cout<<"YES"<<endl;
    return;
  }else if((a%2==b%2)&&(a%2==n%2)){
    cout<<"YES"<<endl;
    return;
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

## [B. Add 0 or K](https://codeforces.com/contest/2134/problem/B)

- 思路：参考k=1的时候，要么全变为奇数，要么全变为偶数，考虑对(k+1)进行取模，那么每加一次k，模后就会减一，同一加到模值为0即可。但不过题目还有一种取小质数使得g、k互质（因为互质）然后直接加k加到为可以为g倍数为止

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
inline void sol(){
  int n;
  ll k;
  cin>>n>>k;
  vector<ll>a(n+5,0);
  for(int i=1;i<=n;i++)cin>>a[i];
  for(int i=1;i<=n;i++){
    a[i]+=(a[i]%(k+1))*k;
    cout<<a[i]<<' ';
  }cout<<endl;
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

```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  int t;
  cin >> t;
  while (t--) {
    int n, k;
    cin >> n >> k;
    vector<long long> a(n);
    for (auto &i : a) {
      cin >> i;
    }

    for (int g = 2;; g++) {
      if (gcd(g, k) != 1)
        continue;
      for (auto &i : a) {
        while (i % g != 0)
          i += k;
      }
      for (auto i : a)
        cout << i << ' ';
      cout << '\n';
      break;
    }
  }
}
```

## [C. Even Larger](https://codeforces.com/contest/2134/problem/C)

- 思路：发现可以把子数组划分为3个一组，偶数下标在三个数的中间，然后要求中间的数大于两边的数之和，由于不能减到负数，前面的数如果大于中间的数就先减前面的数，否则根据贪心，优先减后面的数

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
inline void sol(){
  int n;
  cin>>n;
  vector<int>a(n+5,0);
  ll ans=0;
  for(int i=1;i<=n;i++)cin>>a[i];
  for(int i=2;i<=n;i+=2){
    if(a[i]<=a[i-1]+a[i+1]){
      if(a[i-1]>a[i]){
        ans+=a[i-1]-a[i];
        a[i-1]=a[i];
      }
      ans+=a[i-1]+a[i+1]-a[i];
      a[i+1]-=a[i-1]+a[i+1]-a[i];
    }
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

## [D. Sliding Tree](https://codeforces.com/contest/2134/problem/D)

- 思路：分析发现题目应该是和树的链、直径相关，每次操作最多使树的直径长度增加一，而增加到n时就肯定时到目标状态了，而进一步分析发现，如果是在直径所在链上找a、b，c定位b所在节点非直径上节点时，变换一定可以使直径加一，所以找直径然后跑一遍就可以了
  
- 赛时反思：找最短链来左右挪的思路在很多样例下有问题，这种贪心类题目可以先想想可能考虑哪些性质，找到问题的本质
  
```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
const int MAXN=2e5+5;
using namespace std;
int n;
struct EDGE{
  int u,v,nxt;
}e[MAXN<<1];
vector<int>head;
int tot=0;
void addedge(int u,int v){
  e[++tot]={u,v,head[u]},head[u]=tot;
}
int dis[MAXN],rcd[MAXN];
void dfs(int u,int fa){
  // debug(u)
  rcd[u]=0;
  dis[u]=0;
  for(int i=head[u];i;i=e[i].nxt){
    int v=e[i].v;
    if(v==fa)continue;
    dfs(v,u);
    if(dis[v]+1>dis[u]){
      dis[u]=dis[v]+1;
      rcd[u]=v;
    }
  }
}
inline void sol(){
  cin>>n;
  head.assign(n+5,0);
  vector<int>in(n+5,0);
  tot=0;
  for(int i=1;i<n;i++){
    int u,v;
    cin>>u>>v;
    addedge(u,v);
    addedge(v,u);
    in[v]++;
    in[u]++;
  }
  dfs(1,0);
  // for(int i=1;i<=n;i++)cout<<rcd[i]<<' ';
  // cout<<endl;
  int u=1;
  while(u){
    if(!rcd[u])break;
    u=rcd[u];
  }
  // debug(u);
  vector<int>lc;
  vector<bool>vis(n+5,0);
  dfs(u,0);
  while(u){
    lc.push_back(u);
    vis[u]=1;
    u=rcd[u];
  }
  // vdebug(lc);
  for(int i=0;i<lc.size();i++){
    if(in[lc[i]]>2){
      int tmp=lc[i];
      for(int j=head[tmp];j;j=e[j].nxt){
        int v=e[j].v;
        if(vis[v])continue;
        cout<<lc[i-1]<<' '<<lc[i]<<' '<<v<<endl;
        return;
      }
    }
  }
  
  cout<<-1<<endl;
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

## [A. Painting With Two Colors](https://codeforces.com/contest/2134/problem/A)

- ˼·��
  ��ΪҪ�Գƣ�������ɫ�ܸ��Ǻ�ɫ�����b>=a��n��b��ż��һ�����Ϳ��ԶԳƣ����b< a����ô��Ҫa��b��n��ż����ͬ���ܶԳ�
- ��ʱ��˼��

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

- ˼·���ο�k=1��ʱ��Ҫôȫ��Ϊ������Ҫôȫ��Ϊż�������Ƕ�(k+1)����ȡģ����ôÿ��һ��k��ģ��ͻ��һ��ͬһ�ӵ�ģֵΪ0���ɡ���������Ŀ����һ��ȡС����ʹ��g��k���ʣ���Ϊ���ʣ�Ȼ��ֱ�Ӽ�k�ӵ�Ϊ����Ϊg����Ϊֹ

- ��ʱ��˼��

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

- ˼·�����ֿ��԰������黮��Ϊ3��һ�飬ż���±������������м䣬Ȼ��Ҫ���м�����������ߵ���֮�ͣ����ڲ��ܼ���������ǰ�������������м�������ȼ�ǰ��������������̰�ģ����ȼ��������

- ��ʱ��˼��

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

- ˼·������������ĿӦ���Ǻ���������ֱ����أ�ÿ�β������ʹ����ֱ����������һ�������ӵ�nʱ�Ϳ϶�ʱ��Ŀ��״̬�ˣ�����һ���������֣��������ֱ������������a��b��c��λb���ڽڵ��ֱ���Ͻڵ�ʱ���任һ������ʹֱ����һ��������ֱ��Ȼ����һ��Ϳ�����
  
- ��ʱ��˼���������������Ų��˼·�ںܶ������������⣬����̰������Ŀ������������ܿ�����Щ���ʣ��ҵ�����ı���
  
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

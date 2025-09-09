# Codeforces Round 1043 (Div. 3)

## [A. Homework](https://codeforces.com/contest/2132/problem/A)

- 思路：根据题意插入字符即可

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
  string a;
  cin>>a;
  int m;
  cin>>m;
  string b,c;
  cin>>b>>c;
  for(int i=0;i<m;i++){
    if(c[i]=='V'){
      a=b[i]+a;
    }else a+=b[i];
  }
  cout<<a<<endl;
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

## [B. The Secret Number](https://codeforces.com/contest/2132/problem/B)

- 思路：根据题意，即找x，满足11x=n,101x=n，1001x=n......的x的即可

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
ll pow10[20];
void sol(){
  ll n;
  cin>>n;
  vector<ll>ans;
  for(int i=18;i>=1;i--){
    if(n%(pow10[i]+1))continue;
    ll res=n/(pow10[i]+1);
    // if(res)cout<<res<<' ';
    if(res)ans.push_back(res);
  }
  cout<<ans.size()<<endl;
  for(auto i:ans)cout<<i<<' ';
  if(ans.size())cout<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  pow10[0]=1;
  for(int i=1;i<=18;i++){
    pow10[i]=pow10[i-1]*10;
  }
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [C1. The Cunning Seller (easy version)](https://codeforces.com/contest/2132/problem/C1)

- 思路：题目要求交易次数最少，那么最小交易次数就肯定是对应3进制的各位数之和，那么就按照题目给出的算式统计最终花费的硬币之和即可

- 赛时反思：

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
const int MAXN=1e9;
ll pow3[50];
void sol(){
  int n;
  cin>>n;
  int x=0;
  ll ans=0;
  while(n){
    ans+=(n%3)*(pow3[x+1]+x*pow3[x-1]);
    n/=3;
    x++;
  }
  cout<<ans<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  pow3[0]=1;
  for(int i=1;pow3[i-1]<=MAXN;i++){
    pow3[i]=pow3[i-1]*3;
  }
  int t;
  cin>>t;
  while(t--)sol();

  return 0;
}
```

## [C2. The Cunning Seller (hard version)](https://codeforces.com/contest/2132/problem/C2)

- 思路：题目要求改成不超过k次购买次数，首先应该判断最少购买次数是否小于等于k，再用多余的购买次数去进行替换，发现当用3次机会买$x_1-1$的方案比用1次机会买$x_1$省下来$3^{x_1-1}$个硬币，也就是说，尽可能将大的替换成小的，就是最省钱的方案

- 赛时反思：枚举换的“距离”，但是这等价于将$x_1$直接换到$x_1-j$，如果中间有没换的应该优先将中间没换的大的尽可能换成小的，不然会出现因为一次就换到位了，导致最后没购买机会把中间的往小的换的情况了

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
const int MAXN=1e9;
ll pow3[50];
void sol(){
  int n,k;
  cin>>n>>k;
  int buy=0;
  int tmp=n;
  ll ans=0;
  int x=0;
  vector<ll>cnt(50,0);
  while(tmp){
    ans+=(tmp%3)*(pow3[x+1]+x*pow3[x-1]);
    buy+=tmp%3;
    cnt[x]+=tmp%3;
    tmp/=3;
    x++;
  }
  if(buy>k){
    cout<<-1<<endl;
    return;
  }
  ll rm=k-buy;
  for(int j=1;j<=1;j++){//这里不需要枚举更大的换的“距离”
    for(int i=49;i>=j;i--){
    if(!cnt[i])continue;
      if(!pow3[j]){
        continue;
        
      }
      if(rm+1<pow3[j])continue;
      int cg=min(rm/(pow3[j]-1),cnt[i]);
      ans-=cg*j*pow3[i-1];
      rm-=cg*(pow3[j]-1);
      cnt[i]-=cg;
      cnt[i-j]+=cg*pow3[j];
    }
  }
  cout<<ans<<endl;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  pow3[0]=1;
  for(int i=1;pow3[i-1]<=MAXN;i++){
    pow3[i]=pow3[i-1]*3;
  }
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [D. From 1 to Infinity](https://codeforces.com/contest/2132/problem/D)

- 思路：对于每个给定的k，输出长度为k的序列中的数字之和。那么就可以先求出k位于哪两个数中间（使用二分求，在此之前要写根据数求长度的函数），然后再写根据数求数位上各数之和的函数即可

- 赛时反思：这题纯板子看手速，要提高熟练度

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
ll f[17][10],pow10[17];//前i(0开始)位数的数位和,首位为j
inline ll qry(ll x){//给数x，求1~x的数位和
  ll res=0;
  // x++;
  ll cnt=0;
  for(int i=16;i>=0;i--){
    int tmp=x/pow10[i];
    for(int j=0;j<tmp;j++){
      res+=f[i][j]+cnt*pow10[i];
    }
    cnt+=tmp;
    x%=pow10[i];
  }
  return res;
}
inline ll getlen(ll x){//给数x求，1~x数长度和
  ll res=0;
  int tmp=16;//0对应一位
  while(x<pow10[tmp])tmp--;
  res+=(x-pow10[tmp]+1)*(tmp+1);
  // debug((x-pow10[tmp]+1)*(tmp+1))
  // tmp--;
  // debug(tmp)
  while(tmp){
    ll add=(tmp)*(pow10[tmp]-pow10[tmp-1]);
    // debug(add)
    // debug((pow10[tmp]-(tmp?pow10[tmp-1]:0)))
    res+=add;
    tmp--;
  }
  return res;
}
inline void sol(){
  ll k;
  cin>>k;
  ll l=1,r=1e15,ans1=0;
  while(l<=r){
    ll mid=(l+r)>>1;
    if(getlen(mid)<=k){
      ans1=mid;
      l=mid+1;
    }else r=mid-1;
  }
  l=1,r=1e15;
  ll ans2=0;
  while(l<=r){
    ll mid=(l+r)>>1;
    if(getlen(mid)>=k){
      ans2=mid;
      r=mid-1;
    }else l=mid+1;
  }
  // debug(ans1)
  // debug(ans2)
  ll ans=0;
  ans+=qry(ans1+1);
  k-=getlen(ans1);
  ll tmp=ans2;
  deque<int>bits;
  while(tmp){
    bits.push_front(tmp%10);
    tmp/=10;
  }
  while(k--){
    ans+=bits.front();
    bits.pop_front();
  }
  cout<<ans<<endl;
}
inline void init(){
  pow10[0]=1;
  for(int i=1;i<=16;i++)pow10[i]=pow10[i-1]*10;
  for(int i=1;i<=9;i++)f[0][i]=i;
  for(int i=1;i<=16;i++){
    for(int j=0;j<=9;j++){
      for(int k=0;k<=9;k++){
        f[i][j]+=pow10[i-1]*j+f[i-1][k];
      }
    }
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  init();
  // debug(getlen(111))
  // debug(f[1][1])
  // debug(qry(114+1))
  // int cnt=0;
  // for(int i=1;i<=111;i++){
  //   int tmp=i;
  //   while(tmp){
  //     // debug(tmp%10)
  //     // cnt+=tmp%10;
  //     cnt++;
  //     tmp/=10;
  //   }
  // }
  // debug(cnt)
  int t;
  cin>>t;
  while(t--)sol();

  return 0;
}
```

# 春季集训1

## [A - Easy Problem](https://codeforces.com/problemset/problem/1096/D)

- 主要是想出怎么定义状态
设 $f[i][j]$ 为前i个字符最多连到hard第j个字母的最小模糊度

- 细节问题：例如 $s[i]=h$ 时 $f[i][1]=min(f[i-1][1],f[i-1][0])$
因为如果之前已经连到第一个字母h，那么再接一个字母h是对此没有影响的
然后可以使用滚动数组优化

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define ll long long
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<" ";cout<<endl;
using namespace std;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  int n;
  cin>>n;
  string s;
  cin>>s;
  s=" "+s;
  vector<int>a(n+5,0);
  for(int i=1;i<=n;i++)cin>>a[i];
  vector<ll>f(4,1e18);
  f[0]=0;
  for(int i=1;i<=n;i++){
    if(s[i]=='h'){
      f[1]=min(f[1],f[0]);
      f[0]+=a[i];
    }else if(s[i]=='a'){
      f[2]=min(f[2],f[1]);
      f[1]+=a[i];
    }else if(s[i]=='r'){
      f[3]=min(f[3],f[2]);
      f[2]+=a[i];
    }else if(s[i]=='d'){
      f[3]+=a[i];
    }
  }
  ll ans=1e18;
  for(int i=0;i<=3;i++)ans=min(ans,f[i]);
  cout<<ans;
  return 0;
}
```

## [B - 子集 mex](https://loj.ac/p/4942)

- 首先分析题目中操作的过程，无非就是要一个个凑然后合成上去，初始凑多少个不知道，但是最后肯定只要凑出一个n，那么可以倒着统一处理
先把大的凑了，然后把需要的减去，不足的就记作负数

- 细节问题：极端情况下凑出n的次数是类似斐波那契数列增长的，所以要开`long long`

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define ll long long
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<" ";cout<<endl;
using namespace std;
void sol(){
  int n;
  cin>>n;
  vector<ll>f(n+5,0);
  for(int i=0;i<n;i++)cin>>f[i];
  f[n]=-1;
  ll ans=0;
  for(int i=n;i>=0;i--){
    if(f[i]<0){
      ans+=abs(f[i]);
      for(int j=0;j<i;j++){
        f[j]+=f[i];
      }
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

## [C - Travelling Salesman and Special Numbers](https://codeforces.com/problemset/problem/914/C)

### STL技巧

`__builtin_popcount()`表示括号内数的二进制表示数1的个数

- 总体思路就是数位dp
首先n的二进制位不超过一千位，也就是1的数量不超过一千
那么我们可以先预处理出各个1的数量要转化到1所需的操作次数`f[i]`，然后统计`f[i]=k`中的i可以对应到多少数
那么就是需要求前i位有j个1的数的数量，这就是数位Dp预处理的内容
再用根据n的大小统计累加即可

- 细节问题：记得考虑n本身是否合法
1转化到1的次数是0，所以在k=1时需要特判减一，然后不能在`dp[1][1]`处直接进行特判修改`dp[1][1]=0`，因为例如n是110，k=2，那么修改后会在idx-1=1时少计入一次101
`dp[i][j]`统计的是含前导零的情况，因为你在按位遍历n的时候是有前面接数字的，后面要接含前导零的数才能保证不漏

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define ll long long
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<" ";cout<<endl;
using namespace std;
const int mod=1e9+7;
int f[1005];//预处理出二进制位有i个1要几步到1（不包括1）
ll dp[1005][1005];//前i位有j个1的数的数量
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  string n;
  cin>>n;
  int k;
  cin>>k;
  for(int i=1;i<=1000;i++)f[i]=f[__builtin_popcount(i)]+1;
  
  dp[0][0]=1;//可能会错误的计入1，要注意
  for(int i=1;i<=1000;i++){
    for(int j=0;j<=1000;j++){
      dp[i][j]=dp[i-1][j]+(j?dp[i-1][j-1]:0);
      dp[i][j]%=mod; 
    }
  }
  int cnt1=0;//统计前导1的数量
  ll ans=0;
  for(int i=0;i<n.size();i++){
    int idx=n.size()-i;
    //需要x-cnt1个1,f[x]=k
    if(n[i]=='1'){
      for(int j=cnt1;j<=n.size();j++){
        if(f[j]==k){
          ans+=dp[idx-1][j-cnt1];
          ans%=mod;
        }
      }
      cnt1++;
    }
  }
  if(f[cnt1]==k)ans=(ans+1)%mod;
  if(k!=1)cout<<ans<<endl;
  else cout<<ans-1<<endl;//特判
  return 0;
}
```

## [D - Backpack](https://acm.hdu.edu.cn/showproblem.php?pid=7140)

- 总体思路：背包+bitset
首先从最暴力的情况来设计状态
设`bool f[i][j][k]`为前 $i$ 个物品使用了 $j$ 个空间，价值异或和为是否可达
然后我们发现$j$可以进行线性运算，考虑使用bitset压缩空间和时间
然后可以将$i$滚动掉

- 细节问题：状态转移的时候遍历异或和时不能到1024，最多要1023，不然会溢出到至多2047

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define ll long long
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<" ";cout<<endl;
using namespace std;
const int MAXN =1050;
int n,m;
int v[MAXN],w[MAXN];
bitset<MAXN> f[MAXN];
void sol(){
  cin>>n>>m;
  memset(f,0,sizeof f);
  f[0][0]=1;
  for(int i=1;i<=n;i++)cin>>v[i]>>w[i];
  for(int i=1;i<=n;i++){
    for(int j=0;j<1024;j++){
      f[j]|=(f[j^w[i]]<<v[i]);
    }
  }
  for(int i=1024;i>=0;i--){
    if(f[i][m]){
      cout<<i<<endl;
      return;
    }
  }
  cout<<-1<<endl;
  return;
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

## [E - Non-equal Neighbours](https://codeforces.com/problemset/problem/1585/F)

- 思路1:暴力dp+线段树优化
首先可以比较容易的分析出可行的dp方法，设 $f[i][j]$ 为前\(i\)个数最后一个数是\(j\)的方案数
有转移方程

$$
f[i][j]=\sum_{k=1,k\neq j}^{a_{i-1}}f[i-1][k]=\sum_{k=1}^{a_{i-1}}f[i-1][k]-f[i-1][j]
$$

设 $sum=\sum_{k=1}^{a_{i-1}}f[i-1][k]$
然后类似滚动的思想，先记录 $sum$ 再对 $[1,a_{i-1}]$ 乘 $-1$ ，对 $[a_i+1,MAX\{a_i\}]$ 乘 $0$ ，然后对 $[1,a_i]$ 加 $sum$
就可以使用**线段树**来进行快速的维护

- 细节问题：
  函数放参数时要分析要不要开`long long`，有时候放的是`long long`类型的，结果参数类型是`int`，就会导致溢出
  维护一个可以加和乘的线段树要妥善处理两个tag之间的关系，尤其是在`pushdown`函数中

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define ll long long
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<" ";cout<<endl;
using namespace std;
const ll mod=998244353;
const int MAXN=2e5+5;
struct SGT{
  int lc,rc;
  ll dat,atag,mtag;
  #define lc(x) sgt[x].lc
  #define rc(x) sgt[x].rc
  #define dat(x) sgt[x].dat
  #define atag(x) sgt[x].atag
  #define mtag(x) sgt[x].mtag
  #define mid ((l+r)>>1)
}sgt[MAXN*60];
int root,tot;
int build(){
  tot++;
  lc(tot)=rc(tot)=dat(tot)=atag(tot)=0;
  mtag(tot)=1;
  return tot;
}
void pushdown(int p,int l,int r){
  if(!lc(p))lc(p)=build();
  if(!rc(p))rc(p)=build();
  dat(lc(p))*=mtag(p);dat(lc(p))%=mod;
  dat(rc(p))*=mtag(p);dat(rc(p))%=mod;
  dat(lc(p))+=(mid-l+1)*atag(p);dat(lc(p))%=mod;
  dat(rc(p))+=(r-mid)*atag(p);dat(rc(p))%=mod;
  mtag(lc(p))*=mtag(p);mtag(lc(p))%=mod;
  mtag(rc(p))*=mtag(p);mtag(rc(p))%=mod;
  atag(lc(p))*=mtag(p);atag(lc(p))%=mod;
  atag(rc(p))*=mtag(p);atag(rc(p))%=mod;
  atag(lc(p))+=atag(p);atag(lc(p))%=mod;
  atag(rc(p))+=atag(p);atag(rc(p))%=mod;
  mtag(p)=1;
  atag(p)=0;
  // dat(lc(p))*=mtag(p);dat(lc(p))%=mod;
  // mtag(lc(p))*=mtag(p);mtag(lc(p))%=mod;
  // atag(lc(p))*=mtag(p);atag(lc(p))%=mod;
  // dat(rc(p))*=mtag(p);dat(rc(p))%=mod;
  // mtag(rc(p))*=mtag(p);mtag(rc(p))%=mod;
  // atag(rc(p))*=mtag(p);atag(rc(p))%=mod;
  // atag(p)*=mtag(p);atag(p)%=mod;不能这样写，不然加入先加A再乘B，在多次pushdown的过程中A会反复的乘B
  // mtag(p)=1;
  // dat(lc(p))+=(mid-l+1)*atag(p);dat(lc(p))%=mod;
  // atag(lc(p))+=atag(p);atag(lc(p))%=mod;
  // dat(rc(p))+=(r-mid)*atag(p);dat(rc(p))%=mod;
  // atag(rc(p))+=atag(p);atag(rc(p))%=mod;
  // atag(p)=0;
}
void add(int p,int l,int r,int x,int y,ll k){
  if(l>=x&&r<=y){
    atag(p)+=k;atag(p)%=mod;
    dat(p)+=(r-l+1)*k;dat(p)%=mod;
    return;
  }
  pushdown(p,l,r);
  if(mid>=x)add(lc(p),l,mid,x,y,k);
  if(mid+1<=y)add(rc(p),mid+1,r,x,y,k);
  dat(p)=(dat(lc(p))+dat(rc(p)))%mod;
}
void mul(int p,int l,int r,int x,int y,ll k){
  if(l>=x&&r<=y){
    atag(p)*=k;atag(p)%=mod;//根据乘法分配律，这里要乘dat和atag
    mtag(p)*=k;mtag(p)%=mod;
    dat(p)*=k;dat(p)%=mod;
    return;
  }
  pushdown(p,l,r);
  if(mid>=x)mul(lc(p),l,mid,x,y,k);
  if(mid+1<=y)mul(rc(p),mid+1,r,x,y,k);
  dat(p)=(dat(lc(p))+dat(rc(p)))%mod;//括号注意嵌套关系是否正确
}
// ll qry(int p,int l,int r,int x,int y,ll k)
ll a[MAXN];
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  tot=0;
  root=build();
  int n;
  cin>>n;
  for(int i=1;i<=n;i++)cin>>a[i];
  add(root,1,1e9,1,a[1],1);
  for(int i=2;i<=n;i++){
    ll sum=(dat(root)%mod+mod)%mod;
    // debug(sum)
    mul(root,1,1e9,a[i]+1,1e9,0);
    mul(root,1,1e9,1,a[i-1],-1);
    add(root,1,1e9,1,a[i],sum);
  }
  cout<<(dat(root)%mod+mod)%mod;
  return 0;
}
```

# Codeforces Round 1035 (Div. 2)

## [A. Add or XOR](https://codeforces.com/contest/2119/problem/A)

思路：两种方式处理，一种是加一，一种是异或1，在偶数的时候使用都可以达到加一的效果，在奇数时使用前者加一，后者减一，然后对于a,b的情况进行分类讨论处理即可

``` cpp
#pragma optimize(2)
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int a,b,x,y;
  cin>>a>>b>>x>>y;
  if((a%2==0&&b<a)||(a%2==1&&b<a-1)){
    cout<<-1<<endl;
    return;
  }
  if(a%2==1&&b==a-1)cout<<y<<endl;
  else{
    int ans=0;
    if(a%2==1&&a<b){
      a++;
      ans+=x;
    }
    if(b%2==1&&a<b){
      b--;
      ans+=min(x,y);
    }
    ans+=(b-a)/2*(min(2*x,x+y));
    cout<<ans<<endl;
  }
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

## [B. Line Segments](https://codeforces.com/contest/2119/problem/B)

思路：画图分析可以知道，可以达到的距离是一个连续的区间，分别计算该区间的上下限然后判断是否可行即可

赛时反思：取下限的时候直接取所有的min，忽视了题目的要把所有 $a_i$ 都用上的条件

``` cpp
#pragma optimize(2)
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  ll px,py,qx,qy;
  cin>>px>>py>>qx>>qy;
  vector<ll>a(n+5,0);
  for (int i=1;i<=n;i++)cin>>a[i];
  ll mx=a[1],mi=a[1];
  for(int i=2;i<=n;i++){
    if(mi>=a[i])mi=mi-a[i];
    else{
      //mi<a[i]
      if(mx>=a[i])mi=0;
      else mi=a[i]-mx;//mx<ai
    }
    mx+=a[i];
  }
  // debug(mx)
  // debug(mi)
  ll dis=(px-qx)*(px-qx)+(py-qy)*(py-qy);
  if(dis<=mx*mx&&dis>=mi*mi){
    cout<<"YES"<<endl;
  }else cout<<"NO"<<endl;
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

## [C. A Good Problem](https://codeforces.com/contest/2119/problem/C)

思路：构造题，注意题目要求**字典序最小**，要按位与和与按位异或和相等，对于每位0,1分类讨论分析发现，两个数的时候是不可能有满足条件的结果的。然后分类讨论可以发现，当 $n$ 为奇数时，直接全部放 $l$ 是可以的（异或和会相互抵消，结果为l），当为偶数时，由于前面要字典序最小，所以我们也要尽可能多的放l，但是最终异或和抵消的结果是0，而按位与和不为零，但是我们可以考虑把前 $n-2$ 个等于 $l$ 的数与后 $2$ 个数的 $1$ 位错开,这样按位与的和必然是 $0$ ,然后可以发现将 $l$ 乘 $2$ 然后取**high bit**就是能取到的满足条件的最小的数

赛时反思：一般这种构造都是往一些特殊结果上构造，要多留意

``` cpp
#pragma optimize(2)
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  ll n,l,r,k;
  cin>>n>>l>>r>>k;
  if(n%2==1)cout<<l<<endl;
  else{
    ll tmp=l*2;
    while(tmp-(tmp&(-tmp))>0)tmp-=(tmp&(-tmp));
    if(n>2&&r>=tmp){  
      if(k<=n-2)cout<<l<<endl;
      else cout<<tmp<<endl;
    }else{
      cout<<-1<<endl;
    }
  }
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

## [D. Token Removing](https://codeforces.com/contest/2119/problem/D)

思路：不妨逆向来想，设 $f(i,j)$ $i~n$ 位取了 $j$ 个token的方案数，从第 $n$ 个开始决策是否取，一直取到第 $1$ 个。当取第 $i$ 个token时，因为第 $i$ 个回合能取的token区间为 $[a_i,i]$ ，可以将取token的回合也当作区间右端点来进行分配，这样就\(n-i-j+2\)个

``` cpp
#pragma optimize(2)
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n;
  ll m;
  cin>>n>>m;
  vector<vector<ll>>f(n+5,vector<ll>(n+5,0));
  // f[n][0]=1,f[n][1]=n;
  f[n+1][0]=1;
  for(int i=n;i>=1;i--){
    for(int j=0;j<=n-i+1;j++){
      f[i][j]+=f[i+1][j];
      if(j>0){
        f[i][j]+=((f[i+1][j-1]*(n-i+2-j))%m)*i%m;
      }
      f[i][j]%=m;
    }
  }
  ll ans=0;
  for(int i=0;i<=n;i++){
    ans+=f[1][i];
    ans%=m;
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

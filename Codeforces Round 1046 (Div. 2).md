# Codeforces Round 1046 (Div. 2)

## [A. In the Dream](https://codeforces.com/contest/2136/problem/A)

- 思路：纯贪心，最极限的情况就是一个少的前面放两个多的，如果多的减去少的的两倍后还超过2个，说明不可行

- 赛时反思：明确数学关系，不要弄出错的数学公式

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
bool ck(int a,int b){
  if(a>b)swap(a,b);
  // debug(a)
  // debug(b)
  return (b-2*a)<=2;
}
void sol(){
  int a,b,c,d;
  cin>>a>>b>>c>>d;
  if(ck(a,b)&&ck(c-a,d-b)){
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

## [B. Like the Bitset](https://codeforces.com/contest/2136/problem/B)

- 思路：分析可知，如果有k个相连的1连一起，一定有个区间可以恰好把该连通块全包含，就肯定不满足。如果没有，则将所有0置较大数，1置较小数

- 赛时反思：不要犯唐，这种贪心的题目一般都是分析特例或者显著性质，然后分析共同点

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
void sol(){
  int n,k;
  cin>>n>>k;
  string s;
  cin>>s;
  s=" "+s;
  vector<int>f(n+5,0);
  for(int i=1;i<=n;i++){
    if(s[i]=='1'){
      f[i]=f[i-1]+1;
    }else f[i]=0;
    if(f[i]>=k){
      cout<<"NO"<<endl;
      return;
    }
  }
  int cur=n;
  vector<int>ans(n+5,0);
  for(int i=1;i<=n;i++){
    if(s[i]=='0'){
      ans[i]=cur--;
    }
  }
  for(int i=1;i<=n;i++){
    if(s[i]=='1'){
      ans[i]=cur--;
    }
  }
  cout<<"YES"<<endl;
  for(int i=1;i<=n;i++)cout<<ans[i]<<' ';
  cout<<endl;
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

## [D. For the Champion](https://codeforces.com/contest/2136/problem/D)

- 思路：当锚点只有一个的时候，可以发现，穿过锚点的坐标轴时，前后的曼哈顿距离可以加减算出原坐标。然后多个锚点的情况下，可以先找出最右下角和最左下角的锚点，然后将点挪到最左下角和右下角，通过前后两次的曼哈顿距离结合锚点的坐标就能够算出来了

- 赛时反思：题目看清，不是给所有的锚点的曼哈顿距离，是给最近的

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';cout<<endl;
#define ll long long
using namespace std;
const int MAXN=105;
const ll INF=1e9;
struct node{
  ll x,y;
}p[MAXN];
inline bool cmp1(node a,node b){
  return -a.x-a.y<-b.x-b.y;
}
inline bool cmp2(node a,node b){
  return a.x-a.y<b.x-b.y;
}
void sol(){
  int n;
  cin>>n;
  for(int i=1;i<=n;i++){
    cin>>p[i].x>>p[i].y;
  }
  ll x1,x2,y1,y2;
  sort(p+1,p+1+n,cmp1);
  x1=p[n].x;
  y1=p[n].y;
  sort(p+1,p+1+n,cmp2);
  x2=p[n].x;
  y2=p[n].y;
  ll tmp;
  ll dis1,dis2;
  cout<<"? D "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? D "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? R "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? R "<<INF<<endl;cout.flush();
  cin>>dis1;

  cout<<"? L "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? L "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? L "<<INF<<endl;cout.flush();
  cin>>tmp;
  cout<<"? L "<<INF<<endl;cout.flush();
  cin>>dis2;

  ll x0=(dis1-dis2+x1+x2+y1-y2+4*INF)/2,y0=(dis1+dis2-x1-y1+x2-y2-4*INF)/(-2);
  cout<<"! "<<x0-2*INF<<' '<<y0+2*INF<<endl;
  cout.flush();
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

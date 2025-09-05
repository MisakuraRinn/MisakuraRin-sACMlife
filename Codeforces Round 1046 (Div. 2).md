# Codeforces Round 1046 (Div. 2)

## [A. In the Dream](https://codeforces.com/contest/2136/problem/A)

- ˼·����̰�ģ���޵��������һ���ٵ�ǰ���������ģ������ļ�ȥ�ٵĵ������󻹳���2����˵��������

- ��ʱ��˼����ȷ��ѧ��ϵ����ҪŪ�������ѧ��ʽ

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

- ˼·��������֪�������k��������1��һ��һ���и��������ǡ�ðѸ���ͨ��ȫ�������Ϳ϶������㡣���û�У�������0�ýϴ�����1�ý�С��

- ��ʱ��˼����Ҫ���ƣ�����̰�ĵ���Ŀһ�㶼�Ƿ������������������ʣ�Ȼ�������ͬ��

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

- ˼·����ê��ֻ��һ����ʱ�򣬿��Է��֣�����ê���������ʱ��ǰ��������پ�����ԼӼ����ԭ���ꡣȻ����ê�������£��������ҳ������½Ǻ������½ǵ�ê�㣬Ȼ�󽫵�Ų�������½Ǻ����½ǣ�ͨ��ǰ�����ε������پ�����ê���������ܹ��������

- ��ʱ��˼����Ŀ���壬���Ǹ����е�ê��������پ��룬�Ǹ������

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

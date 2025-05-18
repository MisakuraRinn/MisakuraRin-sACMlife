# Codeforces Round 1025 (Div. 2)

## [A. It's Time To Duel](https://codeforces.com/contest/2109/problem/A)

- 思路：首先从特例分析`0 0`和`1 0 0 1`可以发现有**两个连续的0肯定是不行的**
然后没有`0`是肯定不行的
那么剩下的情况就是诸如`1 0 1 1 1 0 1'这类1中间接若干个0的情况，由于有0向1的区间内加入至少一个胜场，所以1的区间是一定可以实现全1的情况的
也就是说**1的两边至少要有1个0,两个相邻的0不行**
这就印证了我们的贪心策略的正确性

- 赛时反思：一开始方向就偏了导致想的很复杂，应该先由特例入手，确保方向正确，避免进死胡同

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
  int sum=0;
  for(int i=1;i<=n;i++)cin>>a[i],sum+=a[i];
  for(int i=1;i+1<=n;i++){
    if(a[i]+a[i+1]==0){
      cout<<"YES"<<endl;
      return;
    }
  }
  if(sum==n){
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

## [B. Slice to Survive](https://codeforces.com/contest/2109/problem/B)

- 思路：由于最开始第一刀是位置不能动的，后面的位置能任意动，
所以后面就是切一刀剩下较小的位置，然后分别加上剩下的x的空间和y的空间大小的分别的log2的向上取整的值

- 细节问题：不能直接看第一刀切掉的空间的值大小来直接评判第一刀切x还是切y更优，因为最后对刀数的贡献是由**切的所占比例**来决定的
为了方便，直接枚举第一刀切x和第一刀切y的情况的刀数然后取最小值就好了

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';
#define ll long long
using namespace std;
void sol(){
  int n,m,a,b;
  cin>>n>>m>>a>>b;
  int x=min(a,n-a+1),y=min(b,m-b+1);
  // debug
  // cout<<ceil(log2(x))<<endl;
  // debug(x)
  // debug
  cout<<min(1+ceil(log2(x))+ceil(log2(m)),1+ceil(log2(n))+ceil(log2(y)))<<endl;
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

## [C1. Hacking Numbers (Easy Version)](https://codeforces.com/contest/2109/problem/C1)

- 思路：交互题，x最开始的范围很大且不确定，然后最后要到n，所以大题想法就是通过若干操作使其确定为一个数，然后再直接一步变成n


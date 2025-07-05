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
缩范围最显著的显然是digit命令，一步digit能将n从[1,1e9]缩小至[1,81]，然后再来一步digit就缩小至[1,16],
但是如果我们再来一步digit就只能缩小至[1,9],除去最后变换到n的操作，只有3步可用，而 $\lceil\log_2 9\rceil=4$ 步数不够二分用，但是如果我们只缩小到[1,16], $\lceil\log_2 16\rceil=4$ 可以省出来一步用来二分，那么就可行

- 细节问题：交互题要注意把数据都读上，不然会影响后面的读取

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  int t;
  for(int i=1;i<=2;i++){
    cout<<"digit"<<endl;
    cin>>t;
  }
  cout<<"add -8"<<endl;
  cin>>t;
  cout<<"add -4"<<endl;
  cin>>t;
  cout<<"add -2"<<endl;
  cin>>t;
  cout<<"add -1"<<endl;
  cin>>t;
  cout<<"mul "<<n<<endl;
  cin>>t;
  cout<<"!"<<endl;
  cin>>t;
  if(t==-1){
    cout<<flush;
    return;
  }
  cout<<flush;
}
int main(){
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [C2. Hacking Numbers (Medium Version)](https://codeforces.com/contest/2109/problem/C2)

- 思路：这次最多只能用4步，主要就是在digit和二分上省步数，由于3的倍数位数和是3的倍数，9的倍数和是9的倍数的特殊性质，
  当我们先mul 9，x的范围变成了[9,9e9],再digit，由于上述性质，范围变成[9,81],再digit，范围变为[9,9],确定下来了，就可以直接赋值了

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  int t;
  cout<<"mul 9"<<endl;
  cin>>t;
  for(int i=1;i<=2;i++){
    cout<<"digit"<<endl;
    cin>>t;
  }
  cout<<"add "<<n-9<<endl;
  cin>>t;
  cout<<"!"<<endl;
  cin>>t;
  if(t==-1){
    cout<<flush;
    return;
  }
  cout<<flush;
}
int main(){
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [C3. Hacking Numbers (Hard Version)](https://codeforces.com/contest/2109/problem/C3)

- 思路：要求设计最优算法，使查询数尽可能小

对于所有 \( x \in [1, 10^d] \)，均有 \( S[(10^d - 1)x] = 9d \)。

**观察：**

\[
x \cdot (10^d - 1) = x \cdot 10^d - x = (x - 1) \cdot 10^d + 10^d - x = (x - 1) \cdot 10^d + [(10^d - 1) - (x - 1)].
\]

- 第一项表示 \( (x - 1) \) 左移 \( d \) 位（即乘以 \( 10^d \)）。
- 第二项是一个由 \( d \) 个 9 组成的数字减去 \( (x - 1) \)：

  \[
  \underbrace{999\ldots999}_{d \text{ 个 9}} - (x - 1).
  \]

因此，对于每个 \( i \)（\( 1 \leq i \leq d \)），第二项的第 \( i \) 位数字与第一项的第 \( i + d \) 位数字互补，二者之和恒为 9。
即数位和为81
然后再直接add n-81即可，刚刚好覆盖了x的范围，**超绝观察力**

然后特殊情况，如果n=81,就不需要再add了，保证最优

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define debug(x) cout<<#x<<" = "<<x<<endl;
#define vdebug(x) cout<<#x<<" : ";for(auto i:x)cout<<i<<' ';
#define ll long long
using namespace std;
void sol(){
  int n;
  cin>>n;
  int t;
  cout<<"mul "<<int(1e9-1)<<endl;
  cin>>t;
  cout<<"digit"<<endl;
  cin>>t;
  if(n!=81){
    cout<<"add "<<n-81<<endl;
    cin>>t;
  }
  cout<<"!"<<endl;
  cin>>t;
  if(t==-1){
    cout<<flush;
    return;
  }
  cout<<flush;
}
int main(){
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

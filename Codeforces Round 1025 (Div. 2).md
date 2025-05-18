# Codeforces Round 1025 (Div. 2)

## [A. It's Time To Duel](https://codeforces.com/contest/2109/problem/A)

- ˼·�����ȴ���������`0 0`��`1 0 0 1`���Է�����**����������0�϶��ǲ��е�**
Ȼ��û��`0`�ǿ϶����е�
��ôʣ�µ������������`1 0 1 1 1 0 1'����1�м�����ɸ�0�������������0��1�������ڼ�������һ��ʤ��������1��������һ������ʵ��ȫ1�������
Ҳ����˵**1����������Ҫ��1��0,�������ڵ�0����**
���ӡ֤�����ǵ�̰�Ĳ��Ե���ȷ��

- ��ʱ��˼��һ��ʼ�����ƫ�˵�����ĺܸ��ӣ�Ӧ�������������֣�ȷ��������ȷ�����������ͬ

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

- ˼·�������ʼ��һ����λ�ò��ܶ��ģ������λ�������⶯��
���Ժ��������һ��ʣ�½�С��λ�ã�Ȼ��ֱ����ʣ�µ�x�Ŀռ��y�Ŀռ��С�ķֱ��log2������ȡ����ֵ

- ϸ�����⣺����ֱ�ӿ���һ���е��Ŀռ��ֵ��С��ֱ�����е�һ����x������y���ţ���Ϊ���Ե����Ĺ�������**�е���ռ����**��������
Ϊ�˷��㣬ֱ��ö�ٵ�һ����x�͵�һ����y������ĵ���Ȼ��ȡ��Сֵ�ͺ���

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

- ˼·�������⣬x�ʼ�ķ�Χ�ܴ��Ҳ�ȷ����Ȼ�����Ҫ��n�����Դ����뷨����ͨ�����ɲ���ʹ��ȷ��Ϊһ������Ȼ����ֱ��һ�����n
����Χ����������Ȼ��digit���һ��digit�ܽ�n��[1,1e9]��С��[1,81]��Ȼ������һ��digit����С��[1,16],
���������������һ��digit��ֻ����С��[1,9],��ȥ���任��n�Ĳ�����ֻ��3�����ã��� $\lceil\log_2 9\rceil=4$ �������������ã������������ֻ��С��[1,16], $\lceil\log_2 16\rceil=4$ ����ʡ����һ���������֣���ô�Ϳ���

- ϸ�����⣺������Ҫע������ݶ����ϣ���Ȼ��Ӱ�����Ķ�ȡ

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

- ˼·��������ֻ����4������Ҫ������digit�Ͷ�����ʡ����������3�ı���λ������3�ı�����9�ı�������9�ı������������ʣ�
  ��������mul 9��x�ķ�Χ�����[9,9e9],��digit�������������ʣ���Χ���[9,81],��digit����Χ��Ϊ[9,9],ȷ�������ˣ��Ϳ���ֱ�Ӹ�ֵ��

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

- ˼·��Ҫ����������㷨��ʹ��ѯ��������С

�������� \( x \in [1, 10^d] \)������ \( S[(10^d - 1)x] = 9d \)��

**�۲죺**

\[
x \cdot (10^d - 1) = x \cdot 10^d - x = (x - 1) \cdot 10^d + 10^d - x = (x - 1) \cdot 10^d + [(10^d - 1) - (x - 1)].
\]

- ��һ���ʾ \( (x - 1) \) ���� \( d \) λ�������� \( 10^d \)����
- �ڶ�����һ���� \( d \) �� 9 ��ɵ����ּ�ȥ \( (x - 1) \)��

  \[
  \underbrace{999\ldots999}_{d \text{ �� 9}} - (x - 1).
  \]

��ˣ�����ÿ�� \( i \)��\( 1 \leq i \leq d \)�����ڶ���ĵ� \( i \) λ�������һ��ĵ� \( i + d \) λ���ֻ���������֮�ͺ�Ϊ 9��
����λ��Ϊ81
Ȼ����ֱ��add n-81���ɣ��ոպø�����x�ķ�Χ��**�����۲���**

Ȼ��������������n=81,�Ͳ���Ҫ��add�ˣ���֤����

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

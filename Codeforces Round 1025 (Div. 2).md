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


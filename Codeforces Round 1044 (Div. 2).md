# Codeforces Round 1044 (Div. 2)

## [A. Redstone?](https://codeforces.com/contest/2133/problem/A)

- ˼·���ѹ�ʽ�����¾Ϳ��Է���ֻҪ��β������ͬ���Ϳ���ʹ��βת��Ϊ1�����Ƿ������ͬ�ļ��ɡ�
  
- ��ʱ��˼��

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
  for(int i=1;i<=n;i++)cin>>a[i];
  sort(a.begin()+1,a.begin()+1+n);
  for(int i=1;i<=n;i++){
    if(a[i]==a[i-1]){
      cout<<"YES"<<endl;
      return;
    }
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

## [B. Villagers](https://codeforces.com/contest/2133/problem/B)

- ˼·������һ����ģ�ֻҪ��һ������Ƣ��ֵ����С����ô����Խ��Խ�ã�����Ŀ϶���Ҫ�������ģ����Ծ���̰��˼·��ÿ���ҳ��������ģ�Ȼ��ȡ���ֵ�ۼӽ��𰸣�ֱ��ȫ����Ϊֹ

- ��ʱ��˼��

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
  vector<int>g(n+5,0);
  for(int i=1;i<=n;i++)cin>>g[i];
  ll ans=0;
  priority_queue<int,vector<int>,less<int>>gmx;
  for(int i=1;i<=n;i++)gmx.push(g[i]);
  for(int i=1;i<n;i++){
    int cur1=gmx.top();
    gmx.pop();
    int cur2=gmx.top();
    gmx.pop();
    ans+=max(cur1,cur2);
    gmx.push(0);
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

## [C. The Nether](https://codeforces.com/contest/2133/problem/C)

- ˼·����2n�λ����ѯ���������ҳ���Ƕ೤����ÿ�������ߵ���Զ���룬�ҳ��·������㣬��n�λ���Ȼ��ÿ�����޳����ٲ�һ�Σ����벻��˵���Ƕ���ģ�ȥ�������˾�˵��Ӧ�ñ�����Ȼ��͵����·�������ĵ㣬����Ҫ��˳�����У���ʱ���ʼ����Զ�����ʱ���¼һ�¸������Ӧ�ľ��룬Ȼ�����ͨ����֤��֤���������·���ϵĵ㣬�Ӹõ㿪ʼ����Զ������յ�϶�Ҳ���·���ϵ��յ㣬�����Ϳ���ֱ�Ӹ����·���ϵ�ÿ����ľ��������򣬵ó�ǰ��˳��

- ��ʱ��˼��

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
  int mx=0,x=0;
  vector<int>dis(n+5,0);
  for(int i=1;i<=n;i++){
    cout<<"? ";
    cout<<i<<' ';
    cout<<n<<' ';
    cout<<i<<' ';
    for(int j=1;j<=n;j++){
      if(j==i)continue;
      cout<<j<<' ';
    }
    cout<<endl;
    cout.flush();
    int cur;
    cin>>cur;
    dis[i]=cur;
    if(cur>mx){
      mx=cur;
      x=i;
    }
  }
  vector<int>tmp,ans;
  for(int i=1;i<=n;i++){
    if(i!=x)tmp.push_back(i);
  }
  int cnt=tmp.size();
  for(int &i:tmp){
    cout<<"? ";
    cout<<x<<' ';
    cout<<cnt<<' ';
    cout<<x<<' ';
    for(int &j:tmp){
      if(j==i||j==0)continue;
      cout<<j<<' ';
    }
    cout<<endl;
    cout.flush();
    int cur;
    cin>>cur;
    if(cur==mx){//ɾ��û�µ�
      i=0;
      cnt--;
    }else{
      // ans[cur]=i;
      ans.push_back(i);
    }
  }
  #define PII pair<int,int>
  vector<PII>res;
  // res.push_back(PII{})
  for(int &i:ans){
    res.push_back(PII{dis[i],i});
  }
  sort(res.begin(),res.end());
  cout<<"! ";
  cout<<mx<<' ';
  cout<<x<<' ';
  // for(int i=1;i<mx;i++)cout<<ans[i]<<' ';
  for(auto i=res.rbegin();i!=res.rend();i++){
    cout<<(*i).second<<' ';
  }
  // for(int i:ans)cout<<i<<' ';
  cout.flush();
}
int main(){
  // ios::sync_with_stdio(0);
  // cin.tie(0);
  // cout.tie(0);
  int t;
  cin>>t;
  while(t--)sol();
  return 0;
}
```

## [D. Chicken Jockey](https://codeforces.com/contest/2133/problem/D)

- ˼·�����ȿ��Է�����֪������Ҫ����������յ���ˤ���˺������Դ����һ������־��Բ������ŵķ�����Ȼ�����һ�ѹ֣���ֵķ������������ֲ�ͬ�Ĳ�����ɣ�һ������������壬һ���������м�ĳ��������������γ�һ���µ�һ�ѹ֣�Ȼ���������������ҷ������Է��֣���������ǰ�ĳ��˳��������ĳ���֣��Ȱ�����Ĺ������������ģ���Ȼ����������ʹ�߶Ƚ�����������ĵ�Ч�ʸ��ߣ�����������Ĳ�Ӱ������ģ�Ҳ�������޺�Ч�ԣ����������һ���ܽ�������ֽ��״̬��f(i)��ʾ���i~n�Ĺֵ����ٹ�������Ȼ���Ƴ�״̬ת�Ʒ���`f[i]=h[i]+max(0,h[i+1]-i)+(i+2<=n?i+2-cnt[i+1]+(!gmi.empty()?gmi.front():0):0)`�����õ�������ά��`-j+cnt[j-1]+f[i]`����Сֵ����
  
- ��ʱ��˼����dp��Ӧ���ȷ�����Ŀ������һ�ᣬȻ�������������ɣ�Ȼ��ѡ��dp˳����Ƴ�һ��û�к�Ч�Ե�״̬�����Բ�ֳ�״̬��ȷ���޺�Ч�ԣ�Ȼ������״̬ת�Ʒ��̽���ʵ�֡�����ʱ��Ƶĵ��ű���Ȼ��ȡ��ȡ�����к�Ч�Եģ������ȡ����������ȡ�ĵ�Ӱ�죬���Բ��������״̬��dp

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
  vector<int>h(n+5,0);
  for(int i=1;i<=n;i++)cin>>h[i];
  vector<ll>f(n+5,0),cnt(n+5,0);//fi����Ϊ�ɵ�i~n�������С������
  deque<ll>gmi;
  for(int i=1;i<=n;i++)cnt[i]=cnt[i-1]+h[i];
  gmi.push_back(-n-1+cnt[n]);
  for(int i=n;i>=1;i--){
    f[i]=h[i]+max(0,h[i+1]-i)+(i+2<=n?i+2-cnt[i+1]+(!gmi.empty()?gmi.front():0):0);
    if(i+1<=n){
      ll cur=-i-1+cnt[i]+f[i+1];
      while(!gmi.empty()&&cur<=gmi.back())gmi.pop_back();
      gmi.push_back(cur);   
    }
  }
  cout<<f[1]<<endl;
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

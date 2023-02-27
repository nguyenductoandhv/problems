<!-- https://www.codechef.com/problems/CHSTR -->
<!-- Difficult: 3 -->

# CHSTR

Chef có một xâu $S$ chỉ chứa $n$ kí tự latinh thường. Anh ấy đã chuẩn bị một danh sách $L$ chứa tất cả các xâu con liên tiếp không rỗng của $S$.

Bây giờ anh ấy có $Q$ câu hỏi. Câu hỏi thứ $i$, bạn cần đếm số cách chọn chính xác $K_i$ xâu bằng nhau từ tập $L$. Kết quả có thể rất lớn nên hãy module cho $10^9+7$

## Input

- Dòng đầu chứa một số nguyên dương $T$ là số bộ test.
- Mỗi bộ test sẽ có định dạng sau:
    - Dòng đầu chứa 2 số nguyên $n, Q$.
    - Dòng 2 chứa xâu $S$.
    - $Q$ dòng tiếp theo, mỗi dòng chứa một số nguyên $K_i$.

## Output

- Với mỗi giá trị $K_i$, in ra kết quả tương ứng trên một dòng mới.

## Constraints

- $1\le T\le 100$
- $1\le N\le 5000$
- $1\le Q\le 10^5$
- $1\le K_i\le 10^9$

## Subtasks

- Subtask 1: $10\%$ điểm
    - $1\le T\le 100$
    - $1\le n,q,k_i\le 5$
- Subtask 2: $20\%$ điểm
    - $T = 1$
    - $1\le n\le 500$
    - $1\le q\le 10^5$
    - $1\le k_i\le 10^9$
- Subtask 2: $70\%$ điểm
    - $T = 1$
    - $1\le n\le 5000$
    - $1\le q\le 10^5$
    - $1\le k_i\le 10^9$
 

## Example

|Input|Output|
|-|-|
|1<br>5 4<br>ababa<br>2<br>1<br>3<br>4|7<br>15<br>1<br>0|

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;

const int N = 2e4 + 5;
const int mod = 1e9 + 7;
const int M = 5e3 + 5;
int C[M][M];

char s[N];
int ch[N][26], fa[N], d[N], ct, last;
int cn[N];
vector<int> G[N];
int newnode();
void ini();
void extend(int);
void dfs(int);

int ans[N];

void init();
void work();
int ca = 1;
int main()
{
  //    freopen("in.txt","r",stdin);
  ios::sync_with_stdio(0);
  cin.tie(0), cout.tie(0);
  init();
  int T;
  cin >> T;
  while (T--)
    work();
  return 0;
}
void work()
{
  int n, q;
  cin >> n >> q;
  ini();
  cin >> (s + 1);
  for (int i = 1; i <= n; i++)
  {
    extend(s[i] - 'a');
    cn[last]++;
  }
  for (int i = 2; i <= ct; i++)
    G[fa[i]].push_back(i);
  dfs(1);
  memset(ans, 0, (n + 1) << 2);
  for (int i = 2; i <= ct; i++)
  {
    for (int j = 1; j <= cn[i]; j++)
    {
      ans[j] = (ans[j] + 1LL * (d[i] - d[fa[i]]) * C[cn[i]][j]) % mod;
    }
  }
  int k;
  while (q--)
  {
    cin >> k;
    if (k > n)
      cout << 0 << '\n';
    else
      cout << ans[k] << '\n';
  }
  for (int i = 1; i <= ct; i++)
    G[i].clear();
}
void dfs(int u)
{
  for (auto v : G[u])
  {
    dfs(v);
    cn[u] += cn[v];
  }
}
void extend(int c)
{
  int p = last, np = newnode();
  d[np] = d[p] + 1, last = np;
  while (p && ch[p][c] == 0)
    ch[p][c] = np, p = fa[p];
  if (!p)
    fa[np] = 1;
  else
  {
    int q = ch[p][c];
    if (d[q] == d[p] + 1)
      fa[np] = q;
    else
    {
      int nq = ++ct;
      memcpy(ch[nq], ch[q], 26 << 2);
      d[nq] = d[p] + 1;
      fa[nq] = fa[q];
      cn[nq] = 0;
      fa[np] = fa[q] = nq;
      while (p && ch[p][c] == q)
        ch[p][c] = nq, p = fa[p];
    }
  }
}
void ini()
{
  ct = 0;
  newnode();
  last = 1;
}
int newnode()
{
  memset(ch[++ct], 0, 26 << 2);
  cn[ct] = 0;
  return ct;
}
void init()
{
  C[0][0] = 1;
  for (int i = 1; i < M; i++)
  {
    C[i][0] = 1;
    for (int j = 1; j <= i; j++)
    {
      C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % mod;
    }
  }
}
```

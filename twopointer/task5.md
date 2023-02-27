<!-- https://codeforces.com/contest/1186/problem/C -->

# Cossack and String

Cossack có 2 xâu nhị phân. Chúng ta gọi chúng là $a$ và $b$. Ta có $|b|\le|a|$, tức là độ dài xâu $b$ không vượt quá độ dài xâu $a$.

Cossack xem xét tất cả các xâu con liên tiếp độ dài $|b|$ trong $a$. Giả sử một xâu thoả mãn là $c$. Anh ấy đặt chúng thẳng hàng với nhau, sau đó anh ấy đếm số lượng vị trí mà 2 xâu có các kí tự tương ứng khác nhau. Chúng ta gọi đây là hàm $f(b,c)$.

Ví dụ, $b=00110$ và $c=11000$, vị trí thứ nhất, thứ 2, thứ 3 và thứ 4 khác nhau.

Cossack muốn biết có bao nhiêu xâu $c$ mà $f(b,c)$ là chẵn.

Ví dụ, với $a=01100010$ và $b=00110$. $a$ có $4$ xâu con độ dài $|b|$ là $01100,11000,10001,00010$.

- $f(00110,01100)=2$.
- $f(00110,11000)=4$.
- $f(00110,10001)=4$.
- $f(00110,00010)=1$.

Từ kết quả tính toán trên, $f(b,c)$ chẵn có $3$ xâu, do đó kết quả là $3$.

Cossack không thể trả lời với các xâu lớn. Đó là lý do anh ấy tìm đến bạn.

## Input

- Dòng đầu chứa xâu nhị phân $a$.
- Dòng 2 chứa xâu nhị phân $b$.

## Output

- In ra một số là số lượng xâu tìm được.

## Constraints

- $1\le |b|\le |a|\le 10^6$.

## Example

|Input|Output|
|-|-|
|01100010<br>00110|3|
|1010111110<br>0110|4|

```cpp
#include <bits/stdc++.h>
using namespace std;
string a, b;
int s, k = 1, i;

int main()
{
  cin >> a >> b;
  for (; b[i]; i++)
    k ^= a[i] ^ b[i];
  for (s = k; a[i]; i++)
    s += k ^= a[i] ^ a[i - b.size()];
  printf("%d\n", s);

  return 0;
}
```
+++
title = "AtCoder Beginner Contest 304"
description = ""
date = 2023-06-02
[taxonomies]
tags = ["競プロ","C++","AtCoder"]
+++

久々にABCに参加したので記事書こうかと思います。
今回はコンテスト内のジャッジ時間が遅すぎて20分延長したうえ、unratedになってしまったのでレート変動はありませんでした。

# A-First Player
- 最年少のindexを起点にしてmodをとりながら全探索

C++実装
```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  vector<string> s(n);

  int mn_index = -1;
  long long mn_age = 9223372036854775807; // MAX_LONG_LONG
  for (int i = 0; i < n; i++) {
    string str;
    long long A;
    cin >> str >> A;

    if (mn_age > A) {
      mn_index = i;
      mn_age = A;
    }

    s[i] = str;
  }

  for (int i = 0; i < n; i++) {
    cout << s[(i + mn_index) % n] << endl;
  }

  return 0;
}
```

Rust実装
```rs
use proconio::input;

fn main() {
    input! {
        n:usize,
        mut ss:[(String,usize);n],
    }
    let mut mn_age: usize = std::usize::MAX;
    let mut mn_index: usize = 0;
    for i in 0..n {
        if mn_age > ss[i].1 {
            mn_index = i;
            mn_age = ss[i].1;
        }
    }
    for i in 0..n {
        println!("{}", ss[(i + mn_index) % n].0)
    }
}
```

# B - Subscribers

- 整数Nを文字列として受け取る
    - うしろの文字を'0'に変えればおｋ

- 整数で受け取る問題でも文字列で受け取ると簡単に解ける場合がある。

C++実装
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
  string s;
  cin >> s;
  if (s.length() <= 3) {
    cout << s << endl;
    return 0;
  }
  int len = s.length() - 3;
  for (int i = 3; i < s.length(); i++) {
    s[i] = '0';
  }
  cout << s << endl;
  return 0;
}
```

# C - Virus

- 一見すると2点のユークリッド距離をとって全探索かなと思う

- 順番に見ても伝染しない人が現れるかも？

- DFSで実装すれば簡潔になる（なお私のコードは素麺な模様）

```c++
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
bool chk[20000] = {false};
vector<ll> x(2000), y(2000);
vector<bool> seen(20000);

ll n, d;

void dfs(ll x1, ll y1, ll i) {
  for (ll l = 0; l < n; l++) {
    if (chk[l] == true)
      continue;
    if ((sqrt(((x1 - x[l]) * (x1 - x[l])) + ((y1 - y[l]) * (y1 - y[l]))) <=
         d) &&
        l != i) {
      chk[l] = true;
      dfs(x[l], y[l], l);
    }
  }
  return;
}

int main() {
  cin >> n >> d;
  chk[0] = true;
  vector<vector<int>> G(n);

  for (int i = 0; i < n; i++) {
    cin >> x[i] >> y[i];
    for (int j = 0; j < n; j++) {
      G[i].push_back(j);
    }
  }
  dfs(x[0], y[0], 0);
  for (int i = 0; i < n; i++) {
    cout << (chk[i] ? "Yes" : "No") << endl;
  }

  return 0;
}
```

# 総評
久々にやったけど安定して茶パフォでした。今年中に緑入れるか怪しい。
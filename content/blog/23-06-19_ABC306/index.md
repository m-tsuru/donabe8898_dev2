+++
title = "AtCoder Beginner Contest 306"
description = ""
date = 2023-06-19
[taxonomies]
tags = ["競プロ","C++","AtCoder"]
+++

また*unrated*だよチ゛キ゛シ゛ョ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛オ゛







はい、心が乱れましたね（ガン萎え）

# A - Echo

- やるだけ

```cpp
int main() {
    int n;
    cin >> n;
    string s;
    cin >> s;
    for (int i = 0; i < n; i++) {
        cout << s[i] << s[i];
    }
    cout << endl;
    return 0;
}
```

# B - Base 2

- 普通に2^nを標準ライブラリに含まれる`pow`関数でやるとオーバーフローします

- 2進数を10進数に変換するならbit演算がおすすめですよ

```cpp
int main() {
    int a[64];
    unsigned long long ans = 0;
    for (int i = 0; i < 64; i++) {
        cin >> a[i];
    }
    for (unsigned long long i = 0; i < 64; i++) {
        if (a[i] == 1) {
            ans += (unsigned long long)1 << i;
        }
    }
    cout << ans << endl;

    return 0;
}
```

# C - Centers

- 3つの変数の真ん中の座標をmapで保持する

- mapはキーを常に昇順で保つので便利。アクセスも`O(log n)`なので高速
    
    
```cpp
int main() {
  int n;
  map<int, int> mp;
  set<int> st;
  cin >> n;
  vector<bool> a_flag(3 * n, false);
  vector<int> a(3 * n);
  for (int i = 0; i < 3 * n; i++) {
    cin >> a[i];
  }
  for (int i = 0; i < 3 * n; i++) {
    if (st.count(a[i]) == 1 && a_flag[a[i]] == false) {
      mp[i] = a[i];
      a_flag[a[i]] = true;
    }
    st.insert(a[i]);
  }
  for (auto [i, j] : mp) {
    cout << j << " ";
  }
  cout << endl;
 
  return 0;
}
```


# D

- 誰がどう見てもDPでした（時間足りなくて実装間に合わなかったけど）

```cpp
int main() {
  i64 n;
  cin >> n;
  vector<i64> x(n), y(n);
  for (i64 i = 0; i < n; i++) {
    cin >> x[i] >> y[i];
  }

  vector<vector<i64>> dp(n + 1, vector<i64>(2, -4e18));
  dp[0][0] = 0;
  for (int i = 0; i < n; i++) {
    // 食べる
    if (x[i] == 0) {
      dp[i + 1][0] = max(max(dp[i][0], dp[i][1]) + y[i], dp[i][0]);
    } else {
      dp[i + 1][1] = max(dp[i][1], dp[i][0] + y[i]);
    }
    // 食べない
    dp[i + 1][0] = max(dp[i + 1][0], dp[i][0]);
    dp[i + 1][1] = max(dp[i + 1][1], dp[i][1]);
  }
  cout << max(dp[n][0], dp[n][1]) << endl;
  return 0;
}
```
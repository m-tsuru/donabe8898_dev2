+++
title = "Rust チートシート"
description = ""
date = 2023-08-11
[taxonomies]
tags = ["競プロ","Rust"]
+++

# はじめに
Rust書いてて使えそうな小ネタ等を投げる場所。随時更新していく。

# イテレータ
- 任意数から任意数までの値を全列挙して格納

```rust
    // 昇順
    let  asc = (num[i]..num[j]).collect();
    // 降順
    let desc = (num[i]..num[j]).rev().collect();
```

- 空白区切りでVec<[i|u]*>を文字列として格納
```rust
    let numbers = vec![5, 10, 15, 20, 25];

    let output: String = numbers
        .iter()
        .map(|&x| x.to_string()) // 整数を文字列に変換
        .collect::<Vec<String>>()
        .join(" "); // スペースで連結
```

# システム

- 処理を強制的に停止(C++だと`return 0;` みたいな)

```rust
    std::process::exit(0);
```
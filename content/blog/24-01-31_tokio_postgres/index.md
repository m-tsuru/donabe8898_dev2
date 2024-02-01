+++
title = "tokio_postgresでUUID使ったらコケまくった話"
description = "DBって、エモいな"
date = 2024-01-28
[taxonomies]
tags = ["Rust"]
+++


RustでPostgreSQLを使ったDiscordBotを開発していたら, UUIDとの兼ね合いで面倒くさいことが起こった.

## その壱 "with-uuid-0_8"を追記しろ

tokio_postgresのpostgres-typesには`features=["hogehoge"]`という何かしらのフューチャーフラグを建てられる項目が有ると思う（~~思うじゃなくて絶対ある。え、無い？ ......煩悩まみれで見えてないだけだから滝行してきて★~~）

そこに`"with-uuid-1"`を追加するだけ. UUIDクレートのバージョンが0.8以前なら`"with-uuid-0_8"`を追記.

```toml
[package]
name = "oresamano_package"
version = "0.1.0"
edition = "2021"


[dependencies]
postgres-types = {version="~0.2.0", features=["derive", "with-serde_json-1", "with-uuid-1"]}
tokio-postgres = "0.7.10"
tokio = { version = "1", features = ["full"] }


[dependencies.uuid]
version = "1.7.0"
features = [
    "v4",                # Lets you generate random UUIDs
    "fast-rng",          # Use a faster (but still sufficiently random) RNG
    "macro-diagnostics", # Enable better diagnostics for compile-time UUIDs
]

```

## その弐 型推論に頼ってはいけない

DBから得たデータを分解する際に, 変数に格納したりするはず. 以下のようにしないとUUID to Stringへ変換できなかった.

```rust
    // row分解
    let mut response = String::new();
    for row in rows {
        let id: String = row.get::<&str, uuid::Uuid>("id").to_string(); // これ。もっといいやり方あれば教えてもらえるとアライグマタスカル
        let tast_name: String = row.get("tast_name");
        let users: String = row.get("users");

        response += &format!("id: {:?}, tast_name: {}, users: {}\n", id, tast_name, users);
    }

```


## URL
[tolio_postgres/rust-doc]("https://docs.rs/postgres-types/0.1.3/postgres_types/trait.ToSql.html")

[RustからPostgreSQLを使う ]("https://unicorn.limited/jp/rd/rust/20200726-postgres.html")
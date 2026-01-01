---
title: "Rust 连接 MongoDB 和 MySQL 数据库完整教程"
date: 2025-05-26T18:34:25+08:00
description: 本文介绍Rust 连接 MongoDB 和 MySQL 数据库的过程。
draft: false
categories:
- 计算机
- 编程
tags:
- Rust
- MongoDB
- MySQL
---

# Rust 连接 MongoDB 和 MySQL 数据库完整教程

## 前言

本文将详细讲解如何在 Rust 项目中连接 MongoDB 和 MySQL 数据库，包括从 `.env` 文件读取配置，初始化连接，以及解决常见的连接和配置问题。文章分为两部分：

- 一、使用 Rust 连接 MongoDB
- 二、使用 Rust 连接 MySQL

---

## 一、Rust 连接 MongoDB

### 1. 项目依赖配置

在`Cargo.toml`中添加所需依赖：

```toml
[dependencies]
mongodb = "2.3.0"    # 官方 MongoDB 驱动
dotenv = "0.15.0"    # 用于加载 .env 文件中的环境变量
tokio = { version = "1", features = ["full"] }  # 异步运行时
```

### 2. 创建 `.env` 文件

在项目根目录新建 `.env`，添加 MongoDB URI 和数据库名：

```
MONGODB_URI=mongodb://localhost:27017
MONGODB_DB_NAME=life_track
```

如果使用 MongoDB Atlas，请将 URI 替换为 Atlas 提供的连接字符串。

### 3. 编写 `database.rs` 来读取配置并建立连接

```rust
use mongodb::{Client, Database, options::ClientOptions};
use dotenv::dotenv;
use std::{env, time::Duration};

pub struct DatabaseManager {
    pub db: Database,
}

impl DatabaseManager {
    pub async fn new() -> Result<Self, mongodb::error::Error> {
        // 加载.env环境变量
        dotenv().ok();

        // 从环境变量读取数据库连接配置
        let mongodb_uri = env::var("MONGODB_URI")
            .unwrap_or_else(|_| "mongodb://localhost:27017".to_string());
        let db_name = env::var("MONGODB_DB_NAME")
            .unwrap_or_else(|_| "life_track".to_string());

        println!("Connecting to MongoDB at: {}", mongodb_uri);

        // 解析客户端配置，设置连接超时
        let mut client_options = ClientOptions::parse(&mongodb_uri).await?;
        client_options.connect_timeout = Some(Duration::from_secs(30));
        client_options.server_selection_timeout = Some(Duration::from_secs(30));
        client_options.max_pool_size = Some(10);

        // 创建客户端
        let client = Client::with_options(client_options)?;

        // 连接测试，提前发现连接问题
        client.list_database_names(None, None).await?; 
        println!("Database connection successful!");

        let database = client.database(&db_name);

        Ok(DatabaseManager { db: database })
    }
}
```

### 4. 在主程序中初始化数据库，保证初始化完成再继续

以 Tauri 应用为例，在 `lib.rs` 中使用阻塞方式确保初始化：

```rust
tauri::async_runtime::block_on(async {
    match DatabaseManager::new().await {
        Ok(db_manager) => {
            app_handle.manage(db_manager);
            println!("Database initialized successfully");
        }
        Err(e) => {
            eprintln!("Failed to initialize database: {}", e);
            std::process::exit(1);
        }
    }
});
```

这样可以防止在数据库连接未准备好时执行数据库操作，避免类似 `"state not managed for field db"` 的错误。

---

## 二、Rust 连接 MySQL

### 1. 项目依赖配置

在 `Cargo.toml` 中添加 MySQL 连接相关依赖：

```toml
[dependencies]
mysql_async = "0.28.0" # 异步 MySQL 驱动
dotenv = "0.15.0"
tokio = { version = "1", features = ["full"] }
```

### 2. 配置 `.env` 文件

```
MYSQL_URI=mysql://username:password@localhost:3306/life_track
```

请根据实际情况修改用户名和密码。

### 3. 编写 `mysql_database.rs` 读取配置并创建连接池

```rust
use mysql_async::{Pool, Conn, prelude::*};
use dotenv::dotenv;
use std::env;

pub struct MySQLManager {
    pub pool: Pool,
}

impl MySQLManager {
    pub async fn new() -> Result<Self, mysql_async::Error> {
        dotenv().ok();

        let mysql_uri = env::var("MYSQL_URI")
            .unwrap_or_else(|_| "mysql://root:password@localhost:3306/life_track".to_string());

        println!("Connecting to MySQL at: {}", mysql_uri);

        // 创建连接池
        let pool = Pool::new(mysql_uri);

        // 测试连接
        let mut conn = pool.get_conn().await?;
        conn.ping().await?;

        println!("MySQL connection successful!");

        Ok(MySQLManager { pool })
    }
}
```

### 4. 在主程序中初始化 MySQL 连接池

```rust
tokio::runtime::Runtime::new()?.block_on(async {
    match MySQLManager::new().await {
        Ok(mysql_manager) => {
            app_handle.manage(mysql_manager);
            println!("MySQL database initialized successfully");
        }
        Err(e) => {
            eprintln!("Failed to initialize MySQL database: {}", e);
            std::process::exit(1);
        }
    }
});
```

---

## 总结

- 通过 `dotenv` 读取 `.env` 文件中的数据库连接配置，方便灵活管理环境参数。
- 配置 MongoDB 客户端连接超时参数提高远程连接稳定性，避免连接Atlas时的问题。
- 使用异步函数并确保数据库完全初始化后再继续业务逻辑，避免状态管理错误。
- 对 MySQL，使用 `mysql_async` 创建连接池并测试连接。
- 运行程序时只需修改 `.env`，无需改动代码即可切换数据库配置和环境。

希望本文能帮你快速上手 Rust 中 MongoDB 和 MySQL 的数据库连接。






# wp-specs

`wp-specs` 提供 WordPress Labs 生态里配置层与插件层共享的“核心规格”类型。crate 用极少的依赖表达 sinks/sources 的基本字段与通配符匹配数组，方便在 CLI、服务端或插件运行时之间传输序列化结构。

## Features

- **Core specs** – `CoreSinkSpec` 与 `CoreSourceSpec` 以 serde 结构描述 name/type/params/tags 等字段，保持 params 扁平以便在各层统一传递。
- **Wildcard helpers** – `WildArray` 封装 `wildmatch::WildMatch`，让通配符列表可以以数组方式序列化和匹配。
- **Serde-first** – 所有公开类型都实现 `Serialize`/`Deserialize`，适用于 JSON/TOML 配置交流。
- **No runtime state** – 只关心静态规格，避免依赖运行期上下文，使 crate 可以被 config loader、插件 factory 或 API crates 复用。

## Installation

```toml
[dependencies]
wp-specs = "0.1"
```

## Usage

```rust
use wp_specs::{CoreSinkSpec, CoreSourceSpec, WildArray};

let sink = CoreSinkSpec {
    name: "stdout".into(),
    kind: "console".into(),
    params: Default::default(),
    filter: None,
    tags: vec!["dev".into()],
};

let source = CoreSourceSpec {
    name: "events".into(),
    kind: "http".into(),
    params: Default::default(),
    tags: vec!["prod".into()],
};

let topics = WildArray::new1(vec!["metrics-*", "events-*"]);
assert!(!topics.is_empty());
```

## License

本项目在 [Elastic License 2.0](LICENSE) 下分发。

## Contribution

Issues 与 PR 欢迎提出，提交前请确保通过 `cargo fmt` / `cargo clippy` / `cargo test`。

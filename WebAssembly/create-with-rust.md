Rustで記述したコードをJavaScriptで動かすまでの手順

```bash
# Final Directory Structure
.
├── Cargo.toml
├── index.html
├── package.json
├── project_name.wasm
├── src
│   └── lib.rs
└── target
    ...
    └── wasm32-unknown-unknown
        └── debug
            ...
            └── project_name.wasm
```

```bash
cargo new --lib project_name
cd project_name
```

```
vi src/lib.rs
```

```rust
#[no_mangle]
pub fn sum(a: i32, b: i32) -> i32 {
    a + b
}
```

```
vi Cargo.toml
```

```toml
[package]
name = "project_name"
version = "0.1.0"
authors = ["penguinshunya <penguinshunya@hotmail.com>"]
edition = "2018"

[dependencies]

[lib]
crate-type = ["cdylib"]
```

```bash
cargo build --target wasm32-unknown-unknown
cp target/wasm32-unknown-unknown/debug/project_name.wasm .
```

```bash
vi index.html
```

```html
<!DOCTYPE html>
<script>
const exports = async (wasm, imports = {}) => {
  return await fetch(wasm)
    .then(response => response.arrayBuffer())
    .then(bytes => WebAssembly.instantiate(bytes, imports))
    .then(results => results.instance.exports);
};
    
(async () => {
  const { sum } = await exports("project_name.wasm");

  document.body.appendChild(document.createTextNode(sum(1, 2)));
})();
</script>
```

```bash
npx http-server -o
```

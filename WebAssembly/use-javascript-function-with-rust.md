テンプレート作成手順: [create-with-rust.md](create-with-rust.md)

```bash
vi src/lib.rs
```

```rust
extern "C" {
    fn sqrt(_: f64) -> f64;
}

#[no_mangle]
pub fn sqrt2(a: f64) -> f64 {
    unsafe { sqrt(a) * 2.0 }
}
```

```bash
cargo build --target=wasm32-unknown-unknown
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
    
const imports = {
  env: {
    sqrt: Math.sqrt
  }
};

(async () => {
  const { sqrt2 } = await exports("project_name.wasm", imports);
  
  document.body.appendChild(document.createTextNode(sqrt2(10))); //=> 6.32455532...
})();
</script>
```

```bash
npx http-server -o
```

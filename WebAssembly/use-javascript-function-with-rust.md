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
const imports = {
  env: {
    sqrt: Math.sqrt
  }
};

(async () => {
  const fetcher = fetch('project_name.wasm');
  const results = await WebAssembly.instantiateStreaming(fetcher, imports);
  const { sqrt2 } = results.instance.exports;
  
  document.body.appendChild(document.createTextNode(sqrt2(10))); //=> 6.32455532...
})();
</script>
```

```bash
npx http-server -o
```

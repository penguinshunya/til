```bash
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk
./emsdk update
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
```

C++をコンパイルするためのコマンド`em++`が使えるようになっている。

```bash
vi index.cpp
```

```c++
#include <iostream>
using namespace std;

int main() {
    cout << "hello, world" << endl;
    return 0;
}
```

```bash
em++ index.cpp -s WASM=1 -o index.html
python3 -m http.server 8000
```

## 参考

### Emscriptenのインストール手順
https://emscripten.org/docs/getting_started/downloads.html

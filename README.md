# rust-hello-cross-zig

A simple Rust "Hello World" with the necessary config for cross-compiling with Zig

(DOESN'T WORK YET)

Adapted from <https://actually.fyi/posts/zig-makes-rust-cross-compilation-just-work/>.

```bash
git clone https://github.com/coolaj86/rust-hello-cross-zig.git

pushd ./rust-hello-cross-zig/

# Add the bin/zig-cc-* to the current PATH
export PATH="$PWD/bin:$PATH"
```

Assuming that you have [`rust`](https://webinstall.dev/rust) and
[`zig`](https://webinstall.dev/zig) installed, try adding a few toolchains and
building manually (`.cargo/config.yaml` is already correctly configured).

```bash
rustup target add aarch64-apple-darwin
CC="zig-cc-aarch64-macos-gnu" cargo build --target aarch64-apple-darwin

rustup target add x86_64-pc-windows-gnu
CC="zig-cc-x86_64-windows-gnu" cargo build --target x86_64-pc-windows-gnu

rustup target add x86_64-unknown-linux-musl
CC="zig-cc-x86_64-linux-musl" cargo build --target x86_64-unknown-linux-musl
```

Cross-compile a few more, if you so desire.

```bash
./bin/rustup-target-add-common

./bin/cargo-build-common
```

## Install Rust & Zig

1. Install `webi`

    ```bash
    curl https://webinstall.dev/ | bash

    export PATH="$HOME/.local/bin:$PATH"
    ```

2. Install `rustup`, `cargo`, and `rustc`

    ```bash
    webi rust

    pathman add ~/.cargo/bin
    export PATH="$HOME/.cargo/bin:$PATH"
    ```

3. Install `zig`

    ```bash
    webi zig

    export PATH="$HOME/.local/opt/zig:$PATH"
    ```

## The Cross Toolchains

Here's a short table of some target host triples for toolchains that Rust & Zig appear to have in common.

| `rustup target list`       | `zig targets \| jq -r '.libc[]'` |
| -------------------------- | -------------------------------- |
| aarch64-apple-darwin       | aarch64-macos-gnu                |
| aarch64-pc-windows-msvc    | -                                |
| -                          | aarch64-windows-gnu              |
| aarch64-unknown-linux-gnu  | aarch64-linux-gnu                |
| aarch64-unknown-linux-musl | aarch64-linux-musl               |
| wasm32-wasi                | wasm32-wasi-musl                 |
| x86_64-apple-darwin        | x86_64-macos-gnu                 |
| x86_64-pc-windows-gnu      | x86_64-windows-gnu               |
| x86_64-pc-windows-msvc     | -                                |
| x86_64-unknown-linux-gnu   | x86_64-linux-gnu                 |
| x86_64-unknown-linux-musl  | x86_64-linux-musl                |

How to install Rust Toolchains:

```bash
#rustup target add <target-host-triple>
rustup target add aarch64-apple-darwin
rustup target add x86_64-unknown-linux-musl
```

## How this was created

```bash
cargo new hello

mkdir -p .cargo/
```

`.cargo/config.toml`:

```toml
[target.aarch64-apple-darwin]
linker = "zig-cc-aarch64-macos-gnu"

[target.aarch64-unknown-linux-gnu]
linker = "zig-cc-aarch64-linux-gnu"

[target.aarch64-unknown-linux-musl]
linker = "zig-cc-aarch64-linux-musl"

[target.x86_64-apple-darwin]
linker = "zig-cc-x86_64-macos-gnu"

[target.x86_64-pc-windows-gnu]
linker = "zig-cc-x86_64-windows-gnu"

[target.x86_64-unknown-linux-gnu]
linker = "zig-cc-x86_64-linux-gnu"

[target.x86_64-unknown-linux-musl]
linker = "zig-cc-x86_64-linux-musl"
```

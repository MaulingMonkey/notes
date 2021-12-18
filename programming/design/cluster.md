# Init Script

```sh
#!/usr/bin
sudo apt update --yes
sudo apt upgrade --yes
sudo apt install curl git --yes
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --default-toolchain none -y
set PATH=~/.cargo/bin:${PATH}
cargo install ...
...
```

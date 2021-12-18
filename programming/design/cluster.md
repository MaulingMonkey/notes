# Boot/Cron

* Shouldn't kick off upgrades on (re)boot - possibly triggered by power loss, exact wrong time to schedule shit
* Should periodically apt update && upgrade via cron?

# Install Script

### Root

```sh
#!/usr/bin
apt update --yes
apt upgrade --yes
apt install curl git --yes
```

### Server

```sh
#!/usr/bin
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --default-toolchain none -y
set PATH=~/.cargo/bin:${PATH}
cargo install ...
...
```

### Update

```sh
#!/usr/bin
rustup update stable
cargo install ...
killall --wait ...
...
```

# Kiosk / Login

* <https://unix.stackexchange.com/questions/253928/how-to-configure-agetty-to-autologon-on-only-one-terminal>

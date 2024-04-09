**__Rust-ESP32__**

This document should help you get the [Esp-32-H2 RISC-V MCU](./docs/esp32-h2_datasheet_en.pdf) running the rust programing language. Currently the esp32-h2 board doesn't support [wifi or coex](https://github.com/esp-rs/esp-wifi/blob/main/esp-wifi/README.md) 



**__Dependices__**
* libssl-dev
* libudev-dev

**__Libraries and ToolChain__**
* [RustUP](https://rustup.rs/) `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
* `rustup component add llvm-tools` debugging 
* `cargo install cargo-binutils`, debugging
* `cargo install cargo-embed` debugging
* `cargo install cargo-espflash`
    * Installs flash tool for flashing device

**__Building and Debuging Project__**
* Generate Project with [std support](https://docs.esp-rs.org/book/overview/using-the-standard-library.html) 
    * `cargo generate esp-rs/esp-idf-template cargo`
* Generate Project with [no std support](https://docs.esp-rs.org/book/overview/using-the-core-library.html)
    * `cargo generate esp-rs/esp-template`
* `cargo build`
    * Builds the project
* `cargo espflash flash --list-all-ports`
    * Flashes newly built project to device on all availible ports.
* `cargo espflash monitor --chip esp32h2 --baud=115200  --port /dev/ttyUSB0`
* `cargo size -- -Ax` 
    * shows linker script output
* [rtt debuging](https://crates.io/crates/rtt-target) currently no working


**__Documentation and Links__**
* [Data Sheet](./docs/esp32-h2_datasheet_en.pdf)
* [Technical Manual](./docs/esp32-h2_technical_reference_manual_en.pdf)
* [Esp IDF Hal](https://github.com/esp-rs/esp-idf-hal)
* [Esp Book](https://docs.esp-rs.org/book/)
* [Rust Target Support](https://doc.rust-lang.org/nightly/rustc/platform-support/esp-idf.html)
    * Have to get `cargo embed` working first
* [GDB](/docs/RISC-V-GDB-tutorial.pdf)
    * Have to get `cargo embed` working first
* [Video](https://www.youtube.com/watch?v=TOAynddiu5M&t=1s) Configuring toolchain for different chip
* [ESP rust-build](https://github.com/esp-rs/rust-build?tab=readme-ov-file#risc-v-installation) RISC-V Installation github repository
* [Pin Out Diagram](https://espressif-docs.readthedocs-hosted.com/projects/esp-dev-kits/en/latest/_images/esp32-h2-devkitm-1-v1.2_pinlayout_20230303.png)


**__Errors__**

* **__Running Command__**: `cargo install cargo-espflash`
    * **__Recieved__**: `failed to run custom build command for libudev-sys v0.1.4`
    * **__Resolution__**: `sudo apt-get install libudev-dev`
* **__Running Command__**: `cargo embed`
    * **__Recieved__**: `Error No supported probe was found`
    * **__Resolution__**: `working on it still`
    ```YAML
    ---
    Notes
    ---
    notes: |
        The cargo embed subcommand looks for a file in the root directory called embed.toml. list chip support with the following command `cargo embed --list-chips`, which doesn't return anything regarding the esp32-h2 risc-v chip. I believe this is going to prevent gdb debugging.
    ```

**__VS_CODE__**
if encountering `#![no_std] can't find crate for test` warning try the follwing

* modify .vscode/settings.json to include the following 
```JSON
{
    "rust-analyzer.check.allTargets": false,
    "rust-analyzer.check.extraArgs": [
        "--target", 
        "riscv32imac-unknown-none-elf"
    ]
}
```

* make sure you are not running vs code from the paraent directory, not sure why but seems to be an issue.

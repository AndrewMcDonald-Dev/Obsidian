
## Kaisy_OS

A simple operating system written purely in Rust based on Philipp Oppermann's blog 
[here](https://os.phil-opp.com/).

I have simply build the kernel and not the the bootloader. That is handled by the [bootloader](https://crates.io/crates/bootloader). Linking the bootloader and my kernel and then automatically launching a [QEMU](https://www.qemu.org/) instance is handled by the [bootimage](https://github.com/rust-osdev/bootimage) application.
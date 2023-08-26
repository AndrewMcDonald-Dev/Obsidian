# Bare-Bones OS

## Freestanding Rust Binary

We have to make a rust binary that does not rely on the `std` library. That allows our binary to run on `bare metal` without an underlying OS.

Features that are restricted when no OS is being used
- Threads
- Files
- Heap Memory
- Network
- Random Numbers
- Standard Output
- Any other features requiring OS abstractions

Features that are not restricted
- Iterators
- Closures
- Pattern Matching
- Option & Result
- String formatting
- Ownership System

### Disabling the Standard Library

Most Rust crates link the standard library which partly depends on the C standard library `libc` , which closely interacts with OS services.

To start, create a new cargo project.

```bash
cargo new kaisy_os --bin --edition 2018
```

Special note we are using the 2018 version of rust.

#### The `no_std` Attribute

```rust
// main.rs

#![no_std]

fn main() {
	println!("Hello, world!");
}
```

When we try to build this the following error occurs:

```bash
error: cannot find macro `println!` in this scope
 --> src/main.rs:4:5
  |
4 |     println!("Hello, world!");
  |     ^^^^^^^
```

The `println!` macro is a part of the standard library so we can not use it.

So let us remove that.
```rust
// main.rs

#![no_std]

fn main() {}
```

We now get the error:
```bash
> cargo build
error: `#[panic_handler]` function required, but not found
error: language item required, but not found: `eh_personality`
```

The compiler is now missing a `#[panic_handler]` function and a *language* item

### Panic Implementation

The `panic_handler` attribute defines the function that the compiler should invoke when a [panic](https://doc.rust-lang.org/stable/book/ch09-01-unrecoverable-errors-with-panic.html) occurs. The standard library provides its own panic handler function, but in a `no_std` environment we need to define it ourselves:
```rust
// in main.rs

use core::panic::PanicInfo;

//This function is called on panic.
#[panic_handler]
fn panic(_info: &PanicInfo) -> ! {
	loop{}
}
```

The [`PanicInfo` parameter](https://doc.rust-lang.org/nightly/core/panic/struct.PanicInfo.html) contains the file and line where the panic happened and the optional panic message. The function should never return, so it is marked as a [diverging function](https://doc.rust-lang.org/1.30.0/book/first-edition/functions.html#diverging-functions) by returning the [“never” type](https://doc.rust-lang.org/nightly/std/primitive.never.html) `!`. There is not much we can do in this function for now, so we just loop indefinitely.

### The `eh_personality` Language Item

Language items are special functions that are needed for some key functionality of the compiler. Another example is the `Copy` trait which tells the compiler which types have *copy semantics*.

The `eh_personality` language item marks a function which implements stack unwinding. This is used for freeing all memory in the case of a panic. To do this though OS-specific libraries are needed, so we don't want that for our OS.

#### Disabling Unwinding

To disable unwinding we simply enable abort on panic which disables the generation of unwinding symbol information and reduces binary size.

```toml
[profile.dev]
panic = "abort"

[profile.release]
panic = "abort"
```

Now we fixed both of the above errors. However when we tried to build another one occurs:

```bash
> cargo build
> error: requires `start` lang_item
```

Our program is missing the `start` language item, which defines the entry point.

### The `start` attribute

One might think that the `main` function is the first function called when you run a program. However, most languages have a [runtime system](https://en.wikipedia.org/wiki/Runtime_system), which is responsible for things such as garbage collection (e.g. in Java) or software threads (e.g. goroutines in Go). This runtime needs to be called before `main`, since it needs to initialize itself.

In a typical Rust binary that links the standard library, execution starts in a C runtime library called `crt0` (“C runtime zero”), which sets up the environment for a C application. This includes creating a stack and placing the arguments in the right registers. The C runtime then invokes the [entry point of the Rust runtime](https://github.com/rust-lang/rust/blob/bb4d1491466d8239a7a5fd68bd605e3276e97afb/src/libstd/rt.rs#L32-L73), which is marked by the `start` language item. Rust only has a very minimal runtime, which takes care of some small things such as setting up stack overflow guards or printing a backtrace on panic. The runtime then finally calls the `main` function.

Our freestanding executable does not have access to the Rust runtime and `crt0`, so we need to define our own entry point. Implementing the `start` language item wouldn’t help, since it would still require `crt0`. Instead, we need to overwrite the `crt0` entry point directly.

#### Overwriting the Entry Point

To tell the RUst compiler that we don't want to use the normal entry pint chain, we add the `#![no_main]` attribute.

```rust
#![no_std]
#![no_main]

use core::panic::PanicInfo;

/// This function is called on panic.
#[panic_handler]
fn panic(_info: &PanicInfo) -> ! {
    loop {}
}
```

We remove the `main` function because it doesn't make sense to have without an underlying runtime that calls it. Instead, we are now overwriting the OS entry point with our own `_start` function:

```rust
#[no_mangle]
pub extern "C" fn _start() -> ! {
    loop {}
}
```

By using the `#[no_mangle]` attribute, we disable [name mangling](https://en.wikipedia.org/wiki/Name_mangling) to ensure that the Rust compiler really outputs a function with the name `_start`. Without the attribute, the compiler would generate some cryptic `_ZN3blog_os4_start7hb173fedf945531caE` symbol to give every function a unique name. The attribute is required because we need to tell the name of the entry point function to the linker in the next step.

We also have to mark the function as `extern "C"` to tell the compiler that it should use the [C calling convention](https://en.wikipedia.org/wiki/Calling_convention) for this function (instead of the unspecified Rust calling convention). The reason for naming the function `_start` is that this is the default entry point name for most systems.

The `!` return type means that the function is diverging, i.e. not allowed to ever return. This is required because the entry point is not called by any function, but invoked directly by the operating system or bootloader. So instead of returning, the entry point should e.g. invoke the [`exit` system call](https://en.wikipedia.org/wiki/Exit_(system_call)) of the operating system. In our case, shutting down the machine could be a reasonable action, since there’s nothing left to do if a freestanding binary returns. For now, we fulfill the requirement by looping endlessly.

When we run `cargo build` now, we get an ugly _linker_ error.

### Linker Errors 

The linker is a program that combines the generated code into an executable. Linux, Windows, and MacOS each have their own linker that throws different errors. The cause of there errors is that the linkers assume the use of the C runtime.

To solve the errors, we need to tell the linker that is should not include the C runtime. To solve this we are going to be building for a bare metal target.

#### Building for a Bare Metal Target

Later we will be using an x86_64 bare metal environment but for now to get it working we will be using an easy to get working embedded ARM system. To add this target we run this command:

```bash
rustup target add thumbv7em-none-eabihf
```

Now we build our executable with this command:

```bash
cargo build --target thumbv7em-none-eabihf
```

Since the target system has no operating system, the linker does not try to link the C runtime and our build succeeds without any linker errors.

### Linker Arguments

A bunch of not really needed information here just the fix needed.

Make a file `.cargo/config.toml` and set this as the data inside.

```toml
[target.'cfg(target_os = "linux")']
rustflags = ["-C", "link-arg=-nostartfiles"]

[target.'cfg(target_os = "windows")']
rustflags = ["-C", "link-args=/ENTRY:_start /SUBSYSTEM:console"]

[target.'cfg(target_os = "macos")']
rustflags = ["-C", "link-args=-e __start -static -nostartfiles"]
```

## A Minimal Rust Kernel

The objective of this section is to build on the last by creating a bootable disk image that prints something to the screen.

### The Boot Process

Process of Execution 
1. Executes firmware code in the motherboard's ROM
2. This code detects available RAM and pre-intializes the CPU and hardware. 
3. Looks for a bootable disk and starts booting the OS

There are two firmware standards on x86 BIOS and UEFI. We will be dealing with BIOS because it is older/simpler and UEFI enabled motherboards can simulate a BIOS boot.

#### BIOS Boot

There is not much needed information here given that creating our own bootloader is out of the scope of this project.

##### The Multiboot Standard

A standard created to simplify booting across many Operating Systems.

#### UEFI

Not going to be used

### A Minimal Kernel

We are going to need to specify an entry point for cargo to properly compile the kernel. This is because by default cargo compiles for the host operating system.

#### Installing Rust Nightly

We will be using Rust Nightly due to some experimental features being necessary.

I have Rust Nightly already installed so I just have to update to the latest version with the command:
```bash
rustup update
```

#### Target Specification

Cargo supports different target systems through the `--target` parameter. This target is a *target triple* which defines various things about the target system including the CPU architecture, the vendor, the OS, and the ABI.

For our target system, we are going to define some special configuration parameters meaning no existing target triples fits. 

We start by creating a x86_64-kaisy_os.json file with this content:
```json
{
    "llvm-target": "x86_64-unknown-none",
    "data-layout": "e-m:e-i64:64-f80:128-n8:16:32:64-S128",
    "arch": "x86_64",
    "target-endian": "little",
    "target-pointer-width": "64",
    "target-c-int-width": "32",
    "os": "none",
    "executables": true
}
```

We changed the OS in the `llvm-target` and the `os` field to `none`.

We add the following build-related entries:
```json
"linker-flavor": "ld.lld",
"linker": "rust-lld",
```

We use the cross-platform LLD linker that is shipped with Rust for linking our kernel.
```json
"panic-strategy": "abort",
```

This setting specifies that the target doesn't support stack unwinding on panic.
```json
"disable-redzone": true
```

We're writing a kernel, and we will handle interrupts at some point. To do that safely we have to disable a certain stack pointer optimization called the *"red zone"*, because it would cause stack corruption otherwise.
```json
"features": "-mmx,-sse,+soft-float",
```

The `features` field enables/disables target features. We disable the `mmx` and `sse` features and enable the `soft-float` feature.

The `mmx` and `sse` features determine support for [Single Instruction Multiple Data (SIMD)](https://en.wikipedia.org/wiki/SIMD) instructions, which can often speed up programs significantly. However, using the large SIMD registers in OS kernels leads to performance problems. The reason is that the kernel needs to restore all registers to their original state before continuing an interrupted program. This means that the kernel has to save the complete SIMD state to main memory on each system call or hardware interrupt. Since the SIMD state is very large (512–1600 bytes) and interrupts can occur very often, these additional save/restore operations considerably harm performance. To avoid this, we disable SIMD for our kernel (not for applications running on top!).

A problem with disabling SIMD is that floating point operation on `x86_64` require SIMD registers by default. TO solve this problem, we add the `soft-float` feature, which emulates all floating point operation through software functions based on normal integers.

##### Putting it Together

Our targets specification file now looks like this:
```json
{
    "llvm-target": "x86_64-unknown-none",
    "data-layout": "e-m:e-i64:64-f80:128-n8:16:32:64-S128",
    "arch": "x86_64",
    "target-endian": "little",
    "target-pointer-width": "64",
    "target-c-int-width": "32",
    "os": "none",
    "executables": true,
    "linker-flavor": "ld.lld",
    "linker": "rust-lld",
    "panic-strategy": "abort",
    "disable-redzone": true,
    "features": "-mmx,-sse,+soft-float"
}
```

#### Building our Kernel

We can now build the kernel for our new target by passing the name of the JSON file as `--target`:
```
> cargo build --target x86_64-blog_os.json

error[E0463]: can't find crate for `core`
```

The error tells us that the Rust compiler no longer finds the `core` library. This library contains basic Rust types such as `Result`, `Option` and iterators, and is implicitly linked to all `no_std` crates.

The problem is that the core library is distributed together with the Rust compiler as a _precompiled_ library. So it is only valid for supported host triples (e.g., `x86_64-unknown-linux-gnu`) but not for our custom target. If we want to compile code for other targets, we need to recompile `core` for these targets first.

##### The `build-std` Option

That's where the `build-std` feature of cargo comes in. It allows to recompile `core` and other standard library crates on demand, instead of using the precompiled versions shipped with the Rust installation. This feature is very new and still not finished, so iti s marked as "unstable" and only available on night Rust compilers.

To use the feature, we need to at this content to our `.cargo/config.toml`:
```toml
[unstable]
build-std = ["core", "compiler_builtins"]
```

This tells cargo that it should recompile the `core` and `compiler_builtins` libraries. The latter is required because it is a dependency or `core`. In order to recompile these libraries, cargo needs access to the rust source code, which we can install with `rustup +nightly component add rust-src`.

Now we can rerun our build command:
```
> cargo +nightly build --target x86_64-blog_os.json
   Compiling core v0.0.0 (/…/rust/src/libcore)
   Compiling rustc-std-workspace-core v1.99.0 (/…/rust/src/tools/rustc-std-workspace-core)
   Compiling compiler_builtins v0.1.32
   Compiling blog_os v0.1.0 (/…/blog_os)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29 secs
```

We see that `cargo build` now recompiles the `core`, `rustc-std-workspace-core`, and `compiler-builtins` libraries for our custom target.

To get rid of the `+nightly` in our command we create a `rust-toolchain.toml` file at the root of the project and put this inside.
```toml
[toolchain]
channel = "nightly"
```

##### Memory-Related Intrinsics

The Rust compiler assumes that a certain set of built-in functions is available for all systems. Most of these functions are provided by the `compiler_builtins` crate that we just recompiled. However, there are some memory-related functions in that crate that are not enabled by default because they are normally provided by the C library on the system. These functions include `memset`, which sets all bytes in a memory block to a given value, `memcpy`, which copies one memory block to another, and `memcmp`, which compares two memory blocks. While we didn’t need any of these functions to compile our kernel right now, they will be required as soon as we add some more code to it (e.g. when copying structs around).

Since we can’t link to the C library of the operating system, we need an alternative way to provide these functions to the compiler. One possible approach for this could be to implement our own `memset` etc. functions and apply the `#[no_mangle]` attribute to them (to avoid the automatic renaming during compilation). However, this is dangerous since the slightest mistake in the implementation of these functions could lead to undefined behavior. For example, implementing `memcpy` with a `for` loop may result in an infinite recursion because `for` loops implicitly call the [`IntoIterator::into_iter`](https://doc.rust-lang.org/stable/core/iter/trait.IntoIterator.html#tymethod.into_iter) trait method, which may call `memcpy` again. So it’s a good idea to reuse existing, well-tested implementations instead.

Fortunately, the `compiler_builtins` crate already contains implementations for all the needed functions, they are just disabled by default to not collide with the implementations from the C library. We can enable them by setting cargo’s [`build-std-features`](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#build-std-features) flag to `["compiler-builtins-mem"]`. Like the `build-std` flag, this flag can be either passed on the command line as a `-Z` flag or configured in the `unstable` table in the `.cargo/config.toml` file. Since we always want to build with this flag, the config file option makes more sense for us:

```toml
[unstable]
build-std-features = ["compiler-builtins-mem"]
build-std = ["core", "compiler_builtins"]
```

Behind the scenes, this flag enables the [`mem` feature](https://github.com/rust-lang/compiler-builtins/blob/eff506cd49b637f1ab5931625a33cef7e91fbbf6/Cargo.toml#L54-L55) of the `compiler_builtins` crate. The effect of this is that the `#[no_mangle]` attribute is applied to the [`memcpy` etc. implementations](https://github.com/rust-lang/compiler-builtins/blob/eff506cd49b637f1ab5931625a33cef7e91fbbf6/src/mem.rs#L12-L69) of the crate, which makes them available to the linker.

With this change, our kernel has valid implementations for all compiler-required functions, so it will continue to compile even if our code gets more complex.

##### Set a Default Target

To avoid passing the `--target` parameter on every invocation of `cargo build`, we can override the default target. TO do this, we add the following to our file at `.cargo/config.toml`:
```toml
[build]
target = "x86_64-kaisy_os.json"
```

We are now ablet to build our kernel for a bare metal target with a simple `cargo build`. However, our `_start` entry point, which will be called by the boot loader, is still empty. It's time that we output something to screen from it.

#### Printing to the Screen

The easiest way to p r int text to the screen at this stage is the VGA text buffer. It is a special memory area mapped to the VGA hardware that contains the contents displayed on screen. It normally consists of 25 lines that each contain 80 character cells. Each character cell displays an ASCII character with some foreground and background colors.

We will discuss the exact layout of the VGA buffer in the next section, where we write a first small driver for it. For printing "Hello World!", we just need to know that the buffer is located at address `0xb8000` and that each character cell consists of an ASCII byte and a color byte.

The implementation looks like this:

```rust
static HELLO: &[u8] = b"Hello World!";

#[no_mangle]
pub extern "C" fn _start() -> ! {
    let vga_buffer = 0xb8000 as *mut u8;

    for (i, &byte) in HELLO.iter().enumerate() {
        unsafe {
            *vga_buffer.offset(i as isize * 2) = byte;
            *vga_buffer.offset(i as isize * 2 + 1) = 0xb;
        }
    }

    loop {}
}
```

First, we cast the integer `0xb8000` into a [raw pointer](https://doc.rust-lang.org/stable/book/ch19-01-unsafe-rust.html#dereferencing-a-raw-pointer). Then we [iterate](https://doc.rust-lang.org/stable/book/ch13-02-iterators.html) over the bytes of the [static](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#the-static-lifetime) `HELLO` [byte string](https://doc.rust-lang.org/reference/tokens.html#byte-string-literals). We use the [`enumerate`](https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.enumerate) method to additionally get a running variable `i`. In the body of the for loop, we use the [`offset`](https://doc.rust-lang.org/std/primitive.pointer.html#method.offset) method to write the string byte and the corresponding color byte (`0xb` is a light cyan).

Note that there’s an [`unsafe`](https://doc.rust-lang.org/stable/book/ch19-01-unsafe-rust.html) block around all memory writes. The reason is that the Rust compiler can’t prove that the raw pointers we create are valid. They could point anywhere and lead to data corruption. By putting them into an `unsafe` block, we’re basically telling the compiler that we are absolutely sure that the operations are valid. Note that an `unsafe` block does not turn off Rust’s safety checks. It only allows you to do [five additional things](https://doc.rust-lang.org/stable/book/ch19-01-unsafe-rust.html#unsafe-superpowers).

I want to emphasize that **this is not the way we want to do things in Rust!** It’s very easy to mess up when working with raw pointers inside unsafe blocks. For example, we could easily write beyond the buffer’s end if we’re not careful.

So we want to minimize the use of `unsafe` as much as possible. Rust gives us the ability to do this by creating safe abstractions. For example, we could create a VGA buffer type that encapsulates all unsafety and ensures that it is _impossible_ to do anything wrong from the outside. This way, we would only need minimal amounts of `unsafe` code and can be sure that we don’t violate [memory safety](https://en.wikipedia.org/wiki/Memory_safety). We will create such a safe VGA buffer abstraction in the next section.

### Running our Kernel

Now that we have an executable that does something perceptible, it is time to run it. First, we need to turn our compiled kernel into a bootable disk image by linking it with a bootloader. Then we can run the disk image in the QEMU virtual machine or boot it on real hardware using a USB stick.

#### Creating a Bootimage

To turn our compiled kernel into a bootable disk image, we need to link it with a bootloader. As we learned in the section about booting, the bootloader is responsible for initializing the CPU and loading our kernel.

Instead of writing our own bootloader, which is a project on its own, we use the `bootloader` crate. This crate implements a basic BIOS bootloader without any C dependencies, just Rust and inline assembly. To use it for booting our kernel, we need to add a dependency on it:
```toml
# in Cargo.toml

[dependencies]
bootloader = "0.9.23"
```

Adding the bootloader as a dependency is not enough to actually create a bootable disk image. The problem is that we need to link our kernel with the bootloader after compilation, but cargo has no support for [post-build scripts](https://github.com/rust-lang/cargo/issues/545).

To solve this problem, we created a tool named `bootimage` that first compiles the kernel and bootloader, and then links them together to create a bootable disk image. To install the tool, execute the following command in your terminal:
```
cargo install bootimage
```

For running `bootimage` and building the bootloader, you need to have the `llvm-tools-preview` rustup component installed. You can do so by executing `rustup component add llvm-tools-preview`.
```
> cargo bootimage
```

We see that the tool recompiles our kernel using `cargo build`, so it will automatically pick up any changes you make. Afterwards, it compiles the bootloader, which might take a while. Like all crate dependencies, it is only built once and then cached, so subsequent builds will be much faster. Finally, `bootimage` combines the bootloader and your kernel into a bootable disk image.

After executing the command, you should see a bootable disk image named `bootimage-kaisy_os.bin` in your `target/x86_64-kaisy_os/debug` directory. You can boot it in a virtual machine or copy it to a USB drive to boot it on real hardware. (Note that this is not a CD image, which has a different format, so burning it to a CD doesn’t work).

##### How does it work?

The `bootimage` tool performs the following steps behind the scenes:

- It compiles our kernel to an [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) file.
- It compiles the bootloader dependency as a standalone executable.
- It links the bytes of the kernel ELF file to the bootloader.

When booted, the bootloader reads and parses the appended ELF file. It then maps the program segments to virtual addresses in the page tables, zeroes the `.bss` section, and sets up a stack. Finally, it reads the entry point address (our `_start` function) and jumps to it.

#### Booting it in QEMU

We can now boot the disk image in a virtual machine. To boot it in [QEMU](https://www.qemu.org/), execute the following command:
```
> qemu-system-x86_64 -drive format=raw,file=target/x86_64-blog_os/debug/bootimage-blog_os.bin
```

#### Using `cargo run`


To make it easier to run our kernel in QEMU, we can set the `runner` configuration key for cargo:

```toml
# in .cargo/config.toml

[target.'cfg(target_os = "none")']
runner = "bootimage runner"
```

The `target.'cfg(target_os = "none")'` table applies to all targets whose target configuration file’s `"os"` field is set to `"none"`. This includes our `x86_64-blog_os.json` target. The `runner` key specifies the command that should be invoked for `cargo run`. The command is run after a successful build with the executable path passed as the first argument. See the [cargo documentation](https://doc.rust-lang.org/cargo/reference/config.html) for more details.

The `bootimage runner` command is specifically designed to be usable as a `runner` executable. It links the given executable with the project’s bootloader dependency and then launches QEMU. See the [Readme of `bootimage`](https://github.com/rust-osdev/bootimage) for more details and possible configuration options.

Now we can use `cargo run` to compile our kernel and boot it in QEMU.
## VGA Text Mode

The [VGA text mode](https://en.wikipedia.org/wiki/VGA-compatible_text_mode) is a simple way to print text to the screen. In this post, we create an interface that makes its usage safe and simple by encapsulating all unsafety in a separate module. We also implement support for Rust’s [formatting macros](https://doc.rust-lang.org/std/fmt/#related-macros).

### The VGA Text Buffer

To print a character to the screen in VGA text mode, one has to write it to the text buffer of the VGA hardware. The VGA text buffer is a two-dimensional array with typically 25 rows and 80 columns, which is directly rendered to the screen. Each array entry describes a single screen character through the following format:

|Bit(s)|Value|
|---|---|
|0-7|ASCII code point|
|8-11|Foreground color|
|12-14|Background color|
|15|Blink|

The first byte represents the character that should be printed in the [ASCII encoding](https://en.wikipedia.org/wiki/ASCII). To be more specific, it isn’t exactly ASCII, but a character set named [_code page 437_](https://en.wikipedia.org/wiki/Code_page_437) with some additional characters and slight modifications. For simplicity, we will proceed to call it an ASCII character in this post.

The second byte defines how the character is displayed. The first four bits define the foreground color, the next three bits the background color, and the last bit whether the character should blink. The following colors are available:

|Number|Color|Number + Bright Bit|Bright Color|
|---|---|---|---|
|0x0|Black|0x8|Dark Gray|
|0x1|Blue|0x9|Light Blue|
|0x2|Green|0xa|Light Green|
|0x3|Cyan|0xb|Light Cyan|
|0x4|Red|0xc|Light Red|
|0x5|Magenta|0xd|Pink|
|0x6|Brown|0xe|Yellow|
|0x7|Light Gray|0xf|White|

Bit 4 is the _bright bit_, which turns, for example, blue into light blue. For the background color, this bit is repurposed as the blink bit.

The VGA text buffer is accessible via [memory-mapped I/O](https://en.wikipedia.org/wiki/Memory-mapped_I/O) to the address `0xb8000`. This means that reads and writes to that address don’t access the RAM but directly access the text buffer on the VGA hardware. This means we can read and write it through normal memory operations to that address.

Note that memory-mapped hardware might not support all normal RAM operations. For example, a device could only support byte-wise reads and return junk when a `u64` is read. Fortunately, the text buffer [supports normal reads and writes](https://web.stanford.edu/class/cs140/projects/pintos/specs/freevga/vga/vgamem.htm#manip), so we don’t have to treat it in a special way.

### A Rust Module

Now that we know how the VGA buffer works, we can create a Rust module to handle printing:
```rust
// in src/main.rs
mod vga_buffer;
```

For the content of this module, we create a new `src/vga_buffer.rs` file. All of the code below goes into our new module (unless specified otherwise)

#### Colors

First, we represent the different colors using an enum:
```rust
// in src/vga_buffer.rs

#[allow(dead_code)]
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
#[repr(u8)]
pub enum Color {
    Black = 0,
    Blue = 1,
    Green = 2,
    Cyan = 3,
    Red = 4,
    Magenta = 5,
    Brown = 6,
    LightGray = 7,
    DarkGray = 8,
    LightBlue = 9,
    LightGreen = 10,
    LightCyan = 11,
    LightRed = 12,
    Pink = 13,
    Yellow = 14,
    White = 15,
}
```


We use a C-like enum here to explicitly specify the number for each color. Because of the `repr(u8)` attribute, each enum variant is stored as a `u8`. Actually 4 bits would be sufficient, but Rust doesn't have a u4 type.

Normally the compiler would issue a warning for each unused variant. By using the `#[allow(dead_code)]` attribute, we disable these warnings for the `Color` enum.

By [deriving](https://doc.rust-lang.org/rust-by-example/trait/derive.html) the [`Copy`](https://doc.rust-lang.org/nightly/core/marker/trait.Copy.html), [`Clone`](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html), [`Debug`](https://doc.rust-lang.org/nightly/core/fmt/trait.Debug.html), [`PartialEq`](https://doc.rust-lang.org/nightly/core/cmp/trait.PartialEq.html), and [`Eq`](https://doc.rust-lang.org/nightly/core/cmp/trait.Eq.html) traits, we enable [copy semantics](https://doc.rust-lang.org/1.30.0/book/first-edition/ownership.html#copy-types) for the type and make it printable and comparable.

To represent a full color code that specifies foreground and background color, we create a [newtype](https://doc.rust-lang.org/rust-by-example/generics/new_types.html) on top of `u8`:

```rust
// in src/vga_buffer.rs

#[derive(Debug, Clone, Copy, PartialEq, Eq)]
#[repr(transparent)]
struct ColorCode(u8);

impl ColorCode {
    fn new(foreground: Color, background: Color) -> ColorCode {
        ColorCode((background as u8) << 4 | (foreground as u8))
    }
}
```

The `ColorCode` struct contains the full color byte, containing foreground and background color. Like before, we derive the `Copy` and `Debug` traits for it. To ensure that the `ColorCode` has the exact same data layout as a `u8`, we use the `repr(transparent}` attribute.

#### Text Buffer

Now we can add structures to represent a screen character and the text buffer:
```rust
// in src/vga_buffer.rs

#[derive(Debug, Clone, Copy, PartialEq, Eq)]
#[repr(C)]
struct ScreenChar {
    ascii_character: u8,
    color_code: ColorCode,
}

const BUFFER_HEIGHT: usize = 25;
const BUFFER_WIDTH: usize = 80;

#[repr(transparent)]
struct Buffer {
    chars: [[ScreenChar; BUFFER_WIDTH]; BUFFER_HEIGHT],
}
```

`repr(c)` attribute makes sure that the structs are laid out exactly like in C. This is because Rust field ordering in default structs is undefined.

To actually write to screen, we now create a writer type:
```rust
// in src/vga_buffer.rs

pub struct Writer {
    column_position: usize,
    color_code: ColorCode,
    buffer: &'static mut Buffer,
}
```

The writer will always write to the last line and shift lines up when a line is full. The `column_position` field keeps track of the current position in the last row. The current foreground and background colors are specified by `color_code` and a reference to VGA buffer is stored in `buffer`. Note that we need an explicit lifetime to tell the compiler how long the reference is valid. The `'static` lifetime specifies that the reference is valid for the whole program run time.

#### Printing

Now we can use the `Writer` to modify the buffer's characters. First we create a method to write a single ASCII byte:
```rust
// in src/vga_buffer.rs

impl Writer {
    pub fn write_byte(&mut self, byte: u8) {
        match byte {
            b'\n' => self.new_line(),
            byte => {
                if self.column_position >= BUFFER_WIDTH {
                    self.new_line();
                }

                let row = BUFFER_HEIGHT - 1;
                let col = self.column_position;

                let color_code = self.color_code;
                self.buffer.chars[row][col] = ScreenChar {
                    ascii_character: byte,
                    color_code,
                };
                self.column_position += 1;
            }
        }
    }

    fn new_line(&mut self) {/* TODO */}
}
```

If the byte is the [newline](https://en.wikipedia.org/wiki/Newline) byte `\n`, the writer does not print anything. Instead, it calls a `new_line` method, which we’ll implement later. Other bytes get printed to the screen in the second `match` case.

When printing a byte, the writer checks if the current line is full. In that case, a `new_line` call is used to wrap the line. Then it writes a new `ScreenChar` to the buffer at the current position. Finally, the current column position is advanced.

To print whole strings, we can convert them to bytes and print them one-by-one:
```rust
// in src/vga_buffer.rs

impl Writer {
    pub fn write_string(&mut self, s: &str) {
        for byte in s.bytes() {
            match byte {
                // printable ASCII byte or newline
                0x20..=0x7e | b'\n' => self.write_byte(byte),
                // not part of printable ASCII range
                _ => self.write_byte(0xfe),
            }

        }
    }
}
```

The VGA text buffer only supports ASCII and the additional bytes of [code page 437](https://en.wikipedia.org/wiki/Code_page_437). Rust strings are [UTF-8](https://www.fileformat.info/info/unicode/utf8.htm) by default, so they might contain bytes that are not supported by the VGA text buffer. We use a `match` to differentiate printable ASCII bytes (a newline or anything in between a space character and a `~` character) and unprintable bytes. For unprintable bytes, we print a `■` character, which has the hex code `0xfe` on the VGA hardware.

##### Try it out!

To write some characters to the screen, you can create a temporary function:
```rust
// in src/vga_buffer.rs

pub fn print_something() {
    let mut writer = Writer {
        column_position: 0,
        color_code: ColorCode::new(Color::Yellow, Color::Black),
        buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
    };

    writer.write_byte(b'H');
    writer.write_string("ello ");
    writer.write_string("Wörld!");
}
```

It first creates a new Writer that points to the VGA buffer at `0xb8000`. The syntax for this might seem a bit strange: First, we cast the integer `0xb8000` as a mutable [raw pointer](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#dereferencing-a-raw-pointer). Then we convert it to a mutable reference by dereferencing it (through `*`) and immediately borrowing it again (through `&mut`). This conversion requires an [`unsafe` block](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html), since the compiler can’t guarantee that the raw pointer is valid.

Then it writes the byte `b'H'` to it. The `b` prefix creates a [byte literal](https://doc.rust-lang.org/reference/tokens.html#byte-literals), which represents an ASCII character. By writing the strings `"ello "` and `"Wörld!"`, we test our `write_string` method and the handling of unprintable characters. To see the output, we need to call the `print_something` function from our `_start` function:
```rust
// in src/main.rs

#[no_mangle]
pub extern "C" fn _start() -> ! {
    vga_buffer::print_something();

    loop {}
}
```

When we run our project now, a `Hello W■■rld!` should be printed in the _lower_ left corner of the screen in yellow:
![[Pasted image 20230627154033.png]]

Notice that the `ö` is printed as two `■` characters. That’s because `ö` is represented by two bytes in [UTF-8](https://www.fileformat.info/info/unicode/utf8.htm), which both don’t fall into the printable ASCII range. In fact, this is a fundamental property of UTF-8: the individual bytes of multi-byte values are never valid ASCII.

#### Volatile

We just saw that our message was printed correctly. However, it might not work with future Rust compilers that optimize more aggressively.

The problem is that we only write to the `Buffer` and never read from it again. The compiler doesn’t know that we really access VGA buffer memory (instead of normal RAM) and knows nothing about the side effect that some characters appear on the screen. So it might decide that these writes are unnecessary and can be omitted. To avoid this erroneous optimization, we need to specify these writes as _[volatile](https://en.wikipedia.org/wiki/Volatile_(computer_programming))_. This tells the compiler that the write has side effects and should not be optimized away.

In order to use volatile writes for the VGA buffer, we use the [volatile](https://docs.rs/volatile) library. This _crate_ (this is how packages are called in the Rust world) provides a `Volatile` wrapper type with `read` and `write` methods. These methods internally use the [read_volatile](https://doc.rust-lang.org/nightly/core/ptr/fn.read_volatile.html) and [write_volatile](https://doc.rust-lang.org/nightly/core/ptr/fn.write_volatile.html) functions of the core library and thus guarantee that the reads/writes are not optimized away.

We can add a dependency on the `volatile` crate by adding it to the `dependencies` section of our `Cargo.toml`:
```toml
# in Cargo.toml

[dependencies]
volatile = "0.2.6"
```

Make sure to specify `volatile` version `0.2.6`. Newer versions of the crate are not compatible with this post. `0.2.6` is the [semantic](https://semver.org/) version number. For more information, see the [Specifying Dependencies](https://doc.crates.io/specifying-dependencies.html) guide of the cargo documentation.

Let’s use it to make writes to the VGA buffer volatile. We update our `Buffer` type as follows:
```rust
// in src/vga_buffer.rs

use volatile::Volatile;

struct Buffer {
    chars: [[Volatile<ScreenChar>; BUFFER_WIDTH]; BUFFER_HEIGHT],
}
```

Instead of a `ScreenChar`, we’re now using a `Volatile<ScreenChar>`. (The `Volatile` type is [generic](https://doc.rust-lang.org/book/ch10-01-syntax.html) and can wrap (almost) any type). This ensures that we can’t accidentally write to it “normally”. Instead, we have to use the `write` method now.

This means that we have to update our `Writer::write_byte` method:
```rust
// in src/vga_buffer.rs

impl Writer {
    pub fn write_byte(&mut self, byte: u8) {
        match byte {
            b'\n' => self.new_line(),
            byte => {
                ...

                self.buffer.chars[row][col].write(ScreenChar {
                    ascii_character: byte,
                    color_code,
                });
                ...
            }
        }
    }
    ...
}
```

Instead of a typical assignment using `=`, we’re now using the `write` method. Now we can guarantee that the compiler will never optimize away this write.

#### Formatting Macros

It would be nice to support Rust's formatting macros, too. That way, we can easily print different types, like integers or floats. To support them, we need to implement the `core::fmt::Write` trait. The only required method of this trait is `wrtie_str`, which looks quite similar to our `write_string` method, just with a `fmt::Result` return type.

```rust
// in src/vga_buffer.rs

use core::fmt;

impl fmt::Write for Writer {
    fn write_str(&mut self, s: &str) -> fmt::Result {
        self.write_string(s);
        Ok(())
    }
}
```

Now we can use Rust’s built-in `write!`/`writeln!` formatting macros:
```rust
// in src/vga_buffer.rs

pub fn print_something() {
    use core::fmt::Write;
    let mut writer = Writer {
        column_position: 0,
        color_code: ColorCode::new(Color::Yellow, Color::Black),
        buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
    };

    writer.write_byte(b'H');
    writer.write_string("ello! ");
    write!(writer, "The numbers are {} and {}", 42, 1.0/3.0).unwrap();
}
```

Now you should see a `Hello! The numbers are 42 and 0.3333333333333333` at the bottom of the screen. The `write!` call returns a `Result` which causes a warning if not used, so we call the [`unwrap`](https://doc.rust-lang.org/core/result/enum.Result.html#method.unwrap) function on it, which panics if an error occurs. This isn’t a problem in our case, since writes to the VGA buffer never fail.

#### Newlines

Right now, we just ignore newlines and characters that don't fit into the line anymore. Instead, we want to move every character one line up (the top line gets deleted) and start at the beginning of the last line again. To do this, we add an implementation for the `new_line` method of `Writer`:
```rust
// in src/vga_buffer.rs

impl Writer {
    fn new_line(&mut self) {
        for row in 1..BUFFER_HEIGHT {
            for col in 0..BUFFER_WIDTH {
                let character = self.buffer.chars[row][col].read();
                self.buffer.chars[row - 1][col].write(character);
            }
        }
        self.clear_row(BUFFER_HEIGHT - 1);
        self.column_position = 0;
    }

    fn clear_row(&mut self, row: usize) {/* TODO */}
}
```

We iterate over all the screen characters and move each character one row up.

To finish the newline code, we add the `clear_row` method:
```rust
// in src/vga_buffer.rs

impl Writer {
    fn clear_row(&mut self, row: usize) {
        let blank = ScreenChar {
            ascii_character: b' ',
            color_code: self.color_code,
        };
        for col in 0..BUFFER_WIDTH {
            self.buffer.chars[row][col].write(blank);
        }
    }
}
```

This method clears a row by overwriting all of its characters with a space character.

### A Global Interface

To  provide a global writer that can be used as an interface from other modules without carrying a `Writer` instance around, we try to create a static `WRTIER`:
```rust
// in src/vga_buffer.rs

pub static WRITER: Writer = Writer {
    column_position: 0,
    color_code: ColorCode::new(Color::Yellow, Color::Black),
    buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
};
```

However, if we try to compile it now, the following errors occur:
```
error[E0015]: calls in statics are limited to constant functions, tuple structs and tuple variants
 --> src/vga_buffer.rs:7:17
  |
7 |     color_code: ColorCode::new(Color::Yellow, Color::Black),
  |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error[E0396]: raw pointers cannot be dereferenced in statics
 --> src/vga_buffer.rs:8:22
  |
8 |     buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
  |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ dereference of raw pointer in constant

error[E0017]: references in statics may only refer to immutable values
 --> src/vga_buffer.rs:8:22
  |
8 |     buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
  |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ statics require immutable values

error[E0017]: references in statics may only refer to immutable values
 --> src/vga_buffer.rs:8:13
  |
8 |     buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ statics require immutable values
```

To understand what’s happening here, we need to know that statics are initialized at compile time, in contrast to normal variables that are initialized at run time. The component of the Rust compiler that evaluates such initialization expressions is called the “[const evaluator](https://rustc-dev-guide.rust-lang.org/const-eval.html)”. Its functionality is still limited, but there is ongoing work to expand it, for example in the “[Allow panicking in constants](https://github.com/rust-lang/rfcs/pull/2345)” RFC.

The issue with `ColorCode::new` would be solvable by using [`const` functions](https://doc.rust-lang.org/reference/const_eval.html#const-functions), but the fundamental problem here is that Rust’s const evaluator is not able to convert raw pointers to references at compile time. Maybe it will work someday, but until then, we have to find another solution.

#### Lazy Statics

The one-time initialization of statics with non-const functions is a common problem in Rust. Fortunately, there already exists a good solution in a crate named [lazy_static](https://docs.rs/lazy_static/1.0.1/lazy_static/). This crate provides a `lazy_static!` macro that defines a lazily initialized `static`. Instead of computing its value at compile time, the `static` lazily initializes itself when accessed for the first time. Thus, the initialization happens at runtime, so arbitrarily complex initialization code is possible.

Let’s add the `lazy_static` crate to our project:
```toml
# in Cargo.toml

[dependencies.lazy_static]
version = "1.0"
features = ["spin_no_std"]
```

We need the `spin_no_std` feature, since we don’t link the standard library.

With `lazy_static`, we can define our static `WRITER` without problems:
```rust
// in src/vga_buffer.rs

use lazy_static::lazy_static;

lazy_static! {
    pub static ref WRITER: Writer = Writer {
        column_position: 0,
        color_code: ColorCode::new(Color::Yellow, Color::Black),
        buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
    };
}
```

However, this `WRITER` is pretty useless since it is immutable. This means that we can’t write anything to it (since all the write methods take `&mut self`). One possible solution would be to use a [mutable static](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#accessing-or-modifying-a-mutable-static-variable). But then every read and write to it would be unsafe since it could easily introduce data races and other bad things. Using `static mut` is highly discouraged. There were even proposals to [remove it](https://internals.rust-lang.org/t/pre-rfc-remove-static-mut/1437). But what are the alternatives? We could try to use an immutable static with a cell type like [RefCell](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html#keeping-track-of-borrows-at-runtime-with-refcellt) or even [UnsafeCell](https://doc.rust-lang.org/nightly/core/cell/struct.UnsafeCell.html) that provides [interior mutability](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html). But these types aren’t [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html) (with good reason), so we can’t use them in statics.

#### Spinlocks

To get synchronized interior mutability, users of the standard library can use [Mutex](https://doc.rust-lang.org/nightly/std/sync/struct.Mutex.html). It provides mutual exclusion by blocking threads when the resource is already locked. But our basic kernel does not have any blocking support or even a concept of threads, so we can’t use it either. However, there is a really basic kind of mutex in computer science that requires no operating system features: the [spinlock](https://en.wikipedia.org/wiki/Spinlock). Instead of blocking, the threads simply try to lock it again and again in a tight loop, thus burning CPU time until the mutex is free again.

To use a spinning mutex, we can add the [spin crate](https://crates.io/crates/spin) as a dependency:
```toml
# in Cargo.toml
[dependencies]
spin = "0.5.2"
```

Then we can use the spinning mutex to add safe [interior mutability](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html) to our static `WRITER`:
```rust
// in src/vga_buffer.rs

use spin::Mutex;
...
lazy_static! {
    pub static ref WRITER: Mutex<Writer> = Mutex::new(Writer {
        column_position: 0,
        color_code: ColorCode::new(Color::Yellow, Color::Black),
        buffer: unsafe { &mut *(0xb8000 as *mut Buffer) },
    });
}
```

Now we can delete the `print_something` function and print directly from out `_start` function:
```rust
// in src/main.rs
#[no_mangle]
pub extern "C" fn _start() -> ! {
    use core::fmt::Write;
    vga_buffer::WRITER.lock().write_str("Hello again").unwrap();
    write!(vga_buffer::WRITER.lock(), ", some numbers: {} {}", 42, 1.337).unwrap();

    loop {}
}
```

We need to import the `fmt::Write` trait in order to be able to use its functions.

#### Safety

Note that we only have a single unsafe block in our code, which is needed to create a `Buffer` reference pointing to `0xb8000`. Afterwards, all operations are safe. Rust uses bounds checking for array accesses by default, so we can’t accidentally write outside the buffer. Thus, we encoded the required conditions in the type system and are able to provide a safe interface to the outside.

#### A println Macro

Now that we have a global writer, we can add a `println` macro that can be used from anywhere in the codebase. Rust’s [macro syntax](https://doc.rust-lang.org/nightly/book/ch19-06-macros.html#declarative-macros-with-macro_rules-for-general-metaprogramming) is a bit strange, so we won’t try to write a macro from scratch. Instead, we look at the source of the [`println!` macro](https://doc.rust-lang.org/nightly/std/macro.println!.html) in the standard library:
```rust
#[macro_export]
macro_rules! println {
    () => (print!("\n"));
    ($($arg:tt)*) => (print!("{}\n", format_args!($($arg)*)));
}
```

Macros are defined through one or more rules, similar to `match` arms. The `println` macro has two rules: The first rule is for invocations without arguments, e.g., `println!()`, which is expanded to `print!("\n")` and thus just prints a newline. The second rule is for invocations with parameters such as `println!("Hello")` or `println!("Number: {}", 4)`. It is also expanded to an invocation of the `print!` macro, passing all arguments and an additional newline `\n` at the end.

The `#[macro_export]` attribute makes the macro available to the whole crate (not just the module it is defined in) and external crates. It also places the macro at the crate root, which means we have to import the macro through `use std::println` instead of `std::macros::println`.

The [`print!` macro](https://doc.rust-lang.org/nightly/std/macro.print!.html) is defined as:
```rust
#[macro_export]
macro_rules! print {
    ($($arg:tt)*) => ($crate::io::_print(format_args!($($arg)*)));
}
```

The macro expands to a call of the [`_print` function](https://github.com/rust-lang/rust/blob/29f5c699b11a6a148f097f82eaa05202f8799bbc/src/libstd/io/stdio.rs#L698) in the `io` module. The [`$crate` variable](https://doc.rust-lang.org/1.30.0/book/first-edition/macros.html#the-variable-crate) ensures that the macro also works from outside the `std` crate by expanding to `std` when it’s used in other crates.

The [`format_args` macro](https://doc.rust-lang.org/nightly/std/macro.format_args.html) builds a [fmt::Arguments](https://doc.rust-lang.org/nightly/core/fmt/struct.Arguments.html) type from the passed arguments, which is passed to `_print`. The [`_print` function](https://github.com/rust-lang/rust/blob/29f5c699b11a6a148f097f82eaa05202f8799bbc/src/libstd/io/stdio.rs#L698) of libstd calls `print_to`, which is rather complicated because it supports different `Stdout` devices. We don’t need that complexity since we just want to print to the VGA buffer.

To print to the VGA buffer, we just copy the `println!` and `print!` macros, but modify them to use our own `_print` function:
```rust
// in src/vga_buffer.rs

#[macro_export]
macro_rules! print {
    ($($arg:tt)*) => ($crate::vga_buffer::_print(format_args!($($arg)*)));
}

#[macro_export]
macro_rules! println {
    () => ($crate::print!("\n"));
    ($($arg:tt)*) => ($crate::print!("{}\n", format_args!($($arg)*)));
}

#[doc(hidden)]
pub fn _print(args: fmt::Arguments) {
    use core::fmt::Write;
    WRITER.lock().write_fmt(args).unwrap();
}
```

One thing that we changed from the original `println` definition is that we prefixed the invocations of the `print!` macro with `$crate` too. This ensures that we don’t need to import the `print!` macro too if we only want to use `println`.

Like in the standard library, we add the `#[macro_export]` attribute to both macros to make them available everywhere in our crate. Note that this places the macros in the root namespace of the crate, so importing them via `use crate::vga_buffer::println` does not work. Instead, we have to do `use crate::println`.

The `_print` function locks our static `WRITER` and calls the `write_fmt` method on it. This method is from the `Write` trait, which we need to import. The additional `unwrap()` at the end panics if printing isn’t successful. But since we always return `Ok` in `write_str`, that should not happen.

Since the macros need to be able to call `_print` from outside of the module, the function needs to be public. However, since we consider this a private implementation detail, we add the [`doc(hidden)` attribute](https://doc.rust-lang.org/nightly/rustdoc/write-documentation/the-doc-attribute.html#hidden) to hide it from the generated documentation.

#### Hello World using `println`

Now we can use `println` in our `_start` function:
```rust
// in src/main.rs

#[no_mangle]
pub extern "C" fn _start() {
    println!("Hello World{}", "!");

    loop {}
}
```

As expected, we now see a *"Hello World!"* on the screen:
![[Pasted image 20230627163225.png]]

#### Printing Panic Messages

Now that we have a `println` macro, we can use it in our panic function to print the panic message and the location of the panic:
```rust
// in main.rs

/// This function is called on panic.
#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    println!("{}", info);
    loop {}
}
```

When we now insert `panic!("Some panic message");` in our `_start` function, we get the following output:
![QEMU printing “panicked at ‘Some panic message’, src/main.rs:28:5](https://os.phil-opp.com/vga-text-mode/vga-panic.png)

So we know not only that a panic has occurred, but also the panic message and where in the code it happened.


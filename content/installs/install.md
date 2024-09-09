+++
title = 'Install The Sage Programming Language'
date = 2024-09-08T21:01:05-04:00
+++


{{< alert >}}
**Pre-installation:** You'll the [Rust programming language](https://www.rust-lang.org/) to build the compiler from source. **If you're on Windows**, then [you'll need to download and install the MSVC toolchain](https://visualstudio.microsoft.com/es/vs/features/cplusplus/) prior to building everything.
{{< /alert >}}

Once you've installed Rust, and you can use the `cargo` package manager, run the following to install Sage.

### Install With Cargo

```bash
$ cargo install --git https://github.com/adam-mcdaniel/main
```

### Clone And Build

You can also download the compiler from GitHub manually and build it.

```bash
$ git clone https://github.com/adam-mcdaniel/sage
$ cd sage
$ # Just build the compiler from source, don't install it:
$ cargo build --release # The `sage` exe will be in `target/release`
$ # Install it from this github clone:
$ cargo install -f --path .
```

### Post-Install

Once you've installed everything, you can get started with Sage by running the `sage` command! Use `sage -h` to see some basic help on the CLI!

{{< alert >}}
**What next?** Check out the [documentation](../../docs/), and take a look at the [compiler playground](../../playgrounds/playground) for some example programs!
{{< /alert >}}

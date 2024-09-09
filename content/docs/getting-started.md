+++
title = 'Getting Started'
date = 2024-09-09T15:36:39-04:00
series = ['Documentation']
series_order = 3
+++

## Writing And Executing A Sage Program

{{< alert "download" >}}
**Install Sage:** First, head to the [installation page](../../installs/install) to get Sage on your machine!
{{< /alert >}}

Once you have Sage installed, you can start writing Sage code!

Begin by creating a new folder for your Sage project:

```bash
$ mkdir hello-world
$ cd hello-world
```

Create a Sage program with the `.sg` file extension:

```bash
$ echo 'println("Hello world!");' > main.sg
```

Then, compile and run your program!

```bash
$ # Compile and interpret the VM code
$ sage main.sg
$ # Compile and translate the VM code to C
$ sage main.sg -tc
$ gcc out.c -O3 -o main # Compile the generated C code
```

## CLI Interface

For more information about Sage's CLI, you can run `sage -h`:

```bash
$ sage -h

   █████   ██████    ███████  ██████   `-.        _.-'
  ███░░   ░░░░░███  ███░░███ ███░░███   \ `,    .'/.'
 ░░█████   ███████ ░███ ░███░███████     \`.`. :.-'.-= .-'/
  ░░░░███ ███░░███ ░███ ░███░███░░░       `-.:/o  .'-'/ .'
  ██████ ░░████████░░███████░░██████         o\o / ._/.'
 ░░░░░░   ░░░░░░░░  ░░░░░███ ░░░░░░            \| /o|\`.
                    ███ ░███                    |'o `.`.'.
                   ░░██████                           `--'
                    ░░░░░░            

Usage: sage [OPTIONS] <INPUT>

Arguments:
  <INPUT>  The input file to compiler

Options:
  -o, --output <OUTPUT>
          The file to write the output of the compiler to [default: out]
  -s <SOURCE_TYPE>
          The source language to compile [default: sage] [possible values: sage, low-ir, core-asm, std-asm, core-vm, std-vm]
  -t <TARGET_TYPE>
          The target language to compile to [default: run] [possible values: run, core-asm, std-asm, core-vm, std-vm, c]
  -c, --call-stack-size <CALL_STACK_SIZE>
          The number of cells allocated for the call stack [default: 8192]
  -l, --log-level <LOG_LEVEL>
          The log level to use [default: off] [possible values: error, warn, info, debug, trace, off]
  -d, --debug <DEBUG>
          The symbol to debug (if any exists). This will also enable debug logging
  -h, --help
          Print help (see more with '--help')
  -V, --version
          Print version
```

Now that you have Sage on your machine, let's dive into the language itself and write some neat programs!
---
title: "Introduction"
description: "Installation and basic usage instructions"
lead: "Installation and basic usage instructions"
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 110
toc: true
---

## Installation

Make sure you have [cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html) installed

```bash
git clone https://github.com/langjam/jam0003
cargo install --path jam0003/bam
```

To verify the installation, run
```bash
bam --help
```

## Usage
In BAM!, every stream is infinite, so 'running a file' does not really make sense. 
Instead, running `bam <file>` loads a file in the REPL for interactive evaluation.

It is also possible to add definitions inside the repl by entering 'definition mode' with `:d`.

When entering a stream expression, the REPL enters 'stream mode'. Press `enter` to advance the stream by one element.

All modes can be exited with `Ctrl + D`.

*Warning:* currently, stream expressions entered at the REPL are **not** typechecked. This will be fixed in a later version. 

All definitions, entered at the REPL or loaded from a file, are fully typechecked. This only applies to stream expressions.


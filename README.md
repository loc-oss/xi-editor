# Xi editor (Core)

[![CI](https://github.com/loc-oss/xi-editor/actions/workflows/ci.yml/badge.svg)](https://github.com/loc-oss/xi-editor/actions/workflows/ci.yml)

> **Note:** This repository contains the core of the xi-editor project, actively maintained by [LOC](https://github.com/loc-oss).
> The original xi-editor project was discontinued by its creators. We are modernizing the core and building a new ecosystem around it.
>
> To see how we use this core in production, check out:
>
> - **[xi-client](https://github.com/loc-oss/xi-client)** — The SDK for interacting with the core.
> - **[Lumen](https://github.com/loc-oss/lumen)** — Our native frontend built on top of `xi-client`.

The xi-editor core is a high-performance text editing backend written in Rust. It operates as a standalone JSON-RPC server over pipes and is designed to be completely UI-agnostic. It handles file buffers and all potentially expensive editing operations, while leaving the actual UI rendering to whatever frontend connects to it.

## Goals

- **_Incredibly high performance_**. All editing operations should commit and paint in under 16ms. The editor should never make you wait for anything.
- **_Beauty_**. Text drawing should be done with the best technology available (Core Text on Mac, DirectWrite on Windows, etc.), and support Unicode fully.
- **_Reliability_**. Crashing, hanging, or losing work should never happen.
- **_Developer friendliness_**. It should be easy to customize the editor, whether by adding plug-ins or hacking on the core.

## Getting started

### Building the core

Xi-editor targets 'recent stable Rust'. We recommend installing via [rustup](https://www.rustup.rs).

To build the xi-editor core from the root directory of this repo:

```bash
cargo build
```

Once built, the `xi-core` binary acts as a JSON-RPC server. You can connect to it using any frontend or client that implements the protocol.

## Architecture and Design decisions

Here are some of the design decisions and the motivation behind them:

- **_Separation into front-end and back-end modules_**. The front-end is responsible for presenting the user interface and drawing a screen full of text. The back-end (this repo) holds the file buffers and is responsible for all editing operations.
- **_Rust_**. The back-end needs to be extremely performant. It should use little more memory than the buffers being edited. Rust offers a reliable and high-level platform for this.
- **_A persistent rope data structure_**. Persistent ropes are efficient even for very large files. They present a simple interface to their clients - conceptually, they're a sequence of characters just like a string.
- **_Asynchronous operations_**. The editor should never block. For example, autosave spawns a thread with a snapshot of the current editor buffer, allowing the user to keep typing while the file is written to disk.
- **_JSON_**. The protocol for front-end / back-end communication, as well as between the back-end and plug-ins, is based on simple JSON messages. This lowers friction for developing plug-ins and clients (like `xi-client`).

## Authors & History

The original xi-editor project was created by Raph Levien and received contributions from many others (see the [AUTHORS](AUTHORS) file).

This fork is maintained by [LOC](https://github.com/loc-oss).

## License

This project is licensed under the Apache 2 [license](LICENSE).

## Contributions

We gladly accept contributions via GitHub pull requests. Please see
[CONTRIBUTING.md](.github/CONTRIBUTING.md) for more details.

# Awesome Wasm Components

A curated list of WebAssembly Component tooling and ready-to-use components.

- [Tools](#tools)
	- [Programming Language Support](#programming-language-support)
	- [Bindings Generators](#bindings-generators)
	- [Host Runtimes](#host-runtimes)
	- [Registries](#registries)
- [Components](#components)
	- [Applications](#applications)
	- [Libraries](#libraries)
	- [Interfaces](#interfaces)
- [Resources](#resources)
	- [Tutorials](#tutorials)

> [!NOTE] Under Development
> There is a lot of work happening on Wasm Components right now, and I *know* I’m forgetting to include a lot of helpful projects, resources, and links.

## What are Wasm Components?

WebAssembly (Wasm) Components are a typed package format that packages core Wasm Instructions into typed libraries and binaries. 

Without a standard package format every language, toolchain, and platform has to define their own way of packaging up Wasm instructions. WebAssembly Components standardize this, enabling tools, languages, and platforms to all natively interoperate with each other.

Wasm Components follow a structure that is very similar to other
platforms, here is how they compare:

|                                   | WebAssembly                          | Linux                             | Windows               | macOS                                      |
| --------------------------------- | ------------------------------------ | --------------------------------- | --------------------- | ------------------------------------------ |
| **Instruction Format**            | [Core Wasm]                          | x86, ARM, etc.                    | x86 or ARM            | ARM                                        |
| **Container Format**              | [Wasm Components]                    | [ELF]                             | [PE]                  | [Mach-O]                                   |
| **Interface Definition Language** | [Wasm Interface Types][WIT] (WIT)    | C header files                    | [Microsoft IDL][MIDL] | (Objective-)C header files + Swift Modules |
| **System Interfaces**             | [Wasm System Interface][WASI] (WASI) | [POSIX] + [Linux User-Space APIs] | [Win32] + [UWP]       | [POSIX] + Darwin Syscalls                  |

[Core Wasm]: https://webassembly.github.io/spec/core/
[ELF]: https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
[Linux User-Space APIs]: https://docs.kernel.org/userspace-api/index.html
[MIDL]: https://learn.microsoft.com/en-us/uwp/midl-3/
[Mach-O]: https://en.wikipedia.org/wiki/Mach-O
[PE]: https://en.wikipedia.org/wiki/Portable_Executable
[POSIX]: https://en.wikipedia.org/wiki/POSIX
[UWP]: https://en.wikipedia.org/wiki/Universal_Windows_Platform
[WASI]: https://wasi.dev/
[WIT]: https://component-model.bytecodealliance.org/design/wit.html
[Wasm Components]: https://github.com/WebAssembly/component-model
[Win32]: https://en.wikipedia.org/wiki/Windows_API

[^wasi]: WASI 0.1 predates Wasm Components and relied on the C ABI. WASI 0.2 and later make use of Wasm Components. This removes the need for plaftform-specific tools such as [wasm-bindgen](https://github.com/wasm-bindgen/wasm-bindgen).

## Tools

### Programming Language Support

These languages can be compiled to Wasm Components. Some languages have first-party support for Wasm Components, while other languages rely on externally maintained tools.

| Language                | Name                                                                                                   | WASI Version | Maintained By     | Notes                                                                                                                                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------------------------ | ------------ | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| C and C++               | [`wasi-libc`](https://github.com/WebAssembly/wasi-libc)                                                | 0.2 + 0.3    | Bytecode Alliance | 0.3 is in-progress                                                                                                                                                                                  |
| C#                      | [`componentize-dotnet`](https://github.com/bytecodealliance/componentize-dotnet)                       | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Go                      | [`go-modules`](https://github.com/bytecodealliance/go-modules)                                         | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Java                    | [`graal`](https://github.com/oracle/graal)                                                             | -            | Oracle            | Planned - [Tracking Issue](https://github.com/oracle/graal/issues/9762), [Roadmap](https://github.com/orgs/oracle/projects/21/views/1)                                                              |
| JavaScript + TypeScript | [`jco`](https://github.com/bytecodealliance/jco)                                                       | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Python                  | [`componentize-py`](https://github.com/bytecodealliance/componentize-py)                               | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Python                  | [`cpython`](https://snarky.ca/state-of-wasi-support-for-cpython-march-2024/)                           | 0.2          | Python            | In-progress                                                                                                                                                                                         |
| Ruby                    | [`ruby.wasm`](https://github.com/ruby/ruby.wasm)                                                       | 0.2          | Ruby              | In-progress                                                                                                                                                                                         |
| Rust                    | [`wasm32-wasip2`](https://doc.rust-lang.org/rustc/platform-support/wasm32-wasip2.html) compiler target | 0.2 + 0.3    | Rust Project      | [0.2 Introduction](https://blog.rust-lang.org/2024/04/09/updates-to-rusts-wasi-targets/), [0.2 Stabilization](https://blog.rust-lang.org/2024/04/09/updates-to-rusts-wasi-targets/), 0.3 is planned |
| Swift                   | [Swift](https://www.swift.org/)                                                                        | 0.2          | Swift             | Planned - [Roadmap Accepted](https://forums.swift.org/t/accepted-vision-a-vision-for-webassembly-support-in-swift/80332)                                                                            |

### Bindings Generators

Being able to compile a language to Wasm Components is a good first step, but
it’s also important to be able to make function calls. Most languages don’t yet
provide a native way to call Wasm functions, so the second best option is to use
bindings generators that can generate those calls for you.

| From                 | To                   | Name                                                                                              |
| -------------------- | -------------------- | ------------------------------------------------------------------------------------------------- |
| Wasm Interface Types | C and C++            | [wit-bindgen c](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/c)               |
| Wasm Interface Types | C#                   | [wit-bindgen csharp](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/csharp)     |
| Wasm Interface Types | Json Schema          | [component2json](https://github.com/microsoft/wassette/tree/main/crates/component2json)           |
| Wasm Interface Types | Markdown             | [wit-bindgen markdown](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/markdown) |
| Wasm Interface Types | Moonbit              | [wit-bindgen moonbit](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/moonbit)   |
| Wasm Interface Types | Rust                 | [wit-bindgen rust](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/guest-rust)   |
| WebIDL               | Wasm Interface Types | [webidl2wit](https://github.com/wasi-gfx/webidl2wit)                                              |

### Host Runtimes

Host runtimes can take Wasm Components and execute them. Runtimes can either be loaded as language-native libraries, run as standalone binaries, or be hosted by third party providers:

| Kind               | Name                                                                | WASI Version | Maintained By          | Notes                                                                                              |
| ------------------ | ------------------------------------------------------------------- | ------------ | ---------------------- | -------------------------------------------------------------------------------------------------- |
| Build System       | [MS Build](https://github.com/dotnet/msbuild)                       | 0.2          | Microsoft              | [Proposed](https://github.com/dotnet/msbuild/blob/main/documentation/specs/proposed/Wasm-tasks.md) |
| C Library          | [Wasmtime crate](https://crates.io/crates/wasmtime)                 | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Command Line       | [Wasmtime](https://wasmtime.dev)                                    | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Durable Compute    | [Golem](https://github.com/golemcloud/golem)                        | 0.2          | Golem Cloud            |                                                                                                    |
| Durable Compute    | [Obelisk](https://github.com/obeli-sk/obelisk)                      | 0.2          | Obelisk                |                                                                                                    |
| Edge Compute       | [Edgee](https://www.edgee.cloud/)                                   | 0.2          | Edgee                  |                                                                                                    |
| Edge Compute       | [Fastly Edge Compute](https://www.fastly.com/products/edge-compute) | 0.2          | Fastly                 |                                                                                                    |
| Editor             | [VS Code](https://vscode.dev)                                       | 0.2          | Microsoft              | [Plugin system](https://code.visualstudio.com/blogs/2024/05/08/wasm)                               |
| Editor             | [Zed](https://zed.dev)                                              | 0.2          | Zed Industries         | [Plugin system](https://github.com/zed-industries/zed/tree/main/crates/extension_api)              |
| Game Backend       | [SpacetimeDB](https://spacetimedb.com/)                             | 0.2          | Clockwork Laboratories |                                                                                                    |
| Go Library         | [Gravity](https://github.com/arcjet/gravity)                        | 0.2          | Arcjet                 | In-progress                                                                                        |
| Serverless Runtime | [Spin](https://spinframework.dev/)                                  | 0.2          | CNCF                   |                                                                                                    |
| Hypervisor Guest   | [Hyperlight Wasm](https://github.com/hyperlight-dev/hyperlight-wasm)                                                 | 0.2          | CNCF                   | 0.2 async APIs in-progress                                                                         |
| Kubernetes         | [SpinKube](https://www.spinkube.dev/)                                                        | 0.2          | CNCF                   |                                                                                                    |
| Kubernetes         | [WasmCloud](https://wasmcloud.com/)                                                       | 0.2          | CNCF                   |                                                                                                    |
| MCP Server         | [WasMCP](https://github.com/wasmcp/wasmcp)                          | 0.2          | WasMCP                 |                                                                                                    |
| MCP Server         | [Wassette](https://github.com/microsoft/wassette)                                                        | 0.2          | Microsoft              |                                                                                                    |
| Node.js + Web      | [Jco](https://github.com/bytecodealliance/jco)                                                             | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Rust Library       | [Wasmtime crate](https://crates.io/crates/wasmtime)                 | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Web Framework      | [Leptos](https://leptos.dev)                                        | 0.2          | Leptos                 | [Plugin system](https://benw.is/posts/plugins-with-rust-and-wasi)                                  |

### Registries

Wasm Components can be uploaded to any [OCI v1.1]-compatible registry, using the [Wasm OCI Artifact Layout][wasm-oci]. The following registries support pushing and pulling Wasm Components:

[wasm-oci]: https://tag-runtime.cncf.io/wgs/wasm/deliverables/wasm-oci-artifact/
[OCI v1.1]: https://opencontainers.org/posts/blog/2024-03-13-image-and-distribution-1-1/

- [AWS ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html)
- [Azure ACR](https://learn.microsoft.com/en-us/azure/container-registry/)
- [Docker Hub](https://hub.docker.com/)
- [GitHub Packages](https://docs.github.com/en/packages)
- [Google Cloud Artifact Registry](https://cloud.google.com/artifact-registry/docs)

> [!HINT] Pulling Wasm Components from OCI Registries
> To download a `.wasm` application, library, or interface from an OCI registry we want to use the [`wkg` command line tool](https://github.com/bytecodealliance/wasm-pkg-tools). We can install it via `cargo`:
> 
> ```bash
> $ cargo install wkg
> ```
> 
> With `wkg` installed, we can point it at any OCI URL to fetch a package and get the `.wasm`. Here is an example to download a basic HTTP server:
> 
> ```bash
> $ wkg oci pull ghcr.io/bytecodealliance/sample-wasi-http-rust/sample-wasi-http-rust:latest
> ```

## Components
### Applications

- [sample-wasi-http-rust](https://github.com/bytecodealliance/sample-wasi-http-rust) ([package](https://github.com/bytecodealliance/sample-wasi-http-rust/pkgs/container/sample-wasi-http-rust%2Fsample-wasi-http-rust)): A sample HTTP server written in Rust.
- [sample-wasi-http-js](https://github.com/bytecodealliance/sample-wasi-http-js): A sample HTTP server written in JavaScript.

### Libraries

- [fetch-rs](https://github.com/microsoft/wassette/tree/main/examples/fetch-rs) ([package](https://github.com/microsoft/wassette/pkgs/container/fetch-rs)): Shows how to fetch content from a URL in Rust.
- [eval-py](https://github.com/microsoft/wassette/tree/main/examples/eval-py) ([package](https://github.com/microsoft/wassette/pkgs/container/eval-py)): Shows how to dynamically evaluate a Python expression.

### Interfaces

- [`wasi:io`](https://github.com/WebAssembly/wasi-io) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fio)): Standard interfaces for I/O stream abstractions. 
- [`wasi:clocks`](https://github.com/WebAssembly/wasi-clocks) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fclocks)): Standard interfaces for reading the current time and measuring elapsed time.
- [`wasi:random`](https://github.com/WebAssembly/wasi-random) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Frandom)): Standard interfaces for obtaining pseudo-random data.
- [`wasi:filesystem`](https://github.com/WebAssembly/wasi-filesystem) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Ffilesystem)): Standard interfacing for interacting with filesystems.
- [`wasi:sockets`](https://github.com/WebAssembly/wasi-sockets) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fsockets)):  Standard interfaces for TCP, UDP, and domain name lookup.
- [`wasi:cli`](https://github.com/WebAssembly/wasi-cli) ([package](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fcli)): Standard interfaces for Command-Line environments.
- [`wasi:i2c`](https://github.com/WebAssembly/wasi-i2c): provides an interface that Wasm Components can use to read and write data over an I2C connection.
- [`wasmcp:mcp`](https://github.com/wasmcp/wasmcp/tree/main/wit) ([package](https://github.com/wasmcp/wasmcp/pkgs/container/mcp)): A WIT representation of the MCP specification.

## Resources

### Tutorials

- [Building Native Plugin Systems with WebAssembly Components](https://tartanllama.xyz/posts/wasm-plugins/) by Sy Brand

## License

Apache-2.0 with LLVM Exception

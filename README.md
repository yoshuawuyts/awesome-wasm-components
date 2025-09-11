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

## What are Wasm Components?

WebAssembly (Wasm) Components are a typed package format that packages core Wasm Instructions into typed libraries and binaries. Wasm Components are also used by the Wasm System Interface (WASI) to create standard interfaces for various environments.

Without a standard package format every language, toolchain, and platform has to define their own way of packaging up Wasm instructions. WebAssembly Components standardize this, enabling tools, languages, and platforms to all natively interoperate with each other.

Wasm Components follow a structure that is very similar to other
platforms, here is how they compare:

|                                   | WebAssembly                          | Linux                                       | Windows                           | macOS                                      |
| --------------------------------- | ------------------------------------ | ------------------------------------------- | --------------------------------- | ------------------------------------------ |
| **Instruction Format**            | [Core Wasm]                          | x86, ARM, etc.                              | x86, ARM                        | ARM                                        |
| **Container Format**              | [Wasm Components]                    | [Executable and Linkable Format][ELF] (ELF) | [Portable Executable][PE] (PE)    | [Mach-O]                                   |
| **Interface Definition Language** | [Wasm Interface Types][WIT] (WIT)    | C header files                              | [Windows Metadata][WINMD] (WinMD) | (Objective-)C header files + Mach IDL + Swift Modules |
| **System Interfaces**             | [Wasm System Interface][WASI] (WASI) | [POSIX] + [Linux User-Space APIs]           | [Win32] + [UWP]                   | [POSIX] + Darwin Syscalls                  |

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
[WINMD]: https://learn.microsoft.com/en-us/uwp/winrt-cref/winmd-files

> [!NOTE]
> WASI 0.1 was released in late 2019 and was exclusively designed for UNIX-like environments. That meant that all the work put into WASI couldn't be reused for the web and other environments. Wasm Components were released in early 2023 together with WASI 0.2, which is entirely portable across environments.
> 
> WASI 0.3 is planned for the end of 2025, using the newly added native async
> support in Wasm Components. This will enable components with async interfaces
> to directly link to each other. WASI 0.2 was a big departure from WASI 0.1.
> WASI 0.3 will be a relatively small iteration of WASI 0.2.
 
## Tools

### Programming Language Support

These languages can be compiled to Wasm Components. Some languages have first-party support for Wasm Components, while other languages rely on externally maintained tools.

| Language                  | Name                                                                                                   | WASI Version | Maintained By     | Notes                                                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------ | ------------ | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| C and C++                 | [`wasi-sdk`](https://github.com/WebAssembly/wasi-sdk)                                                  | 0.2 + 0.3    | W3C Wasm CG       | 0.3 is in-progress                                                                                                                                                                                  |
| C#                        | [`componentize-dotnet`](https://github.com/bytecodealliance/componentize-dotnet)                       | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Go                        | [`go-modules`](https://github.com/bytecodealliance/go-modules)                                         | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Java                      | [`graal`](https://github.com/oracle/graal)                                                             | -            | Oracle            | Planned - [Tracking Issue](https://github.com/oracle/graal/issues/9762), [Roadmap](https://github.com/orgs/oracle/projects/21/views/1)                                                              |
| JavaScript and TypeScript | [`jco`](https://github.com/bytecodealliance/jco)                                                       | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Python                    | [`componentize-py`](https://github.com/bytecodealliance/componentize-py)                               | 0.2          | Bytecode Alliance |                                                                                                                                                                                                     |
| Python                    | [`cpython`](https://snarky.ca/state-of-wasi-support-for-cpython-march-2024/)                           | 0.2          | Python            | In-progress                                                                                                                                                                                         |
| Ruby                      | [`ruby.wasm`](https://github.com/ruby/ruby.wasm)                                                       | 0.2          | Ruby              | In-progress                                                                                                                                                                                         |
| Rust                      | [`wasm32-wasip2`](https://doc.rust-lang.org/rustc/platform-support/wasm32-wasip2.html) compiler target | 0.2 + 0.3    | Rust Project      | [0.2 Introduction](https://blog.rust-lang.org/2024/04/09/updates-to-rusts-wasi-targets/), [0.2 Stabilization](https://blog.rust-lang.org/2024/11/26/wasip2-tier-2/), 0.3 is planned |
| Swift                     | [Swift](https://www.swift.org/)                                                                        | 0.2          | Swift             | Planned - [Roadmap Accepted](https://forums.swift.org/t/accepted-vision-a-vision-for-webassembly-support-in-swift/80332)                                                                            |

### Bindings Generators

Being able to compile a language to Wasm Components is a good first step, but
it’s also important to be able to make function calls. Most languages don’t yet
provide a native way to call Wasm functions, so the second best option is to use
bindings generators that can generate those calls for you.

| From                 | To                   | Name                                                                                              |
| -------------------- | -------------------- | ------------------------------------------------------------------------------------------------- |
| Wasm Interface Types | C and C++            | [wit-bindgen c](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/c)               |
| Wasm Interface Types | C#                   | [wit-bindgen csharp](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/csharp)     |
| Wasm Interface Types | JSON Schema          | [component2json](https://github.com/microsoft/wassette/tree/main/crates/component2json)           |
| Wasm Interface Types | Markdown             | [wit-bindgen markdown](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/markdown) |
| Wasm Interface Types | Moonbit              | [wit-bindgen moonbit](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/moonbit)   |
| Wasm Interface Types | Rust                 | [wit-bindgen rust](https://github.com/bytecodealliance/wit-bindgen/tree/main/crates/guest-rust)   |
| WebIDL               | Wasm Interface Types | [webidl2wit](https://github.com/wasi-gfx/webidl2wit)                                              |

### Host Runtimes

Host runtimes can take Wasm Components and execute them. Runtimes can either be loaded as language-native libraries, run as standalone binaries, or be hosted by third party providers:

| Kind               | Name                                                                 | WASI Version | Maintained By          | Notes                                                                                              |
| ------------------ | -------------------------------------------------------------------- | ------------ | ---------------------- | -------------------------------------------------------------------------------------------------- |
| Build System       | [MS Build](https://github.com/dotnet/msbuild)                        | 0.2          | Microsoft              | [Proposed](https://github.com/dotnet/msbuild/blob/main/documentation/specs/proposed/Wasm-tasks.md) |
| C Library          | [Wasmtime crate](https://crates.io/crates/wasmtime)                  | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Command Line       | [Wasmtime](https://wasmtime.dev)                                     | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Durable Compute    | [Golem](https://github.com/golemcloud/golem)                         | 0.2          | Golem Cloud            |                                                                                                    |
| Durable Compute    | [Obelisk](https://github.com/obeli-sk/obelisk)                       | 0.2          | Obelisk                |                                                                                                    |
| Edge Compute       | [Edgee](https://www.edgee.cloud/)                                    | 0.2          | Edgee                  |                                                                                                    |
| Edge Compute       | [Fastly Edge Compute](https://www.fastly.com/products/edge-compute)  | 0.2          | Fastly                 |                                                                                                    |
| Editor             | [VS Code](https://vscode.dev)                                        | 0.2          | Microsoft              | [Plugin system](https://code.visualstudio.com/blogs/2024/05/08/wasm)                               |
| Editor             | [Zed](https://zed.dev)                                               | 0.2          | Zed Industries         | [Plugin system](https://github.com/zed-industries/zed/tree/main/crates/extension_api)              |
| Game Backend       | [SpacetimeDB](https://spacetimedb.com/)                              | 0.2          | Clockwork Laboratories |                                                                                                    |
| Go Library         | [Gravity](https://github.com/arcjet/gravity)                         | 0.2          | Arcjet                 | In-progress                                                                                        |
| Serverless Runtime | [Spin](https://spinframework.dev/)                                   | 0.2          | CNCF                   |                                                                                                    |
| Hypervisor Guest   | [Hyperlight Wasm](https://github.com/hyperlight-dev/hyperlight-wasm) | 0.2          | CNCF                   | 0.2 async APIs in-progress                                                                         |
| Kubernetes         | [SpinKube](https://www.spinkube.dev/)                                | 0.2          | CNCF                   |                                                                                                    |
| Kubernetes         | [WasmCloud](https://wasmcloud.com/)                                  | 0.2          | CNCF                   |                                                                                                    |
| MCP Server         | [WasMCP](https://github.com/wasmcp/wasmcp)                           | 0.2          | WasMCP                 |                                                                                                    |
| MCP Server         | [Wassette](https://github.com/microsoft/wassette)                    | 0.2          | Microsoft              |                                                                                                    |
| Node.js + Web      | [Jco](https://github.com/bytecodealliance/jco)                       | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Rust Library       | [Wasmtime crate](https://crates.io/crates/wasmtime)                  | 0.2 + 0.3    | Bytecode Alliance      | 0.3 support in-progress                                                                            |
| Web Framework      | [Leptos](https://leptos.dev)                                         | 0.2          | Leptos                 | [Plugin system](https://benw.is/posts/plugins-with-rust-and-wasi)                                  |

### Registries

Wasm Components can be uploaded to any [OCI v1.1]-compatible registry, using the [Wasm OCI Artifact Layout][wasm-oci]. The following registries support pushing and pulling Wasm Components:

[wasm-oci]: https://tag-runtime.cncf.io/wgs/wasm/deliverables/wasm-oci-artifact/
[OCI v1.1]: https://opencontainers.org/posts/blog/2024-03-13-image-and-distribution-1-1/

- [AWS ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html)
- [Azure ACR](https://learn.microsoft.com/en-us/azure/container-registry/)
- [Docker Hub](https://hub.docker.com/)
- [GitHub Packages](https://docs.github.com/en/packages)
- [Google Cloud Artifact Registry](https://cloud.google.com/artifact-registry/docs)

> [!TIP]
> Pulling Wasm Components from OCI Registries
> To download a `.wasm` application, library, or interface from an OCI registry we want to use [wkg][wkg].
> With it we can download Wasm Components from OCI URLs:
> 
> ```bash
> $ wkg oci pull ghcr.io/yoshuawuyts/fetch:latest -o fetch.wasm
> ```

## Components
### Applications

Wasm Components don't really distinguish between *libraries* and *applications*;
Components are always sets of function imports and exports. Still though, there
are components which don't define their own typed interfaces and instead
directly map to common environments such as `wasi:http` or `wasi:cli`. These
components feel very much "binary-shaped" and tend to be easy to run.

- [sample-wasi-http-rust](https://github.com/bytecodealliance/sample-wasi-http-rust) ([*package*](https://github.com/bytecodealliance/sample-wasi-http-rust/pkgs/container/sample-wasi-http-rust%2Fsample-wasi-http-rust)): A sample `wasi:http` server written in Rust.
- [sample-wasi-http-js](https://github.com/bytecodealliance/sample-wasi-http-js): A sample `wasi:http` server written in JavaScript.

### Libraries

Wasm Components provide fully typed interfaces, which can be linked to from any language using [Bindings Generators](#bindings-generators). This makes Wasm Components a particularly portable way to package up libraries. Libraries can also be layered and implemented in terms of one another. This makes it possible to bring a large amount of complex functionality out of the runtime, and into components.

- [fetch-rs](https://github.com/microsoft/wassette/tree/main/examples/fetch-rs) ([*package*](https://github.com/microsoft/wassette/pkgs/container/fetch-rs)): Fetch content from a URL.
- [eval-py](https://github.com/microsoft/wassette/tree/main/examples/eval-py) ([*package*](https://github.com/microsoft/wassette/pkgs/container/eval-py)): Dynamically evaluate a Python expression.
- [filesystem-rs](https://github.com/microsoft/wassette/tree/main/examples/filesystem-rs) ([*package*](https://github.com/microsoft/wassette/pkgs/container/filesystem-rs)): Access the filesystem.
- [get-weather-js](https://github.com/microsoft/wassette/tree/main/examples/get-weather-js) ([*package*](https://github.com/microsoft/wassette/pkgs/container/get-weather-js)): Get the weather for a specific location.
- [gomodule-go](https://github.com/microsoft/wassette/tree/main/examples/gomodule-go) ([*package*](https://github.com/microsoft/wassette/pkgs/container/gomodule-go)): Look up Go module information by name.
- [timeserver-js](https://github.com/microsoft/wassette/tree/main/examples/timeserver-js) ([*package*](https://github.com/microsoft/wassette/pkgs/container/timeserver-js)): Get the current time.
- [qr-code-webassembly](https://github.com/attackordie/qr-code-webassembly): Generate a QR code from a URL.

### Interfaces

_Interfaces_ in Wasm are defined using WebAssembly Interface Types, or WIT for short. There are the standard WASI-based interfaces which are developed as part of the Wasm SG in the W3C. But anyone can define their own `.wit` interfaces and package them up.

- [wasi:io](https://github.com/WebAssembly/wasi-io) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fio)): Standard interfaces for I/O stream abstractions. 
- [wasi:clocks](https://github.com/WebAssembly/wasi-clocks) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fclocks)): Standard interfaces for reading the current time and measuring elapsed time.
- [wasi:random](https://github.com/WebAssembly/wasi-random) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Frandom)): Standard interfaces for obtaining pseudo-random data.
- [wasi:filesystem](https://github.com/WebAssembly/wasi-filesystem) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Ffilesystem)): Standard interfacing for interacting with filesystems.
- [wasi:sockets](https://github.com/WebAssembly/wasi-sockets) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fsockets)): Standard interfaces for TCP, UDP, and domain name lookup.
- [wasi:cli](https://github.com/WebAssembly/wasi-cli) ([*package*](https://github.com/WebAssembly/WASI/pkgs/container/wasi%2Fcli)): Standard interfaces for Command-Line environments.
- [wasi:i2c](https://github.com/WebAssembly/wasi-i2c): Standard interfaces for reading and writing data on embedded devices over an I²C connection.
- [wasmcp:mcp](https://github.com/wasmcp/wasmcp/tree/main/wit) ([*package*](https://github.com/wasmcp/wasmcp/pkgs/container/mcp)): A WIT representation of the MCP specification.
- [zed:extension](https://github.com/zed-industries/zed/tree/6ae83b474079263273d512b68e4bf2b9b390a17d/crates/extension_api/wit/since_v0.6.0): The extension interface for the [Zed](https://zed.dev) editor. Includes support for interacting with [LSP] and [DAP].
- [browser.wit](https://github.com/MendyBerger/browser.wit): An automatic translation of the browser's Web APIs into WIT.

> [!TIP]
> To convert a directory of `.wit` files to a `.wasm` file to publish it on a registry you can use [wkg]:
> 
> ```bash
> $ wkg wit build -d wit/ -o my-interface.wasm
> ```
>
> To get the interfaces from a `.wasm` and store it as a `.wit` you can use [wasm-tools]:
>
> ```bash
> $ wasm-tools component wit my-interface.wasm -o my-interface.wit
> ```

## Resources

### Tutorials

- [Building Native Plugin Systems with WebAssembly Components](https://tartanllama.xyz/posts/wasm-plugins/) by Sy Brand

## License

Apache-2.0 with LLVM Exception

[wasm-tools]: https://github.com/bytecodealliance/wasm-tools
[wkg]: https://github.com/bytecodealliance/wasm-pkg-tools
[LSP]: https://microsoft.github.io/language-server-protocol/
[DAP]: https://microsoft.github.io/debug-adapter-protocol/

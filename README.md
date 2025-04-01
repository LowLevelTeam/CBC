# CBC: COIL Byte Code Interpreter

[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](https://unlicense.org)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()

## Overview

CBC is the official interpreter and runtime for COIL Byte Code, a lightweight bytecode format derived from COIL (Computer Oriented Intermediate Language). CBC provides a portable execution environment for COIL programs in scenarios where ahead-of-time compilation to native code is impractical or undesirable.

CBC serves as:
- A runtime for dynamic languages built on COIL
- An execution environment for portable COIL applications
- A Just-In-Time compiler for high-performance interpretation
- A component for mixed execution environments (native + interpreted)

## Features

- **Efficient Interpreter**: Fast, direct-threaded interpretation
- **JIT Compilation**: Optional Just-In-Time compilation for hot code paths
- **Compact Bytecode Format**: Small memory footprint
- **Variable System**: Complete support for COIL's variable abstraction
- **Memory Management**: Efficient memory model with multiple allocation strategies
- **Multi-platform**: Runs on Windows, macOS, Linux, and embedded systems
- **Extensibility**: Pluggable backends for different execution strategies
- **Debugging Support**: Full debug information and breakpoint support

## Installation

### Prerequisites

- C++17 compliant compiler
- CMake 3.15+
- libcoil-dev 1.0.0+
- LLVM 13+ (optional, for enhanced JIT capabilities)

### Building from Source

```bash
# Clone the repository
git clone https://github.com/LLT/cbc.git
cd cbc

# Create build directory
mkdir build && cd build

# Generate build files
cmake ..

# Build
cmake --build .

# Install
cmake --install .
```

### Pre-built Binaries

Pre-built binaries are available for major platforms:

- [Windows x64](https://github.com/LLT/cbc/releases/download/v1.0.0/cbc-1.0.0-win-x64.zip)
- [macOS x64](https://github.com/LLT/cbc/releases/download/v1.0.0/cbc-1.0.0-macos-x64.tar.gz)
- [Linux x64](https://github.com/LLT/cbc/releases/download/v1.0.0/cbc-1.0.0-linux-x64.tar.gz)

## Usage

### Basic Usage

```bash
cbcrun [options] program.cbc [program arguments]
```

### Command Line Options

| Option | Description |
|--------|-------------|
| `-v, --verbose` | Enable verbose output |
| `-t <trace>` | Enable tracing (none, basic, detailed) |
| `-m <memory>` | Set memory limit (e.g., 512M, 1G) |
| `-j` | Enable JIT compilation |
| `-j-threshold <n>` | Set JIT compilation threshold |
| `-O <level>` | Set optimization level (0-3) |
| `-g` | Enable debugging support |
| `-h, --help` | Display help message |
| `--version` | Display version information |

### Examples

```bash
# Run CBC program with JIT enabled
cbcrun -j program.cbc

# Run with memory limit and optimization
cbcrun -m 256M -O2 program.cbc

# Run with tracing for diagnostics
cbcrun -t detailed program.cbc

# Pass arguments to the program
cbcrun program.cbc arg1 arg2
```

## CBC Bytecode Format

CBC uses a compact bytecode format optimized for interpretation:

```
┌────────────┬────────────┬─────────────┬──────────────┐
│  Opcode    │ Type Code  │   Format    │   Operands   │
│  (8 bits)  │  (4 bits)  │  (4 bits)   │  (variable)  │
└────────────┴────────────┴─────────────┴──────────────┘
```

Key characteristics:
- **Type-specific opcodes** for faster dispatch
- **Compact instruction format** for efficient decoding
- **Constant pool** for shared values
- **Structured function information** for easy analysis

## Memory Model

CBC uses a simplified memory model with five distinct regions:

1. **Code Memory**: Read-only section containing instructions
2. **Constant Memory**: Read-only section containing constants
3. **Stack Memory**: Automatic memory for local variables and operands
4. **Heap Memory**: Dynamic memory allocation
5. **Variable Storage**: Abstract storage for variables (similar to COIL)

## Execution Models

CBC supports three primary execution models:

### 1. Pure Interpretation

The interpreter directly executes CBC instructions without compilation:
- Decodes each instruction and performs the corresponding operation
- Maintains a variable table mapping IDs to storage locations
- Suitable for environments where JIT is unavailable or startup time is critical

### 2. Just-In-Time (JIT) Compilation

The JIT compiler translates hot code paths to native code:
- Initially interprets all code
- Monitors execution frequency to identify hot paths
- Compiles frequently executed code to native instructions
- Maintains compatibility with the interpreted portions
- Balances startup time with runtime performance

### 3. Ahead-of-Time (AOT) Compilation

For environments that support it, CBC can be pre-compiled:
- Translates all CBC to native code before execution
- Eliminates interpretation overhead
- Maintains the CBC format for portability
- Suitable for performance-critical applications with predictable behavior

## Converting COIL to CBC

CBC includes tools for converting COIL to CBC format:

```bash
# Convert COIL object to CBC
coil2cbc [options] input.coil -o output.cbc

# Extract CBC sections from COILO
coilo-extract [options] input.coilo -o output.cbc
```

## Documentation

Comprehensive documentation is available in the `docs/` directory and online at [coil-lang.org/cbc/docs](https://coil-lang.org/cbc/docs):

- [CBC Format Reference](https://coil-lang.org/cbc/docs/format.html)
- [Instruction Set Reference](https://coil-lang.org/cbc/docs/instructions.html)
- [Memory Model](https://coil-lang.org/cbc/docs/memory-model.html)
- [JIT Configuration](https://coil-lang.org/cbc/docs/jit.html)
- [Debugging Guide](https://coil-lang.org/cbc/docs/debugging.html)
- [Performance Guide](https://coil-lang.org/cbc/docs/performance.html)
- [Examples](https://coil-lang.org/cbc/docs/examples/)

## Integration with Other Tools

CBC is designed to work seamlessly with other COIL ecosystem tools:

- `casm`: The COIL assembler can target CBC directly
- `coilp`: The COIL processor can embed CBC sections in COILO files
- `coil2cbc`: Converts COIL objects to CBC format
- `cbcdbg`: CBC debugger for interactive debugging

Typical workflow:
```bash
# Assemble CASM to COIL
casm program.casm -o program.coil

# Convert COIL to CBC
coil2cbc program.coil -o program.cbc

# Run CBC program
cbcrun program.cbc
```

## Contributing

Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to contribute to CBC.

## Implementation

For details on the implementation approach, architecture, and development plans, see [IMPLEMENTATION.md](IMPLEMENTATION.md).

## License

This project is released under the Unlicense. See [LICENSE](LICENSE) for details.
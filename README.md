# CBC: COIL Byte Code Interpreter

[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](https://unlicense.org)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()

## Overview

CBC is the interpreter and runtime for COIL Byte Code, a lightweight bytecode format derived from COIL (Computer Oriented Intermediate Language). CBC provides an execution environment for situations where ahead-of-time compilation to native code is impractical or undesirable.

CBC serves as an alternative execution path in the COIL Toolchain, offering flexibility and portability over raw performance.

## Features

- **Efficient Interpreter**: Fast, direct-threaded interpretation
- **JIT Compilation**: Optional Just-In-Time compilation for hot code paths
- **Compact Bytecode Format**: Small memory footprint
- **Variable System**: Complete support for COIL's variable abstraction
- **Memory Management**: Efficient memory model with multiple allocation strategies
- **Multi-platform**: Runs on Windows, macOS, Linux, and embedded systems
- **Debugging Support**: Full debug information and breakpoint support

## COIL Toolchain Integration

CBC provides an alternative execution path in the COIL compilation workflow:

```
                     ┌─── COILP ─── OS Linker ─── Native Executable
                     │
Source (.casm) ─── CASM ─┤
                     │
                     └─── COIL2CBC ─── CBC Interpreter ─── Interpreted Execution
```

CBC is particularly useful for:
- Dynamic language implementations
- Quick iteration during development
- Cross-platform portability
- Environments with limited native compilation support

## Installation

### Prerequisites

- C++17 compliant compiler
- CMake 3.15+
- libcoil-dev 1.0.0+
- LLVM 13+ (optional, for JIT capabilities)

### Building from Source

```bash
git clone https://github.com/LLT/cbc.git
cd cbc
mkdir build && cd build
cmake ..
cmake --build .
cmake --install .
```

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
| `-O <level>` | Set optimization level (0-3) |
| `-g` | Enable debugging support |
| `-h, --help` | Display help message |

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

CBC uses a structured memory model with five distinct regions:

1. **Code Memory**: Read-only section containing instructions
2. **Constant Memory**: Read-only section containing constants
3. **Stack Memory**: Automatic memory for local variables and operands
4. **Heap Memory**: Dynamic memory allocation
5. **Variable Storage**: Abstract storage for variables (similar to COIL)

## Converting COIL to CBC

CBC includes tools for converting COIL to CBC format:

```bash
# Convert COIL object to CBC
coil2cbc [options] input.coil -o output.cbc

# Extract CBC sections from COILO
coilo-extract [options] input.coilo -o output.cbc
```

## Documentation

Documentation is available in the `docs/` directory:

- [CBC Format Reference](docs/format.md)
- [Instruction Set Reference](docs/instructions.md)
- [Memory Model Documentation](docs/memory-model.md)
- [JIT Configuration Guide](docs/jit.md)
- [Debugging Guide](docs/debugging.md)

## Workflow Integration

CBC works seamlessly with other COIL ecosystem tools:

```bash
# Assemble CASM to COIL
casm program.casm -o program.coil

# Convert COIL to CBC
coil2cbc program.coil -o program.cbc

# Run CBC program
cbcrun program.cbc
```

## License

This project is released under the Unlicense. See [LICENSE](LICENSE) for details.
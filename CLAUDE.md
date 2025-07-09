# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## MicroPython Project Overview

This is the MicroPython project - a lean implementation of Python 3.x for microcontrollers and small embedded systems. The codebase implements the entire Python 3.4 syntax with select features from later versions.

## Architecture & Key Directories

- **py/** - Core Python implementation including compiler, runtime, and core library
- **mpy-cross/** - MicroPython cross-compiler for converting scripts to precompiled bytecode (.mpy files)
- **ports/** - Platform-specific implementations for different architectures (esp32, stm32, unix, etc.)
- **extmod/** - Additional non-core modules implemented in C
- **lib/** - External dependencies as git submodules
- **tests/** - Comprehensive test framework and test scripts
- **tools/** - Various utilities including pyboard.py module and code formatting tools

## Common Development Commands

### Building MicroPython

Build the cross-compiler first (required for most ports):
```bash
cd mpy-cross
make
```

Build specific ports:
```bash
# Unix port (for development/testing)
cd ports/unix
make submodules  # Initialize required submodules
make

# ESP32 port
cd ports/esp32
make submodules
make

# STM32 port
cd ports/stm32
make submodules
make
```

### Testing

Run the complete test suite:
```bash
cd tests
python run-tests.py
```

Run tests for specific port:
```bash
# Test unix port
cd tests
python run-tests.py --target unix

# Test with specific micropython executable
python run-tests.py --micropython ../ports/unix/build-standard/micropython
```

### Code Formatting

Format C code using uncrustify (requires uncrustify v0.71 or v0.72):
```bash
tools/codeformat.py
```

Format Python code:
```bash
ruff format
```

Run pre-commit hooks:
```bash
pre-commit run --all-files
```

## Port-Specific Builds

Each port has its own build system and requirements:

- **unix**: Standard `make`, produces `build-standard/micropython`
- **esp32**: Uses ESP-IDF build system, requires ESP-IDF setup
- **stm32**: ARM cross-compilation, various board targets
- **rp2**: Raspberry Pi Pico, uses CMake with Pico SDK

## Development Workflow

1. **Submodules**: Always run `make submodules` in port directory before building
2. **Code Style**: Follow conventions in CODECONVENTIONS.md - use `tools/codeformat.py` for C code and `ruff format` for Python
3. **Testing**: Run relevant tests before committing changes
4. **Cross-compilation**: Build mpy-cross first if working with bytecode

## Git Conventions

- Commit messages must start with directory/file prefix (e.g., "py/obj: Add new method")
- All commits must be signed off with `git commit -s`
- Use `tools/codeformat.py` and `ruff format` before committing

## Key Configuration Files

- **mpconfigport.h** - Port-specific MicroPython configuration
- **boards/*/mpconfigboard.h** - Board-specific configuration
- **pyproject.toml** - Python tooling configuration (ruff, codespell)
- **tools/uncrustify.cfg** - C code formatting rules
# yabai

yabai is a tiling window manager for macOS, designed as an extension to the built-in window manager. It uses a binary space partitioning algorithm to automatically manage window layouts and allows for advanced control over windows, spaces, and displays via a command-line interface.

## Project Structure

*   **`src/`**: Contains the source code for the main application and its core logic.
    *   `yabai.c`: The main entry point and initialization logic.
    *   `*_manager.c`: Modules managing specific entities (processes, displays, windows, spaces).
    *   `event_loop.c`: Core event loop implementation.
    *   `message.c`: Handling of IPC messages.
    *   `osax/`: Source for the scripting addition payload (injected code).
*   **`scripts/`**: Utility scripts for installation, signing, and setting icons.
*   **`tests/`**: Contains the test suite and its makefile.
*   **`doc/`**: Documentation and man pages.
*   **`examples/`**: Sample configuration files (`yabairc`, `skhdrc`).
*   **`makefile`**: The main build configuration file.

## Build and Installation

### Prerequisites

*   macOS (Intel or Apple Silicon)
*   Xcode Command Line Tools
*   `codesign` (for signing the binary)

### Building from Source

To build the project (debug build):

```sh
make
```

This will produce the `yabai` binary in the `./bin` directory.

To build an optimized release version:

```sh
make install
```

### Other Build Targets

*   `make asan`: Build with AddressSanitizer.
*   `make tsan`: Build with ThreadSanitizer.
*   `make sign`: Sign the binary with a certificate named "yabai-cert".
*   `make icon`: Update the application icon.

## Testing

To build and run the test suite:

```sh
cd tests
make
```

This compiles `src/tests.m` and executes the resulting binary.

## Architecture & Conventions

*   **Language**: C (C11 standard) and Objective-C.
*   **Frameworks**: Heavily relies on macOS frameworks: Carbon, Cocoa, CoreServices, CoreVideo, and the private **SkyLight** framework for window management internals.
*   **Event-Driven**: The core logic is driven by an event loop and callbacks for system events (accessibility API, SkyLight notifications).
*   **IPC**: Uses a socket-based IPC mechanism to allow the `yabai` command-line tool (client) to communicate with the running daemon (server).
*   **Scripting Addition (SA)**: Includes a component (`src/osax`) that is injected into the Dock.app process to enable features requiring higher privileges (like space switching without animation).

## Usage

After building, the binary can be run directly:

```sh
./bin/yabai
```

Or used to send commands to a running instance:

```sh
./bin/yabai -m query --windows
```

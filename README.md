# QK4 - Intel Mac Fork

A fork of [mikeg-dal/QK4](https://github.com/mikeg-dal/QK4) specifically modified to support Intel Mac compatibility.

This fork resolves Homebrew library paths dynamically to support both Intel Mac (/usr/local) and Apple Silicon (/opt/homebrew) architectures, enabling QK4 to build and run on Intel-based Macs.

## Changes from Original

- Modified CMakeLists.txt to dynamically resolve Homebrew prefixes for opus, hidapi, and libusb libraries
- Added support for Intel Mac Homebrew installation paths (/usr/local)
- Maintains full compatibility with Apple Silicon Macs

## Supported Platforms

| Platform | Minimum Version | Architecture |
|----------|-----------------|--------------|
| macOS | 14 (Sonoma) | **Intel x64** and Apple Silicon (M1/M2/M3/M4) |

## About QK4

QK4 is a cross-platform desktop application for remote control of Elecraft K4 radios over TCP/IP with real-time audio streaming and spectrum display.

### Key Features
- **TLS/PSK Encrypted Connection** — Secure connection via TLS v1.2 with Pre-Shared Key
- **Dual VFO Display** — Frequency, mode, S-meter, and tuning rate indicator
- **GPU-Accelerated Spectrum** — Real-time panadapter with waterfall via Metal on macOS
- **Dual-Channel Audio** — Opus-encoded stereo with independent volume controls
- **KPOD / KPOD+ Support** — USB integration with Elecraft tuning hardware
- **CAT Server** — Built-in CAT server for integration with logging software

For complete feature details, see the [original QK4 repository](https://github.com/mikeg-dal/QK4).

## Building from Source on macOS

### Requirements

| Dependency | Installation |
|------------|--------------|
| Xcode Command Line Tools | `xcode-select --install` |
| Homebrew | Install from [brew.sh](https://brew.sh) |
| Qt 6.7+ | `brew install qt` |
| libopus | `brew install opus` |
| OpenSSL 3 | `brew install openssl@3` |
| HIDAPI | `brew install hidapi` |
| libusb 1.0 | `brew install libusb` |
| CMake | `brew install cmake` |

### Build Instructions

```bash
# Install dependencies
brew install qt opus openssl@3 hidapi libusb cmake

# Clone this fork
git clone https://github.com/krainika/QK4.git
cd QK4

# Configure build (works on both Intel and Apple Silicon Macs)
cmake -B build -DCMAKE_PREFIX_PATH="$(brew --prefix qt)"

# Build
cmake --build build

# Run
./build/QK4.app/Contents/MacOS/QK4

# Create distributable app bundle (optional)
cmake --build build --target deploy
```

**Note**: This fork automatically detects your Mac architecture (Intel or Apple Silicon) and configures the appropriate Homebrew library paths.

## Testing

```bash
# Run all tests
ctest --test-dir build --output-on-failure
```

## Usage

For detailed usage instructions, please refer to the [original QK4 repository](https://github.com/mikeg-dal/QK4#usage).

## Technical Details

The key modification in this fork is in `CMakeLists.txt` (lines 49-53), where Homebrew library paths are resolved dynamically:

```cmake
# macOS with Homebrew - resolve prefix dynamically to support both
# Intel (/usr/local) and Apple Silicon (/opt/homebrew)
execute_process(COMMAND brew --prefix opus OUTPUT_VARIABLE OPUS_PREFIX OUTPUT_STRIP_TRAILING_WHITESPACE)
execute_process(COMMAND brew --prefix hidapi OUTPUT_VARIABLE HIDAPI_PREFIX OUTPUT_STRIP_TRAILING_WHITESPACE)
execute_process(COMMAND brew --prefix libusb OUTPUT_VARIABLE LIBUSB_PREFIX OUTPUT_STRIP_TRAILING_WHITESPACE)
```

This change allows the build system to work correctly on both Intel Macs (where Homebrew installs to `/usr/local`) and Apple Silicon Macs (where Homebrew installs to `/opt/homebrew`).

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE), same as the original QK4 project.

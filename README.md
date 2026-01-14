# LVGL Simulator for Studio (Docker Build)

A web-based LVGL simulator built with Emscripten that compiles to WebAssembly. This project provides a browser-based environment for testing and developing LVGL user interfaces with support for multiple LVGL versions and configurable display dimensions.

## Features

- **Multi-version LVGL Support**: Compatible with LVGL versions 8.4.0, 9.2.2, 9.3.0, and 9.4.0
- **Configurable Display**: Customizable display width and height
- **WebAssembly-based**: Runs directly in the browser with no native compilation required
- **SDL2 Integration**: Uses SDL2 for rendering and input handling
- **FreeType Support**: Built-in TrueType font rendering

## Integration with Studio Docker Build Tool

This repository can be used with [studio-docker-build-tool](https://github.com/mvladic/studio-docker-build-tool) for automated Docker-based builds and deployment workflows.

## Requirements

- [Emscripten SDK (emsdk)](https://emscripten.org/docs/getting_started/downloads.html)
- Git

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd lvgl-simulator-for-studio-docker-build
```

2. Initialize submodules:
```bash
git submodule update --init --recursive
```

## Usage

### Building

Build the simulator with a specific LVGL version and display dimensions:

```bash
./build.sh --lvgl=<version> [--display-width=<width>] [--display-height=<height>]
```

**Example:**
```bash
./build.sh --lvgl=9.4.0 --display-width=480 --display-height=272
```

**Arguments:**
- `--lvgl=<version>` (required): LVGL version to use (e.g., 8.4.0, 9.2.2, 9.3.0, 9.4.0)
- `--display-width=<width>` (optional): Display width in pixels
- `--display-height=<height>` (optional): Display height in pixels

### Running

After building, open `build/index.html` in a web browser to run the simulator.

## Project Structure

```
.
├── build.sh              # Build script
├── CMakeLists.txt        # CMake configuration
├── main.c                # Main application entry point
├── lv_conf.h             # LVGL configuration
├── lv_drv_conf.h         # LVGL drivers configuration
├── src/                  # Source files for UI
│   └── ui/               # UI components
├── versions/             # Version-specific configurations
│   ├── 8.4.0/
│   ├── 9.2.2/
│   ├── 9.3.0/
│   └── 9.4.0/
├── lvgl/                 # LVGL library (submodule)
└── lv_drivers/           # LVGL drivers (submodule)
```

## UI Files

Place your generated UI files in the `src/` directory:
- `src/screens.c` / `src/screens.h` - Screen definitions
- `src/ui.c` / `src/ui.h` - UI initialization
- `src/images/` - Image assets
- `src/fonts/` - Font files

The build system automatically includes all `.c` and `.cpp` files from the `src/` directory.

## Development

### Adding New LVGL Versions

1. Create a new directory under `versions/<version>/`
2. Add version-specific configuration files (`CMakeLists.txt`, `lv_conf.h`, etc.)
3. Ensure LVGL submodule has the corresponding version tag

### Customizing the Build

Edit the CMakeLists.txt file in the respective version directory to:
- Modify compiler flags
- Adjust memory settings
- Enable/disable LVGL features

## Troubleshooting

### Build Fails
- Ensure Emscripten is properly installed and activated
- Check that all submodules are initialized
- Verify the LVGL version exists as a tag in the submodule

### Clock Skew Warnings
These warnings can be safely ignored. They occur due to file timestamp differences.

### Undefined Symbols
Ensure all required functions (e.g., `ui_init`, `ui_tick`) are implemented in your UI files.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [LVGL](https://lvgl.io/) - Light and Versatile Graphics Library
- [Emscripten](https://emscripten.org/) - WebAssembly compiler toolchain
- [SDL2](https://www.libsdl.org/) - Simple DirectMedia Layer

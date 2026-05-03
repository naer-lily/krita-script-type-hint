Python krita module stub file generated from C++ headers via Doxygen XML, used for IDE auto-completion when writing Krita plugins. Inspired by [scottpetrovic/krita-python-auto-complete](https://github.com/scottpetrovic/krita-python-auto-complete).

> 2024 年 5 月由小仓那尔编写，2026 年 5 月由 deepseek-v4-pro 重构为纯 Python 实现。

# Usage

1. Clone this repository.
2. Copy the file `krita.pyi` into your plugin directory.
3. Enjoy type-safety.

![Alt text](image.png)

# Building from source

Requires [Doxygen](https://www.doxygen.nl/) and Python 3.9+.

1. Download the `libs/libkis` directory from [Krita's source](https://github.com/KDE/krita/tree/master/libs/libkis):
   ```bash
   git clone --depth 1 --filter=blob:none --sparse https://github.com/KDE/krita.git krita-src
   cd krita-src && git sparse-checkout set libs/libkis && git checkout
   ```
2. Generate Doxygen XML:
   ```bash
   cd krita-src/libs/libkis
   doxygen -s -g
   # Edit Doxyfile: set GENERATE_XML = YES, GENERATE_HTML = NO
   doxygen
   ```
3. Copy the generated XML to this project's `xml/` directory:
   ```bash
   cp doxygen-out/xml/class_*.xml doxygen-out/xml/namespace_*.xml /path/to/krita-script-type-hint/xml/
   cp doxygen-out/xml/*.xsd doxygen-out/xml/*.xslt /path/to/krita-script-type-hint/xml/
   ```
4. Run the build script:
   ```bash
   cd /path/to/krita-script-type-hint
   python build.py --xml-dir ./xml --output ./krita.pyi
   ```

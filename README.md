# krita.pyi — Krita scripting type stubs

为 Krita 插件开发提供 IDE 自动补全和类型检查的类型标注文件，从 C++ 头文件通过 Doxygen XML 生成。

> 2024 年 5 月由我编写，2026 年 5 月由 deepseek-v4-pro 重构为纯 Python 实现。

**当前版本：Krita v6.0.1.1**（与 5.3.1 共用 libkis API，Qt6 构建为 Krita 6，Qt5 构建为 5.3）

# 架构

`krita/` 目录是一个 stub-only 包，将 43 个 API 类拆分为 `_types/` 下的独立文件，每个类一个 `.pyi`。`__init__.pyi` 作为索引，re-export 所有类并附带完整 docstring——这样 AI/LLM 工具只需先读 `__init__.pyi` 即可获取全局视图，需要细节时再按需读取 `_types/xxx.pyi`，避免一次性加载 7000+ 行的单文件。

# Usage

1. Clone this repository.
2. Copy the `krita/` directory into your plugin project.
3. `from krita import Node` — IDE will auto-complete as usual.

![Alt text](image.png)

# Building from source

需要 [Doxygen](https://www.doxygen.nl/) 和 Python 3.9+。Krita 主仓库在 [invent.kde.org/graphics/krita](https://invent.kde.org/graphics/krita)。

## 列举所有 release 版本

```bash
git ls-remote --tags https://invent.kde.org/graphics/krita.git | grep -oP 'v\d+\.\d+\.\d+[.\d]*(?=\^|\s|$)' | sort -Vu
```

## 为指定版本生成

将 `TAG` 替换为你要的版本号，例如 `v6.0.1.1`：

```bash
git clone --depth 1 --branch TAG \
  --filter=blob:none --sparse \
  https://invent.kde.org/graphics/krita.git krita-src
cd krita-src
git sparse-checkout set libs/libkis && git checkout

cd libs/libkis
doxygen -s -g
# 编辑 Doxyfile：设置 GENERATE_XML = YES，GENERATE_HTML = NO
doxygen

cp doxygen-out/xml/class_*.xml doxygen-out/xml/namespace_*.xml /path/to/krita-script-type-hint/xml/
cp doxygen-out/xml/*.xsd doxygen-out/xml/*.xslt /path/to/krita-script-type-hint/xml/
cd /path/to/krita-script-type-hint
python build.py --xml-dir ./xml --output ./krita
```

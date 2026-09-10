# semver

[![CI](https://github.com/alya-lang/semver/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/semver/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Alya](https://img.shields.io/badge/Alya-%3E%3D0.0.5-orange.svg)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/version-0.1.0-brightgreen.svg)](alya.toml)

Semantic Versioning (SemVer 2.0.0) parser, comparator, and range satisfaction library for Alya

---

## 🌟 Features

- ⚡ **Lightweight & Fast**: Built for speed with minimal overhead
- 📦 **Zero Dependencies**: Pure Alya code, entirely self-contained
- 🛡️ **Reliable**: Fully typed API and predictable behavior
- 🧪 **Well Tested**: Comprehensive test suite included

---

## 📦 Installation

Add `semver` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
semver = { git = "https://github.com/alya-lang/semver", tag = "v0.1.0" }
```

Or install it directly using the Alya package CLI:

```bash
alyac add semver --git https://github.com/alya-lang/semver --tag v0.1.0
alyac install
```

---

## 🚀 Quick Start

```alya
import "semver" as pkg

function main()
    let greeting = pkg::hello("Alya")
    say greeting
end

main()
```

---

## 📖 API Reference

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `hello(name)` | `name: string = "World"` | `string` | Returns a friendly greeting message. |

---

## 🧪 Running Tests

Run the test suite using `alyac`:

```bash
alyac run tests/test_basic.alya
```

Or run directly from the package directory:

```bash
alyac run
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps to contribute:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/my-new-feature`)
3. Commit your changes (`git commit -m "feat: add some feature"`)
4. Push to the branch (`git push origin feature/my-new-feature`)
5. Open a Pull Request

Please make sure tests pass before submitting a PR.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

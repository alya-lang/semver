# semver

[![CI](https://github.com/alya-lang/semver/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/semver/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/semver?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fsemver%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fsemver%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Semantic Versioning (SemVer 2.0.0) parser, comparator, and range satisfaction library for Alya

---

## 🌟 Features

- 📜 **Full SemVer 2.0.0 Compliance**: Implements Section 11 precedence, pre-release tags, and build metadata.
- 🎯 **Smart Parser**: Handles standard (`1.2.3`), prefixed (`v1.2.3`, `=1.2.3`), loose (`1.2`), and pre-release/build versions.
- ⚖️ **Precise Comparators**: `compare`, `eq`, `neq`, `gt`, `gte`, `lt`, `lte`, `min`, `max`.
- 📐 **Advanced Range Satisfaction (`satisfies`)**:
  - Exact & Comparators: `1.2.3`, `>=1.0.0`, `<=2.0.0`, `!=1.5.0`
  - Conjunctions (AND): `>=1.0.0 <2.0.0`
  - Caret Ranges (`^`): `^1.2.3`, `^0.2.3`, `^0.0.3`
  - Tilde Ranges (`~`): `~1.2.3`, `~1.2`
  - Wildcards: `1.*`, `1.x`, `*`
  - Union Ranges (OR): `^1.0.0 || ^2.0.0`
- 🚀 **Version Bumping**: `bump_major`, `bump_minor`, `bump_patch`, `bump_prerelease`.

---

## 📁 Project Architecture

```text
semver/
├── alya.toml               # Package manifest
├── src/
│   ├── lib.alya            # Public API facade
│   ├── types.alya          # Version struct and string helpers
│   ├── parser.alya         # Parse, clean, format, and validation
│   ├── comparator.alya     # Precedence comparison and operators
│   ├── bumper.alya         # SemVer version bump and diff
│   └── range.alya          # Caret, tilde, wildcard range satisfaction
├── examples/
│   └── demo.alya           # Runnable usage example
├── tests/
│   └── test_basic.alya     # Automated test suite
└── benches/
    └── bench_basic.alya    # Micro-benchmarks
```

---

## 📦 Installation

Add `semver` to your `alya.toml`:

```toml
[dependencies]
semver = { git = "https://github.com/alya-lang/semver", branch = "main" }
```

Or install via Alya CLI:

```bash
alya add semver --git https://github.com/alya-lang/semver --branch main
alya install
```

---

## 🚀 Quick Start

```alya
import "semver"

function main()
    # 1. Parse version
    let v = semver::parse("v1.5.0-rc.1+sha.abcdef")
    say "Major: " + str(v.major)     # 1
    say "Minor: " + str(v.minor)     # 5
    say "Patch: " + str(v.patch)     # 0
    say "Pre:   " + v.prerelease     # rc.1

    # 2. Compare versions
    let v1 = semver::parse("1.0.0-alpha")
    let v2 = semver::parse("1.0.0")
    if semver::lt(v1, v2)
        say "1.0.0-alpha is older than 1.0.0"
    end

    # 3. Check dependency range compatibility
    if semver::satisfies_string("1.5.2", "^1.2.0")
        say "Compatible with ^1.2.0!"
    end

    # 4. Bump version
    let next_v = semver::bump_minor(v2)
    say semver::format(next_v)       # 1.1.0
end

main()
```

---

## 📖 API Reference

### Core Functions

| Function | Signature | Description |
|---|---|---|
| `parse(v_str)` | `(string) -> Version` | Parses a SemVer string into a `Version` struct. |
| `create(maj, min, pat, pre, build)` | `(int, int, int, string, string) -> Version` | Creates a new `Version` instance. |
| `format(v)` / `to_string(v)` | `(Version) -> string` | Formats a `Version` struct into standard string representation. |
| `clean(v_str)` | `(string) -> string` | Strips whitespace, `= `, and leading `v`/`V`. |
| `is_valid(v_str)` | `(string) -> int` | Returns `1` if string is valid SemVer, `0` otherwise. |

### Comparisons

| Function | Signature | Description |
|---|---|---|
| `compare(v1, v2)` | `(Version, Version) -> int` | Returns `-1` (`v1 < v2`), `0` (`v1 == v2`), or `1` (`v1 > v2`). |
| `compare_strings(s1, s2)` | `(string, string) -> int` | Parses and compares two SemVer strings directly. |
| `eq(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 == v2`. |
| `neq(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 != v2`. |
| `gt(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 > v2`. |
| `gte(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 >= v2`. |
| `lt(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 < v2`. |
| `lte(v1, v2)` | `(Version, Version) -> int` | Returns `1` if `v1 <= v2`. |
| `max(v1, v2)` | `(Version, Version) -> Version` | Returns the higher version. |
| `min(v1, v2)` | `(Version, Version) -> Version` | Returns the lower version. |

### Range Matching & Bumping

| Function | Signature | Description |
|---|---|---|
| `satisfies(v, range_str)` | `(Version, string) -> int` | Returns `1` if `v` satisfies the given range expression. |
| `satisfies_string(s, range_str)` | `(string, string) -> int` | Convenience method checking string version against range. |
| `bump_major(v)` | `(Version) -> Version` | Increments major, resets minor & patch to 0. |
| `bump_minor(v)` | `(Version) -> Version` | Increments minor, resets patch to 0. |
| `bump_patch(v)` | `(Version) -> Version` | Increments patch version. |
| `bump_prerelease(v, tag)` | `(Version, string) -> Version` | Increments or initializes prerelease identifier. |
| `bump(v, type)` | `(Version, string) -> Version` | Bumps version according to release type (`"major"`, `"minor"`, `"patch"`, `"prerelease"`). |
| `diff(v1, v2)` | `(Version, Version) -> string` | Returns difference level (`"major"`, `"minor"`, `"patch"`, `"prerelease"`, or `""`). |

---

## 🧪 Running Tests & Benchmarks

Run the automated test suite:

```bash
alya test
```

Run the performance micro-benchmarks:

```bash
alya run benches/bench_basic.alya
```

Run the runnable usage demo:

```bash
alya run examples/demo.alya
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   alya fmt . --check
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

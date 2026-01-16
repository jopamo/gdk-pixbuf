# Repository Guidelines

## Project Structure & Module Organization
- `gdk-pixbuf/` holds the core library sources, loaders, and public headers.
- `tests/` contains GLib test programs plus test images and data fixtures.
- `docs/` includes documentation sources and assets.
- `build-aux/` provides build helper scripts; `meson.build` and `meson_options.txt` define the Meson build.
- `po/` contains translations; `thumbnailer/` provides the thumbnailer helper.

## Build, Test, and Development Commands
This project uses Meson + Ninja. A typical local workflow:

```sh
meson setup build
meson compile -C build
meson test -C build
```

Useful variations:
- `meson setup build -Dtests=true` ensures tests are built (default is on).
- `meson test -C build --suite format` runs a specific test suite defined in `tests/meson.build`.
- `meson install -C build` installs locally (useful for testing installed loaders).

## Coding Style & Naming Conventions
- C style is enforced with `.clang-format` (Chromium-based, 4-space indents, 120 column limit).
- Use existing file naming patterns (e.g., `io-*.c` for loaders, `pixbuf-*.c` for tests).
- Follow existing GLib/GObject naming and error-handling patterns in nearby code.

## Testing Guidelines
- Tests are GLib `g_test` executables built from sources in `tests/`.
- Test data lives under `tests/test-images/`; add new fixtures there and wire them in `tests/meson.build`.
- Run all tests with `meson test -C build`; target a subset with `--suite` or `--name`.

## Commit & Pull Request Guidelines
- Commits are short and imperative; optional scopes are used (e.g., `docs: Fix …`).
- Keep changes focused, and group related updates (e.g., translations) together.
- For PRs, include a clear description, list test commands run, and note any loader/format impact.

## Security & Configuration Tips
- Loaders depend on external libraries (libpng, libjpeg, libtiff); use `meson_options.txt` to enable/disable them.
- If you add a new loader or parser, include tests that cover malformed inputs and size/overflow checks.

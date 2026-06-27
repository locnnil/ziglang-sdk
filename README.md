# Zig SDK for Workshop

This SDK provides the Zig toolchain — the compiler, build system, and bundled
standard library — for systems programming and C/C++ cross-compilation. The Zig
global build cache is persisted on the host so compiled artifacts survive
workshop updates.

The SDK publishes two channels on the `latest` track: `latest/beta` tracks the
most recent stable Zig release, and `latest/edge` tracks Zig's `master`
(nightly) builds.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: zig-app
base: ubuntu@24.04
sdks:
  - name: ziglang
    channel: latest/beta

actions:
  build: |
    zig build
  test: |
    zig build test
```

This demonstrates a basic Zig build workflow with a persistent global cache.
Switch the channel to `latest/edge` to use the latest nightly Zig.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. Your Zig project (with a `build.zig` file) should be in your project
   directory:

   ```bash
   git clone <YOUR_REPO_URL>
   ```

3. On launch, the SDK adds `zig` to `PATH` and points `ZIG_GLOBAL_CACHE_DIR` at
   the persistent mount. No compilation happens automatically.

### Build the project

Once the workshop is ready:

```bash
workshop shell
zig build
```

Compiled artifacts are cached in `~/.cache/zig`, which is mapped to your host via
the `global-cache` mount plug. Subsequent builds reuse the cache across workshop
updates.

### Compile and cross-compile

From within the workshop shell:

```bash
workshop shell
zig build-exe hello.zig                          # native build
zig build-exe hello.zig -target aarch64-linux    # cross-compile
zig cc -o app app.c                              # use Zig as a C compiler
```

Zig ships with its own C/C++ cross-compiler (`zig cc` / `zig c++`), so no
separate toolchain is required to target other architectures.

### Choosing stable or nightly

- `latest/beta` — the most recent Zig release (recommended).
- `latest/edge` — the latest `master` (nightly) build, for testing unreleased
  features. Nightly builds change frequently and may introduce breaking changes.

Confirm the active version from the command line:

```bash
workshop shell
zig version
```

---

## Plugs (resources this SDK consumes)

### `global-cache`

- Interface: `mount`
- Workshop target: `/home/workshop/.cache/zig`
- Purpose: Persists Zig's global compilation cache between workshop updates,
  speeding up rebuilds.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Zig official documentation](https://ziglang.org/documentation/master/)
- [Zig learn resources](https://ziglang.org/learn/)
- [Zig downloads and release notes](https://ziglang.org/download/)
- [Workshop documentation](https://ubuntu.com/workshop/docs/)

---

## Community and support

- Zig community:
  [Zig community resources](https://github.com/ziglang/zig/wiki/Community)
- Workshop forum:
  [Discourse](https://discourse.ubuntu.com/c/project/workshop/513)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2026 Lincoln Wallace

This program is free software: you can redistribute it and/or modify it under the
terms of the
[GNU Lesser General Public License version 2.1 (LGPLv2.1)](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html)
as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

Zig is licensed under the
[MIT License](https://codeberg.org/ziglang/zig/src/branch/master/LICENSE).

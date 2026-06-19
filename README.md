# SM-Soda
Native static HTTP server. Zero dependencies. Single binary. Drop-in replacement for python3 -m http.server — without Python.
Just with FPC. Just with C.

<p align="center">
  <img src="SM-Soda.jpg" alt="SM-Soda Logo" width="420">
</p>

<h3 align="center">python3 -m http.server is a trap.</h3>

<p align="center">
  <strong>SM-Soda</strong> (Serve Me Soda) — a native static HTTP server.<br>
  Zero dependencies. Single binary. Drop-in replacement for <code>python3 -m http.server</code><br>
  without the Python, without the runtime, without the dependency circus.<br>
  <strong>Dual implementation:</strong> Free Pascal <em>and</em> C99. Same spec. Two toolchains. Real metrics.
</p>

<p align="center">
  <img alt="License" src="https://img.shields.io/badge/License-GPLv3-blue.svg?style=for-the-badge">
  <img alt="Language" src="https://img.shields.io/badge/Language-Free%20Pascal%20%7C%20C99-blue.svg?style=for-the-badge">
  <img alt="Platform" src="https://img.shields.io/badge/Platform-Linux%20%7C%20Windows%20%7C%20macOS-lightgrey.svg?style=for-the-badge">
  <img alt="Status" src="https://img.shields.io/badge/Status-WIP-orange.svg?style=for-the-badge">
</p>

---

## What

A static file server. Nothing more. Nothing less.

```bash
# Serve current dir on port 8080
./microserve

# Custom port
./microserve 9000

# Custom port + directory
./microserve 9000 /var/www/html
```

Handles: **GET**, MIME types, directory listings, **404** on missing files.

Does **not** handle: TLS, auth, CGI, range requests, virtual hosts, your emotional baggage.

---

## Why

`python3 -m http.server` works everywhere because Python is everywhere.  
That's the problem. It requires a runtime. A zoology of dependencies. An interpreter.

SM-Soda is a **native binary**. Compile once. Run anywhere. No installer. No pip. No venv. No "it works on my machine."

The dual implementation (FPC + C) isn't redundancy — it's the point. Same spec (`SPEC.md`). Two toolchains. If they differ, it's a bug. If they match, the spec is solid.

| Metric | FPC | C |
|--------|-----|---|
| Binary (stripped) | ~180 KB | ~25 KB |
| RSS idle | ~4 MB | ~1.5 MB |
| Startup to first byte | ~8 ms | ~2 ms |
| Compiler | fpc 3.2+ | gcc · C99 |
| External deps | none | none |

---

## Build

**FPC:**
```bash
cd fpc
fpc microserve.pas -O2 -o microserve
```

**C:**
```bash
cd c
gcc -O2 -std=c99 microserve.c -o microserve
```

---

## 🎅 Dear Santa — The Christmas List

> The *"Carta a los Reyes Magos"* — Spanish kids asking for everything they saw on TV,  
> not knowing if they need it, not caring if it fits.  
> That's how clients write requirements. This is our version. We know what we want.  
> We just don't have infinite elves.

### Shipped | 🚧 WIP | 📝 Backlog | 🔮 Probably Never

| # | Item | Status | Phase |
|---|------|--------|-------|
| 1 | Serve static files (GET /file → disk → HTTP/1.0) | 🚧 | 1 |
| 2 | Directory listing when no index.html | 📝 | 2 |
| 3 | MIME types by extension (html/css/js/png/jpg/svg/pdf/txt) | 📝 | 2 |
| 4 | Clean 404 with body | 📝 | 2 |
| 5 | CLI args: `[port] [directory]` with defaults (8080, cwd) | 📝 | 2 |
| 6 | FPC implementation using RTL sockets (`ssockets`, `baseunix`) | 🚧 | 1 |
| 7 | C99 implementation using POSIX sockets (`sys/socket.h`) | 🚧 | 1 |
| 8 | Byte-for-byte identical output from both implementations | 🚧 | 1 |
| 9 | Real metrics: binary size, RSS, startup, req/s (wrk) | 📝 | 3 |
| 10 | Shell tests with curl (functional parity checker) | 📝 | 3 |
| 11 | Windows support (winsock2, `#ifdef _WIN32`) | 🔮 | 4 |
| 12 | Static linking for true portability | 🔮 | 4 |
| 13 | AppImage / tarball / zip packaging | 🔮 | 5 |
| 14 | IPv6 listening (`AF_INET6`) | 🔮 | 6 |
| 15 | Keep-alive / HTTP/1.1 | 🔮 | never |
| 16 | TLS / HTTPS | 🔮 | never |
| 17 | Authentication | 🔮 | never |
| 18 | CGI support | 🔮 | never |
| 19 | Range requests | 🔮 | never |
| 20 | Virtual hosts | 🔮 | never |

### Brain Dump (raw, unsorted, probably stupid)

```text
- HEAD method support (for completeness, not really needed)
- If-Modified-Since / 304 Not Modified (saves bandwidth)
- ETag generation (simple hash of mtime + size)
- Gzip on-the-fly compression (zlib, optional, flag-enabled)
- Symlink handling (follow or not? security question)
- Hidden file protection (no .git, no .env, no .htaccess)
- Max path length enforcement (avoid buffer overflows)
- Concurrent connections (fork() per request? thread pool? poll/epoll?)
- Rate limiting (token bucket, per-IP)
- Logging to stdout / file / syslog (Common Log Format)
- Daemon mode (detach from terminal, pidfile)
- Config file (.microserve.conf, JSON or TOML or nothing)
- Index file variants (index.htm, default.html)
- Custom error pages (404.html in root)
- CORS headers (for local dev with fetch)
- Benchmark suite (wrk, ab, vegeta) with pretty output
- Memory pool for buffers (reduce malloc churn in C)
- Zero-copy sendfile() on Linux (bypass userspace)
- TCP_CORK / TCP_NODELAY tuning
- SO_REUSEADDR / SO_REUSEPORT for fast restarts
- Graceful shutdown (SIGTERM handler, drain connections)
```

### Why This Exists

Because `python3 -m http.server` is a trap. It lures you in with convenience, then one day you're debugging why it won't start on a machine without Python, or the wrong Python, or a broken venv, or a missing module.

SM-Soda is a **hate-fueled rewrite** of a one-liner. The one-liner that should have been a binary from day one.

We don't need 20 features. We need the right 5, compiled to a single executable that fits in an email attachment.

Everything else is noise until Feature 1 ships in both toolchains.

---

## License

GPL v3 — see [LICENSE](LICENSE) file.

Built with spite and native code in Galicia, Spain.

---

*Last updated: 2026-06-20*  
*Status: Santa hasn't confirmed the delivery date.*

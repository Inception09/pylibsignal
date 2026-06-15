<div align="center">

# pylibsignal

**A pure-Python research implementation of libsignal's XEdDSA (XEd25519) signing algorithm.**

[![Status](https://img.shields.io/badge/status-research%20in%20progress-orange?style=for-the-badge)](#status)
[![Topic](https://img.shields.io/badge/topic-cryptography-8A2BE2?style=for-the-badge)](#background)
[![Pure Python](https://img.shields.io/badge/pure-python-success?style=for-the-badge)](#)

</div>

---

## About

`pylibsignal` is a research project that explores and reproduces the **XEd25519** signature scheme used by the [Signal protocol](https://github.com/signalapp/libsignal) — entirely in pure Python, with no Rust, no native binaries, and no C extensions.

The goal is to understand, document, and faithfully re-implement one of the more subtle pieces of Signal's cryptography in a readable, self-contained form for study and verification.

## What this enables

Because `pylibsignal` produces signatures that are byte-compatible with `libsignal`, it makes it possible to perform **Signal server API operations directly from Python** — without the original Rust `libsignal` library, JNI bindings, or any native dependency.

Many of Signal's API interactions require cryptographically **signed key material** (identity keys signing signed pre-keys and post-quantum last-resort pre-keys). Reproducing XEd25519 correctly removes the last blocker to building a fully Python-only Signal client for research: the rest of the flow is ordinary HTTPS requests. In short — **pure Python in, valid Signal-accepted signatures out.**

## Background

Most signature systems keep their signing and key-agreement keys separate. Signal takes a different approach: it reuses a single **X25519 (Montgomery) key pair** for both Diffie-Hellman key agreement **and** digital signatures, via a construction known as **XEdDSA / XEd25519**.

This matters because the long-term **identity key** in Signal is an X25519 key, yet it must also *sign* material such as signed pre-keys and post-quantum (Kyber) last-resort pre-keys. XEd25519 makes that possible by deriving an Ed25519 signing key from the Montgomery scalar — with careful handling of clamping, the Edwards sign bit, and a domain-separated nonce.

The result is a signature that a verifier can check using only the X25519 *public* identity key — without the signer ever holding a separate Ed25519 key.

## Research focus

This project investigates:

- **Signature correctness** — reproducing XEd25519 so its output is accepted by Signal exactly as `libsignal`'s would be.
- **API compatibility** — signing the key material that Signal's server endpoints require, so protocol operations succeed without the native library.
- **Wire-format fidelity** — matching the precise byte encodings Signal expects for identity, signed pre-keys, and post-quantum pre-keys.
- **Transparency** — keeping the implementation plain and auditable rather than a compiled black box.

## Why pure Python?

The official `libsignal` is a high-quality Rust library, but for research, teaching, and protocol analysis it is valuable to have a minimal, dependency-light reference that can be read top to bottom in a single sitting. Clarity over performance.

## Status

> 🔬 **Research in progress.** This repository is an early placeholder. It will be updated with full details once the research phase is complete.

## References

- Signal — official `libsignal` repository: <https://github.com/signalapp/libsignal/tree/main>
- Signal — *The XEdDSA and VXEdDSA Signature Schemes*: <https://signal.org/docs/specifications/xeddsa/>
- Signal — protocol documentation: <https://signal.org/docs/>

## Disclaimer

For **research and educational purposes only**. This project is not affiliated with, authorized, or endorsed by Signal Messenger. It is not intended for production use.

## Contact

<div align="center">

For more information, reach out on Telegram.

[![Telegram](https://img.shields.io/badge/Telegram-%40inception00007-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/inception00007)

</div>

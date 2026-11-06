# feedback_core

The native crypto core for the Basecamp **Feedback app**. It seals each feedback response to the
Feedback Desk's public key with libsodium `crypto_box_seal` (anonymous sealed box), so only the Desk
can open it. The sender stays anonymous: a sealed box carries no sender identity.

Part of a pair:
- **feedback_core** (this repo): the C++ core module. Exposes `seal(payloadJson) -> { ok, ciphertext_b64 }`.
- **feedback-app**: the QML UI (`feedback_ui`) that collects feedback and calls this module, then sends
  over Logos Messaging.

The always-on collector (the Feedback Desk) and the design spec live separately in
`xAlisher/feedback-bc` (`feedback_desk/`); its secrets stay on the host, never in a repo.

## API

| Method | Returns |
|---|---|
| `seal(payloadJson: string)` | `{ "ok": true, "ciphertext_b64": "..." }` or `{ "ok": false, "error": "..." }` |

The Desk public key is compiled in; the matching private key lives only on the Desk host.

## Build

```
nix build            # -> result/lib/feedback_core_plugin.<ext> + metadata.json
```

Requires the Logos C++ SDK (pinned in `flake.nix`). Builds on linux-amd64, linux-arm64 and
darwin-arm64 (the darwin linker needs `Qt6::RemoteObjects` linked explicitly, already handled).

## Status

Dogfooded end to end on the Logos testnet (quick report + questionnaire, app to Desk to Discord).
Tracking: logos-co/ecosystem#273.

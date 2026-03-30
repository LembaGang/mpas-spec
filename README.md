# MPAS — Multi-Party Attestation Aggregation Specification

**Version**: 1.0.0 | **Status**: Draft | **License**: Apache 2.0

**Canonical URL**: [headlessoracle.com/docs/mpas](https://headlessoracle.com/docs/mpas)

---

## What is MPAS?

MPAS (Multi-Party Attestation Aggregation Specification) defines a protocol for aggregating cryptographically signed market-state attestations from N independent oracle operators into a single verifiable quorum result. A consumer that requires 2-of-3 independent operators to agree on market state before acting cannot be fooled by a single compromised or coerced operator. MPAS is a composition layer above the [SMA Protocol](https://github.com/LembaGang/sma-protocol) — individual receipts inside an `AggregatedAttestation` are standard SMA receipts; MPAS adds quorum logic on top without modifying the underlying signing scheme.

---

## AggregatedAttestation Schema

```json
{
  "mpas_version": "1.0",
  "mic": "XNYS",
  "status": "OPEN",
  "quorum": {
    "required": 2,
    "provided": 3
  },
  "window_ms": 5000,
  "aggregated_at": "2026-03-30T14:32:10.000Z",
  "attestations": [
    {
      "operator_id": "headlessoracle.com",
      "receipt": {
        "mic": "XNYS",
        "status": "OPEN",
        "issued_at": "2026-03-30T14:32:09.121Z",
        "expires_at": "2026-03-30T14:33:09.121Z",
        "issuer": "headlessoracle.com",
        "key_id": "03dc27993a2c90856cdeb45e228ac065f18f69f0933c917b2336c1e75712f178",
        "receipt_mode": "live",
        "schema_version": "v5.0",
        "receipt_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
        "source": "SCHEDULE",
        "signature": "hex-encoded-ed25519-signature"
      },
      "verified": true
    },
    {
      "operator_id": "oracle-b.example.com",
      "receipt": { "...": "SMA receipt from operator B" },
      "verified": true
    },
    {
      "operator_id": "oracle-c.example.com",
      "receipt": { "...": "SMA receipt from operator C" },
      "verified": true
    }
  ],
  "consensus": "AGREE"
}
```

**Consensus values**: `AGREE` (>= required verified attestations agree) | `DISAGREE` (quorum reached but disagrees) | `INSUFFICIENT` (not enough valid attestations). Treat `DISAGREE`, `INSUFFICIENT`, and any verification failure as `UNKNOWN` — halt execution.

Full field definitions, quorum rules, consumer verification algorithm, on-chain verification sketch, and trust model comparison are in [SPEC.md](./SPEC.md).

---

## Reference Implementation

[Headless Oracle](https://headlessoracle.com) (`headlessoracle.com`) is the reference SMA operator and implements MPAS-1.0 as the aggregation layer over its signed market receipts.

- Live oracle endpoint: `https://headlessoracle.com/v5/status?mic=XNYS`
- Key discovery: `https://headlessoracle.com/.well-known/oracle-keys.json`
- MPAS spec page: `https://headlessoracle.com/docs/mpas`
- SMA Protocol: [github.com/LembaGang/sma-protocol](https://github.com/LembaGang/sma-protocol)

---

## License

Apache 2.0 — see [LICENSE](./LICENSE).

Copyright 2026 Headless Oracle Project

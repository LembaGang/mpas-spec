# MPAS-1.0 Implementations

This file lists known implementations of the Multi-Party Attestation Aggregation Specification.

To add your implementation, open a pull request with a new row in the table below.

---

## Oracle Operators

Operators that produce SMA-compliant receipts compatible with MPAS aggregation.

| Operator | Endpoint | Key Discovery | Exchanges | Status |
|---|---|---|---|---|
| [Headless Oracle](https://headlessoracle.com) | `https://headlessoracle.com/v5/status?mic={MIC}` | `https://headlessoracle.com/.well-known/oracle-keys.json` | 28 (equities, derivatives, 24/7 crypto) | Live |

---

## Aggregators

Implementations that fetch receipts from multiple operators and assemble an `AggregatedAttestation`.

| Name | Language | Source | Notes |
|---|---|---|---|
| — | — | — | First aggregator implementation welcome |

---

## Consumer SDKs

Libraries that implement the MPAS consumer verification algorithm (Section 5 of SPEC.md).

| Name | Language | Source | Notes |
|---|---|---|---|
| — | — | — | First consumer SDK welcome |

---

## Listing Requirements

To be listed as an oracle operator, your implementation must:

1. Produce SMA Protocol v1.0.0 compliant signed receipts (Ed25519, canonical alphabetical payload sort, 60s TTL)
2. Publish your public key at `/.well-known/oracle-keys.json` per RFC 8615
3. Operate independently — no shared signing keys or pre-coordination with other listed operators
4. Support at least one MIC code from the ISO 10383 standard

To be listed as an aggregator or consumer SDK, your implementation must:

1. Implement the full verification algorithm from Section 5 of SPEC.md
2. Treat `DISAGREE`, `INSUFFICIENT`, and any verification failure as `UNKNOWN` (fail-closed)
3. Re-verify each individual SMA receipt signature independently (never trust the aggregator's `verified` field)

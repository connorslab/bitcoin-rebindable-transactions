# Bitcoin rebindable transactions

An unassigned draft proposal for deploying BIP 448's three operations alongside Bitcoin's RDTS validation rules:

- OP_TEMPLATEHASH: commit to the spending transaction.
- OP_CHECKSIGFROMSTACK: verify a signature over a supplied message.
- OP_INTERNALKEY: access the Taproot internal public key.

Read the [draft specification](bip-taproot-rebindable-rdts.mediawiki) and [signet deployment evidence](deployment.md).

The operation semantics retain SHA256 template hashes and BIP 340 signatures. The draft documents how the existing validation rules affect compatibility. Under RDTS, allowing these operations requires a consensus relaxation, not the soft-fork treatment described by upstream BIP 448.

The implementation is live on an experimental signet. Production activation is unspecified. This repository is an independent draft publication, not an assigned BIP, an upstream submission, or an endorsement by the original authors.

Feedback can be filed as an issue. Please identify the affected rule, expected behavior, and a reproducible example where possible.

Specification text is CC0-1.0. The linked implementation retains its own license.

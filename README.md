# Bitcoin rebindable transactions: unnumbered BIP draft

This is an **unnumbered Bitcoin Improvement Proposal (BIP) draft** for deploying BIP 448's three operations alongside Bitcoin's RDTS validation rules. No BIP number has been assigned; the specification uses `BIP: ?` and `Assigned: ?` pending assignment by the BIP editors.

The proposed operations are:

- OP_TEMPLATEHASH: commit to the spending transaction.
- OP_CHECKSIGFROMSTACK: verify a signature over a supplied message.
- OP_INTERNALKEY: access the Taproot internal public key.

Read the [draft specification](bip-taproot-rebindable-rdts.mediawiki) and [signet deployment evidence](deployment.md).

The reference implementation builds on [Bitcoin Knots](https://github.com/bitcoinknots/bitcoin), whose code provides the RDTS and signature-validation context discussed here. The experimental changes are maintained in the separate [signet implementation repository](https://github.com/connorslab/paperclip-bitcoin-signet). This reference does not imply acceptance or endorsement by Bitcoin Knots maintainers.

The operation semantics retain SHA256 template hashes and BIP 340 signatures. The draft documents how the existing validation rules affect compatibility. Under RDTS, allowing these operations requires a consensus relaxation, not the soft-fork treatment described by upstream BIP 448.

The implementation is live on an experimental signet. Production activation is unspecified. This repository is an independent draft publication, not an assigned BIP, an upstream submission, or an endorsement by the original authors.

Feedback can be filed as an issue. Please identify the affected rule, expected behavior, and a reproducible example where possible.

Specification text is CC0-1.0. The linked implementation retains its own license.

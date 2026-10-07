# Bitcoin rebindable transactions: unnumbered BIP draft

This is an **unnumbered Bitcoin Improvement Proposal (BIP) draft** for deploying BIP 448's three operations alongside Bitcoin's RDTS validation rules. No BIP number has been assigned; the specification uses `BIP: ?` and `Assigned: ?` pending assignment by the BIP editors.

The proposed operations are:

- OP_TEMPLATEHASH: commit to the spending transaction.
- OP_CHECKSIGFROMSTACK: verify a signature over a supplied message.
- OP_INTERNALKEY: access the Taproot internal public key.

Read the [draft specification](bip-taproot-rebindable-rdts.mediawiki) and [signet deployment evidence](deployment.md).

The reference implementation builds on [Bitcoin Knots](https://github.com/bitcoinknots/bitcoin), whose code provides the RDTS and signature-validation context discussed here. The experimental changes are maintained in the separate [signet implementation repository](https://github.com/connorslab/paperclip-bitcoin-signet). This reference does not imply acceptance or endorsement by Bitcoin Knots maintainers.

The operation semantics retain SHA256 template hashes and BIP 340 signatures. The draft documents how the existing validation rules affect compatibility. Under RDTS, allowing these operations requires a consensus relaxation, not the soft-fork treatment described by upstream BIP 448.

## How this differs from BIP 448

This draft keeps BIP 448's three opcode numbers and their meanings. It does not change the template hash to a different hashing algorithm or introduce a new signature scheme. The difference is how those operations fit into Bitcoin's existing RDTS rules:

| Area | Upstream BIP 448 | This draft |
| --- | --- | --- |
| Upgrade type | Tightens ordinary Tapscript OP_SUCCESS behavior, allowing a soft fork. | RDTS already rejects unknown OP_SUCCESS operations. Permitting these three operations relaxes that rule and requires a coordinated consensus upgrade. |
| Script limits | Uses the ordinary Tapscript rules. | Retains RDTS limits: 256-byte stack elements, at most seven Taproot Merkle branch hashes, no annexes, and no OP_IF or OP_NOTIF in Tapscript. |
| Signature and template hashing | Uses BIP 340 signatures and BIP 446's tagged SHA256 template hash. | Uses the same definitions. Existing transaction-signature rules do not automatically add replay protection to CSFS messages. |
| Activation | Deployment remains unspecified. | Production deployment also remains unspecified. All three operations are running on the experimental signet. |

The signet implementation additionally defaults to a configurable 32-byte CSFS message limit for relay and local mining. That is local policy, not an added consensus requirement of this draft.

The practical consequence is that a construction valid under upstream BIP 448 may still fail here if it needs features that RDTS prohibits. This draft documents that narrower execution environment; it does not claim unrestricted BIP 448 compatibility.

The implementation is live on an experimental signet. Production activation is unspecified. This repository is an independent draft publication, not an assigned BIP, an upstream submission, or an endorsement by the original authors.

Feedback can be filed as an issue. Please identify the affected rule, expected behavior, and a reproducible example where possible.

Specification text is CC0-1.0. The linked implementation retains its own license.

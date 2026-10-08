# Experimental signet deployment

## Current upgrade: fixed template-only CSFS

Deployed October 8, 2026 UTC at height 3240; version
`paperclip-signet3-template-csfs`.
[Source commit](https://github.com/connorslab/paperclip-bitcoin-signet/commit/8b0c84ea587189af4bb3a97faea21845b77fb83d).
[Test binaries and checksums](https://github.com/connorslab/paperclip-bitcoin-signet/releases/tag/v0.2.0-bitcoin-signet).

Consensus activation is **height 3250 inclusive**. Updated mempools enforce the
rule immediately. CSFS requires exactly 32 bytes equal to the current input's
template hash, including empty-signature calls. There is no configurable size
opt-out: remove the old `maxcsfsmsgsize` option from configurations.

Keep the existing datadir, challenge and network configuration. No new chain or
initial synchronization is required for an existing synchronized node. Earlier
blocks retain their old rules. Participants must upgrade to enforce the new rule;
old clients may accept blocks rejected by upgraded clients. Outstanding outputs
relying on arbitrary-message CSFS may not be spendable after activation.

The final build passed 19 selected unit suites, six node functional suites and
both experimental Ark harnesses. Node2 passed `verifychain 4 0` after installation.
RPC downtime was 1.48 seconds; the main Bitcoin node was not restarted.

- bitcoind SHA256: `1e586a18863badd1d5205ca32704fb5b1383287d03a4727a52fe6912547102bb`
- bitcoin-cli SHA256: `74b004d2035848d40f8a189b287dfd445c580054ce0ab0c019dc74516fc0451c`

[Detailed validation](https://github.com/connorslab/paperclip-bitcoin-signet/blob/main/doc/paperclip-signet-validation.md).

## Earlier three-opcode deployment (historical record)


Deployed on 2026-10-07. Test coins only.

- Seed: node2.paperclippool.xyz:48333.
- Version: paperclip-signet2-bip448.
- RPC remains private.
- Existing chain and signing challenge retained.
- [Implementation](https://github.com/connorslab/paperclip-bitcoin-signet/commit/7d73834cd626ca84bcc814a8bcf790626576085d).

## Confirmed three-operation spend

Script: OP_TEMPLATEHASH OP_INTERNALKEY OP_CHECKSIGFROMSTACK.

- Funding: 2b97ea920ded18aefb4887e0596354ddc4f6d68afc24415b0a23a035ab1ccbba
- Spend: 973537dde4725fcdc2fabb0d923cf455e51c45aa52b7e31a3ce7cfd6c6c54083
- Height: 3179
- Block: 12efe2b4c124965637322592e81abdd5cc22308a084ff4128d5973ccf2dfb8ea
- Template hash: f0d90585d85534826c8d88db16342bc6dfbf516b4a542c345a9d1537b2a2d78a

[View the transaction](https://node2.paperclippool.xyz/signet/#tx/973537dde4725fcdc2fabb0d923cf455e51c45aa52b7e31a3ce7cfd6c6c54083).

Reducing the committed output amount by one satoshi caused an Invalid Schnorr signature rejection. The valid spend confirmed. The demonstration uses a public test key that also allows key-path spending; it is an opcode test, not a secure covenant application.

## Validation

Nineteen selected native unit suites passed. Functional tests passed for:

- Existing template and CSFS spends.
- Combined three-operation spends using two different internal keys.
- Altered-output rejection, mining, and restart verification.
- Signed signet block verification and peer synchronization.
- Configurable CSFS policy, including valid blocks containing transactions excluded by local policy.
- Existing RDTS and unified-sighash behavior.

Node2 passed full-chain verification after upgrade. An independent node running the old binary rejected block 3179 as an unknown OP_SUCCESS operation under RDTS. After upgrading and reconsidering that block, it accepted the block and passed full-chain verification. This confirms that old clients must upgrade.

## Existing participants

Build the updated implementation and retain the existing signet configuration and data directory. Do not reuse this experimental data directory for another network.

If an old client has already marked the demonstration block invalid, upgrade first, then run:

    bitcoin-cli -datadir=<existing-signet-directory> reconsiderblock 12efe2b4c124965637322592e81abdd5cc22308a084ff4128d5973ccf2dfb8ea

Then check the chain tip and run verifychain. Rebuilding the block index with the upgraded binary is an alternative.

This is not an independent audit or a complete Ark or Lightning integration test. The full BIP 446 script-assets corpus, additional fuzzing, and broader consensus review remain outstanding.


Activation confirmed: signet block **3250**, hash
`4a310b0732f864f73ec808946ab153cb85db2cf5200dbe0c0f40ca3eb8b938ec`.
The node continued beyond activation and passed full-chain verification at
height 3251. No forced blocks, chain reset or activation-time restart was used.

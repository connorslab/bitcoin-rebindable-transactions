# Experimental signet deployment

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

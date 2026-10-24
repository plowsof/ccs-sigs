# ccs-sigs

Attestations for releases of the Monero CCS: [ccs-bash](https://github.com/plowsof/ccs-bash),
[ccs-front-bash](https://github.com/plowsof/ccs-front-bash) and
and [ccs-front-hugo](https://github.com/plowsof/ccs-front-hugo). Each signer rebuilt the release
themselves and signed what came out. (The wallet's signed subaddress list isn't attested here: a
Core Team member makes and signs it, and contributors can regenerate it and confirm its hash.)

Modelled on [feather-sigs](https://github.com/feather-wallet/feather-sigs) (thank you, tobtoht),
which follows Bitcoin Core's [guix.sigs](https://github.com/bitcoin-core/guix.sigs):

```
<name>/<signer>/all.SHA256SUMS        what the signer's build gave
<name>/<signer>/all.SHA256SUMS.asc    their signature
builder-keys/<signer>.asc             their public key
```

`<name>` is a release: `ccs-bash-v0.2.7`, `ccs-front-bash-v0.1.8`, `ccs-front-hugo-v0.1.7`.

## Attesting

See [ccs-bash's contrib/release](https://github.com/plowsof/ccs-bash/tree/master/contrib/release):
`build` rebuilds a release from its tag (with that repository's own pinned StageX build), `attest`
writes and signs your folder here. Your first time, add `builder-keys/<signer>.asc` in the same
pull request. The pull request's `lint` check accepts only new attestations, in pairs, and
builder keys, and checks every signature.

## Checking

ccs-bash has this repository as a submodule at `external/ccs-sigs`, as Feather has feather-sigs:

```
git clone --recursive https://github.com/plowsof/ccs-bash && cd ccs-bash
git submodule update --remote external/ccs-sigs     # the newest attestations
contrib/release/verify                              # every release; or name one: ccs-bash-v0.2.7
```

It checks each signature against `builder-keys/` (in a keyring of its own) and that all signers
of a release agree. Then compare that ID or sha256 with the release notes and with what you
run.

`contrib/touched-files-check` is feather-sigs', which is Bitcoin Core guix.sigs'.

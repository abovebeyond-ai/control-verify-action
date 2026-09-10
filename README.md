# Proof-of-Control, the far end

A GitHub Action for the runner that is asked to do a repair, and for the check that guards
a merge. It refuses to act unless the evidence holds: the record signed by the gateway
whose key the DID log at abovebeyond.ai names, a verdict of ALLOW for this repository and
this verb, written on Intel TDX under the measurement the public mirror attests, and the
principal's capability that the record names. Nothing here needs a secret, and nothing
here can be switched off from the operator's side: it runs where the repository's owner
put it. That is row 8.3.5 of the Advanced AI Society's Proof-of-Control standard.

Before a fix workflow does anything:

```yaml
- uses: abovebeyond-ai/control-verify-action@v1
  with:
    mode: dispatch
    evidence: ${{ inputs.evidence }}
    capability: ${{ inputs.capability }}
```

As a required status check on pull requests:

```yaml
on: pull_request
jobs:
  evidence:
    runs-on: ubuntu-latest
    steps:
      - uses: abovebeyond-ai/control-verify-action@v1
        with:
          mode: pull
```

The check is `cmd/relying` of https://github.com/abovebeyond-ai/control, built from source
at a pinned release. The evidence itself is mirrored at
https://github.com/abovebeyond-ai/control-evidence.

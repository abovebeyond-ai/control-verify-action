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

## Out of reach is not the same as rejected

The verifier fetches the DID document over the network, and until 21 September 2026 a
failed fetch came out as a refusal like any other. That day two runs went red in a row on
a repository whose evidence was perfectly good: the host served a certificate for
`*.netlify.app` on one, the connection dropped on the next. Both looked like the control
check had caught something.

A refusal that names the fetch is now retried four times over half a minute. If the
document stays out of reach the step still fails - an unreachable document is never
treated as a pass - but it says it could not judge, instead of claiming the evidence was
rejected. A verdict the verifier actually reached is never retried, in either direction.

Worth doing next, and not done here: a second address for the document, so one host
having a bad afternoon is not the whole story.

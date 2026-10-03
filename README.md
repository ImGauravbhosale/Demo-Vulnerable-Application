# MalwHunter SCA demo

Proves [MalwHunter](https://github.com/ImGauravbhosale/Malwhunter) catches
a malware-shaped npm dependency inside a real GitHub Actions pipeline —
not just when run by hand.

## History

- **Commit 1 (clean baseline)** — `chalk` + `left-pad` only. CI passed
  clean: 0 malicious, 0 suspicious.
- **Commit 2 (this one — simulated malicious update)** — adds
  [`@imgauravbhosale/malwhunter-ci-demo-fixture`](https://www.npmjs.com/package/@imgauravbhosale/malwhunter-ci-demo-fixture),
  a package published specifically for this test. It's **safe and inert**
  (does nothing when installed or required) but its code is deliberately
  written to match known npm supply-chain malware shapes: an
  install-script that looks like a dropper, bulk environment-variable
  enumeration, a command-exec call with a non-literal argument, and a
  reference to a known exfiltration channel (Discord webhook URL). See
  that package's own README for full disclosure.

  This simulates a real dependency update silently introducing something
  bad — exactly the scenario MalwHunter exists to catch.

## What should happen

MalwHunter resolves this fixture as `suspicious` (4 static signals, no
corroborating dynamic behavior — the correct, cautious verdict for
Recon-only findings). The workflow sets `fail-on: suspicious`, so CI
should fail on this commit.

## Don't

Don't depend on the fixture package for anything else, and don't use
this repo as a template for a real project's CI — it exists only to
exercise MalwHunter itself.

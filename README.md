# MalwHunter SCA demo

Proves [MalwHunter](https://github.com/ImGauravbhosale/Malwhunter) catches
a malware-shaped npm dependency inside a real GitHub Actions pipeline —
not just when run by hand.

## Current state: clean baseline

`package.json` has two real, boring, well-known dependencies — `chalk`
and `left-pad`. `.github/workflows/malwhunter-scan.yml` runs MalwHunter
on every push/PR. This should pass clean.

## Next step (not yet done)

A follow-up PR will bump in a deliberately malware-shaped test package —
simulating a real dependency update that introduces something bad — and
the same CI workflow should catch it and fail the build.

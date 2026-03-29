# OSV-Scanner Vulnerability PoC Repository

This repository demonstrates real-world exploitation of vulnerabilities in osv-scanner v2.3.5 through GitHub Actions CI/CD workflows.

## Attack Chains

1. **Supply Chain Attack (Path Traversal → Credential Theft)**
   - Workflow: `.github/workflows/attack-chain-1.yml`
   - Demonstrates: Arbitrary file read leading to AWS credential exposure

2. **Cache Poisoning in CI/CD**
   - Workflow: `.github/workflows/attack-chain-2.yml`
   - Demonstrates: False negatives shipping vulnerable code to production

3. **Symlink Privilege Escalation**
   - Workflow: `.github/workflows/attack-chain-3.yml`
   - Demonstrates: Cross-boundary exploitation via symlinks + race conditions

## How to Reproduce

Each workflow can be triggered manually from the GitHub Actions tab.

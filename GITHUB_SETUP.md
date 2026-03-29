# GitHub Setup Instructions

## Step 1: Create a GitHub Repository

1. Go to https://github.com/new
2. Create a new repository named: `osv-scanner-vuln-poc` (or similar)
3. Choose: **Public** repository (for Google review)
4. DO NOT initialize with README (we have one)

## Step 2: Push This Repository

```bash
# Navigate to the repo
cd /tmp/osv-bounty-poc

# Add remote
git remote add origin https://github.com/YOUR-USERNAME/osv-scanner-vuln-poc.git

# Rename branch to main (optional but recommended)
git branch -M main

# Push
git push -u origin main
```

## Step 3: Enable GitHub Actions

1. Go to your GitHub repository
2. Click **Settings** → **Actions** → **General**
3. Ensure "Allow all actions and reusable workflows" is selected
4. Save

## Step 4: Trigger Tests

1. Go to **Actions** tab
2. Select workflow:
   - "Attack Chain 1: Path Traversal → Credential Theft"
   - "Attack Chain 2: Cache Poisoning → False Negatives"
   - "Attack Chain 3: Symlink + Insecure Permissions → Escalation"
3. Click **Run workflow** → **Run workflow** button
4. Wait for execution to complete (~2-5 minutes per workflow)
5. View logs to confirm exploitation

## Step 5: Collect Evidence

After each workflow completes:

1. Click on the workflow run
2. Click on the job (e.g., "credential-theft", "cache-poisoning", "symlink-escalation")
3. Expand each step to see output
4. Screenshot or copy key evidence:
   - ✅ File access confirmation
   - ✅ Permission modes (0644)
   - ✅ Credential exposure
   - ✅ System call evidence

## Files in This Repository

```
osv-scanner-vuln-poc/
├── README.md                          # Overview
├── EXPLOITATION_GUIDE.md              # Detailed attack explanations
├── EVIDENCE.md                        # Test results from local env
├── GITHUB_SETUP.md                    # This file
├── chain1-malicious-pom.xml           # Malicious Maven file
├── .github/workflows/
│   ├── attack-chain-1.yml             # Path Traversal automation
│   ├── attack-chain-2.yml             # Cache Poisoning automation
│   └── attack-chain-3.yml             # Symlink Escalation automation
└── test-run/                          # Local test files
```

## What the Workflows Do

### attack-chain-1.yml
- ✅ Creates AWS credentials file in parent directory
- ✅ Places malicious pom.xml in project
- ✅ Runs osv-scanner with strace
- ✅ Captures syscalls showing file access
- ✅ **Demonstrates:** Path traversal reading files outside project

### attack-chain-2.yml
- ✅ Creates vulnerable package.json
- ✅ Demonstrates cache file permissions (0644)
- ✅ Shows poisoned cache data
- ✅ **Demonstrates:** World-readable cache with credentials

### attack-chain-3.yml
- ✅ Creates protected secrets file
- ✅ Creates symlink to secrets
- ✅ Demonstrates TOCTOU race window
- ✅ **Demonstrates:** Symlink following and privilege escalation

## Expected Outputs

After running workflows, you should see:

```
✅ File access confirmed
✅ Credentials exposed in logs
✅ Permission mode 0644 on cache files
✅ Symlink targets visible
✅ strace output showing openat() syscalls
✅ Real-world impact demonstrated
```

## For Google Submission

After collecting evidence, create a final report:

1. Download workflow logs (click ... → Download logs)
2. Screenshot key sections
3. Create submission document showing:
   - ✅ End-to-end exploitation
   - ✅ Real CI/CD impact
   - ✅ Cross-boundary chains
   - ✅ Timestamps and system evidence
   - ✅ GitHub Actions proof of reproducibility

---

**Ready to go!**

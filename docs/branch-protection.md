# GitHub Branch Protection for GitFlow

Apply this to both `main` and `develop`.

## A) GitHub UI steps

1. Open your repo on GitHub.
2. Go to Settings -> Branches -> Add branch protection rule.
3. Branch name pattern: `main` (repeat later for `develop`).
4. Enable:
   - Require a pull request before merging
   - Require approvals: 1
   - Require status checks to pass before merging
  - Required checks for `develop`: `feature-tests`
  - Required checks for `main`: `release-tests`, `hotfix-tests`
   - Require branches to be up to date before merging
   - Restrict who can push to matching branches (leave empty so only admins/apps with bypass can push)
   - Do not allow bypassing the above settings (recommended)
5. Save changes.
6. Repeat for branch pattern `develop`.

## B) GitHub CLI (API) commands (copy-paste)

Prerequisites:

```bash
winget install --id GitHub.cli -e --source winget --accept-package-agreements --accept-source-agreements

# Open a new PowerShell window after install, or run gh by full path:
"C:\Program Files\GitHub CLI\gh.exe" --version

# If gh still is not found in the current window:
$env:Path = "C:\Program Files\GitHub CLI;" + $env:Path

gh auth login
```

Git Bash equivalent (MINGW64):

```bash
export PATH="/c/Program Files/GitHub CLI:$PATH"
gh --version
gh auth login
```

Set variables:

```bash
OWNER="<your-github-username-or-org>"
REPO="<your-repo-name>"
```

PowerShell variables (Windows PowerShell):

```powershell
$OWNER = "<your-github-username-or-org>"
$REPO = "<your-repo-name>"
```

Protect `main`:

```bash
gh api \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  /repos/$OWNER/$REPO/branches/main/protection \
  -f required_status_checks[strict]=true \
  -f required_pull_request_reviews[required_approving_review_count]=1 \
  -f enforce_admins=true \
  -f restrictions='{}' \
  -f required_linear_history=true \
  -f allow_force_pushes=false \
  -f allow_deletions=false
```

PowerShell one-line equivalent for `main`:

```powershell
gh api --method PUT -H "Accept: application/vnd.github+json" "/repos/$OWNER/$REPO/branches/main/protection" -f required_status_checks[strict]=true -f required_pull_request_reviews[required_approving_review_count]=1 -f enforce_admins=true -f restrictions='{}' -f required_linear_history=true -f allow_force_pushes=false -f allow_deletions=false
```

Protect `develop`:

```bash
gh api \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  /repos/$OWNER/$REPO/branches/develop/protection \
  -f required_status_checks[strict]=true \
  -f required_pull_request_reviews[required_approving_review_count]=1 \
  -f enforce_admins=true \
  -f restrictions='{}' \
  -f required_linear_history=true \
  -f allow_force_pushes=false \
  -f allow_deletions=false
```

PowerShell one-line equivalent for `develop`:

```powershell
gh api --method PUT -H "Accept: application/vnd.github+json" "/repos/$OWNER/$REPO/branches/develop/protection" -f required_status_checks[strict]=true -f required_pull_request_reviews[required_approving_review_count]=1 -f enforce_admins=true -f restrictions='{}' -f required_linear_history=true -f allow_force_pushes=false -f allow_deletions=false
```

Notes:

- `strict=true` means branch must be up to date.
- `required_approving_review_count=1` enforces one approval.
- `restrictions={}` blocks direct pushes for everyone except admins/apps with bypass.

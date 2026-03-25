<<<<<<< HEAD
# GitFlow Demo (GitHub + Python + pytest)

Small, practical template to run a GitFlow branch model on GitHub.

## 1) Branch model

- `main`: production branch
- `develop`: integration branch
- `feature/*`: branch from `develop`, merge back to `develop` via PR
- `release/*`: branch from `develop`, merge to `main`, then back to `develop`
- `hotfix/*`: branch from `main`, merge to `main`, then back to `develop`

## 2) Branch protection (GitHub)

Protect `main` and `develop` with:

- Require a pull request before merging
- Require at least 1 approval
- Require status checks to pass
- Restrict direct pushes
- Require branches to be up to date before merging

Detailed click-by-click and `gh` CLI API commands are in `docs/branch-protection.md`.

## 3) GitHub Actions included

- PR checks for `feature/* -> develop`
- PR checks for `release/* -> main`
- PR checks for `hotfix/* -> main`
- Optional auto-tag when PR is merged into `main`

Workflows are in `.github/workflows/`.

## 4) Minimal Python + pytest sample

- `app/calculator.py`
- `tests/test_calculator.py`

Run locally:

```bash
python -m pip install -r requirements.txt
pytest -q
```

## 5) End-to-end GitFlow demo commands

Exact copy-paste command sequence is in `docs/gitflow-demo-commands.md`.
=======
# gitflow-demo
>>>>>>> 9e78dc2a778900b037dd20ee5ef995c77132e29f

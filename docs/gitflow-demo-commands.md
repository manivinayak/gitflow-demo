# End-to-End GitFlow Demo Commands

These commands assume:

- You are in this repo folder.
- `gh` CLI is installed and authenticated.
- Default branch is `main`.

## 0) Initialize repo and push

```bash
git init
git add .
git commit -m "chore: initial gitflow demo"

git branch -M main
git remote add origin https://github.com/<owner>/<repo>.git
git push -u origin main
```

## 1) Create `develop`

```bash
git checkout -b develop
git push -u origin develop
```

## 2) Feature flow: `feature/*` -> `develop`

```bash
git checkout develop
git pull
git checkout -b feature/add-subtract

git commit --allow-empty -m "feat: sample feature branch"
git push -u origin feature/add-subtract

gh pr create --base develop --head feature/add-subtract --title "feat: add subtract" --body "Feature PR"
gh pr merge --squash --auto
```

## 3) Release flow: `release/*` -> `main`, then back to `develop`

```bash
git checkout develop
git pull
git checkout -b release/1.0.0

git commit --allow-empty -m "chore: prepare release 1.0.0"
git push -u origin release/1.0.0

gh pr create --base main --head release/1.0.0 --title "release: 1.0.0" --body "Release PR"
gh pr merge --squash --auto

gh pr create --base develop --head release/1.0.0 --title "chore: back-merge release 1.0.0" --body "Back-merge release changes"
gh pr merge --squash --auto
```

## 4) Hotfix flow: `hotfix/*` -> `main`, then back to `develop`

```bash
git checkout main
git pull
git checkout -b hotfix/1.0.1

git commit --allow-empty -m "fix: hotfix 1.0.1"
git push -u origin hotfix/1.0.1

gh pr create --base main --head hotfix/1.0.1 --title "hotfix: 1.0.1" --body "Hotfix PR"
gh pr merge --squash --auto

gh pr create --base develop --head hotfix/1.0.1 --title "chore: back-merge hotfix 1.0.1" --body "Back-merge hotfix changes"
gh pr merge --squash --auto
```

## 5) Verify graph

```bash
git fetch --all --prune
git log --graph --oneline --decorate --all
```

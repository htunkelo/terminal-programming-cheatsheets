# 📝 Git & GitHub (gh) Cheatsheet

## 📦 Everyday Git
- `git status` → Show changed files  
- `git add <file>` → Stage file  
- `git add .` → Stage all changes  
- `git commit -m "msg"` → Commit staged changes  
- `git commit --amend` → Edit last commit  
- `git push` → Push to remote  
- `git pull` → Pull latest changes  
- `git fetch` → Update refs from remote (no merge)  

## 🌿 Branching
- `git branch` → List branches  
- `git branch <name>` → Create branch  
- `git checkout <name>` → Switch branch  
- `git switch <name>` → Switch branch (newer)  
- `git checkout -b <name>` → New branch + switch  
- `git merge <branch>` → Merge into current branch  
- `git rebase <branch>` → Rebase onto branch  

## 🔎 Inspect & Undo
- `git log --oneline --graph --decorate` → Pretty log  
- `git diff` → Show unstaged changes  
- `git diff --staged` → Show staged changes  
- `git restore <file>` → Undo unstaged changes  
- `git reset HEAD <file>` → Unstage file  
- `git reset --hard` → Reset to last commit (⚠️)  
- `git revert <commit>` → Revert commit safely  

## 🐙 GitHub CLI (gh)
- `gh auth login` → Authenticate with GitHub  
- `gh repo clone <user/repo>` → Clone repo  
- `gh repo create` → Create new repo  
- `gh pr create` → Create pull request  
- `gh pr checkout <num>` → Checkout PR  
- `gh pr merge <num>` → Merge PR  
- `gh issue list` → List issues  
- `gh issue create` → Create issue  

## 🚀 Workflow Examples

### 1. Feature Branch & PR
```bash
git checkout -b feature-x
# edit files
git add .
git commit -m "Add feature x"
git push -u origin feature-x
gh pr create --fill
```

### 2. Initialize Local Repo → Push to GitHub
```bash
git init
git add .
git commit -m "Initial commit"
gh repo create NAME --private --source=. --remote=origin --push
```


## ⚙️ Setup Tip (Match GitHub default branch)
For new setups, make sure Git creates `main` instead of `master` by default:
```bash
git config --global init.defaultBranch main
```

---
⚡ **Tip**: Use `gh pr view --web` or `gh issue view --web` to open in browser.

# 📘 GIT.md

## 1. Check Git User Configuration

### 1.1 Global Config (System-wide)

```bash
git config --global user.name
git config --global user.email
```

### 1.2 Local Config (Current Repository)

```bash
git config user.name
git config user.email
```

### 1.3 View All Configurations

```bash
git config --list
```

### 1.4 Show Config with Source

```bash
git config --list --show-origin
```

---

## 2. Change Remote Repository URL

### 2.1 Update Remote URL

```bash
git remote set-url origin <new-url>
```

### 2.2 Example

```bash
git remote set-url origin https://github.com/username/repo.git
```

### 2.3 Verify Remote

```bash
git remote -v
```

---

## 3. Check Repository Status & Changes

### 3.1 Current Status

```bash
git status
```

### 3.2 View Unstaged Changes

```bash
git diff
```

### 3.3 View Staged Changes

```bash
git diff --cached
```

---

## 4. Check Pending Commits (Not Pushed)

### 4.1 View Commits Not Pushed

```bash
git log origin/<branch-name>..HEAD
```

### 4.2 Example

```bash
git log origin/main..HEAD
```

### 4.3 Short Summary

```bash
git cherry -v
```

### 4.4 Ahead/Behind Status

```bash
git status -sb
```

👉 If you see `ahead X`, those commits are **pending to push**

---

## 5. Push Code to Remote

### 5.1 Push Changes

```bash
git push origin main
```

### 5.2 First-Time Push (Set Upstream)

```bash
git push -u origin main
```

### 5.3 Verify Push Status

```bash
git status -sb
```

✅ Expected Output:

```
## main...origin/main
```

---

## 6. Commit (Add Comment)

### 6.1 Commit with Message

```bash
git commit -m "your message"
```

### 6.2 Example

```bash
git commit -m "Fix login API issue"
```

---

## 7. Update Last Commit Message

```bash
git commit --amend -m "updated message"
```

---

## 8. Best Practice for Commit Messages

- Keep message **short and clear**
    
- Use action words (Fix, Add, Update)
    

### Example

```bash
git commit -m "Add QR login timeout logic"
```

---

## 9. Multi-line Commit Message

```bash
git commit
```

Then write:

```
Short Title

Detailed description of changes
```

---

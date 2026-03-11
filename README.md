This guide explains how to configure **multiple GitHub and GitLab accounts on the same machine using SSH keys**.  
It allows you to clone, commit, and push to different repositories using different accounts.

---

# 1. Remove Existing Git Configuration (Optional)

If your system already has Git users or SSH keys configured, you may remove them first.

## 1.1 Remove Global Git User

```bash
git config --global --unset-all user.name
git config --global --unset-all user.email
```

## 1.2 Clear Credential Helper

```bash
git config --global --unset credential.helper
```

## 1.3 Remove Local Git User (Project Specific)

```bash
cd ~/Documents/Github/SpecificProject
git config --local --unset-all user.name
git config --local --unset-all user.email
```

## 1.4 Delete Global Git Configuration File

```bash
rm ~/.gitconfig
```

## 1.5 Remove Existing SSH Keys and Config

```bash
rm -rf ~/.ssh/id_rsa*
rm ~/.ssh/config
```

---

# 2. Create SSH Key for First Account (GitHub)

Example account:

```
github-demo-example@google.com
```

## 2.1 Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "rahul_github_demo" -f ~/.ssh/id_ed25519_rahul_github_demo
```

### Command Explanation

- `-t ed25519` → Defines the **type of key** (modern and secure SSH algorithm).
    
- `-C` → Comment used as a **label for the key**.
    
- `-f` → Defines the **file name and location** where the key will be saved.
    

---

## 2.2 Start SSH Agent and Add Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_rahul_github_demo
```

---

## 2.3 Verify Generated Files

```bash
ls ~/.ssh/
```

You should see:

```
id_ed25519_rahul_github_demo
id_ed25519_rahul_github_demo.pub
```

Explanation:

- `id_ed25519_rahul_github_demo` → **Private key** (used locally).
    
- `id_ed25519_rahul_github_demo.pub` → **Public key** (upload to GitHub/GitLab).
    

---

# 3. Configure SSH Config File

Create the SSH config file:

```bash
nano ~/.ssh/config
```

Add the following configuration:

```bash
# GitHub Personal account for github-demo-example@google.com
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_rahul_github_demo
    IdentitiesOnly yes
```

## Configuration Explanation

- `# Comment`  
    Used for reference to identify which key belongs to which account.
    
- `Host github-personal`  
    Alias name used when cloning repositories.
    
- `HostName github.com`  
    Actual server address.
    
- `User git`  
    GitHub SSH user (always `git`).
    
- `IdentityFile`  
    Path to the SSH private key.
    
- `IdentitiesOnly yes`  
    Forces SSH to use only the specified key.
    

Save the file:

```
CTRL + X → Y → Enter
```

---

# 4. Add SSH Key to GitHub

## 4.1 Copy Public Key

```bash
cat ~/.ssh/id_ed25519_rahul_github_demo.pub
```

Example output:

```
ssh-ed25519 AAAAC3...xyz github-demo-example@gmail.com
```

---

## 4.2 Add Key to GitHub

1. Open **GitHub**
    
2. Click **Profile Icon (Top Right)**
    
3. Go to **Settings**
    
4. Open **SSH and GPG Keys**
    
5. Click **New SSH Key**
    

Fill the details:

|Field|Value|
|---|---|
|Title|rahul_github_demo|
|Key Type|Authentication Key|
|Key|Paste copied public key|

---

## 4.3 Test GitHub SSH Connection

```bash
ssh -T git@github-personal
```

Expected output:

```
Hi your-username! You've successfully authenticated
```

---

# 5. Create SSH Key for Second Account (GitLab)

Example account:

```
gitlab-demo-example@google.com
```

## 5.1 Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "rahul_gitlab_demo" -f ~/.ssh/id_ed25519_rahul_gitlab_demo
```

---

## 5.2 Add Key to SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_rahul_gitlab_demo
```

---

## 5.3 Verify Key Files

```bash
ls ~/.ssh/
```

Expected files:

```
id_ed25519_rahul_gitlab_demo
id_ed25519_rahul_gitlab_demo.pub
```

---

# 6. Update SSH Config File

```bash
nano ~/.ssh/config
```

Update with both accounts:

```bash
# GitHub Personal account for github-demo-example@google.com
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_rahul_github_demo
    IdentitiesOnly yes

# GitLab Work account for gitlab-demo-example@google.com
Host gitlab-work
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519_rahul_gitlab_demo
    IdentitiesOnly yes
```

Save:

```
CTRL + X → Y → Enter
```

---

# 7. Add SSH Key to GitLab

## 7.1 Copy Public Key

```bash
cat ~/.ssh/id_ed25519_rahul_gitlab_demo.pub
```

Example:

```
ssh-ed25519 AAAAC3...xyz gitlab-demo-example@google.com
```

---

## 7.2 Add Key to GitLab

1. Open **GitLab**
    
2. Click **Profile Icon**
    
3. Go to **Preferences**
    
4. Open **SSH Keys**
    
5. Paste the copied key
    

Example:

|Field|Value|
|---|---|
|Title|rahul_a_neovify|
|Key|Paste SSH key|

---

## 7.3 Test GitLab Connection

```bash
ssh -T git@gitlab-work
```

Expected output:

```
Welcome to GitLab, @your-gitlab-username!
```

---

# 8. Clone Repositories

## 8.1 Create Project Directories

```bash
cd ~/Documents/Projects
mkdir GitHub
mkdir GitLab
```

---

# 9. Clone GitHub Repository

```bash
cd ~/Documents/Projects/GitHub
```

Original GitHub SSH URL:

```
git@github.com:your-username/project-name.git
```

Replace `github.com` with the SSH alias:

```bash
git clone git@github-personal:your-username/project-name.git
```

---

# 10. Clone GitLab Repository

```bash
cd ~/Documents/Projects/GitLab
```

Original GitLab SSH URL:

```
git@gitlab.com:group/project-name.git
```

Replace with alias:

```bash
git clone git@gitlab-work:group/project-name.git
```

---

# 11. Project Directory Structure

### GitHub Projects

```
~/Documents/Projects/GitHub/project-name
```

This repository will use the account:

```
github-demo-example@google.com
```

---

### GitLab Projects

```
~/Documents/Projects/GitLab/project-name
```

This repository will use the account:

```
gitlab-demo-example@google.com
```

---

# Summary

Using this setup:

- One machine can manage **multiple GitHub and GitLab accounts**
    
- Each repository uses a **separate SSH key**
    
- Commits and pushes automatically use the **correct account**
    


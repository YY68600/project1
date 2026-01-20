# Git Repository Setup & Push Cheatsheet

> Step-by-step guide to create a local repository and push to GitHub

**GitHub Username:** `ammar-utexas`

---

## Quick Start (TL;DR)

```bash
# From your project folder:
cd /path/to/your/project
git init

# ⚠️ CREATE .gitignore FIRST to exclude venv/
echo -e "venv/\nenv/\n.venv/\n__pycache__/\n.ipynb_checkpoints/\n.DS_Store" > .gitignore

git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/ammar-utexas/REPO_NAME.git
git push -u origin main
```

---

## Step-by-Step Guide

### Step 1: Navigate to Your Project Folder

```bash
# Open terminal and navigate to your project
cd /path/to/your/project

# Example:
cd ~/Documents/my-data-science-project

# Verify you're in the right place
pwd
ls -la
```

---

### Step 2: Initialize Git Repository

```bash
# Initialize a new Git repository
git init

# You should see:
# Initialized empty Git repository in /path/to/your/project/.git/
```

---

### Step 3: Configure Git Identity (First Time Only)

```bash
# Set your name and email for this repository
git config user.name "Ammar"
git config user.email "ammar@utexas.edu"

# Or set globally (applies to all repositories)
git config --global user.name "Ammar"
git config --global user.email "ammar@utexas.edu"

# Verify configuration
git config --list
```

---

### Step 4: Create .gitignore (CRITICAL!)

⚠️ **IMPORTANT:** Always create `.gitignore` BEFORE your first commit to avoid uploading virtual environments (which can be 100s of MBs)!

```bash
# Create a .gitignore file to exclude unnecessary files
cat > .gitignore << 'EOF'
# ===========================================
# VIRTUAL ENVIRONMENTS - DO NOT UPLOAD!
# ===========================================
venv/
env/
.venv/
.env/
ENV/
myenv/
datascience_env/
*_env/
.conda/
conda-env/

# ===========================================
# Python
# ===========================================
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
dist/
*.egg-info/
*.egg
.eggs/
pip-log.txt
pip-delete-this-directory.txt

# ===========================================
# Jupyter Notebooks
# ===========================================
.ipynb_checkpoints/
*/.ipynb_checkpoints/
*.ipynb_checkpoints

# ===========================================
# IDE / Editors
# ===========================================
.vscode/
.idea/
*.swp
*.swo
*~
.project
.pydevproject
.spyderproject
.spyproject

# ===========================================
# OS Generated
# ===========================================
.DS_Store
.DS_Store?
._*
Thumbs.db
ehthumbs.db
Desktop.ini

# ===========================================
# Secrets & Credentials (NEVER UPLOAD!)
# ===========================================
*.pem
*.key
*.env
.env.local
.env.*.local
secrets.yaml
secrets.json
config.local.py
credentials.json
token.json

# ===========================================
# Data files (uncomment if needed)
# ===========================================
# *.csv
# *.xlsx
# *.xls
# *.parquet
# data/
# datasets/
EOF

# Verify the file was created
cat .gitignore
```

#### Why This Matters:

| What to Exclude | Why |
|-----------------|-----|
| `venv/`, `env/` | Virtual environments are **huge** (100s of MB) and machine-specific |
| `__pycache__/` | Compiled Python files - regenerated automatically |
| `.ipynb_checkpoints/` | Jupyter auto-saves - not needed in repo |
| `.env`, `secrets.yaml` | **Security risk** - never upload credentials! |
| `.DS_Store` | macOS system files - not needed |

#### Check Before Committing:

```bash
# See what will be tracked (should NOT include venv/)
git status

# If venv/ appears in the list, your .gitignore isn't working
# Make sure .gitignore exists BEFORE running git add
```

---

### Step 5: Stage Your Files

```bash
# Add all files to staging area
git add .

# Or add specific files
git add filename.py
git add *.ipynb
git add folder/

# Check what's staged
git status
```

---

### Step 6: Create Your First Commit

```bash
# Commit with a message
git commit -m "Initial commit"

# Or with a longer message
git commit -m "Initial commit: Add project structure and Week 1 materials"

# Verify commit
git log --oneline
```

---

### Step 7: Create Repository on GitHub

1. Go to: **https://github.com/new**
2. Repository name: `your-repo-name`
3. Description: (optional)
4. **Leave empty** - Don't initialize with README, .gitignore, or license
5. Click **"Create repository"**

---

### Step 8: Connect Local to GitHub

```bash
# Rename default branch to main (if needed)
git branch -M main

# Add GitHub as remote origin
git remote add origin https://github.com/ammar-utexas/REPO_NAME.git

# Verify remote
git remote -v

# Should show:
# origin  https://github.com/ammar-utexas/REPO_NAME.git (fetch)
# origin  https://github.com/ammar-utexas/REPO_NAME.git (push)
```

---

### Step 9: Push to GitHub

```bash
# Push and set upstream (-u flag)
git push -u origin main

# You'll be prompted for credentials:
# Username: ammar-utexas
# Password: <your Personal Access Token>
```

---

## Authentication Methods

### Option A: Personal Access Token (Recommended)

1. Go to: **https://github.com/settings/tokens**
2. Click **"Generate new token (classic)"**
3. Give it a name: `my-laptop` or `data-science-course`
4. Select scopes: ✅ `repo` (full control)
5. Click **"Generate token"**
6. **Copy the token immediately** (you won't see it again!)

```bash
# When pushing, use token as password:
Username: ammar-utexas
Password: ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### Option B: Store Credentials (Avoid Re-entering)

```bash
# Cache credentials for 1 hour (3600 seconds)
git config --global credential.helper 'cache --timeout=3600'

# Or store permanently (less secure)
git config --global credential.helper store

# On macOS, use Keychain
git config --global credential.helper osxkeychain

# On Windows, use Credential Manager
git config --global credential.helper wincred
```

### Option C: SSH Key (Most Secure)

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "ammar@utexas.edu"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy output and add to: https://github.com/settings/ssh/new

# Use SSH URL instead of HTTPS
git remote set-url origin git@github.com:ammar-utexas/REPO_NAME.git

# Test connection
ssh -T git@github.com
```

---

## Daily Workflow After Setup

```bash
# 1. Make changes to your files

# 2. Check what changed
git status

# 3. Stage changes
git add .
# Or specific files: git add file1.py file2.ipynb

# 4. Commit changes
git commit -m "Add Week 2 notebook and analysis"

# 5. Push to GitHub
git push
```

---

## Useful Commands

### Check Status
```bash
git status              # See current state
git log --oneline       # See commit history
git diff                # See unstaged changes
git diff --staged       # See staged changes
```

### Undo Changes
```bash
git checkout -- file.py     # Discard changes to file
git reset HEAD file.py      # Unstage a file
git reset --soft HEAD~1     # Undo last commit (keep changes)
git reset --hard HEAD~1     # Undo last commit (discard changes)
```

### Branches
```bash
git branch                  # List branches
git branch feature-name     # Create branch
git checkout feature-name   # Switch to branch
git checkout -b feature-name  # Create and switch
git merge feature-name      # Merge into current branch
```

### Remote
```bash
git remote -v               # Show remotes
git pull                    # Get latest from GitHub
git fetch                   # Fetch without merging
```

---

## Common Issues & Solutions

### Issue: "remote origin already exists"
```bash
git remote remove origin
git remote add origin https://github.com/ammar-utexas/REPO_NAME.git
```

### Issue: "failed to push some refs"
```bash
# Pull first, then push
git pull --rebase origin main
git push
```

### Issue: "Authentication failed"
```bash
# Clear stored credentials
git credential reject https://github.com

# Or on macOS
git credential-osxkeychain erase
host=github.com
protocol=https
# Press Enter twice

# Then try pushing again with new token
```

### Issue: "fatal: not a git repository"
```bash
# Make sure you're in the project folder
cd /path/to/your/project
git init
```

### Issue: Accidentally Committed venv/ Folder
```bash
# If you already committed venv/, remove it from git (but keep locally)
git rm -r --cached venv/
git rm -r --cached env/
git rm -r --cached .venv/

# Make sure .gitignore has venv/ listed
echo "venv/" >> .gitignore

# Commit the removal
git commit -m "Remove virtual environment from tracking"

# Push the fix
git push
```

### Issue: .gitignore Not Working
```bash
# .gitignore only ignores UNTRACKED files
# If files were already tracked, you need to remove them first:

git rm -r --cached .
git add .
git commit -m "Re-add files with proper .gitignore"
```

---

## Complete Example Session

```bash
# Starting from scratch with a new project
cd ~/Documents
mkdir bioinfo-course
cd bioinfo-course

# Initialize git
git init

# Configure identity
git config user.name "Ammar"
git config user.email "ammar@utexas.edu"

# ⚠️ CREATE .gitignore FIRST (before adding any files!)
cat > .gitignore << 'EOF'
# Virtual environments - NEVER UPLOAD
venv/
env/
.venv/
*_env/

# Python
__pycache__/
*.py[cod]

# Jupyter
.ipynb_checkpoints/

# OS
.DS_Store
Thumbs.db

# Secrets
*.env
*.pem
*.key
EOF

# Create virtual environment (will be ignored by git)
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install pandas numpy matplotlib jupyterlab

# Create project files
echo "# Biomedical Informatics Course" > README.md
mkdir week1
cp ~/Downloads/student_performance.csv week1/
cp ~/Downloads/week1_notebook.ipynb week1/

# Verify venv is NOT in the list
git status
# Should show README.md, week1/, .gitignore
# Should NOT show venv/

# Stage all files
git add .

# Double-check before committing
git status

# Commit
git commit -m "Initial commit: Add Week 1 materials"

# Create repo on GitHub (https://github.com/new)
# Name: bioinfo-course
# ⚠️ Leave it EMPTY - don't add README or .gitignore on GitHub

# Connect and push
git branch -M main
git remote add origin https://github.com/ammar-utexas/bioinfo-course.git
git push -u origin main

# Enter credentials when prompted:
# Username: ammar-utexas
# Password: <paste your Personal Access Token>

# Done! Check https://github.com/ammar-utexas/bioinfo-course
```

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Initialize repo | `git init` |
| Add all files | `git add .` |
| Commit | `git commit -m "message"` |
| Add remote | `git remote add origin URL` |
| Push (first time) | `git push -u origin main` |
| Push (after) | `git push` |
| Pull updates | `git pull` |
| Check status | `git status` |
| View history | `git log --oneline` |
| Create branch | `git checkout -b name` |

---

## GitHub URLs

| Resource | URL |
|----------|-----|
| Your Profile | https://github.com/ammar-utexas |
| New Repository | https://github.com/new |
| Personal Access Tokens | https://github.com/settings/tokens |
| SSH Keys | https://github.com/settings/ssh/new |

---

*Data Science for Biomedical Informatics | UT Austin*

# GitHub Basics for Bioinformatics

This guide introduces **Git and GitHub** using practical examples common in bioinformatics workflows. You will learn how to securely connect to GitHub using SSH, push a local project to a remote repository, and use branches to manage different analyses reproducibly.

---

## Why GitHub in Bioinformatics?

- Track changes to scripts and pipelines
- Collaborate on analyses
- Reproduce results
- Safely test new methods without breaking working code

---

## Set up SSH Authentication (One-time setup)

SSH keys allow your computer to connect to GitHub securely without repeatedly entering your username and password.

### Generate a new SSH key

```bash
# Replace with the email linked to your GitHub account
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- Press **Enter** to accept the default file location
- Add a passphrase if desired (recommended)

This creates:
- **Private key**: `~/.ssh/id_ed25519` (keep secret)
- **Public key**: `~/.ssh/id_ed25519.pub` (shared with GitHub)

---

### Start the SSH agent

```bash
eval "$(ssh-agent -s)"
```

---

### Add your private key to the agent

```bash
ssh-add ~/.ssh/id_ed25519
```

---

### Copy the public key and add it to GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

1. Copy the output
2. Go to **GitHub → Settings → SSH and GPG keys → New SSH key**
3. Paste the key and save

---

### Test the SSH connection

```bash
ssh -T git@github.com
```

You should see a success message confirming authentication.

---

## Connect a Local Project to GitHub

Move into your local project directory:

```bash
cd path/to/your/local/repository
```

Add the GitHub repository as a remote:

```bash
git remote add origin git@github.com:username/repo-name.git
```

Verify the connection:

```bash
git remote -v
```

---

### Push your project to GitHub

```bash
git push -u origin main
```

After this, `git push` will work without extra options.

---

## Branching: Organizing Bioinformatics Analyses

Branches allow you to work on different analyses independently (e.g., QC, alignment, variant calling).

---

### View all branches

```bash
git branch
```

---

### Ensure you are on the main branch

```bash
git checkout main
# or
git switch main
```

---

### Create and switch to a new analysis branch

```bash
git checkout -b sequence-analysis
# or
git switch -c sequence-analysis
```

Example use cases:
- RNA-seq analysis
- Variant annotation
- Parameter testing

---

### Make changes and commit them

```bash
git add script.py
git commit -m "Add sequence analysis script"
```

Commits act as **documented checkpoints** in your analysis.

---

### Create additional branches

```bash
git checkout -b quality-control
git checkout -b variant-calling
```

---

## Merging Work Back into Main

### Switch to main

```bash
git checkout main
```

---

### Merge an analysis branch

```bash
git merge sequence-analysis
```

If there are no conflicts, the merge completes automatically.

---

## Resolving Merge Conflicts

If Git reports a conflict:

```text
CONFLICT (content): Merge conflict in file.txt
```

Check conflicted files:

```bash
git status
```

Open the file and resolve markers:

```text
<<<<<<< HEAD
Your version (main)
=======
Their version (sequence-analysis)
>>>>>>> sequence-analysis
```

Edit the file to resolve the conflict, then:

```bash
git add resolved-file.txt
git commit
```

---

## Best Practices for Bioinformatics Projects

- Use **one branch per analysis step**
- Commit early and often
- Write clear commit messages
- Keep `main` stable and reproducible

---


Happy reproducible coding! 


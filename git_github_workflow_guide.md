# Git & GitHub Workflow Guide: Pushing Code from Local to Remote

A practical reference for understanding how **Git** (local version control) and **GitHub** (remote hosting platform) work together when pushing project files.

---

## 1. Git vs. GitHub at a Glance

* **Git:** Runs locally on your machine. It creates the hidden `.git` directory, tracks file changes across stages, and stores your local commit history.
* **GitHub:** Hosts your remote repository in the cloud so you can back up code, share it, collaborate, and sync across machines (`git clone`, `git push`, `git pull`).

---

## 2. How Files Move in Git

1. **Working Directory:** Your project folder where you create and edit files (`index.html`, `Dockerfile`, `compose.yaml`, etc.).
2. **Staging Area (Index):** Where you select and prepare specific changes (`git add .`) for the next commit.
3. **Local Repository (`.git`):** Where `git commit -m "..."` saves a permanent snapshot in your local history.
4. **Remote Repository (`origin` on GitHub):** Where `git push` sends your local commits.

---

## 3. First-Time Setup & Push

Run these commands once when connecting a new local folder to a new GitHub repository:

```bash
git init
git add .
git commit -m "Initial commit: add project files"
git branch -M main
git remote add origin https://github.com/rajveerdivrala/<your-repo-name>.git
git push -u origin main
```

### What Each Command Does

* **`git init`**: Initializes a new local Git repository in your current project folder and creates the hidden `.git` directory.
* **`git add .`**: Moves all new, modified, and deleted files in the current directory into the **Staging Area**, making them ready for the next commit.
* **`git commit -m "..."`**: Saves the staged changes as a permanent snapshot in your **local Git repository** along with a descriptive commit message.
* **`git branch -M main`**: Renames the current local branch (often `master` by default) to `main` so it aligns with GitHub's standard `main` branch.
* **`git remote add origin <URL>`**: Links your local repository to your remote GitHub repository URL and gives that URL the short alias `origin`.
* **`git push -u origin main`**: Pushes your local `main` branch commits to the `main` branch on `origin` (GitHub) and sets upstream tracking (`-u`) between local `main` and `origin/main`.

---

## 4. Pushing Updates Next Time

Once the repository is initialized and linked (`origin` and `-u` are already configured), you do **not** need to run `git init`, `git branch -M main`, or `git remote add origin` again for regular updates.

Simply run these three commands whenever you modify or add files:

```bash
git add .
git commit -m "Update project files"
git push
```

---

## 5. Helpful Verification Commands

* **`git status`**: Shows modified or untracked files, staged items, and confirms whether your local branch is up to date with `origin/main`.
* **`git log --oneline`**: Displays a clean summary of your past commits.
* **`git remote -v`**: Verifies the remote repository URLs linked under `origin`.
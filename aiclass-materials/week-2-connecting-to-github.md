# Connecting to GitHub

We are going to review a few ways to connect to GitHub: 

1. The command line
2. Claude Code app
3. VS Code

You only need one of these for the course, but it helps to understand that there are many options. AI can help you explore connectivity options, and often has the ability to configure the connectivity.

> Don't put passwords, keys, or secrets in AI prompts\
> It's easier to assume everything you enter into an AI may become public information

**Before you start**

- You have a GitHub account
- Two-factor authentication is enabled
- You know your GitHub username, the one in `github.com/your-username`

---

## Which one should I use?

| You want to... | Use |
|---|---|
| Understand what is actually happening | Command line |
| Let AI do the Git steps for you | Claude Code app |
| Combines most tools into a single interface | VS Code |

They all talk to the same GitHub account and the same repos. Set up the command line once and the other two mostly take care of themselves.

---

## How authentication works

GitHub does not accept user/pass auth from tools on your computer. Instead you connect one of two ways:

- **HTTPS (with a token or browser login)** 
  - Easiest. The tool opens a browser or asks for a personal access token, and GitHub remembers you after that.
- **SSH Keys** 
  - A pair of files on your computer, one private and one public. You give GitHub the public one. More upfront setup, no browser prompts later.

For this class, **HTTPS is fine**.

---

## Installing software

### Git

**Mac**

- Option 1: type `git --version` in Terminal. If Git is missing, macOS offers to install the Command Line Tools. Click **Install** and wait.
- Option 2 (with Homebrew): `brew install git`
  - If you do not have Homebrew, install it first from https://brew.sh (paste the one command on that page into Terminal).
- Option 3: download the installer from https://git-scm.com/download/mac

**Windows**

1. Download the installer from https://git-scm.com/download/win (it starts automatically).
2. Run it. Accept the defaults on every screen. The defaults include Git Bash (a terminal) and Git Credential Manager (saves your login).
3. Open **Git Bash** from the Start menu and run `git --version` to confirm.
   - Alternative: `winget install --id Git.Git` in PowerShell.

### GitHub CLI (`gh`)

**Mac**

- With Homebrew: `brew install gh`
- Or download from https://cli.github.com

**Windows**

- In PowerShell: `winget install --id GitHub.cli`
- Or download the `.msi` installer from https://cli.github.com
- Close and reopen your terminal after installing so `gh` is found.

Confirm on either platform with `gh --version`.

### Claude Code app

**Mac**

- Download the desktop app from https://claude.ai/download and install.

**Windows**

- Download the desktop app from https://claude.ai/download and run the installer.

Sign in with your Claude account the first time it opens.

---

## 1. Command line (GitHub CLI)

The GitHub CLI (`gh`) handles login for you.

1. Install Git and the GitHub CLI (see **Installing software** above).
2. Log in:
   ```bash
   gh auth login
   ```
3. Answer the prompts:
   - **What account?** GitHub.com
   - **Protocol?** HTTPS
   - **Authenticate Git with your GitHub credentials?** Yes
   - **How to log in?** Login with a web browser
4. Copy the one-time code it shows, press Enter, and finish the login in the browser that opens.
5. Confirm it worked:
   ```bash
   gh auth status
   ```
6. Set your commit identity (once per machine; `gh auth login` does not do this):
   > If you enabled email privacy in Week 1, use the `noreply` address GitHub shows under **Settings > Emails**, directly beneath "Keep my email addresses private". It looks like `12345678+username@users.noreply.github.com`.

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "12345678+username@users.noreply.github.com"
   ```

### Create a repo

Do this on github.com, then clone it in the next section.

1. Go to github.com, click your avatar (top right), select **Your repositories**, then **New**.
2. **Owner** defaults to your account. Give it any **Name**.
3. Check **Add a README file**. This creates the first commit and the `main` branch, so the repo is not empty when you clone it.
4. Click **Create repository**, then use the green **Code** button to copy the HTTPS URL, e.g. `https://github.com/your-username/your-repo.git`.

### Test it
> See [week-2-narrative.md](week-2-narrative.md) for another example

1. Clone the repo you just created
   ```bash
   git clone https://github.com/your-username/your-repo.git
   ```
2. Change to the directory created by the clone
   ```bash
   cd your-repo
   ```
3. Create and switch to a **new branch** (avoid committing to the `main` branch. The branch keeps the changes off `main` and mirrors common real-world PR workflows)
   ```bash
   git checkout -b connection-test
   ```
4. Create or modify a file
   ```bash
   echo "test" >> notes.txt
   ```
5. Stage the file(s)
   ```bash
   git add notes.txt
   ```
6. Commit
   ```bash
   git commit -m "Test commit"
   ```
7. Push the branch and set upstream
   ```bash
   git push -u origin connection-test
   ```

If the push succeeds, your credentials have write access to the repo.

### Clean up

1. Switch off the test branch and delete it locally
   ```bash
   git checkout -
   git branch -D connection-test
   ```
2. Delete the remote test branch
   ```bash
   git push origin --delete connection-test
   ```

---

## 2. Claude Code app (GUI)

Claude Code uses your Git connection, so the cleanest path is to **set up the command line first** (section 1 above). If `git push` works in a terminal, it works in Claude Code.

1. At this point, Git, the GitHub CLI, and the Claude Code app are installed (see **Installing software** above).
2. Having completed section 1, running `gh auth login` once is enough.
3. Open the Claude Code app.
4. Open your project folder (the folder that contains, or will contain, your repo).
   - Claude can explain how to open a project folder
5. Ask Claude Code to do Git work in plain language, for example:
   - "Initialize a git repo here and make the first commit."
   - "Create a GitHub repo for this project and push it."
   - "Commit my changes and push to GitHub."
6. Claude Code runs the same `git` and `gh` commands you would type, and asks permission before each one the first time.

**If Claude Code cannot push:** open a terminal, run `gh auth status`, and fix the login there. Claude Code inherits the fix.

---

## 3. Visual Studio Code

VS Code has Git and GitHub support built in.

1. Download and install VS Code: https://code.visualstudio.com
2. Install Git and complete section 1 (at minimum `gh auth login`). If you installed VS Code before Git, restart VS Code so it detects Git.
3. In VS Code, click the **Accounts** icon at the bottom of the left sidebar (the person icon).
4. Choose **Sign in with GitHub**. A browser opens; approve the request and return to VS Code.
5. If VS Code is not opened in the current project folder, open your project folder: **File > Open Folder**.
6. Use the **Source Control** panel (the branch icon in the left sidebar) to stage, commit, and sync:
   - Type a message in the box and click the check mark to commit.
   - Click **Sync Changes** (or the circular arrows in the status bar) to push and pull.
7. To put a new local folder on GitHub, open Source Control and click **Publish to GitHub**. VS Code creates the repo and pushes it.

**To clone an existing repo:** open the Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`), type `Git: Clone`, paste the repo URL, and pick a folder.

---

## Common problems

| Problem | Fix |
|---|---|
| `Authentication failed` on push | Your saved token expired. Run `gh auth login` again, or make a new token. |
| `GH007: Your push would publish a private email address` | Your `git config --global user.email` is set to your real address. Set it to your GitHub `noreply` address from **Settings > Emails** (looks like `12345678+username@users.noreply.github.com`), then re-run the push. |
| Asked for a password every time | The credential helper is not saving it. On Mac it should be automatic; on Windows install Git for Windows, which includes Credential Manager. |
| VS Code does not show Source Control options | The folder is not a Git repo yet. Run `git init` or use **Publish to GitHub**. |
| Wrong name or email on your commits | Re-run the `git config --global user.name` / `user.email` commands in **section 1**, step 6. Fix the last commit with `git commit --amend --reset-author`. |

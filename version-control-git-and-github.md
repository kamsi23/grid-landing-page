# Version Control, Git & GitHub

Git is the tool that lets you save versions of your work, try things without fear, and build alongside other people without overwriting each other.

## Version control, and the difference between Git and GitHub

Version control is software that records every change to your project, so you can see what changed, go back to any earlier state, and work alongside other people without overwriting them.

You already know the problem it solves. The folder that looks like this:

```
index.html
index-v2.html
index-final.html
index-FINAL-real.html
index-final-FIXED-use-this-one.html
```

That is version control done by hand, badly. Nobody can tell which file is current, no one knows what changed between them, and two people editing at once means one person's work is lost.

### Git and GitHub are not the same thing

This trips up nearly everyone at first.

| | Git | GitHub |
| --- | --- | --- |
| What it is | A program on your computer | A website |
| Made by | Linus Torvalds, 2005 | A company, now owned by Microsoft |
| Needs internet | No | Yes |
| Does | Tracks changes, branches, merges | Hosts Git repos online, adds collaboration |
| Alternatives | Mercurial, SVN | GitLab, Bitbucket |

Git does the actual version control, entirely on your machine. GitHub is one place to put a copy of a Git project online so others can see it and work on it with you.

You can use Git with no GitHub account at all. GitHub without Git would be a file-sharing site.

### One-time setup

Install Git from https://git-scm.com, then tell it who you are. Every commit you ever make is stamped with this, so do it before anything else:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use the same email as your GitHub account, or your commits will not be linked to your profile.

Set the default branch name to `main` while you are here, since that is what GitHub uses:

```bash
git config --global init.defaultBranch main
```

Check it worked:

```bash
git config --list
```

### Where you type these commands

Git is a command-line tool. On Windows use **Git Bash** (installed with Git); on macOS or Linux use **Terminal**. VS Code also has a built-in terminal, which is where most people end up working.

There are graphical clients — GitHub Desktop, the Source Control panel in VS Code — and they are fine to use later. Learn the commands first. When something goes wrong, every answer you find online will be in commands, and the buttons hide what is actually happening.

## How Git works: the four places your code lives

Almost every Git command makes sense once you know this. Your code sits in four places, and commands move it between them.

| Place | What it holds |
| --- | --- |
| Working directory | The files as they are right now on your computer |
| Staging area | The changes you have marked to go into the next commit |
| Local repository | Every commit you have made, in the hidden `.git` folder |
| Remote repository | The copy on GitHub |

The staging area is the part with no equivalent in ordinary software, and the part beginners skip past. It exists so you can commit *some* of your changes and not others. If you fixed a bug and also renamed a variable, you can stage and commit them separately, which keeps the history readable.

```
  WORKING            STAGING            LOCAL              REMOTE
  DIRECTORY          AREA               REPOSITORY         (GitHub)
  your files         marked for         .git folder        github.com
                     next commit

  ┌─────────┐        ┌─────────┐        ┌─────────┐        ┌─────────┐
  │         │ add    │         │ commit │         │ push   │         │
  │  edit   ├───────►│ staged  ├───────►│ history ├───────►│  origin │
  │  files  │        │ changes │        │  saved  │        │         │
  │         │◄───────┴─────────┴────────┴─────────┤◄───────┤         │
  └─────────┘            checkout / restore       │  pull  └─────────┘
                                                  │
       git status  ──  shows where everything currently sits
```

Four commands move your work left to right: `git add`, `git commit`, `git push`. One command brings other people's work back: `git pull`.

When you are lost, `git status` tells you exactly which files are in which state. Run it constantly — it is the single most useful command in Git.

## Repositories

A repository — repo — is a project folder that Git is tracking. It holds your files plus a hidden `.git` folder containing the entire history.

Delete `.git` and you delete every version you ever saved, leaving only the current files. Never move or edit anything inside it by hand.

### Creating a new repo locally

```bash
mkdir my-project
cd my-project
git init
```

`git init` creates the `.git` folder. The project is now tracked, though nothing has been saved yet.

### What not to track

Some files should never go into a repo: dependency folders, secrets, operating-system clutter. List them in a file named `.gitignore` in the project root:

```
node_modules/
.env
.DS_Store
*.log
dist/
```

Create this **before** your first commit. Once a file is committed, adding it to `.gitignore` does not remove it from the history — and a password pushed to a public repo must be treated as leaked, not just deleted.

### Creating a remote repo on GitHub

On https://github.com, click the **+** in the top-right and choose **New repository**. Then:

1. **Repository name** — lowercase with hyphens: `portfolio-site`, not `My Portfolio`.
2. **Description** — one line on what it is.
3. **Public or private** — public is visible to everyone; private is you and whoever you invite.
4. **Initialize with a README** — tick this only if the repo is empty on GitHub's side and you have nothing locally yet. If you already ran `git init` locally, leave all three initialise options **unticked**, or you will get a conflict on your first push.

### Connecting local to remote

GitHub shows you these commands after creating an empty repo. What they mean:

```bash
git remote add origin https://github.com/yourname/my-project.git
git branch -M main
git push -u origin main
```

| Command | What it does |
| --- | --- |
| `git remote add origin <url>` | Saves the GitHub URL under the nickname `origin` |
| `git branch -M main` | Renames the current branch to `main` |
| `git push -u origin main` | Pushes, and remembers the pairing so later you can type just `git push` |

`origin` is only a nickname. It is the conventional name for "the main remote", nothing more.

Check what a repo is connected to:

```bash
git remote -v
```

### Every repo should have a README

`README.md` is shown on the repo's front page on GitHub. For a student project it should say what the project is, how to run it, and what it is built with. It is often the only thing anyone reads.

## Cloning

Cloning downloads a complete copy of an existing repository — every file and the entire history — and connects it to the original.

```bash
git clone https://github.com/someone/project.git
cd project
```

This creates a folder named after the repo, already initialised, with `origin` already pointing at the URL you cloned from. You do not run `git init` afterwards.

To clone into a differently named folder:

```bash
git clone https://github.com/someone/project.git my-folder-name
```

### Clone or download ZIP?

GitHub offers both. The ZIP gives you the files with no `.git` folder, so no history, no ability to commit, no connection to the original. Use clone unless you only want to look at the code.

### Clone or fork?

| | Clone | Fork |
| --- | --- | --- |
| Where the copy goes | Your computer | Your GitHub account |
| Can you push to it? | Only if you have write access | Yes, it is yours |
| Used for | Getting any repo onto your machine | Contributing to a project you do not own |

These are not alternatives — contributing to someone else's project usually means **fork first, then clone your fork**. That flow is in the pull requests section.

### The everyday rhythm

Once a repo is cloned, a working day looks like this:

```bash
git pull                 # get everyone else's changes first
# ...do your work...
git add .
git commit -m "Add contact form validation"
git push
```

Pulling first is the habit worth building. Starting work on an out-of-date copy is how most merge conflicts are created.

## Commits

A commit is a saved snapshot of your project at one moment, with a message explaining why. The history is a chain of these snapshots, and you can return to any of them.

### The cycle

```bash
git status                          # what has changed?
git add index.html styles.css       # stage specific files
git add .                           # or stage everything changed
git commit -m "Add responsive navigation bar"
```

`git add` moves changes into the staging area. `git commit` writes the staged changes into history. Changes you never staged are not in the commit.

See what you have done:

```bash
git log --oneline          # compact history
git log --oneline -5       # last five commits
git diff                   # what changed but is not staged
git diff --staged          # what is staged and about to be committed
```

### Writing descriptive messages

The message is for the person who reads it in six months, which is usually you. The convention that works:

**Write in the imperative, as if completing the sentence "This commit will…"**

```
Add email validation to the signup form
Fix navigation overlap on screens under 400px
Remove unused Bootstrap import
Update README with setup instructions
```

Not:

```
update
fixed stuff
asdf
final version
changes
work from today
```

The second list is what a repo full of useless history looks like. When a bug appears and you need to find which change caused it, "fixed stuff" tells you nothing.

Practical rules:

- Keep the first line under about 50 characters.
- Say **what** changed and **why**, not how — the diff already shows how.
- One logical change per commit. Fixing a bug and restyling the footer are two commits.
- No full stop at the end of the subject line.

For a longer explanation, run `git commit` with no `-m`, which opens an editor. First line is the subject, blank line, then the detail.

### Commit often

A commit is cheap. Commit each time you finish something that works, not once at the end of the day. Small commits are easier to understand, easier to review, and easier to undo.

A commit that touches thirty files with the message "week 3 work" is almost impossible to do anything useful with later.

### Fixing the last commit

Message wrong, or you forgot a file?

```bash
git add forgotten-file.css
git commit --amend -m "Corrected message"
```

Only do this on commits you have **not** pushed. Amending rewrites history, and rewriting history that others have already pulled causes real problems.

### What never goes in a commit

- Passwords, API keys, `.env` files
- `node_modules/` or other dependency folders
- Large binaries and video files
- Commented-out code you are "keeping just in case" — that is what the history is for

## Branches

A branch is an independent line of work. You can build a feature, break things, and experiment freely, while `main` stays exactly as it was.

This is what makes Git worth learning. Without branches, every unfinished experiment sits in the same files everyone else is using.

```
            D---E  feature/login
           /         \
  A---B---C-----------F   main
```

Commits D and E happen on the branch. `main` is untouched until F, where the branch is merged back in.

### Creating and switching

```bash
git branch feature/login        # create it
git checkout feature/login      # switch to it

git checkout -b feature/login   # create and switch, in one step
```

Newer Git versions have `git switch`, which does the same thing more clearly:

```bash
git switch -c feature/login     # create and switch
git switch main                 # go back
```

See your branches, with a star on the current one:

```bash
git branch
```

### Naming

Use a prefix and a hyphenated description:

```
feature/contact-form
fix/mobile-nav-overlap
docs/update-readme
```

Names like `test`, `new`, `branch2` or `johns-branch` tell nobody anything.

### Working on a branch

Nothing changes. Edit, `git add`, `git commit` as usual — the commits land on the branch you are on. Switch back to `main` and your changes vanish from view, because they live on the other branch. They are not lost.

To push a branch to GitHub for the first time:

```bash
git push -u origin feature/login
```

### Combining branches: merge

When the work is done, switch to the branch you want to merge **into**, then merge:

```bash
git checkout main
git pull                        # make sure main is current
git merge feature/login
```

The direction catches people out. You stand on the destination and pull the other branch in.

If the two branches changed different things, Git merges them automatically and you are done.

### Merge conflicts

A conflict happens when both branches changed **the same lines of the same file**. Git cannot guess which version is right, so it stops and asks.

The file is marked up like this:

```
<<<<<<< HEAD
<h1>Welcome to my site</h1>
=======
<h1>Welcome to my portfolio</h1>
>>>>>>> feature/login
```

Between `<<<<<<<` and `=======` is the version on your current branch. Between `=======` and `>>>>>>>` is the incoming version.

To resolve it:

1. Open the file and decide what the final content should be — one version, the other, or a combination.
2. Delete all three marker lines.
3. `git add` the file.
4. `git commit` to finish the merge.

Conflicts are normal and not a sign anything went wrong. They are Git refusing to silently throw away someone's work.

### Cleaning up

```bash
git branch -d feature/login              # delete locally once merged
git push origin --delete feature/login   # delete on GitHub
```

`-d` refuses to delete a branch whose work is not merged. `-D` forces it, which is how you throw an experiment away on purpose.

## Pull Requests

A pull request is a proposal: "here are my commits, please review them and merge them into your project." It is a GitHub feature, not a Git one — Git has branches and merges, GitHub adds the conversation around them.

This is how open source works, and how nearly every professional team ships code.

### Contributing to a project you do not own

You cannot push to someone else's repository. The route around that is fork, change, propose.

1. **Fork** — on the project's GitHub page, click **Fork**. You now own a copy under your account.
2. **Clone your fork** to your machine:
   ```bash
   git clone https://github.com/yourname/their-project.git
   cd their-project
   ```
3. **Branch** for your change:
   ```bash
   git checkout -b fix/typo-in-readme
   ```
4. **Commit** your work with clear messages.
5. **Push the branch to your fork**:
   ```bash
   git push -u origin fix/typo-in-readme
   ```
6. **Open the pull request** — GitHub shows a banner offering to compare and open one. Check the base repository is theirs and the base branch is their `main`.
7. **Respond to review** — push more commits to the same branch and the pull request updates itself automatically.

### Pull requests inside your own team

With write access there is no fork. Branch, push, and open the pull request from your branch into `main` in the same repo. Everything else is the same.

Many teams protect `main` so that nothing can be pushed to it directly — every change has to arrive through a reviewed pull request.

### Writing one worth merging

| Part | What to put |
| --- | --- |
| Title | What the change does, in one line |
| Description | Why it is needed, and what you did |
| Linked issue | `Closes #42` — GitHub closes the issue on merge |
| Screenshots | For anything visual, before and after |
| Testing notes | How the reviewer can check it works |

Keep one pull request to one concern. A reviewer can approve "fix the mobile nav overlap" quickly; "fix nav, restyle footer, upgrade three dependencies" sits unreviewed for a week.

### Reviewing someone else's

On the **Files changed** tab you can comment on specific lines, then submit the review as **Comment**, **Approve**, or **Request changes**.

Be specific and be kind. "This breaks on screens under 400px — try `flex-wrap: wrap` here" is useful. "This is wrong" is not.

### Merging

Once approved, the author or a maintainer clicks **Merge pull request**. Then delete the branch — GitHub offers a button — and locally:

```bash
git checkout main
git pull
git branch -d fix/typo-in-readme
```

### Keeping a fork current

A fork does not update itself. Point it at the original, then pull:

```bash
git remote add upstream https://github.com/original-owner/project.git
git fetch upstream
git checkout main
git merge upstream/main
git push
```

`origin` is your fork; `upstream` is the project you forked from. Do this before starting any new contribution.

### First contribution

Start small — a typo in documentation, a broken link, a clearer sentence in a README. Look for repositories tagging issues `good first issue` or `help wanted`, and read the project's `CONTRIBUTING.md` before opening anything.

## Reference, practice and pitfalls

### Command reference

| Command | Does |
| --- | --- |
| `git init` | Start tracking a folder |
| `git clone <url>` | Copy a remote repo to your machine |
| `git status` | Show what has changed and what is staged |
| `git add <file>` / `git add .` | Stage changes |
| `git commit -m "message"` | Save staged changes to history |
| `git log --oneline` | View the history |
| `git diff` | See unstaged changes |
| `git push` | Send commits to the remote |
| `git pull` | Fetch and merge the remote's commits |
| `git branch` | List branches |
| `git checkout -b <name>` | Create a branch and switch to it |
| `git switch <name>` | Switch branches |
| `git merge <name>` | Merge a branch into the current one |
| `git remote -v` | Show connected remotes |
| `git restore <file>` | Discard changes to a file |

### Practice

**Exercise 1 — solo repo**

- [ ] Configure your name and email
- [ ] Create a folder, `git init`, add a `.gitignore` and a `README.md`
- [ ] Make three commits with descriptive messages
- [ ] Run `git log --oneline` and read your own history back
- [ ] Create an empty repo on GitHub and push to it

**Exercise 2 — branching**

- [ ] Create `feature/about-page` and add a page on it
- [ ] Switch to `main` and confirm the new file is not there
- [ ] Switch back and confirm it returns
- [ ] Merge the branch into `main` and delete it

**Exercise 3 — make a conflict on purpose**

- [ ] On `main`, change the `<h1>` and commit
- [ ] On a branch made before that change, edit the same line differently and commit
- [ ] Merge, read the conflict markers, resolve it, commit

**Exercise 4 — pull request**

- [ ] Fork a classmate's repo
- [ ] Clone your fork, branch, make one small improvement
- [ ] Push and open a pull request with a clear title and description
- [ ] Review someone else's pull request and leave one line comment

### Mistakes beginners make

| Mistake | What happens | Fix |
| --- | --- | --- |
| Committing without `git add` | "Nothing added to commit" | Stage first |
| Committing `node_modules/` | A repo with thousands of files | `.gitignore` before the first commit |
| Pushing an `.env` file | Secrets leaked publicly | Rotate the keys, they are compromised |
| Working directly on `main` | No safe state to return to | Branch for every change |
| Never pulling before work | Constant conflicts | `git pull` first, always |
| `git add .` without checking | Files you did not mean to commit | `git status` before every commit |
| Messages like "update" | Useless history | Imperative, specific, one change |
| Panicking at a conflict | Nothing | Read the markers, pick, delete them, commit |

### When it goes wrong

Almost nothing in Git is unrecoverable once committed. `git reflog` shows every state your repo has been in, including after a bad merge or reset.

The real rule for beginners: **commit early and often.** A committed mistake can always be undone. Uncommitted work that gets overwritten is simply gone.

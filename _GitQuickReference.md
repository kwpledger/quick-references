# Git Quick Reference (9/5/2026)

Ordered by how often it actually gets used: daily workflow first, emergencies last.

---

## 1. Workflow for Committing, Pushing, and Pulling (Not Just Daily -- Every Time)

### At Work When Committing (Using LLNL GitLab)

Note: If you are not on site *and* connected to the wired network, make sure you are on the VPN first!

| \# | Command | Does |
| --- | --- | --- |
| 1 | `git status` | See what's changed since last commit |
| 2 | `git add .` | Stage all changes |
| 3 | `git commit -m "Message in past tense"` | Save changes to history |
| 4 | `git push` | Send commits to GitLab |
| 5 | `git log --oneline [-#]` | Show commit history, one line each [optional: the last # of commits] |

### At Home — Start of Session (Using GitHub, Sitting Down at Either PC)

| \# | Command | Does |
| --- | --- | --- |
| 1 | `git status` | See what's changed since last commit |
| 2 | `git add .` | Stage all changes |
| 3 | `git pull --rebase` | Get whatever the OTHER machine pushed since your last push here (Do this BEFORE you push.) |

**Notes:**
1. `git pull --rebase` avoids merge commits: it fetches the latest remote changes and replays your unpushed local commits on top. **Never use it on a shared branch** — it rewrites commit hashes and disrupts everyone else's work.
2. Remember the Mnemonic: "Pull when you sit down, push when you get up." The committing-point ritual below covers the "get up" half.

### At Home — When Committing (Using GitHub)

| \# | Command | Does |
| --- | --- | --- |
| 1 | `git status` | See what's changed since last commit |
| 2 | `git add .` | Stage all changes |
| 3 | `git commit -m "Message in past tense"` | Save changes to history |
| 4 | `git pull --rebase` | Get whatever the OTHER machine pushed since your last push here (Do this BEFORE you push.) |
| 5 | `git push` | Send commits to GitHub |
| 6 | `git log --oneline [-#]` | Show commit history, one line each [optional: the last # of commits] |

**Notes Again:**
1. `git pull --rebase` avoids merge commits: it fetches the latest remote changes and replays your unpushed local commits on top. **Never use it on a shared branch** — it rewrites commit hashes and disrupts everyone else's work.
2. Remember the Mnemonic: "Pull when you sit down, push when you get up." The committing-point ritual below covers the "get up" half.

### Staging shortcuts

| Command | Does |
| --- | --- |
| `git add .` | Stage everything — new **and** modified files |
| `git commit -am "msg"` | Stage + commit modified **tracked** files in one step. Does **not** include new untracked files |

---

## 2. Pre-Commit Checklist

- [ ] Saved all scenes in Unity (`Ctrl` + `S`) — and the project itself
- [ ] Ran `git status` to see what's actually changing
- [ ] Commit message describes what was done, in **past tense**
- [ ] Not committing `Library/`, `Temp/`, or other generated cruft

Unity-specific pre-commit settings live in `_UnityQuickReference.md` §8.

---

## 3. Investigating

| Command | Shows |
| --- | --- |
| `git diff` | Unstaged changes |
| `git diff --staged` | Staged-but-not-committed changes |
| `git log` | Full commit history with details |
| `git log --oneline -20` | Last 20 commits only — far more readable |
| `git show <hash>` | What changed in one specific commit |
| `git log --oneline --graph --all` | Branches and merges visualized in the terminal |
| `git lfs ls-files` | Files tracked by Git LFS |

---

## 4. Branching & Switching

| Command | Does |
| --- | --- |
| `git branch` | List branches |
| `git branch <name>` | Create a new branch |
| `git switch <name>` | Switch to an existing branch — safer than `checkout` |
| `git switch -c <name>` | Create a new branch and switch to it immediately |
| `git merge <name>` | Merge a branch into the current branch |
| `git branch -vv` | List branches **with** tracking info and how far behind each is |
| `git branch -d <name>` | Delete a merged branch (refuses if it has unmerged work) |
| `git push origin --delete <name>` | Delete the branch on GitHub too |

---

## 5. Reverting & Restoring

| Command | Does |
| --- | --- |
| `git restore <file>` | Discard unstaged changes in one file |
| `git restore .` | Discard all unstaged changes in the current directory |
| `git restore --source=<hash> <file>` | Restore one file from a past commit without moving HEAD |
| `git reset --hard <hash>` | ⚠️ Wipe everything and roll the project back to a commit |
| `git reset --hard HEAD` | ⚠️ Wipe everything and roll back to the last commit |

---

## 6. LLNL GitLab Access

Remote: `sd-git.llnl.gov` — **internal only.**

Must be on the lab network (on-site) or connected to VPN (working from home) before:

- `git push`
- `git pull`
- `git fetch`
- `git clone`

Local commits work fine without VPN. Verify the remote with `git remote -v`.

---

## 7. Home Projects Using Two Computers for One Project.

To start a new project (local on Machine 1) and continue it as local on the Machine 2:

1. Per project (one time on Machine 1, in `.\Claude Code Projects\[NameOfProject]\`):

    | \# | Command |
    | --- | --- | 
    | 1 | `git init` |
    | 2 | `git add .` |
    | 3 | `git commit -m "Initial commit"` |
    | 4 | `gh repo create [Name of Project] --private --source=. --push` |

2. On Machine 2, pick any folder and go to it. It does not need to have the same drive letter or match the path:

    | \# | Command |
    | --- | --- | 
    | 1 | `git clone https://github.com/kwpledger/[NameOfProject].git` |
    | 2 | (Only if the repo has a package.json in its root)<br>`npm install` |

**Note:** `node_modules/` is gitignored on purpose. it's large, machine-specific, and fully derivable. `npm install` reads `package.json` and re-downloads everything locally.

3. Then the daily rhythm is just:

    - Before you start working on a machine: `git pull --rebase`
    - When you stop: `git add -A`; `git commit -m "..."`; `git push`

### Things I learned from someone else **before** I learned them the hard way

- LFS is per-machine, not per-repo. Run `git lfs install` once on the second machine *before* you clone any projects. If you skip it, your FBX/textures/video come down as tiny text "pointer" files instead of the real assets, and Unity opens to a pile of broken pink/missing references. Your `.gitattributes` travels with the repo, but the LFS *program* has to be present locally on each PC.
- Watch GitHub's LFS quota. This is the one that might actually bite you. GitHub's free tier is 10 GB of LFS storage and 10 GB/month bandwidth. A Unity project with baked lighting, video files, and high-res textures blows past that fast, and every git clone/big pull on the laptop spends bandwidth. If a project is asset-heavy, either add metered billing (not cheap; as of 7/30/26, additional storage is $0.07 per GiB and additional data transfer out is $0.0875 per GiB) or see about keeping that particular project on your LLNL GitLab instead, which has its own limits.
- If the "pull when you sit, push when you stand" discipline slips, Git stops you — rejected `push`, run `pull --rebase`, `push` again. The cost of forgetting is thirty seconds and mild annoyance, not lost work. The only way to convert that into real damage is to reach for `--force` when you see the rejection, and now you know exactly why not to.

---

## 8. Troubleshooting

### "fatal: not a git repository"

1. Look at the PowerShell prompt.
2. If it isn't `PS C:\[Program] Projects\[Name of Project]>`, you're in the wrong folder.
3. Fix: `cd "C:\[Program] Projects\[Name of Project]"`
4. Better: open PowerShell from inside the project folder — right-click --> **Open in Terminal**.

> **The silent version of this bug:** a folder that isn't a repo at all throws the fatal error. Being in the *wrong repo* throws nothing. Run `git remote -v` or `git status` to confirm which project you're looking at.

### Things I learned the hard way

- `.gitignore` and `.gitattributes` must start with a dot, no prefix.
- Adding a file to `.gitignore` does nothing once Git already tracks it. Use `git rm --cached <file>`.
- `Library/` should **never** be tracked — Unity regenerates it.
- If `Temp/` gets tracked, untrack it with `git rm -r --cached Temp/`.
- Commit often, small and focused.
- `warning: LF will be replaced by CRLF` is fine. Ignore it.
- Rebasing rewrites parent commits and changes hashes. Never rebase anything already pushed to a shared public branch. `pull --rebase` reorders your *unpushed* commits, which is always fine. The "never rebase shared history" rule is about *already-pushed* commits, a different thing.
- `git checkout <hash>` with **no filename** detaches HEAD — the whole project jumps to that commit.
    - To revert one file instead: `git restore --source=<hash> <file>`
    - To escape detached HEAD: `git switch main`, then `git restore .`

### "GH013: Repository rule violations found for refs/heads/main"

- **Symptom:** `git push` is rejected with `! [remote rejected] main -> main (push declined due to repository rule violations)`.
- **Cause:** The repo has a ruleset requiring a status check — e.g. `Required status check "Workers Builds: kwpledger-site" is expected` — before anything lands on `main`. A direct push can *never* satisfy it: the check runs against a commit on the remote, and the rule won't let the commit reach the remote. Chicken and egg.
- **Fix:** Direct pushes to `main` are impossible in this repo. Always route through a temporary branch. Your commit is already safe locally — only the push failed, nothing needs redoing.

Give the branch a **disposable, descriptive name** — `fix-nav-contrast`, never a reusable one like `updates`. It's `<name>` below.

    | \# | Command | Does |
    | --- | --- | --- |
    | 1 | `git switch -c <name>` | Creates a <name> branch off main and switches to it |
    | 2 | `git push -u origin <name>` | Publishes the branch and sets it to track `origin` |
        
At this point, Git will give you a link to follow:
    
    ```powershell
    remote: Create a pull request for '<name>' on GitHub by visiting:
    remote:      https://github.com/kwpledger/kwpledger-site/pull/new/<name>
    ```

Create the PR as directed and wait for Workers Builds to go green. Then merge the PR. Go back to PowerShell.
    
    | \# | Command | Does |
    | --- | --- | --- |
    | 3 | `git switch main` | Get off the merged branch |
    | 4 | `git pull` | Fast-forward to include your merge |
    | 5 | `git branch -d <name>` | Delete local branch (refuses if unmerged) |

If **Automatically delete head branches** is ticked in the repo's Settings → General, you're done — it removed the GitHub copy at merge time. If not, one more:

    | \# | Command | Does |
    | --- | --- | --- |
    | 6 | `git push origin --delete <name>` | Delete it on GitHub |

Also worth setting once: `git config --global fetch.prune true`. A deleted remote branch then shows as `[origin/<name>: gone]` in `git branch -vv` instead of quietly pulling nothing.


### "git pull says 'Already up to date' but files are missing"

- **Symptom:** GitHub shows files that aren't in your local folder. `git pull` reports `Already up to date.` and changes nothing.
- **Cause:** You're standing on a stale branch. `git pull` only fetches the branch you're *on* — so "up to date" is true and useless at the same time. Usually a leftover PR branch you merged weeks ago and never switched away from.
- **Fix:**

    | \# | Command | Does |
    | --- | --- | --- |
    | 1 | `git status` | **Line 2** names your branch — that's the answer |
    | 2 | `git switch main` | Get back on the real branch |
    | 3 | `git pull` | Now it actually pulls |
    | 4 | `git branch -d <stale>` | Delete the bookmark so it can't recur |

- `Your branch is behind 'origin/main' by N commits, and can be fast-forwarded` is the **safe** message. No merge, no conflict — the pointer just slides forward.

> **Before deleting, prove nothing is stranded on it:**
> `git merge-base --is-ancestor <branch> origin/main && echo "fully merged"`
> Prints the message = every commit on that branch is already in `main`, safe to delete. Silence = it has unique work; look before you delete.

**The trap:** `git status` line 2 has said `On branch <wrong-thing>` the entire time. It doesn't read as a warning, so you skim past it. When a pull comes back empty and you expected files, check the branch before you check the network.

*Seen 9/6/2026 — kwpledger-site, branch `updates`, stranded a month, 44 commits behind, two docs apparently "missing."*

### Escaping the `(END)` prompt

- **Symptom:** A long `git log` / `git diff` / `git branch` fills the screen and stops at a line reading `(END)` or `:`. Typing does nothing useful.
- **Cause:** Git pipes long output through a *pager* (`less`), a separate program from the 1980s. You're not stuck in Git — you're inside `less`.
- **Fix:** Press `q`.
- **Other keys inside the pager:**

    | Key | Action |
    | --- | --- |
    | `q` | Quit back to the prompt |
    | `Space` / `b` | Page down / page up |
    | `j` / `k` or arrows | Scroll one line |
    | `/text` + Enter | Search forward (`n` = next match) |
    | `g` / `G` | Jump to top / bottom |

- **Avoiding it:**
    - One-off: `git --no-pager log --oneline`
    - Never page again: `git config --global core.pager cat`
    - Page only when output exceeds one screen: `git config --global core.pager "less -FRX"`
- **Also applies to:** `man` pages, `top`, `htop`, and anything else that hands output to `less`. `q` works in all of them.

---

## 9. In Case of Dire Emergency

| Command | Does |
| --- | --- |
| `git reset --soft HEAD~1` | this is the safe version (see note below) |

Undoes the last commit but **keeps all changes staged** — perfect for when you committed before saving a Unity scene.

**Note:** On a single machine that's just destructive-to-uncommitted-work. On two machines it's sharper: if you ever `reset --hard` (as opposed to `--soft`) to roll back and then `push`, you can clobber commits the other PC made. If you find yourself wanting to undo already-pushed history across machines, that's the moment to stop and check both are in sync first (or reach for `git revert`, which is safe to push). Not a change to make — just a "handle with extra care now that there are two of you."

Beyond that: [dangitgit.com](https://dangitgit.com) — or the saltier [ohshitgit.com](https://ohshitgit.com). Both cover:

- Committed to the wrong branch
- Need to undo a commit that was already pushed
- Accidentally committed sensitive files
- Lost work in a detached HEAD state
- Made a mess and want to nuke everything and start over

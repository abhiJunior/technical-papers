# Git Mistakes & How to Fix Them 

> Git mistakes happen to everyone.
> This guide shows common Git mistakes, why they happen, and how to fix them in simple language.

---

# 1. I did something terribly wrong! Can Git recover it?

## Situation

You accidentally:

- Deleted files
- Ran the wrong command
- Reset your branch
- Broke your repository

You want to go back to a previous working state.

---

## Solution

```bash
git reflog
```

You'll see something like

```text
HEAD@{0}
HEAD@{1}
HEAD@{2}
HEAD@{3}
```

Find the point where everything was working.

Then run

```bash
git reset HEAD@{index}
```

Example

```bash
git reset HEAD@{2}
```

---

## Why it works

`git reflog` keeps track of where your HEAD has been.

Think of it as Git's **Time Machine**.

It lets you go back to an earlier state.

---

## Remember

Use `git reflog` whenever you think you've lost your work.

---

# 2. I committed, but forgot to include one small change

## Situation

You already committed.

```bash
git commit -m "Added Login Feature"
```

Then you realize...

"I forgot to include one file."

---

## Solution

```bash
git add .

git commit --amend --no-edit
```

---

## Why it works

Instead of creating a new commit,

Git updates your **last commit**.

Before

```
Commit A
```

After

```
Commit A (Updated)
```

---

## Warning

Only use this if the commit has **not been pushed**.

Avoid amending commits on shared branches.

---

# 3. I need to change the last commit message

## Situation

You wrote

```text
Fixed stuff
```

But you wanted

```text
Fix login validation bug
```

---

## Solution

```bash
git commit --amend
```

Git opens the editor.

Update the message.

Save.

Done.

---

## Why it works

Only the commit message changes.

Your code stays exactly the same.

---

# 4. I committed to main, but it should have been a new branch

## Situation

Current history

```
main

Commit A
Commit B
```

Commit B should belong to

```
feature/login
```

---

## Solution

Create a new branch

```bash
git branch feature/login
```

Remove the last commit from main

```bash
git reset HEAD~ --hard
```

Move to the new branch

```bash
git checkout feature/login
```

---

## Result

Before

```
main

A
B
```

After

```
main

A

feature/login

A
B
```

---

## Why it works

The branch is created before removing the commit.

The new branch still contains the commit.

---

# 5. I accidentally committed to the wrong branch

## Situation

You committed on

```
main
```

Instead of

```
feature/payment
```

---

## Solution

Undo the commit but keep the changes

```bash
git reset HEAD~ --soft
```

Save the changes temporarily

```bash
git stash
```

Switch branch

```bash
git checkout feature/payment
```

Restore the changes

```bash
git stash pop
```

Commit again

```bash
git add .

git commit -m "Payment Feature"
```

---

## Why it works

- Soft reset removes only the commit.
- Stash temporarily stores your work.
- After switching branches, you restore and commit it.

---

## Another way

You can also use

```bash
git cherry-pick
```

to copy a commit from one branch to another.

---

# 6. I ran git diff but nothing appeared

## Situation

```bash
git add .

git diff
```

Output

```
Nothing
```

---

## Why?

Because your changes are already in the staging area.

---

## Solution

```bash
git diff --staged
```

---

## Remember

```
Working Directory
        ↓
 Staging Area
        ↓
      Commit
```

`git diff`

Shows

```
Working Directory

↓

Staging Area
```

`git diff --staged`

Shows

```
Staging Area

↓

Last Commit
```

---

# 7. I need to undo an old commit

## Situation

You made a mistake five commits ago.

You want to remove only that commit.

---

## Solution

View commit history

```bash
git log
```

Copy the commit hash.

Run

```bash
git revert <commit-hash>
```

Example

```bash
git revert 3f6a9b2
```

---

## Why it works

Git creates a **new commit**

that reverses the changes.

History remains safe.

---

## Example

Before

```
A
B
C (Bug)
D
E
```

After

```
A
B
C
D
E
F (Undo C)
```

---

# 8. I want to restore one file

## Situation

You accidentally edited

```
config.js
```

Now it is broken.

---

## Solution

Find an older commit

```bash
git log
```

Restore the file

```bash
git checkout <commit-hash> -- config.js
```

Commit the restored file

```bash
git commit -m "Restore config.js"
```

---

## Why it works

Only that file is restored.

Everything else remains unchanged.

---

# 9. My repository is completely broken

## Situation

Nothing is working.

- Merge conflicts
- Wrong commits
- Untracked files
- Broken branch

---

## Solution

Download the latest changes

```bash
git fetch origin
```

Switch to main

```bash
git checkout main
```

Reset to the remote version

```bash
git reset --hard origin/main
```

Remove untracked files

```bash
git clean -fd
```

---

## Warning

This permanently deletes

- Local commits
- Local changes
- Untracked files

Only use this if you are sure.

---

# Summary

| Mistake | Command |
|----------|---------|
| Recover lost work | `git reflog` |
| Forgot to add files | `git commit --amend --no-edit` |
| Wrong commit message | `git commit --amend` |
| Commit should be on new branch | `git branch` + `git reset` |
| Committed on wrong branch | `git stash` + `git checkout` |
| git diff shows nothing | `git diff --staged` |
| Undo old commit | `git revert` |
| Restore one file | `git checkout HASH -- file` |
| Reset repository | `git reset --hard` |

---

# Presentation Tip 🎤

For every topic, follow this flow:

1. Create the mistake.
2. Ask the audience what they would do.
3. Show the problem.
4. Explain why it happened.
5. Run the Git command.
6. Verify using `git status` or `git log`.
7. Share one important takeaway.

This makes Git much easier to understand than simply memorizing commands.
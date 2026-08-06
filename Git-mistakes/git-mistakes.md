# Git Mistakes & How to Fix Them 🚀

> **Goal:** Learn Git by making mistakes first, then fixing them.

---

# Demo Setup

We'll use one Git repository throughout the presentation.

```bash
mkdir git-mistakes-demo
cd git-mistakes-demo

git init

touch index.html style.css

git add .
git commit -m "Initial commit"
```

Check the history

```bash
git log --oneline --graph
```

---

# 1. Forgot to Add a File to the Last Commit

## Scenario

You changed two files.

```
index.html
style.css
```

But you accidentally committed only one.

```bash
git add index.html

git commit -m "Added Homepage"
```

Later you realize

> "Oops! I forgot style.css."

---

## Solution

```bash
git add style.css

git commit --amend --no-edit
```

---

## Why it works

Instead of creating

```
Commit A

Commit B
```

Git updates

```
Commit A (Updated)
```

---

## Remember

✅ Local commits only

❌ Don't amend commits after pushing

---

# 2. Wrong Commit Message

## Scenario

```bash
git commit -m "asdf"
```

Oops...

Wanted

```
Added About Page
```

---

## Solution

```bash
git commit --amend
```

Update the message.

Save.

Done.

---

## Why it works

Only the commit message changes.

The code stays exactly the same.

---

# 3. Accidentally Committed to the Wrong Branch

## Scenario

You're on

```
main
```

You commit

```
Navbar Styling
```

But it belongs to

```
feature/navbar
```

---

## Solution

```bash
git reset --soft HEAD~1

git stash

git checkout -b feature/navbar

git stash pop

git add .

git commit -m "Navbar Styling"
```

---

## Why it works

- Removes the commit
- Keeps your code
- Moves the changes to the correct branch

---

## Alternative

```bash
git cherry-pick
```

---

# 4. Committed to Main Instead of Creating a Feature Branch

## Scenario

History

```
main

A
B
```

Commit B should be on

```
feature/footer
```

---

## Solution

```bash
git branch feature/footer

git reset --hard HEAD~1

git checkout feature/footer
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

feature/footer

A
B
```

---

## Why it works

The new branch keeps the commit.

Main no longer has it.

---

# 5. git diff Shows Nothing

## Scenario

```bash
git add style.css

git diff
```

Nothing appears.

---

## Why?

Because the file is already staged.

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

---

# 6. Undo an Old Commit

## Scenario

History

```
A
B
C (Bug)
D
```

Need to remove

```
C
```

without deleting D.

---

## Solution

```bash
git log

git revert <hash>
```

---

## Result

```
A
B
C
D
E (Undo C)
```

---

## Why it works

Git creates a new commit that reverses the old one.

No history is deleted.

---

# 7. Git Time Machine (git reflog)

## Scenario

You accidentally destroy history.

```bash
git reset --hard HEAD~2
```

Now two commits disappear.

Everyone panics.

---

## Solution

```bash
git reflog
```

Find

```
HEAD@{1}
```

Recover

```bash
git reset --hard HEAD@{1}
```

---

## Why it works

Git remembers where HEAD has been.

Think of reflog as Git's **Time Machine**.

---

# 8. Restore One File

## Scenario

Only one file is broken.

```
style.css
```

Everything else is correct.

---

## Solution

```bash
git log

git checkout <hash> -- style.css

git commit -m "Restore style.css"
```

---

## Why it works

Only that file is restored.

Everything else stays unchanged.

---


---

## Warning

This permanently removes

- Local commits
- Local changes
- Untracked files

---

# Quick Cheat Sheet

| Mistake | Command |
|----------|---------|
| Forgot a file | `git commit --amend --no-edit` |
| Wrong message | `git commit --amend` |
| Wrong branch | `git reset --soft` + `git stash` |
| Should be feature branch | `git branch` + `git reset --hard` |
| git diff empty | `git diff --staged` |
| Undo old commit | `git revert` |
| Recover deleted commits | `git reflog` |
| Restore one file | `git checkout HASH -- file` |
| Reset everything | `git reset --hard origin/main` |

---

# Demo Flow

For every topic follow this order:

1. 🎯 Explain the scenario
2. ❌ Make the mistake live
3. 🤔 Ask the audience what happened
4. 📋 Show `git status`
5. 📜 Show `git log --oneline --graph`
6. ✅ Apply the fix
7. ✔ Verify that it worked
8. 💡 Explain why the command works

```

---

## One important change I'd make

I'd move **`git reflog` from the beginning to near the end** (as shown above).

Why? Because `reflog` is most impressive when you've already shown commands like `reset`, `amend`, and `revert`. Then you can intentionally "lose" commits with:

```bash
git reset --hard HEAD~2
```

Everyone thinks the work is gone, and then you recover it with:

```bash
git reflog
git reset --hard HEAD@{1}
```

That creates a memorable "wow" moment and clearly demonstrates why `git reflog` is often called Git's **time machine**. It's a much stronger finale than opening the presentation with it.

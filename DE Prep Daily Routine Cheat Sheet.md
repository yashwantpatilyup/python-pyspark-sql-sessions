# DE Prep Daily Routine Cheat Sheet

Oct 27, 2026 · @Yashhhhhhhhh

## 1. Start of day

Open PowerShell **Window 1**, activate `myenv`, confirm you are on `my-work`, then start JupyterLab.

```powershell
cd C:\Users\iamya\de-prep
myenv\Scripts\activate
cd python-pyspark-sql-sessions
git branch --show-current
git status --short
jupyter lab
```

- Prompt must start with `(myenv)`; branch must say `my-work` (else `git switch my-work`).
- `git status --short` should print nothing if yesterday was saved.
- SQL days only: `Get-Service postgresql*` should say `Running`; if `Stopped`, run `net start postgresql-x64-16` in an Administrator PowerShell.
- Leave Window 1 alone while Jupyter runs; use a second PowerShell for Git and Postgres.

## 2. During study

Read `notes.md`, run `notebook.ipynb`, then attempt `practice_questions.ipynb` yourself before looking at any solution.

1. Open the day's `notes.md` and read it first.
2. Open `notebook.ipynb`, pick kernel **Python (myenv)** (top right), run cells with Shift+Enter.
3. Try `practice_questions.ipynb` on your own; save often with Ctrl+S.

SQL days 2 to 5: load that day's tables first, in a second PowerShell (no `(myenv)` needed). Each file re-creates shared tables, so reload when you revisit an earlier day.

```powershell
cd C:\Users\iamya\de-prep\python-pyspark-sql-sessions
$env:PGCLIENTENCODING = "UTF8"
psql -U postgres -d postgres -f sql\day02_string_functions\setup_tables.sql
```

Swap the folder name for `day03_date_time_functions`, `day04_aggregation` or `day05_joins`. Day 1's file is already loaded.

## 3. End of day

Save notebooks, stop Jupyter with Ctrl+C then `y`, and push your work to your fork.

```powershell
git branch --show-current
git status --short
git add -A
git commit -m "Day 2 SQL: notes read, notebook run, practice solved"
git push
```

- The branch must say `my-work`.
- Write a commit message that says what you actually did; it becomes your learning diary.
- Optional: `deactivate` leaves the virtual environment.

## 4. When your instructor adds new days

Check for news first; no output from the second command means nothing is new.

```powershell
git fetch upstream
git log my-work..upstream/main --oneline
```

If there are new commits, bring them in. `git status --short` must be empty first, so commit your work before this.

```powershell
git switch main
git merge upstream/main
git push origin main
git switch my-work
git merge main
git push
```

On a conflict message, stop and ask before doing anything else. Never use `git push --force`.

## 5. Quick reference

| Command | What it does |
| --- | --- |
| `git status --short` | Which files changed |
| `git diff --stat` | Lines changed per file |
| `git log --oneline -10` | Your last 10 commits |
| `git branch --show-current` | Which branch you are on |
| `git switch my-work` | Move to your branch |
| `git restore path\to\file` | Discard unsaved changes to one file (cannot be undone) |

| Problem | Fix |
| --- | --- |
| No `(myenv)` in prompt | `myenv\Scripts\activate` from `C:\Users\iamya\de-prep` |
| Notebook cannot import packages | Set the kernel to **Python (myenv)** |
| SQL: connection refused | `net start postgresql-x64-16` as Administrator |
| SQL: password authentication failed | Notebook password must be `postgres` |
| PySpark: port 4040 in use | Kernel, then Restart |
| `git push` rejected | Read the message; never use `--force` |

**Rules:** work on `my-work`; never commit real passwords or personal files (your fork is public); commit and push once per session; never click **Compare & pull request** on GitHub.

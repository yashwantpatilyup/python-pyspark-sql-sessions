# Daily VS Code Routine: DE Prep and Practice

Oct 27, 2026 · @Yashhhhhhhhh

## 1. Your setup at a glance

Two repos, two VS Code windows, one terminal type: PowerShell inside VS Code (`` Ctrl+` ``).

| Item | Value |
| --- | --- |
| Course repo (instructor's material) | `C:\Users\iamya\de-prep\python-pyspark-sql-sessions` |
| Course branch | `my-work` (never work on `main`) |
| Course remotes | `origin` = your fork `yashwantpatilyup/python-pyspark-sql-sessions`; `upstream` = instructor's repo |
| Course Python environment | `C:\Users\iamya\de-prep\myenv`, Python 3.11.9, kernel name **Python (myenv)** |
| Practice repo | `C:\Users\iamya\code\interview-bootcamp` (GitHub: `yashwantpatilyup/python-sql-git-interview-prep`) |
| Database | PostgreSQL 16, user `postgres`, password `postgres`, host `localhost`, port `5432`; pgAdmin to browse |
| Other tools | Java 11, PySpark 3.5.6, Git |

Open each repo in its own VS Code window (File, New Window, then File, Open Folder), so Source Control shows only that repo.

## 2. Daily routine: the DE course

Start by opening the repo and confirming you are on `my-work`, study the day's material, then commit and push before you stop.

**Start of day**

1. Open VS Code, then File, Open Folder, and choose `C:\Users\iamya\de-prep\python-pyspark-sql-sessions`.
2. Check the bottom-left of the window says `my-work`. Click it to switch if it does not.
3. Open the terminal (`` Ctrl+` ``) and run:

```powershell
git branch --show-current
git status --short
```

The branch must say `my-work`. `git status --short` normally prints nothing; if it lists files, they are unsaved work from last time, so commit them first.

**Study**

1. Open the day's `notes.md` and read it first.
2. Open `notebook.ipynb`. Click **Select Kernel** (top right), choose **Python Environments**, then `myenv`. Run cells with Shift+Enter.
3. Open `practice_questions.ipynb` and try each question yourself before looking at any solution.
4. Save with Ctrl+S as you go.

**SQL days 2 to 5: load that day's tables first**

Each day has its own `setup_tables.sql`, and later days re-create shared tables such as `orders` and `customers`, so load only the day you are on. Day 1 is already loaded.

```powershell
Get-Service postgresql*
$env:PGCLIENTENCODING = "UTF8"
psql -U postgres -d postgres -f sql\day02_string_functions\setup_tables.sql
```

- `Get-Service` should say `Running`. If it says `Stopped`, open PowerShell as Administrator and run `net start postgresql-x64-16`.
- Swap `day02_string_functions` for `day03_date_time_functions`, `day04_aggregation` or `day05_joins`.
- The password prompt wants `postgres`; typing shows no characters, which is normal.
- Going back to an earlier day means loading that day's file again.

**End of day**

```powershell
git branch --show-current
git status --short
git add -A
git commit -m "Day 2 SQL: notes read, notebook run, practice solved"
git push
```

Or use Source Control (`Ctrl+Shift+G`): type the message, click **Commit**, then **Sync Changes**. Write a message that says what you actually did, because the history becomes your learning diary.

## 3. Daily routine: practice repo

Use a second VS Code window so this repo's changes never mix with the course's.

1. In VS Code choose File, New Window, then File, Open Folder, and pick `C:\Users\iamya\code\interview-bootcamp`.
2. Solve one problem per file, with the platform and problem in the name, for example `leetcode/0001_two_sum.py`.
3. After each problem, commit it separately:

```powershell
git status --short
git add -A
git commit -m "LeetCode 1: Two Sum"
git push
```

Committing after each problem gives every solution its own snapshot. If the repo contains a virtual environment that stopped working after the folder move, delete that environment folder and recreate it; no code is lost, since it only holds installed packages.

## 4. Weekly: new days from your instructor

Check once a week; no output from the second command means nothing is new. Run this in the course repo.

```powershell
git fetch upstream
git log my-work..upstream/main --oneline
```

If commits are listed, commit your own work first (`git status --short` must print nothing), then bring them in:

```powershell
git switch main
git merge upstream/main
git push origin main
git switch my-work
git merge main
git push
```

On GitHub, the **Sync fork** button on your fork's page is an alternative for the `main` part. If Git reports a conflict, stop and ask for help before doing anything else. Never use `git push --force`.

## 5. If something does not work

Work down the table; the first fix usually solves it.

| Problem | Likely cause | Fix |
| --- | --- | --- |
| No `(myenv)` in the terminal prompt | Environment not active | Run `C:\Users\iamya\de-prep\myenv\Scripts\activate`. If PowerShell blocks scripts, run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` once |
| Notebook cannot import a package | Wrong kernel selected | Top right, **Select Kernel**, then Python Environments, then `myenv` |
| VS Code asks to install `ipykernel` | Wrong interpreter selected | Do not install; pick the `myenv` interpreter (`Ctrl+Shift+P`, then Python: Select Interpreter) |
| SQL: connection refused | PostgreSQL service stopped | Administrator PowerShell: `net start postgresql-x64-16` |
| SQL: password authentication failed | Notebook has the instructor's password | Set `PASSWORD = 'postgres'` in the connection cell |
| SQL: relation does not exist | That day's tables not loaded | Run that day's `setup_tables.sql` (section 2) |
| PySpark will not start | Java or paths problem | Check `java -version` prints Java 11; restart VS Code so it sees `JAVA_HOME` |
| PySpark: port 4040 in use | An old Spark session is running | Restart the notebook kernel |
| Source Control shows thousands of files | A second repo (for example your home folder) is detected | Close that repository in the Repositories list; set `git.openRepositoryInParentFolders` to `never` |
| Wrong branch shown (not `my-work`) | You switched by accident | `git switch my-work` (commit or discard changes first) |
| `git push` rejected | Remote has commits you lack | Read the message and ask for help; never use `--force` |
| Git asks who you are | Identity not set | `git config --global user.name "Yashwant"` and `git config --global user.email "yashwantpatil.yup@gmail.com"` |
| Commit window says nothing to commit | Everything already saved | Nothing to do |

Still stuck: copy the full error text, note which repo and branch you were on, and bring both when you ask for help.

## 6. Safety rules and quick commands

**Before every push**

- Check the repo name and branch in Source Control; the course must say `my-work`.
- Skim the Changes list: only your own study or solution files belong there.
- Never commit passwords, API keys, session cookies or personal files; your fork of the course is public.
- Never click **Compare & pull request** on GitHub; it proposes your changes to your instructor's repo.
- Run `git init` only after you have changed into the project folder, never in `C:\Users\iamya`.

**Quick command list**

| Command | What it does |
| --- | --- |
| `git status --short` | Lists files that changed |
| `git diff --stat` | Shows lines changed per file |
| `git log --oneline -10` | Shows your last 10 commits |
| `git branch --show-current` | Shows the current branch |
| `git switch my-work` | Moves to your branch |
| `git restore path\to\file` | Discards unsaved changes to one file (cannot be undone) |
| `git remote -v` | Shows where the repo pushes and pulls |

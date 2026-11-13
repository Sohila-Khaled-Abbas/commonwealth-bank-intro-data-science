# Local setup — Windows 11 and Microsoft Excel

This guide uses the requested parent directory:

~~~text
D:\courses\Data Analysis 26-27\
└── commonwealth-bank-intro-data-science\
~~~

## 1. Clone the repository

Open PowerShell and run:

~~~powershell
$Parent = "D:\courses\Data Analysis 26-27"
$RepoPath = Join-Path $Parent "commonwealth-bank-intro-data-science"

New-Item -ItemType Directory -Force -Path $Parent | Out-Null

if (Test-Path $RepoPath) {
    Write-Host "The destination already exists. Inspect it before cloning:" -ForegroundColor Yellow
    Get-ChildItem -Force $RepoPath
} else {
    git clone https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science.git $RepoPath
}

Set-Location $RepoPath
git status
~~~

If the folder does not exist, Git clones the repository into it. If it already exists, do not delete or overwrite it blindly. If it is already a clone of this repository, use `git pull --ff-only` to update it.

## 2. Confirm the repository connection

~~~powershell
git remote -v
git branch --show-current
git status
~~~

Expected remote URL:

~~~text
https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science.git
~~~

The current branch should be `main` and the working tree should be clean immediately after cloning.

## 3. Download and store the private Forage workbook

1. Open the [official Commonwealth Bank simulation](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t).
2. Open the Resources section for Task 01 and download `supermarket_transactions.xlsx`.
3. Save the original file to `data\raw\supermarket_transactions.xlsx`.
4. Keep the original unchanged. Make a working copy at `data\working\supermarket_transactions_working.xlsx`.
5. Open the working copy in Microsoft Excel Desktop.

> [!CAUTION]
> The Forage task labels the file for simulation use only. Do not commit the original data, working copy, or completed workbook containing the dataset. The repository `.gitignore` excludes `data/raw/`, `data/working/`, and `outputs/`.

## 4. Start Task 01 in Excel

1. Open `workbooks\Task_01_Excel_Practice_Starter.xlsx` alongside the transaction workbook.
2. Inspect the headers, sample rows, and data types. Confirm what one row represents and whether a quantity field exists.
3. Select the transaction range and press `Ctrl + T`; enable **My table has headers**.
4. In **Table Design → Table Name**, set the name to `Transactions`.
5. Add an `Analysis` sheet to the working copy.
6. Use the formula patterns in the starter workbook, adjusting structured references to match the exact source headers.
7. Build the PivotTables described in the `PIVOT_GUIDE` sheet and reconcile the results.
8. Save the private deliverable as `outputs\task-01\Task_01_Analysis_Completed.xlsx`.

## 5. Commit only safe project materials

After editing documentation or other permitted project files:

~~~powershell
git status
git add README.md LEARNING_LOG.md docs workbooks
git commit -m "docs: update project guide and Excel learning materials"
git push
~~~

Review `git status` before every commit. Do not use `git add -f` on ignored source data or private workbooks.

## 6. Update the local clone later

~~~powershell
Set-Location "D:\courses\Data Analysis 26-27\commonwealth-bank-intro-data-science"
git pull --ff-only
~~~

If Git prompts for authentication, complete the browser sign-in flow. Never paste access tokens into source files, scripts, or chat.

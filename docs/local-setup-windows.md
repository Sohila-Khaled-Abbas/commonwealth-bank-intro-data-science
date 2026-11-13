# Local setup — Windows 11 and Microsoft Excel

This guide uses the project folder:

~~~text
D:\courses\Data Analysis 26-27\commonwealth-bank-intro-data-science\
~~~

## 1. Clone or update the repository

Open PowerShell:

~~~powershell
$Parent = "D:\courses\Data Analysis 26-27"
$RepoPath = Join-Path $Parent "commonwealth-bank-intro-data-science"

New-Item -ItemType Directory -Force -Path $Parent | Out-Null

if (Test-Path $RepoPath) {
    Write-Host "Destination exists. Inspect it before changing anything." -ForegroundColor Yellow
    Get-ChildItem -Force $RepoPath
} else {
    git clone https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science.git $RepoPath
}

Set-Location $RepoPath
git status
~~~

If this is already a clone, update it safely with:

~~~powershell
git pull --ff-only
~~~

## 2. Confirm the repository connection

~~~powershell
git remote -v
git branch --show-current
git status
~~~

Expected remote:

~~~text
https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science.git
~~~

## 3. Private local data folders

The repository keeps only project documentation and permitted learning materials. Use these ignored folders for simulation files:

~~~text
data/
├── raw/       # Original Forage downloads — local only
└── working/   # Copies, Excel workbooks, mapping tables and deliverables — local only
~~~

The root .gitignore excludes files in both folders. Do not force-add them.

## 4. Task 01 — supermarket transaction analysis

1. Download supermarket_transactions.xlsx from the Task 01 Resources section in [Forage](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t).
2. Save the original to data/raw/supermarket_transactions.xlsx.
3. Make a working copy at data/working/supermarket_transactions_working.xlsx.
4. Complete the analysis in the working copy and save the private workbook as data/working/Task_01_Analysis_Completed.xlsx.
5. Keep the raw source file unchanged and submit the appropriate deliverable through Forage.

See [Task 01 — Data Aggregation and Analysis](task-01-data-aggregation-analysis.md) for the completed method and validated results.

## 5. Task 02 — customer-data anonymisation

The task page describes the resource as mobile_customers.csv, but the download supplied by Forage is an Excel workbook named mobile_customers.xlsx. Do not rename the source file to CSV.

1. Download the Task 02 workbook from the [Forage simulation](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t).
2. Save the original to data/raw/mobile_customers.xlsx.
3. Make a working copy at data/working/mobile_customers_working.xlsx.
4. Create the column inventory, transformation plan and QA checks in the private working copy.
5. Save the final release as data/working/mobile_customers_anonymised.csv.
6. Reopen the CSV to check the output, then upload it through the Forage Task 02 submission area.

**Important:** the background mentions linking customer data to supermarket transactions. Confirm whether record-level linkage is required before changing any customer key. If it is required, the same approved token mapping must be applied to both datasets and kept separately by the data custodian. A stable token is pseudonymisation, not proof of irreversible anonymisation.

See [Task 02 — Data Anonymisation](task-02-data-anonymisation.md) and [Data anonymisation and privacy techniques](data-privacy-techniques.md).

## 6. Confirm private files are ignored

~~~powershell
git status --short
git status --ignored
~~~

Files under data/raw/ and data/working/ should be ignored. The public repository should contain instructions and non-sensitive documentation, not source customer records, completed customer-level CSVs, or ID mapping tables.

## 7. Commit documentation only

~~~powershell
git status
git add README.md LEARNING_LOG.md docs workbooks .gitignore
git diff --cached --stat
git diff --cached
git commit -m "docs: expand data science simulation guides"
git push
~~~

Review the staged diff before committing. Do not use git add -f for ignored data, source workbooks, customer-level exports, or mapping tables.

## 8. Update the local clone later

~~~powershell
Set-Location "D:\courses\Data Analysis 26-27\commonwealth-bank-intro-data-science"
git pull --ff-only
~~~

After pulling the documentation changes, you can remove old, empty local outputs and evidence folders if they still exist. Do not remove data/raw or data/working; those are your private local work areas.

Never paste GitHub access tokens or other credentials into source files or chat.

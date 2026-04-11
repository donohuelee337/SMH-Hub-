# Publish SMH-collab-hub to GitHub

Local git is already initialized with `main` and an initial commit. Create the **remote** repository and push:

## Option A — GitHub website

1. Open [https://github.com/new](https://github.com/new).
2. Repository name: **`SMH-collab-hub`** (owner: your account, e.g. `donohuelee337`).
3. Choose **Private** (recommended) or Public.
4. Do **not** add a README, .gitignore, or license (this folder already has content).
5. Create repository, then run in PowerShell (adjust URL if your username differs):

```powershell
cd C:\Users\Garage\Documents\Cursor\SMH-collab-hub
git remote add origin https://github.com/donohuelee337/SMH-collab-hub.git
git push -u origin main
```

## Option B — GitHub CLI (if installed)

```powershell
cd C:\Users\Garage\Documents\Cursor\SMH-collab-hub
gh auth login
gh repo create SMH-collab-hub --private --source=. --remote=origin --push
```

After the first successful push, add this folder to Cursor (**File → Add Folder to Workspace**) if you want it next to **SMH-app**.

# Terminal Quick Reference (8/28/2026)

CMD, PowerShell, Bash — plus paths, redirection, and orientation.

---

## 1. The Three Shells, at a Glance

| | CMD (Command Prompt) | PowerShell | Linux / Bash (Git Bash, WSL) |
|---|---|---|---|
| **Age** | Old Windows shell | Newer Windows shell | Unix standard |
| **Flag style** | Forward slashes — `/S /F /A` | Dashes — `-Recurse -Force` | Dashes — `-r -f -a` |
| **Path style** | Backslashes | Accepts **both** `C:\Users` and `C:/Users` | Forward slashes |
| **Typical commands** | `dir`, `cd`, `copy`, `del`, `type`, `tree` | `Get-ChildItem`, `Set-Location`, `Copy-Item`, `Remove-Item` | `ls`, `cd`, `cp`, `rm`, `cat`, `find`, `grep` |

PowerShell carries aliases that mimic both CMD and Linux — `dir`, `ls`, and `cd` all work. It also runs most CMD commands, but **flag handling differs**, which is where things break.

## 1.1. A Downside, but also an Upside to PowerShell

**Note:** Command Prompt and PowerShell will both open in Windows Terminal. In fact, so will Windows PowerShell. They are different programs. Windows PowerShell (blue icon) is an old version. It has been superseded by PowerShell 7 (x64) (black icon, just referred to in Windows as PowerShell). You want to use this one as it has more functionality. The first time you use it, you need to go into the settings dropdown on Terminal. In the Default Profile dropdown, select the PowerShell with the black icon. This will ensure that you always have the better version load.

How to tell which one you are in: The icon color only helps at launch; if you're already in a tab you need a check: `$PSVersionTable.PSVersion`
- Major version 5.1 = Windows PowerShell (the old one)
- Major version 7.x = PowerShell 7.
- The executables differ too: old is powershell.exe, new is pwsh.exe — useful when writing scripts or shortcuts that must hit the right one.

---

## 2. Listing Files and Folders

| Goal | CMD | PowerShell | Linux |
|---|---|---|---|
| Tree, folders only | `tree` | `Get-ChildItem -Recurse -Directory` | — |
| Tree, folders + files | `tree /F /A` | `Get-ChildItem -Recurse` | — |
| Flat recursive listing | `dir /S` | `Get-ChildItem -Recurse \| Select-Object FullName` | `ls -R` or `find .` |
| Just this folder | `dir` | `ls` | `ls` |

Flag meanings: `/F` = show **F**iles · `/A` = **A**SCII characters · `/S` = include **S**ubdirectories

---

## 3. Flag Gotchas

| Command | Valid flags | Trap |
|---|---|---|
| `tree` | `/F` `/A` | Case and order don't matter — `/F /A` == `/a /f` |
| `dir` | `/S` `/A` `/B` `/O` | **No `/F`** — that's a `tree` flag |

**When PowerShell rejects a CMD command** (symptom: *"Too many parameters"* or other weirdness), pick one:

1. Tunnel through CMD — `cmd /c "tree /F /A"`
2. Call the `.com` binary directly — `tree.com /F /A`
3. Use the PowerShell native equivalent — `Get-ChildItem -Recurse`

---

## 4. Redirecting Output to a File

Same syntax in all three shells:

| Operator | Effect |
|---|---|
| `> file.txt` | Overwrite, or create new |
| `>> file.txt` | Append to existing |

```powershell
tree /F /A > structure.txt

dir "C:\Unity Projects\RHWM Emergency Response v0.5" /s > "C:\Unity Projects\RHWM Emergency Response v0.5\structure.txt"
```

---

## 5. Auto-Suggestions in Terminal

### In the Current Session Only

1. If you want to keep the feature but want to clear the specific command history it uses to make suggestions for your current session, run: `Clear-History`
    *(Note: This only clears the current session's active command history history buffer).*
2. To disable the inline predictions, run this command in your PowerShell window: `Set-PSReadLineOption -PredictionSource None
3.  To turn the auto-suggestions back on, run this command: `Set-PSReadLineOption -PredictionSource History`
4. You can also pull predictions from both your history and installed plugin modules (like Azure or Git predictors) by running: `Set-PSReadLineOption -PredictionSource History`

### To Permanently Disable, Enable, or Tweak

To make sure suggestions stay turned off every time you open Windows Terminal, you should add the configuration to your PowerShell profile.
1. Open your profile script by running: `notepad $PROFILE`
    *(Note: If prompted to create a new file, click Yes.)*
2. Paste one of the following options into the document:
    a. To turn them completely off: `Set-PSReadLineOption -PredictionSource None`
    b. To use history but switch from inline text to a list view (press F2 to toggle): `Set-PSReadLineOption -PredictionViewStyle ListView`

---

## 5. Paths & Orientation

### Path conventions

| Shell | Example |
|---|---|
| Windows | `C:\Users\pledger2\Documents` |
| Linux | `/home/pledger2/documents` |
| PowerShell | either — `C:\Users` or `C:/Users` |

**Always quote paths with spaces:**

```powershell
cd "C:\Unity Projects\RHWM Emergency Response v0.5"
```

### "Where am I?"

| Goal | CMD | PowerShell | Linux |
|---|---|---|---|
| Show current folder | `cd` (no arguments) | `pwd` or `Get-Location` | `pwd` |
| Change folder | `cd "path\to\folder"` | same | same |

### Skip the `cd` entirely

In Windows File Explorer: right-click empty space inside the folder --> **Open in Terminal**. PowerShell opens already in that folder. Hold Ctrl and Shift while clicking **Open in Terminal** to open PowerShell in administrator mode.

---

## 6. Things I Learned the Hard Way

- `tree` flag order and case don't matter (`/F /A` == `/a /f`).
- `dir` has no `/F` flag — that belongs to `tree`.
- *"Too many parameters - -f"* usually means mixed-up commands, or being in PowerShell when the syntax was CMD.
- PowerShell opens in `C:\Users\<me>` by default. `cd` to the project folder **before** running Git commands.
- `cmd1 && cmd2` chaining requires PowerShell 7. If it throws a syntax error, you're in Windows PowerShell 5.1 — check with `$PSVersionTable.PSVersion`. See §1.1.
- Quote paths with spaces. Always.

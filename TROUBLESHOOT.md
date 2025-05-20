# Troubleshooting Guide for `fzf` and `zoxide` on Windows PowerShell

This guide addresses common issues encountered during the installation and configuration of [`fzf`](https://github.com/junegunn/fzf) (fuzzy finder) and [`zoxide`](https://github.com/ajeetdsouza/zoxide) (smart directory jumper) on **Windows PowerShell (5.x)**. Each section includes symptoms, root causes, and actionable solutions.

---

## 1. PowerShell Profile Not Found or Not Loading
**Symptom:**
Running `$PROFILE` returns a path, but the file doesn't exist or changes don’t take effect.

**Cause:**
PowerShell does not create the profile file by default. Edits won’t apply unless the correct profile is sourced.

**Solution:**
```powershell
if (!(Test-Path -Path $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force
}
notepad $PROFILE
. $PROFILE  # Reload the profile after editing
```

---

## 2. Error Overriding `cd` Alias

**Symptom:**
```
Set-Alias : The AllScope option cannot be removed from the alias 'cd'.
```
**Cause:**
`cd` is a built-in alias with the `AllScope` flag and cannot be modified using `Set-Alias`.
**Solution:**
Override `cd` with a function instead:
```powershell
function cd { z @args }
```
---
## 3. `fzf` or `zoxide` Not Recognized After Installation

**Symptom:**
Running `fzf` or `z` returns:
```
The term 'fzf' is not recognized...
```
**Cause:**
* Scoop’s `bin` directory may not be in `PATH`
* Your profile doesn’t contain the init commands
**Solution:**
* Ensure this in your profile:
```powershell
Invoke-Expression (& { (zoxide init powershell | Out-String) })
Set-Alias fzf (Get-Command fzf.exe).Source
```
* Then run:
```powershell
. $PROFILE
```
---
## 4. Execution Policy Blocks Script Loading
**Symptom:**
```
File cannot be loaded because the execution of scripts is disabled on this system.
```
**Cause:**
PowerShell’s default security policy restricts script execution.
**Solution:**
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```
---
## 5. `fzf` Interface Fails or Doesn’t Render

**Symptom:**
* `fzf` flashes and exits
* No interactive interface
**Cause:**
* Not running in an interactive terminal
* Terminal doesn’t support ANSI escape codes
**Solution:**
* Use [Windows Terminal](https://aka.ms/terminal) or PowerShell (not CMD)
* Test with:
```powershell
Get-ChildItem | fzf
```
---
## 6. `zoxide` Doesn’t Track or Jump to Directories
**Symptom:**
The `z` or `zi` command behaves like `cd` and doesn’t track frequent directories.
**Cause:**
Initialization not loaded or incorrect usage
**Solution:**
Ensure the following is in your profile:
```powershell
Invoke-Expression (& { (zoxide init powershell | Out-String) })
```
Reload with:
```powershell
. $PROFILE
```
Optionally:
```powershell
zoxide add C:\Your\Favorite\Dir
```
---
## 7. Scoop Not Found
**Symptom:**
Running `scoop` returns:
```
The term 'scoop' is not recognized...
```
**Cause:**
Scoop not installed or PATH not updated
**Solution:**
Reinstall Scoop:
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
irm get.scoop.sh | iex
```
---
## 8. Profile Changes Don’t Persist
**Symptom:**
Custom functions or aliases reset on PowerShell restart
**Cause:**
Changes were made to the wrong `$PROFILE` or not saved
**Solution:**
* Verify `$PROFILE`:
```powershell
echo $PROFILE
```
* Edit and save it properly:
```powershell
notepad $PROFILE
```
---
## 9. Zoxide Conflicts With PowerShell's Native `cd` Command
**Symptom:**
Inconsistent behavior between `cd` and `z` usage
**Cause:**
Mixing native `cd` with `zoxide` usage without proper redirection
**Solution:**
Always use the custom `cd` function defined as:
```powershell
function cd { z @args }
```
---
## Still Stuck?
* Visit the official documentation:
  * [zoxide](https://github.com/ajeetdsouza/zoxide)
  * [fzf](https://github.com/junegunn/fzf)
* Confirm your PowerShell version with:
```powershell
$PSVersionTable
```
* Try a fresh terminal (e.g., Windows Terminal)
---
Document crafted with precision and professionalism by Sujith — optimizing your command-line workflow.

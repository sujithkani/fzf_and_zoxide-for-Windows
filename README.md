# Windows PowerShell Setup for `fzf` and `zoxide`

This guide walks you through installing and configuring `fzf` (fuzzy finder) and `zoxide` (smart directory jumper) on Windows using PowerShell 5.x (Windows PowerShell). It includes detailed steps, error free setup.

---

## Prerequisites

- Windows OS with **PowerShell 5.x** (Windows PowerShell)  
- [Scoop](https://scoop.sh/) package manager installed

> To install Scoop, run in PowerShell (as current user):

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
irm get.scoop.sh | iex
```
---

## Step 1: Install fzf and zoxide

With Scoop installed, to fetch and install latest versions of `fzf` and `zoxide`, run:

```powershell
scoop install fzf
scoop install zoxide
```
---

## Powershell profile

A PowerShell profile is a script that executes every time you start PowerShell. It enables you to customize your environment by setting aliases, functions, environment variables, and initializing tools automatically.

---

## Step 3: Locate Your PowerShell Profile Path and edit

Open PowerShell and run:

```powershell
echo $PROFILE
notepad $PROFILE
```

Notepad will not open if the profile does not exist. If happens, make a new profile.

Check for existing profile and create a new one if needed.
```powershell
if (!(Test-Path -Path $PROFILE)){
    New-Item -ItemType File -Path $PROFILE -Force
}

notepad $PROFILE
```

## Step 4: Initializing and aliasing

In the opened PROFILE of your powershell, enter:

```notepad
Invoke-Expression (&{(zoxide init powershell | Out-String)})
function cd {z @args}
Set-Alias j z
```
Save and close this file. Apply the change in the Shell:

```powershell
. $PROFILE
```

## Why use these?

```fzf``` (Fuzzy Finder) is a powerful command-line utility that enables interactive, fuzzy searching through lists of data such as files, command history, processes, and more. It dramatically improves productivity by allowing you to quickly filter and select items by typing approximate or partial patterns rather than exact matches.

**Use case: Quickly find and open files or commands without needing to remember full names or paths.
Key feature: Interactive, incremental search with immediate feedback.**



```zoxide``` is a smarter, faster alternative to the traditional cd command. It tracks your most frequently and recently visited directories and lets you jump to them instantly by typing partial directory names.

**Use case: Navigate deep or complex directory trees effortlessly without typing long paths.
Key feature: Uses a ranking algorithm to prioritize frequently used directories.**


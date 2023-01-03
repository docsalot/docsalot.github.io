---
title: Experimenting with ChatGPT
authors:
  - docsalot
draft: true
date: 2022-12-31T20:43:09.362Z
categories:
  - ChatGPT
  - SSGs
description: Lowering the 'technical bar to entry' with ChatGPT. 
---


[Open.ai](https://openai.com/) is getting a lot of air time right now—and for good reason. [Dall-E](https://openai.com/dall-e-2/) and [ChatGPT](https://openai.com/blog/chatgpt/) are practical applications of machine learning from which value is (presumably) self-evident—and you don't need any technical chops to get started. 

I'm trying to better understand what the impact of tools like these will be on the future of technical writing, and on my day-to-day. 

<!-- more -->

## Lowering the technical bar to entry

Before I write this, I want to clearly state that I'm not an automation developer. I really just have a sliver of knowledge gleaned from the experts[^1] I've been lucky enough to work with, and have a basic understanding of what's possible with PowerShell. 

[^1]:
    [Kashif Choudhry](https://www.linkedin.com/in/kashif-choudhry) is a rockstar. Working closely with him helped me understand the impact automation developers can have, and helped me understand PowerShell as a whole.  

One of the side projects I've been working on is proving out a docs like code toolchain. 


What if, instead of walking people through a complicated setup procedure, I could build a PowerShell script to automate the installation of the toolchain? This would include:

* Python and `pip`
* MkDocs and the Material theme
* Other project-specific dependencies

<div class="grid cards" markdown>

:material-language-python: [__Python + `pip`__](#python-and-pip)
{ .card }

:material-engine: [__MkDocs + Material__](#) 
{ .card }

:material-package-down: [__Packages__](#packages)
{ .card }

:material-git: Wait, add [__Git__](#wait-add-git) 
{ .card }

</div>


Here's what I was able to produce with ChatGPT. 

### Python and `pip`

*Write me a powershell script that performs the following tasks:*

*1) Checks if Python is installed. If not, installs Python.* 
*2) Checks if pip is installed. If not, installs pip, and adds pip to the PATH environment variable.* 


```PowerShell
# Check if Python is installed
$pythonInstalled = Get-Command python.exe -ErrorAction SilentlyContinue

# If Python is not installed, install it
if (-not $pythonInstalled) {
    Write-Output "Python is not installed. Installing Python..."

    # Download the Python installer
    $installerUrl = "https://www.python.org/ftp/python/3.9.4/python-3.9.4-amd64.exe"
    $installerPath = "$env:TEMP\python-installer.exe"
    Invoke-WebRequest -Uri $installerUrl -OutFile $installerPath

    # Install Python
    Start-Process -FilePath $installerPath -ArgumentList "/quiet InstallAllUsers=1 PrependPath=1" -Wait

    # Remove the installer
    Remove-Item -Path $installerPath -Force
}
else {
    Write-Output "Python is already installed"
}

# Check if pip is installed
$pipInstalled = Get-Command pip.exe -ErrorAction SilentlyContinue

# If pip is not installed, install it and add it to the PATH environment variable
if (-not $pipInstalled) {
    Write-Output "pip is not installed. Installing pip..."

    # Install pip
    python.exe -m ensurepip --upgrade --default-pip

    # Add pip to the PATH environment variable
    $env:Path = "$env:Path;$env:ProgramFiles\Python\Python38\Scripts"
    [Environment]::SetEnvironmentVariable("Path", $env:Path, "Machine")
}
else {
    Write-Output "pip is already installed"
}

```


### Packages

*Have the script check if the mkdocs-glightbox package is installed. If not, install it with pip.* 

```PowerShell
# Check if the mkdocs-glightbox package is installed
$packageInstalled = python.exe -m pip show mkdocs-glightbox -q

# If the package is not installed, install it with pip
if (-not $packageInstalled) {
    Write-Output "The mkdocs-glightbox package is not installed. Installing it with pip..."
    python.exe -m pip install mkdocs-glightbox
}
else {
    Write-Output "The mkdocs-glightbox package is already installed"
}

```

### Installation summary

*Add a final step to the script that shows the user what was installed.*

```PowerShell
# Print a summary of what was installed
Write-Output "Installation summary:"
if (-not $pythonInstalled) { Write-Output " - Python was installed" }
if (-not $pipInstalled) { Write-Output " - pip was installed" }
if (-not $packageInstalled) { Write-Output " - mkdocs-glightbox package was installed" }
```

### Wait, add Git

*I forgot to add a step to check if Git is installed. If not, install git.*

```PowerShell
# Check if Git is installed
$gitInstalled = Get-Command git.exe -ErrorAction SilentlyContinue

# If Git is not installed, install it
if (-not $gitInstalled) {
    Write-Output "Git is not installed. Installing Git..."

    # Download the Git installer
    $installerUrl = "https://github.com/git-for-windows/git/releases/download/v2.30.2.windows.2/Git-2.30.2.2-64-bit.exe"
    $installerPath = "$env:TEMP\git-installer.exe"
    Invoke-WebRequest -Uri $installerUrl -OutFile $installerPath

    # Install Git
    Start-Process -FilePath $installerPath -ArgumentList "/SILENT" -Wait

    # Remove the installer
    Remove-Item -Path $installerPath -Force
}
else {
    Write-Output "Git is already installed"
}
```




## Work faster, reliably


## 

I've been experimenting with ChatGPT, and am encouraging my teammates to do so as well.


in an effort to evaluate how it can help me be a more effective technical writer, and  like a student in a math class uses a calculator, in an attempt to solve technical problems without development support. 

The problems I'm working on: 

- How to make setting up docs like code tooling simple.
- How to set up a release pipeline for MkDocs and Material sites in Azure DevOps. 
- How to speed up documentation production and accuracy.  





Right now, I'm totally enthralled with *docs like code*. 


First, I want to mention that ChatGPT didn't write this article. Perhaps this exemplifies the subconcious dread I feel after playing with ChatGPT; the tool that documents obscure PowerShell scripts reliably in 15 seconds. 

There's a lot of speculation about what artificial intellegence could mean for the technology industry. 
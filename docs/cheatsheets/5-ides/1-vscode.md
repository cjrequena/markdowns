---
layout: default
title: vscode
parent: ides
grand_parent: cheatsheets
permalink: /docs/ides/vscode
nav_order: 1
---

# Visual Studio Code Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## **Install Visual Studio Code**
Download and install **VS Code** from the official website:  
👉 [https://code.visualstudio.com/](https://code.visualstudio.com/)

---

## **Install Visual Studio Code on macOS using Homebrew**

### **Ensure Homebrew is Installed**
Open **Terminal** (`Cmd + Space` → Type **Terminal** → Press **Enter**) and run:
```sh
brew --version
```
If Homebrew is not installed, install it with:
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Then, restart your terminal.

### **Install Visual Studio Code**
Run:
```sh
brew install --cask visual-studio-code
```

### **Verify Installation**
Check if VS Code is installed:
```sh
code --version
```
If VS Code doesn’t open, restart your terminal and try again.

### **(Optional) Add `code` Command to PATH**
If the `code` command doesn’t work, add it manually:
```sh
ln -s "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" /usr/local/bin/code
```
Now, you can open VS Code from the terminal using:
```sh
code .
```

### **(Optional) Install Useful Extensions via Homebrew**
You can install VS Code extensions directly from Homebrew:
```sh
code --install-extension ms-python.python
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
```

---

## **Enable Profiles for Different Programming Languages**
VS Code introduced **Profiles**, which allow you to customize extensions, settings, and UI layout per profile.

### **Steps to Create a Profile**
1. Open **VS Code**.
2. Press **Ctrl + Shift + P** (or **Cmd + Shift + P** on macOS) to open the command palette.
3. Type and select **"Profiles: Create a Profile"**.
4. Name your profile based on the programming language (e.g., **Python Dev, Web Dev, Java Dev**).
5. Choose what to include in the profile:
    - **Settings** (Language-specific settings)
    - **Extensions** (Only relevant plugins)
    - **Keybindings**
    - **UI State** (Theme, layout, etc.)

### **Switching Between Profiles**
1. Press **Ctrl + Shift + P** (or **Cmd + Shift + P** on macOS) then type **"Profiles: Switch Profile"**.
2. Select your desired profile.

---

## **Install and Configure Extensions Per Profile**
Each profile can have its own set of extensions.

### **Example: Recommended Extensions Per Language**
- **Python Profile**
    - Python (`ms-python.python`)
    - Pylance (`ms-python.vscode-pylance`)
    - Jupyter (`ms-toolsai.jupyter`)
    - Black Formatter (`ms-python.black-formatter`)

- **Web Development Profile**
    - Live Server (`ritwickdey.LiveServer`)
    - Prettier (`esbenp.prettier-vscode`)
    - ESLint (`dbaeumer.vscode-eslint`)
    - Tailwind CSS IntelliSense (`bradlc.vscode-tailwindcss`)

- **Java Profile**
    - Java Extension Pack (`vscjava.vscode-java-pack`)
    - Maven (`vscjava.vscode-maven`)
    - Debugger for Java (`vscjava.vscode-java-debug`)

To install extensions per profile:
1. Switch to the **desired profile**.
2. Open Extensions (`Ctrl + Shift + X`).
3. Search and install relevant extensions.

---

## **Store VS Code Configurations in the Cloud**
To sync your settings, themes, keybindings, and extensions:

### **Enable Settings Sync**
1. Open **VS Code**.
2. Press **Ctrl + Shift + P** (or **Cmd + Shift + P** on macOS), then search for **"Settings Sync: Turn On"**.
3. Sign in with:
    - **GitHub**
    - **Microsoft Account**
4. Choose what to sync:
    - Settings
    - Extensions
    - Keybindings
    - UI State
    - Snippets

> **Note:** Settings are stored in the cloud and can be restored on any new device.

### **Using GitHub to Store Configuration**
If you want more control:
1. Go to `C:\Users\YourUsername\AppData\Roaming\Code\User` (Windows) or `~/.config/Code/User` (Linux/macOS).
2. Backup `settings.json`, `keybindings.json`, `snippets`, and `extensions.json` to a private **GitHub repository**.

Example Git commands:
```sh
git init
git add .
git commit -m "Backup VS Code Config"
git remote add origin https://github.com/yourusername/vscode-config.git
git push -u origin main
```

---
## **Configure Language-Specific Settings**
VS Code allows **per-language settings**. To do this:

1. Open `settings.json` (Press **Ctrl + Shift + P** (or **Cmd + Shift + P** on macOS) → "Preferences: Open Settings (JSON)").
2. Add settings specific to a language.

### **Example: Different Formatting Per Language**
```json
{
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  }
}
```

---

## **Configure Workspaces for Specific Projects**
For team projects or specific settings per folder:

1. Open a project folder in **VS Code**.
2. Create a `.vscode` folder inside the project directory.
3. Add a `settings.json` file inside `.vscode`.

Example:
```json
{
  "editor.tabSize": 4,
  "editor.formatOnSave": true,
  "python.analysis.typeCheckingMode": "basic"
}
```
Now, these settings only apply **inside this project**.

## **Using Dev Containers for Full Isolation**
If you want completely separate environments per language, use **Dev Containers**.

### **Steps:**
1. Install **Dev Containers** extension (`ms-vscode-remote.remote-containers`).
2. Create a `.devcontainer/devcontainer.json` file in your project.
3. Define the container with the language dependencies.

Example for Python:
```json
{
  "name": "Python Dev",
  "image": "mcr.microsoft.com/devcontainers/python:3.10",
  "extensions": ["ms-python.python", "ms-toolsai.jupyter"]
}
```
Now, when you open the project, it runs inside a **containerized environment**.

---

## **Automate Setup on a New Machine**
To restore everything quickly on a new machine:
1. Install **VS Code**.
2. Enable **Settings Sync**.
3. Clone your **GitHub settings backup** (if applicable).
4. Install all necessary **profiles and extensions**.

---

## **Uninstall VS Code**

### **Uninstall using Homebrew**
If you installed **VS Code** using Homebrew, run:
```sh
brew uninstall --cask visual-studio-code
```
To confirm it's gone, check:
```sh
brew list --cask | grep visual-studio-code
```

If it's still listed, force remove it:
```sh
brew uninstall --cask --force visual-studio-code
```

### **Delete Configuration Files and Cache**
Even after uninstalling, VS Code leaves behind settings, extensions, and cache. Remove them manually:

### **Delete VS Code User Data**
```sh
rm -rf ~/Library/Application\ Support/Code
```

### **Delete VS Code Extensions**
```sh
rm -rf ~/.vscode
```

### **Delete Cache Files**
```sh
rm -rf ~/Library/Caches/com.microsoft.VSCode
rm -rf ~/Library/Caches/com.microsoft.VSCode.ShipIt
```

### **Remove Logs**
```sh
rm -rf ~/Library/Logs/Code
```

### **Remove Saved Application State**
```sh
rm -rf ~/Library/Saved\ Application\ State/com.microsoft.VSCode.savedState
```

### **Verify VS Code is Fully Removed**
Run:
```sh
ls ~/Library/Application\ Support/ | grep Code
ls ~/.vscode
```
If nothing appears, **VS Code is completely removed**.

### **Remove Homebrew Cask Data (Optional)**
To ensure all traces are removed from Homebrew:
```sh
brew cleanup
```

### **Restart Your Mac (Recommended)**
This ensures no background processes are running from VS Code.

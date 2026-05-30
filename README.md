
# 🚀 PyInstaller Matrix Builder Action  
**Build native executables for Windows, macOS, and Linux — all in one workflow.**

<p align="center">

  <!-- Stars -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/stargazers">
    <img src="https://img.shields.io/github/stars/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Forks -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/network/members">
    <img src="https://img.shields.io/github/forks/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Watchers -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/watchers">
    <img src="https://img.shields.io/github/watchers/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Issues -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/issues">
    <img src="https://img.shields.io/github/issues/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Pull Requests -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/pulls">
    <img src="https://img.shields.io/github/issues-pr/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- License -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Last Commit -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/commits/main">
    <img src="https://img.shields.io/github/last-commit/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Repo Size -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build">
    <img src="https://img.shields.io/github/repo-size/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Commit Activity -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/commits">
    <img src="https://img.shields.io/github/commit-activity/y/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

  <!-- Contributors -->
  <a href="https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build/graphs/contributors">
    <img src="https://img.shields.io/github/contributors/bytedeveloping/Multi-OS_PyInstaller_Build?style=for-the-badge" />
  </a>

</p>

---

## ✨ What This Action Does

This GitHub Action compiles your Python project into **native executables** for:

- **Windows** → `.exe`  
- **macOS** → `.app` (auto‑zipped for safety)  
- **Linux** → `.bin`

All in a **single matrix workflow**, powered by PyInstaller.

Perfect for distributing CLI tools, desktop apps, utilities, and standalone binaries — without requiring users to install Python.

---

## 🔥 Key Features

- **Multi‑OS Matrix Builds**  
  Build for Windows, macOS, and Linux simultaneously.

- **Automatic Dependency Installation**  
  Detects and installs `requirements.txt` if present.

- **macOS Bundle Protection**  
  Automatically zips `.app` bundles to prevent GitHub from stripping metadata.

- **Flexible Output Options**  
  Choose between:
  - Uploading artifacts  
  - Committing binaries directly into your repo

- **Race‑Condition Safe**  
  Includes a 5‑attempt rebase loop to prevent multi‑runner push collisions.

---

## 🚀 Quick Start

Create `.github/workflows/build.yml`:

```yaml
name: Build Executables

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  compile:
    name: Build on ${{ matrix.os }}
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [windows-latest, macos-latest, ubuntu-latest]

    permissions:
      contents: write # Required for commit-to-repo-folder

    steps:
    - name: Checkout Source Code
      uses: actions/checkout@v4
      with:
        fetch-depth: 0

    - name: Run PyInstaller Builder
      uses: bytedeveloping/Multi-OS_PyInstaller_Build@v1
      with:
        file_path: 'main.py'
        upload_destination: 'actions-artifact'
```

---

## 🎛️ Inputs

| Input | Description | Required | Default | Options |
|-------|-------------|----------|---------|---------|
| `file_path` | Path to the Python entry script. | **Yes** | `main.py` | Any file |
| `upload_destination` | Where to store the compiled binaries. | **Yes** | `actions-artifact` | `actions-artifact`, `commit-to-repo-folder` |
| `repo_folder_path` | Folder to commit binaries into (if using repo mode). | No | `builds/` | Any folder except `dist/` |

---

## 📦 Output Options

### **A) Upload as GitHub Artifacts (Default)**  
Binaries appear in the workflow summary:

- `binary-Windows/main.exe`  
- `binary-Linux/main.bin`  
- `binary-macOS/main.app.zip`

### **B) Commit Directly to Your Repository**

The action pushes compiled binaries into your repo:

```
builds/
 ├── windows/
 ├── linux/
 └── macos/
```

> ⚠️ Requires:  
> `permissions: contents: write`

---

## 🛠️ Custom PyInstaller Arguments

Need icons, hidden imports, windowed mode, or advanced flags?

Just edit the `pyinstaller` command inside your local `action.yml`:

```yaml
pyinstaller --onefile --icon=myicon.ico --hidden-import=my.module {{file_path}}
```

---

## 📄 License

This project is licensed under the [**MIT License**.](https://github.com/bytedeveloping/Multi-OS_PyInstaller_Build?tab=MIT-1-ov-file)



---
layout: default
title: Installation
sectionid: Install
permalink: /Install/
---

# Installation Guide

RCAIDE and RCAIDE-GUI are available on macOS, Linux, and Windows.

> **Install both packages into the same Python environment.** RCAIDE-GUI calls RCAIDE directly at runtime. If they are in different environments the GUI will fail to start, aircraft files will not load, and geometry will not render.

---

## Prerequisites

| Requirement | Version | Notes |
|---|---|---|
| Python | 3.9 – 3.13 | [python.org/downloads](https://www.python.org/downloads/) |
| pip | 26 or newer | Run `pip install --upgrade pip` after installing Python |

---

## Quick Install (recommended)

Create a virtual environment, then install both packages into it before launching the GUI.

**macOS / Linux**

```bash
python3 -m venv rcaide_env
source rcaide_env/bin/activate
pip install --upgrade pip
pip install rcaide-leads rcaide-gui
rcaide-gui
```

**Windows**

```bash
python -m venv rcaide_env
rcaide_env\Scripts\activate
pip install --upgrade pip
pip install rcaide-leads rcaide-gui
rcaide-gui
```

Installing both packages in the same `pip install` command is the easiest way to guarantee they land in the same environment.

---

## Verify the installation

```python
import RCAIDE
print(RCAIDE.__version__)
```

---

## Running RCAIDE-GUI from Source

If you want to run RCAIDE-GUI directly from the GitHub repository, clone it into an environment that already contains RCAIDE:

```bash
# 1. Create and activate an environment
python3 -m venv rcaide_env
source rcaide_env/bin/activate   # Windows: rcaide_env\Scripts\activate

# 2. Install RCAIDE into that environment first
pip install --upgrade pip
pip install rcaide-leads

# 3. Clone and install the GUI into the same environment
git clone https://github.com/leadsgroup/RCAIDE_GUI.git
cd RCAIDE_GUI
pip install -e .
rcaide-gui
```

Or launch without installing:

```bash
python main.py
```

---

## Troubleshooting

**`ModuleNotFoundError: No module named 'RCAIDE.Input_Output'`**

Your pip version is too old. Run:

```bash
pip install --upgrade pip
```

Then re-install:

```bash
pip install rcaide-leads rcaide-gui
```

---

**GUI images and aircraft files do not load / GUI fails to start**

RCAIDE and RCAIDE-GUI are in different Python environments. Check which Python each package is installed under:

```bash
# macOS / Linux
which python
pip show rcaide-leads
pip show rcaide-gui

# Windows
where python
pip show rcaide-leads
pip show rcaide-gui
```

Both `Location:` lines must point to the same environment. If they differ, activate the environment that contains RCAIDE-LEADS and reinstall the GUI into it:

```bash
pip install --force-reinstall rcaide-gui
```

---

**`rcaide-gui` command not found after install**

This usually means the virtual environment is not activated, or the script was installed in a different environment. Activate the correct environment first and try again. If the command is still not found, you can run it directly using the full path:

```bash
# macOS / Linux
rcaide_env/bin/rcaide-gui

# Windows
rcaide_env\Scripts\rcaide-gui
```

---

**Permission errors on macOS / Linux**

Avoid `sudo pip install`. Always install inside a virtual environment or add `--user`:

```bash
pip install --user rcaide-leads rcaide-gui
```

---

## Keeping RCAIDE Up to Date

```bash
pip install --upgrade rcaide-leads rcaide-gui
```

---

## Platform Notes

| Platform | Known issues |
|---|---|
| macOS (Apple Silicon) | If the geometry viewer crashes, try `pip install vtk --pre`. If that still fails, install [Rosetta](https://support.apple.com/en-us/HT211861) (Apple's compatibility layer for running Intel software on Apple Silicon) and re-run the installer. |
| Windows | Run Command Prompt or PowerShell as a regular user, not Administrator, to avoid path permission issues. |
| Linux | `libGL` and `libEGL` are required for the 3D viewer. Install with `sudo apt install libgl1 libegl1` (Debian/Ubuntu). |

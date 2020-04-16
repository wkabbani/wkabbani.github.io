---
layout: post
title: A Practical Guide to Setup Python Development Environment
tags: [Python, DevEnv, Guide]
color: brown
author: wassim
---

My quick practical guide to start a minimal python development environment with VS Code asap.


## 1. Install Python

The first step you need to do obviously is to install Python on your machine. So go ahead and install the latest version for your operating system from the [official website](https://www.python.org/).

You can verify that Python is installed and check the version by running the following command:

- **Mac**

```shell
python3 --version
```

- **Windows**

```shell
py -3 --version
```
## 2. Editor + Plugins

The second step is to install [Visual Studio Code](https://code.visualstudio.com/). Once installed, install also the [Python extension for visual studio code](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

## 3. Create a Repo

You'll most likely want to house your Python project in its own folder, and make that folder a git repo as well. So go ahead and create a folder. Once the folder is created you can then execute the following commands:

```shell
cd new_project_folder
git init
```

## 4. Create a Virtual Environment

Next, you'll need to create a virtual environment to isolate this new project and the packages it uses from the global python environment. To do this you can run the following:

- **Mac**

```shell
cd new_project_folder
python3 -m venv .venv
source .venv/bin/activate
```

- **Windows**

```shell
cd new_project_folder
py -3 -m venv .venv 
py -3-64 -m venv .venv # or this cmd if you had multiple python versions and wanted to be sure about which one to use.
.venv\scripts\activate
```

You'll sometimes need to execute the following command in PowerShell on windows if the above gave an error:

```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
```

## 5. Open VS Code

When you open the new folder using VS Code, it will recognize the virtual environment you created and prompt you to set is as the python interpreter for this project.

If you didn't get prompted, then you can open the Command Palette (⇧⌘P),  start typing _Python: Select Interpreter_, select it and it will allow you to choose the interpreter you want. Choose the virtual environment you created.

## 6. Configure PYTHONPATH

You can now start creating python files and running them. But the issue you'll likely to come across, when you create sub-directories within you project directory to organize the code you want to write, is the famous error of _ModuleNotFound_. And to avoid this you need to do the following:

- **Mac**

You need to add the parent directory of the project directory to the python path. And to do this you need to add it to the user settings of VS Code.

```json
"terminal.integrated.env.osx": {
    "PYTHONPATH": "/path-to-the-parent-of-the-project-directory"
}
```

- **Windows**

You need to add a file in the _venv/lib_ directory with the extension _pth_ (you can name it anything), and add inside it the path of the parent of the project directory.
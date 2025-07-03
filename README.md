# Setup Python Development Environment using pyenv on macOS
(Assumes macOS with Homebrew package manager and Zsh shell)

## Understanding Python Virtual Environments
A Python virtual environment creates a self-contained area for your project's dependencies.
This allows you to manage the specific versions of Python packages required for one project
separately, preventing conflicts with your global Python installation or other projects. 
It's like giving each project its own isolated set of tools.

<p align="center">
    <img src="/images/python-envs.png" />
</p>
## 1. Install pyenv & pyenv-virtualenv
These tools manage different Python versions and the virtual environments associated with them.


```shell
# Installs pyenv (manages Python versions)
brew install pyenv

# Installs the pyenv plugin for managing virtual environments
brew install pyenv-virtualenv
```
## 2. Install Desired Python Version(s)
Install the specific versions of Python you need for your projects.


```shell
# Example: Install Python 3.12.3 (replace with your desired version)
pyenv install 3.12.3

# Optional: Install another version if needed
# pyenv install <another_python_version>

# List installed Python versions managed by pyenv
pyenv versions

# Optional: Set a global default Python version (used when not in a specific project)
# pyenv global 3.12.3
```

## 3. Configure Your Shell (Zsh)
Add the following lines to your ~/.zshrc file to ensure pyenv and pyenv-virtualenv load correctly when you open a terminal.

```shell
# Add pyenv executable to PATH and initialize pyenv
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"

# Initialize pyenv-virtualenv (allows auto-activation)
eval "$(pyenv virtualenv-init -)"
```
> **IMPORTANT:** After saving changes to .zshrc, either close and reopen your terminal window or run source ~/.zshrc for the changes to take effect.

## 4. Create a Project-Specific Virtual Environment
Navigate to your project's directory or create one. 
Then, create a virtual environment linked to a specific installed Python version.
```shell
# Create a virtual environment named <your_env_name> using Python 3.12.3
# Replace <your_env_name> with a descriptive name (e.g., myapp-venv, projectx-312)
# Replace 3.12.3 if you want to base it on a different installed Python version
pyenv virtualenv 3.12.3 <your_env_name>
```
### 5. List All Virtual Environment

```shell
pyenv virtualenvs
```
output
```shell
  3.13.5/envs/demo (created from /Users/mradulpandey/.pyenv/versions/3.13.5)
  3.13.5/envs/learning (created from /Users/mradulpandey/.pyenv/versions/3.13.5)
  demo (created from /Users/mradulpandey/.pyenv/versions/3.13.5)
  learning (created from /Users/mradulpandey/.pyenv/versions/3.13.5)
```
### 5. Remove Specific Virtual Environment

```shell
pyenv uninstall demo

pyenv uninstall learning
```

## 5.  Enable Auto-Activation for Your Project
To make pyenv automatically activate this virtual environment whenever you cd into your project directory, create a .python-version file in the root directory of your project.

```shell
echo "<your_env_name>" > .python-version
```
>  **IMPORTANT:** Use the EXACT same name you chose for <your_env_name> in step 4
echo "<your_env_name>" > .python-version

## 6. Verify Environment Activation
When the environment is active:
- Your shell prompt usually changes to include the environment name (e.g., (<your_env_name>) user@host ~/.../project$).
- You can check the active Python version/environment recognized by pyenv:
    ```shell
    pyenv version
    ```
    (This should output <your_env_name>)
- Or check the standard virtual environment variable:
    ```shell
    echo $VIRTUAL_ENV
    ```
  (This should output the path to your virtual environment)

## 7. Configure IntelliJ IDEA (or PyCharm)
To let your IDE know about the virtual environment:
1.  Go to File -> Project Structure -> Platform Settings -> SDKs.
2. Click the + icon and choose Add Python SDK.... 
3. Select Existing environment. 
4. Set the Interpreter path to the python executable inside your specific pyenv virtual environment. The path will look like this (replace placeholders):
    ```shell
    ~/.pyenv/versions/<myproject>/bin/python
    ```
    Example: /Users/mradul.pandey/.pyenv/versions/Learning/bin/python

5. Click OK.
6. Go to File -> Project Structure -> Project -> SDK and select the SDK you just added.

<p align="center">
    <img src="/images/setup-python.png" />
</p>

## NOTE: Why use pyenv virtualenv instead of python -m venv?
- Centralized Management: Keeps Python versions and their associated virtual environments organized together within the ~/.pyenv directory.
- Automatic Activation: Lets you easily auto-activate/deactivate environments based on the project directory via the .python-version file. No need to manually run source .../activate.
- Version Flexibility: Easily create multiple distinct virtual environments based on the same or different Python versions managed by pyenv.
- Good Integration: Works smoothly with terminal workflows and IDEs like IntelliJ/PyCharm and VSCode.

## Caution: Avoid Mixing venv and pyenv-virtualenv
- Choose one method (pyenv virtualenv recommended here) for managing virtual environments within a single project to prevent confusion and potential path conflicts.



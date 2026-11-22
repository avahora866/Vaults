---
tags:
  - FORMAT
  - NodeJs
---
Certainly\! **nvm** (Node Version Manager) is a very useful tool for managing multiple versions of Node.js on your machine. It allows you to easily switch between different versions of Node.js, which can be particularly helpful when working on multiple projects that require different versions.

# What is nvm?

**nvm** is a command-line utility that enables you to install, manage, and switch between different versions of Node.js. With **nvm**, you can:

* Install multiple versions of Node.js.

* Switch between different versions of Node.js.

* Set a default version for Node.js.

* Manage global npm packages for different versions of Node.js.

# Why Use nvm?

1. **Multiple Versions**: Different projects might require different versions of Node.js (e.g., older projects might require an older version). nvm lets you install multiple versions on the same machine without any conflicts.

2. **Easy Switching**: You can quickly switch between different versions of Node.js with a simple command.

3. **Project-Specific Versions**: You can use different versions of Node.js for different projects to ensure compatibility.

4. **No Need to Uninstall**: nvm allows you to install new versions without the need to uninstall previous ones.

---

# Installing nvm

## For Linux/macOS

1. **Install nvm**:

    You can install nvm by running the following command in your terminal:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

2.   
   Alternatively, you can use **wget**:

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

3.   
   **Restart your terminal** to make sure nvm is loaded properly. Alternatively, you can run the following to source nvm manually:

```
urce ~/.bashrc    # For bash
urce ~/.zshrc     # For zsh
```

4.   
   **Verify installation**:

    After installation, verify that nvm is working by typing:

```
command -v nvm
```

5.   
   This should return `nvm` if the installation was successful.

## For Windows

For Windows, you can use **nvm-windows**, which is a Windows-specific version of nvm:

1. **Download the nvm-windows installer** from the releases page on GitHub:  
    [nvm-windows GitHub](https://github.com/coreybutler/nvm-windows/releases)

2. **Run the installer** and follow the instructions to complete the installation.

3. **Verify installation** by opening a new Command Prompt and typing:

```
nvm version
```

4.   
   It should return the version of nvm installed.

---

# Basic nvm Commands

Once **nvm** is installed, you can use the following commands to manage Node.js versions:

## Install a Version of Node.js

To install a specific version of Node.js, use:

```
nvm install <version>
```

For example, to install **Node.js version 14.17.0**, you would run:

```
nvm install 14.17.0
```

To install the **latest LTS version**:

```
nvm install --lts
```

To install the **latest stable version**:

```
nvm install node
```

## Use a Specific Version

Once you have multiple versions of Node.js installed, you can switch between them using:

```
nvm use <version>
```

For example, to use **Node.js version 14.17.0**, you would run:

```
nvm use 14.17.0
```

## List Installed Versions

To see which versions of Node.js are installed, use:

```
nvm ls
```

This will display all the installed versions and indicate which version is currently being used.

## Set a Default Version

To set a default version of Node.js that nvm will use when you open a new terminal session, use:

```
nvm alias default <version>
```

For example, to set **Node.js version 14.17.0** as the default:

```
nvm alias default 14.17.0
```

## Uninstall a Version

To uninstall a specific version of Node.js, use:

```
nvm uninstall <version>
```

For example, to uninstall **Node.js version 14.17.0**:

```
nvm uninstall 14.17.0
```

---

# Additional Useful nvm Commands

## List Available Versions

If you’re unsure which versions of Node.js are available for installation, you can list them using:

```
nvm ls-remote
```

This will show you all the available versions of Node.js that can be installed.

## Show Current Node Version

To check which version of Node.js is currently in use, simply run:

```
nvm current
```

Alternatively, you can also use the regular `node -v` or `node --version` to check the version of Node.js in use.

## Install and Use a Version in One Command

If you want to install a version and use it immediately, you can combine the two commands:

```
nvm install 16.4.0 && nvm use 16.4.0
```

---

# Why Use nvm in Development?

* **Compatibility**: Different Node.js versions may be required for different projects. nvm makes it easy to switch between versions without conflicts.

* **Upgrade/Downgrade**: You can upgrade or downgrade Node.js versions as needed for compatibility with libraries or specific project requirements.

* **Multiple Projects**: If you’re working on several projects with different Node.js version requirements, nvm allows you to quickly change versions for each project.

---

# Example Scenario

Imagine you're working on two projects:

1. **Project A** requires Node.js 12.x for compatibility.

2. **Project B** requires Node.js 14.x for the latest features.

Using **nvm**, you can easily install both versions and switch between them:

```
nvm install 12
nvm install 14
m use 12    # Switch to Node.js 12 for Project A
m use 14    # Switch to Node.js 14 for Project B
```

This setup allows you to work seamlessly with both projects without manually changing versions or worrying about conflicts.

---

# Conclusion

**nvm** is a powerful tool for managing Node.js versions, and it simplifies working with multiple versions on the same system. It allows you to install, switch, and manage different versions, making it easier to develop and maintain projects with specific version requirements.

Let me know if you need further clarification on any of the steps\!


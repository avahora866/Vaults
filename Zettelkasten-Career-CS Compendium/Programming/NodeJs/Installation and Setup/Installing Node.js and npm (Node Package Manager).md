---
tags:
  - FORMAT
  - NodeJs
---
Certainly\! Here's a step-by-step guide on how to install **Node.js** and **npm** (Node Package Manager), which comes bundled with Node.js.

---

# What is Node.js and npm?

* **Node.js** is a runtime environment that allows you to run JavaScript on the server side.

* **npm** is the **Node Package Manager** and comes bundled with Node.js. It allows you to install and manage libraries and packages for your Node.js projects.

---

## Steps to Install Node.js and npm

### Installing Node.js on Your System

#### For Windows & macOS:

1. **Go to the Node.js website**:  
    Visit the official Node.js website at [https://nodejs.org](https://nodejs.org/).

2. **Download the installer**:

   * You'll see two versions of Node.js available:

     * **LTS (Long Term Support)**: Recommended for most users (stable and tested).

     * **Current**: Includes the latest features but might be less stable.

   * Click on the **LTS** version unless you specifically need the latest features.

3. **Run the installer**:

   * Once downloaded, open the installer.

   * Follow the on-screen instructions to install Node.js and npm.

4. **Verify installation**: After installation is complete, you can verify if Node.js and npm were installed correctly by opening the **Command Prompt** (Windows) or **Terminal** (macOS) and running the following commands:

```
node -v
npm -v
```

   *   
     These commands should return the **version numbers** of Node.js and npm. For example:

     * `node -v` might return something like `v18.x.x`.

     * `npm -v` might return something like `8.x.x`.

---

#### For Linux (Ubuntu/Debian):

1. **Update package index**: Open a terminal and update the local package index with:

```
sudo apt update
```

2.   
   **Install Node.js**: Install Node.js and npm using the package manager. To install the **LTS version** of Node.js, run:

```
sudo apt install nodejs npm
```

3.   
   To install the latest version of Node.js, you can use NodeSource repository:

```
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

4.   
   **Verify installation**: Check if Node.js and npm are correctly installed by running:

```
node -v
npm -v
```

   *   
     If installed correctly, you should see the version of Node.js and npm.

---

## Installing Node.js Using a Version Manager (Optional)

If you need to manage multiple versions of Node.js on your machine (which is common when developing different projects), you can use **nvm (Node Version Manager)**.

### Installing nvm:

1. **Install nvm** (Node Version Manager):

    For **Linux/macOS**:

    Open a terminal and run the following command to install nvm:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

2.   
   For **Windows**, you can use a tool like **nvm-windows** to manage Node.js versions. You can find the installation instructions on the [nvm-windows GitHub page](https://github.com/coreybutler/nvm-windows).

3. **Restart your terminal** to apply the changes.

4. **Install Node.js using nvm**:

    Once nvm is installed, you can install the **LTS version** or the **latest version** of Node.js:

```
vm install --lts      # Install the LTS version
vm install node       # Install the latest version
```

5.   
   **Verify installation**:

    Check the installed Node.js version using:

```
node -v
```

6.   
   You can also switch between different Node.js versions using:

```
nvm use <version_number>
```

---

## Installing Packages with npm

Once you have Node.js and npm installed, you can start using **npm** to install packages (JavaScript libraries, tools, etc.) for your projects.

### Install a package globally (so you can use it anywhere on your system):

```
npm install -g <package-name>
```

For example, to install **Express** (a popular web framework) globally:

```
npm install -g express
```

### Install a package locally (specific to your project):

1. First, initialize a **package.json** file (if you haven't already) by running:

```
npm init
```

2.   
   Then, install a package locally:

```
npm install <package-name>
```

For example, to install **Express** locally to a project:

```
npm install express
```

The `node_modules` folder will be created in your project, containing the installed packages.

---

## Updating Node.js and npm

* To **update npm**:

```
npm install -g npm
```

*   
  To **update Node.js**, you can either reinstall the latest version via the Node.js website or use **nvm** if you're using a version manager:

```
vm install node   # Installs the latest version of Node.js
```

---

## Conclusion

Now you should have **Node.js** and **npm** installed on your system, and you can start developing applications or managing packages for your projects.

Feel free to ask if you need help with any specific part of the installation\!


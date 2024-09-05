---
sidebar_position: 1
---

# How to Install Node.js Using NVM

Node Version Manager (NVM) is a useful tool that allows you to easily install and manage multiple versions of Node.js on the same computer. In this guide, we'll learn how to install NVM and use it to install Node.js.

## Step 1: Install NVM

### On Linux and macOS

1. Open a terminal and run the following command to install NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

2. Close and reopen your terminal, or run the following command to apply the changes:

```bash
source ~/.bashrc
```

### On Windows

1. Download and install NVM for Windows from the [official GitHub page](https://github.com/coreybutler/nvm-windows/releases).

2. Run the installer and follow the prompts.

## Step 2: Verify NVM Installation

To check if NVM has been installed successfully, run the following command in your terminal:

```bash
nvm --version
```

If NVM is installed correctly, you should see the version number of NVM.

## Step 3: Install Node.js

1. To see a list of available Node.js versions, run:

```bash
nvm ls-remote
```

2. Install the desired Node.js version (for example, 14.17.0):

```bash
nvm install 14.17.0
```

3. To use the Node.js version you just installed:

```bash
nvm use 14.17.0
```

## Step 4: Verify Node.js Installation

Check if Node.js has been installed successfully by running:

```bash
node --version
npm --version
```

You should see the version numbers of Node.js and npm.

## Managing Multiple Node.js Versions

- To install the latest version of Node.js:

```bash
nvm install node
```

- To switch between installed versions:

```bash
nvm use <version>
```

- To set a default version:

```bash
nvm alias default <version>
```

By using NVM, you can easily switch between different Node.js versions depending on your project requirements.
```

This English version maintains the same structure and content as the previous Vietnamese version. It's still positioned as the first item in the sidebar for the new `tutorial-nodejs` section. The guide covers the installation process for both Unix-based systems (Linux and macOS) and Windows, as well as basic usage of NVM for managing Node.js versions.
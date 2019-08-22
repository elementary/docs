# The Basic Setup

Before we even think about writing code, you'll need a certain basic setup. This chapter will walk you through the process of getting set up. We will cover the following topics:

* Creating an account on GitHub and importing an SSH key
* Setting up the Git revision control system
* Getting and using the elementary developer "SDK"

We’re going to assume that you’re working from a clean installation of elementary OS Juno Beta or later. This is important as the instructions you’re given may reference apps that are not present \(or even available\) in other GNU/Linux based operating systems like Ubuntu. It is possible to apply the principles of this guide to Ubuntu development, but it may be more difficult to follow along.

## GitHub

GitHub is an online platform for hosting code, reporting issues, tracking milestones, making releases, and more. If you're planning to publish your app through AppCenter, you'll need a GitHub account. If you already have an account, feel free to move on to the next section. Otherwise, [sign up for a GitHub account](https://github.com/join) and return when you're finished.

## Git

To download and upload to GitHub, you'll need the Terminal program `git`. Git is a type of [revision control system](https://en.wikipedia.org/wiki/Version_control) that allows multiple developers to collaboratively develop and maintain code while keeping track of each revision along the way.

If you're ready, let's get you set up to use Git:

1. Open the Terminal and install Git

   ```bash
   sudo apt install git
   ```

2. We need to inform Git who we are so that when we upload code it is attributed correctly. Inform Git of your identity with the following commands

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "You@email.com"
   ```

3. To authenticate and transfer code securely, you’ll need to generate an [SSH](https://en.wikipedia.org/wiki/Secure_Shell) key pair \(a kind of fingerprint for your computer\) and import your public key to GitHub. Type the following in Terminal:

   ```bash
   ssh-keygen -t rsa
   ```

4. When prompted, press Enter to accept the default file name for your key. You can choose to protect your key with a password or press Enter again to use no password when pushing code.
5. Now we're going to import your public key to GitHub. View your public SSH key with the following command, then copy the text that appears

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

6. Visit [your SSH keys page](https://github.com/settings/keys) and click the green button in the upper right-hand corner that says "New SSH key". Paste your key in the "Key" box and give it a title.

We're all done! Now you can download source code hosted on GitHub and upload your own code. We'll revisit using `git` in a bit, but for now you're set up. 

{% hint style="info" %}
For a more in-depth intro to Git, we recommend [Codecademy's course on Git](https://www.codecademy.com/learn/learn-git).
{% endhint %}

## Developer "SDK"

At the time of this writing, elementary OS doesn't have a full SDK like Android or iOS. But luckily, we only need a couple simple apps to get started writing code.

### Code

![](https://elementary.io/images/thirdparty-icons/apps/128/io.elementary.code.svg)

The first piece of our simple "SDK" is Code. This comes by default with elementary OS. It comes with some helpful features like syntax highlighting, auto-save, and a Folder Manager. There are other extensions for Code as well, like the Outline, Terminal, Word Completion, or Devhelp extensions. Play around with what works best for you.

### Terminal

![](https://elementary.io/images/icons/apps/128/utilities-terminal.svg)

We’re going to use Terminal in order to compile our code, push revisions to GitHub \(using `git`\), and other good stuff. Throughout this guide, we’ll be issuing Terminal commands. You should assume that any command is executed from the directory “Projects” in your home folder unless otherwise stated. Since elementary OS doesn’t come with that folder by default, you’ll need to create it.

Open Terminal and issue the following command:

```bash
mkdir Projects
```

### Development Libraries

![](https://elementary.io/images/icons/apps/128/application-default-icon.svg)

In order to build apps you're going to need their development libraries. We can fetch a basic set of libraries and other development tools with the following terminal command:

```bash
sudo apt install elementary-sdk
```

And with that, we're ready to dive into development! Let's move on!


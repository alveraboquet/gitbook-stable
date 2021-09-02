---
description: Instructions to install Gunbot on a macOS machine.
---

# macOS installation

## Instructions

1. Unpack the release .zip file to a new folder.
2. Start running Gunbot with the following command from a terminal in your Gunbot folder:  `./gunthy-macos`. Keep this terminal window open.
3. Open [localhost:5000](http://localhost:5000) in a browser on the same system to access the Gunbot GUI \(modern browsers recommended, preferably Chrome or Firefox\)
4. Make sure to enter your registered ERC-20 wallet \("Gunthy wallet"\) and your [registered API key](../profile-settings/connect-exchange.md) in Gunbot before starting the bot core for the first time.

{% hint style="success" %}
System security settings may show that the `gunthy-macos` file is damaged or dangerous.  
One way to disable the OS restrictions on the Gunbot folder is using this command in a terminal window: `xattr -d -r com.apple.quarantine <foldername>`

To run on an M1 processor, create a Rosetta terminal \([see step 1 here](https://www.courier.com/blog/tips-and-tricks-to-setup-your-apple-m1-for-development)\) and use that terminal to launch Gunbot.
{% endhint %}

{% hint style="info" %}
Depending on your systems settings, you may need to add a firewall rule to allow for incoming traffic on TCP port 5000.
{% endhint %}

{% hint style="danger" %}
### Security notice

Gunbot is intended to run on your local system. Making the Gunbot GUI available from outside networks is inherently risky, only do so on your own responsibility.

Considerable efforts went into securing the GUI, but please understand that achieving 100% security is not realistic.
{% endhint %}

## Installation steps \(visual\)

The following steps use the built in **Terminal** program to handle a few one time steps.

#### 1. Move the downloaded zip file to a folder where you want to unpack it

![](../../.gitbook/assets/image%20%28110%29.png)

#### 2. Unpack the file

![In a terminal window, go to the folder where the .zip file is with the cd command, then unpack the zip](../../.gitbook/assets/image%20%28107%29.png)

After you've done this, type `cd macos` to enter the actual program folder

#### 3. Remove program restrictions \(to make sure macos allows running it\)

![](../../.gitbook/assets/image%20%28118%29.png)

Type `xattr -d -r com.apple.quarantine <foldername>` to remove gatekeeper restrictions. Instead of &lt;foldername&gt;, use the full path to the folder where you have unpacked Gunbot.

#### 4. Make sure the gunthy-macos file is executable

Type `chmod +x gunthy-macos` to make sure it can be ran as a program.

#### 5. Start the bot

Type `./gunthy-macos` to start running the software.

You can now access it in your browser using this link: [http://localhost:5000](http://localhost:5000)


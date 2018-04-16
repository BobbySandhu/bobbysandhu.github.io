---
title: "Fastlane - Automate your android build deployment"
date: 2018-01-14
author: Sahiljeet Singh
categories:
- blog
tags:
- android
---

Before explaining what is *`Fastlane`* and how to configure it on your Linux system, I would like to ask you that what are those steps or series of actions you take when you wanted to deploy your Android application on Google Play Store or any other platform like *Hockey App*…?

This is what you typically do for deploying your app:

 1. Generating build with your android studio.
 2. Taking different screenshots for your app.
 3. Login into your Play Store account and following the instructed steps for the deployment.

This is just for *Play Store* deployment, after that you might have to update various person involved in the project yourself by sending email and the app link, writing all those changes done in the version and keeping track of all the version numbers and related changes manually. Sounds a lot of work and even doing it since ages…?

If that is something you have been doing for your projects, it is time to move to an alternate option that will sort this tedious task for you. This is where Fastlane comes into the game.

**What is `Fastlane`…?**

Fastlane is a build automation tool that will help you to overcome the above said problem with just one line of command. It automates your build generation, like taking app screenshots, app signing, releasing application.

**How to install `fastlane`…?**

If you are running *MacOS* and want to proceed with the configuration you can follow this [Blog Post](https://crownstack.github.io/blog/2017/11/06/fastlane.html) on our blog. If you are running *Windows* then you won’t be able to run fastlane on your machine as *it is not yet supported.*

If you’re on *Linux* (we are using *Ubuntu*), then here are the steps you can follow to get it working for you.

1. Install Ruby on your system as fastlane depends on it. You can install by using:

  - Add repositories for ruby:

	    `$ sudo apt-add-repository ppa:brightbox/ruby-ng`

	    `$ sudo apt-get update`

  - Install ruby:

	    `$ sudo apt-get install ruby2.3 ruby2.3-dev`

  - And you’re up and running:

	    `$ ruby2.3 -v`

2. Once your system have ruby installed, you can install fastlane with:

    `sudo gem install fastlane -NV`

3. After that set these environment variable in your system

    `export LC_ALL=en_US.UTF-8`

    `export LANG=en_US.UTF-8`

    You can find your shell profile at `~/.bashrc, ~/.bash_profile or ~/.zshrc` depending on your system.

4.  In this step you will install `Fastlane` for your project. Move to your project directory and run the following command.

    `fastlane init`

    When you’ll run these commands, you will be asked to confirm few things to get started.

        a. Provide the package name for your application when asked (e.g. io.fabric.yourapp)
        b. Press enter when asked for the path to your json secret file
        c. Answer 'n' when asked if you plan on uploading info to Google Play via fastlane (we can set this up later)

    That's it! fastlane will automatically generate a configuration for you based on the information provided.

5.  After step 4 you will see a fastlane directory in your project folder with two files:

	 - ***Appfile***  which defines configuration information that is global to your app
	 - ***Fastfile***  which defines the "lanes" that drive the behavior of fastlane

      Fastfile is the main configuration file and this is where you will define your configurations for automating your project deployment.

6. For deploying your app to ‘play store’ or other platform like ‘hockey app’, you need to have a *“json_key_file”* configuration file. You can obtain it by following these steps:
	- Open the **Google Play Console**
	- Select **Settings** tab, followed by the **API access** tab
	- Click the **Create New Project button**
	- Click the **Create Service Account** button and follow the **Google Developers Console** link in the dialog
	- Click the **Create Service account** button at the top of the developers console screen
	- Provide a name for the service account
	- Click **Select a role** and choose **Project > Service Account Actor**
	- Check the **Furnish a new private key** checkbox
	- Select **JSON** as the Key type
	- Click **Create** to close the dialog
	- Make a note of the file name of the JSON file downloaded to your computer,  Back on the Google Play developer console, click **Done** to close the dialog
	- Click on **Grant Access** for the newly added service account
	- Choose **Release Manager** from the **Role** dropdown
	- Click **Add user** to close the dialog

    Save this configuration file somewhere in your computer which is secure path.

    Once it is done, you are ready to proceed to the next step to automate the process.

7. Copy the file path with its name and save it in your “*Appfile*” under *fastlane* folder like the screenshot below:

    <img src="/static/google_file.png" alt="Drawing" style="width: 600px;"/>

8. Once the above file path is specified in the Appfile, now we need to define those automation commands in *Fastfile*.

    Here we are going to upload our app/build on “Hockey App” platform.

    - Open Fastfile and add the following code in it:

      <img src="/static/code.png" alt="Drawing" style="width: 600px;"/>

    Important things to notice in the code above:
    - *lane:* defines the name of this complete block of command which we will use to perform the entire automation.
    - *value:* this will increase the generated app version number automatically.
    - *badge:* this will add a badge on app icon and specifying the value of app version number on it.
    - *api_token:* put your hockey app api number here which will upload this app to the respective account automatically.
    - *notes:* here we have used a command, this command will pick last 100 commits from project github system (which means recent 100 changes made in this version of app) and will add it in build changelog.   
    - *end:* end of this block of code which was started with keyword ‘*lane*’.

9. In order to generate app icon with its badge on it we need to Install a plugin for generating ‘*badge*’ for an app. You need to run the below command to install it:

    `sudo gem install badge`

    If you are having earlier version of it and having any problem then you can uninstall it and reinstall it:

    `sudo gem uninstall badge`

10. After following and installing the above tool and instructions, you’re all set to run that one *magical command* that will automate a whole lot of things for you:

    `fastlane  hockey_build`

    here ‘*hockey_build*’ is the name of our lane (block of tasks).

From this point, all you need to do is to run this command to do these whole lot of things for you in a matter of seconds.

In case you need further help, don’t hesitate to drop us an email.

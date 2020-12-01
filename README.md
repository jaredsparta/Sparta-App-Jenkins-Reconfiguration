# Sparta CI/CD for sample app

- This repo will be a development environment that you can copy and setup by running `vagrant up`



<br>

# Pre-requisites

- Install `Oracle Virtual Box` [here](https://www.virtualbox.org/wiki/Downloads). This is the software that allows us to create virtual machines (VM).

- Install `Vagrant` [here](https://www.vagrantup.com/downloads.html). We use Vagrant to manage our virtual machines in Oracle VM.

- Once `Vagrant` is installed, you need the `vagrant-hostsupdater` plugin. 
> Run `vagrant plugin install vagrant-hostsupdater` to install it

> Run `vagrant plugin uninstall vagrant-hostsupdater` to uninstall it

<br>

- One will also need `Ruby` installed, find it [here](https://www.ruby-lang.org/en/downloads/).

- Once `Ruby` is installed, we will need the `bundler` 
> We can install it using the terminal command `gem install bundler`

<br>

- If you do not have `serverspec` and/or `rake` installed for Ruby, you will need it
> Navigate to `/tests` directory where a file called `Gemfile` should be and type in `bundle install`

<br>

# Instructions & Step-by-step

**Running the app and local tests**

1. You will need to first clone this repository into your own machine
2. In your terminal, navigate to the newly-made directory of this project
3. Type `vagrant up`
4. The app can now be seen in your web browser at the web address `development.local` or `192.168.10.100`
5. To run the tests, go into terminal and navigate to `/tests`
> If you do not have the necessary gems installed, run `bundle install` (otherwise you will not be able to run the tests)
6. Run `rake spec` to view whether the tests pass or not
> You can specify `rake spec:app` or `rake spec:db` to test the app and the database respectively on their own
7. There are also app-related tests you can run:
    - Navigate back into the main directory
    - `vagrant ssh` into the virtual machine
    - Run `cd /home/ubuntu/app`
    - Run `ps aux | grep node` and look for the id with the description `node app.js`
    - Run `kill id` with the relevant id
    - Finally, run `npm test`
8. After checking out everything, ensure you have a copy of this repository on GitHub
    - In other words, create a GitHub repository on your account
    - Delete `.git` from this project and `git init`
    - Follow the instructions found in GitHub to create a copy of this project on your profile with a new Git History

<br>

**First Job on Jenkins**

1. To build our CI pipeline, go into Jenkins and click `New Item`
2. Give the job a name and select `Freestyle project`

3. General
    - Give the name a logical description -- maybe what it will test etc.
    - Tick `Discard old builds` and set the `Max # of builds to keep` to 2 -- this will help the Jenkins server with capacity
    - Tick `GitHub project` and input the URL of the repository you have saved this on

4. Office 365 Connector
    - Add a notification webhook by going into the channel you want in Microsoft Teams
    - In the top right hand corner you will be able to click the three dots and select `Connectors`
    - Create a new webhook and name and it will give you a URL to paste
    - Paste the URL from Teams into the box in Jenkins labelled `URL` and give it a name
    - In the `Advanced` settings, specify which events you want notifications on

5. Source Code Management
    - Select the `Git` option
    - In the `Repository URL` input the URL found when you go to your GitHub repository, click on `Code` (found next to `Add file`) and copy the SSH URL
    - Copy the instructions found in [SSH-KEYS-AND-WEBHOOKS](https://github.com/jaredsparta/Sparta-App-Jenkins-Reconfiguration/blob/master/SSH-KEYS.md)
    - Specify the branch of your repo to build

6. Build Triggers
    - You want Jenkins to automatically build everytime you push some changes so tick `GitHub hook trigger for GITScm polling`

7. Build Environment
    - Tick `Provide Node & npm bin/folder to PATH`

8. Build
    - Add a build step with the `Execute shell` option
    - Inside it you want put the following commands:
    ```bash
    cd app
    npm install
    npm test
    ```
9. Finally `Save`

10. Now when you push changes from your local repo from the specified branch in step 5, Jenkins will automatically build and test your code 

<br>

**Second Job on Jenkins**
Now that we have tested the code, we can now add another step to make Jenkins automatically merge the branch from before into master/main and push master/main to GitHub

1. We will proceed to create another job so navigate to the Dashboard in Jenkins and click `New Item`
2. Give the job a name and select `Freestyle project`

3. General
    - Give the name a logical description -- maybe what it will test etc.
    - Tick `Discard old builds` and set the `Max # of builds to keep` to 2 -- this will help the Jenkins server with capacity

4. Office 365 Connector
    - Add a notification webhook by going into the channel you want in Microsoft Teams
    - In the top right hand corner you will be able to click the three dots and select `Connectors`
    - Create a new webhook and name and it will give you a URL to paste
    - Paste the URL from Teams into the box in Jenkins labelled `URL` and give it a name
    - In the `Advanced` settings, specify which events you want notifications on

5. Source Code Management
    - Select the `Git` option
    - In the `Repository URL` input the URL found when you go to your GitHub repository, click on `Code` (found next to `Add file`) and copy the SSH URL
    - From the first job, repeat the information given there
    - Specify the branch of your repo to build
    - In `Additional Behaviours` select `Merge before build`
    - In `Name of repository` put `origin`
    - In `Branch to merge to` put `master` or `main` -- depends on what you have called it

6. Build Triggers
    - You want Jenkins to merge and then push every time a build is successful so tick `Build after others projects are built`
    - In projects to watch put in the item name of your first job from above
    - Tick `Trigger only if build is stable`

7. Post-build Actions
    - From the drop-down of `Add post-build action` select `Git publisher`
    - Tick `Merge Results`

8. Finally save these options
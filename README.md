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
7. After checking out everything, ensure you have a copy of this repository on GitHub 

<br>

**First Job on Jenkins**

1. To build our CI pipeline, go into Jenkins and click `New Item`
2. Give the job a name and select `Freestyle project`

3. GENERAL
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
    - In 
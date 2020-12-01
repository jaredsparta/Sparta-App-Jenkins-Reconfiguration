# Sparta NodeJS App, DevEnv, CI

- This repo will be a development environment that you can copy and setup by running `vagrant up`

<br>

# Pre-requisites

- Install `Oracle Virtual Box` [here](https://www.virtualbox.org/wiki/Downloads). This is the software that allows us to create virtual machines (VM).

- Install `Vagrant` [here](https://www.vagrantup.com/downloads.html). We use Vagrant to manage our virtual machines in Oracle VM.

- Once `Vagrant` is installed, you need the `vagrant-hostsupdater` plugin. 
> Run `vagrant plugin install vagrant-hostsupdater` to install it

> Run `vagrant plugin uninstall vagrant-hostsupdater` to uninstall it

> There may be be some problems with this plugin, if you do encounter some just uninstall and then re-install

- One will also need `Ruby` installed, find it [here](https://www.ruby-lang.org/en/downloads/).

- Once `Ruby` is installed, we will need the `bundler` 
> We can install it using the terminal command `gem install bundler`

<br>

# Instructions & Step-by-step

1. You will need to first clone this repository into your own machine
2. In your terminal, navigate to the newly-made directory of this project
3. Type `vagrant up`
4. The app can now be seen in your web browser at the web address `development.local:3000` or `192.168.10.100:3000`

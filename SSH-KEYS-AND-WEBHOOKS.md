# Creating an SSH key

Note: These instructions assume you have linked your GitHub profile already with an SSH key and therefore have a `.ssh` directory already

<br>

1. Go into terminal and run `cd ~/.ssh` -- this will take you to the home directory
2. Now run `ssh-keygen -t ed25519`
    - When it asks you for a folder, input a logical name such as `jenkins-ssh-key` etc.
    - Don't include a username
    - Don't include a passphrase
3. Now in your GitHub profile, navigate to the settings and click on `SSH and GPG keys`
4. Click `New SSH key`
5. Back to your terminal, type `cat name.pub` where the `name` is the name of the key you have created
6. Copy and paste the result into `Key` in GitHub and give it a logical title such as `key-for-jenkins`
7. Now go into the Jenkins configuration and in `Source Code Management` click `Add` then `Jenkins`
8. Change `Type` to `SSH username with private key`
9. For `Description` change it to something logical such as `yourname-ssh-key`
10. Tick `Enter directly` in the `Private Key` option and click `Add`
11. Go back to your terminal and this time run `cat name` where name is the name of the key you recently created and paste it into the section
> Notice this is now the private key of the SSH unlike the public key used for GitHub
12. Leave everything else blank and press `Add`
13. Now select the recently created key in `Credentials`
14. You are now done with the SSH link to your GitHub profile

<br>

# Creating a webhook 

1. Go into your GitHub repository
2. Click `Settings` and navigate to `Webhooks`
3. Click `Add webhook`
4. In `Payload URL` input the address of your Jenkins server and append it so the ending is `/github-webhook/`
    - For example, `http://address:3000/github-webhook/`
5. (OPTIONAL) Change the `Content type` to `application/json` for a cleaner reading
6. Click `Send me everything` 
7. Finally click `Add webhook`
8. The webhook to the Jenkins server is now completed
# Git Basic Commands
git init

git config --global user.name "Harvey2504"

git config --global user.email "atibsamal96@gmail.com"

git config --global core.editor notepad

git config --global --list

mkdir git_samples

echo "new file content" >> test.txt

cat test.txt

notepad test.txt

git add filename

git add .

git status

git rm --cached filename (to unstage)

git log

git commit -m "meaningful message"

git checkout -b feature1

git branch feature1 (creates only)

git branch

git branch -a

git checkout master

git branch -m main (renamed)

git remote -v (check fetch and pull origin)

git remote add origin https://...

git push origin master

git pull https://

git pull origin master https://

git tag -a dinner

git tag dinner

git tag -a mytag -m "hye"

git tag

git fetch -all

git fetch <branch URL> <branch name>

git fetch <repo URL>

git fetch origin

git clone https://

git rebase mastergit log -1

ls -a :hidden files

ls .git/ :opens .git folder

git push -f

git show -ref master

git diff

git merge (checkout to master and merge)

git branch -d newfeature

git config --global help.autocorrect1 (fuzzy match command)

git add -u (only updated files)

git diff asdfg...sdrtgsxyh (SHA1's)

git diff HEAD~1....HEAD

git diff HEAD~1...

git reset --hard

git reset --soft HEAD~1 (wont remove changes,takes to last commit)

git clean -n (what?)

git clean -f (does)

notepad .gitignore (wont commit those types of files)

git commit -am "sharing" (just for updated files only)

git log --oneline

git log --online | wc -l (counts commits)

gir shortlog

git revert HEAD (also asks for message)

git push --tags

git branch -d bug1234

git branch -D bug1234

git stash

git stash list

git stash pop (removes)

git mergetool

git push origin : rmotebranchname (branch to be deleted)

git push origin v1.0 :  (creates a remote branch with same name)

git push origin v1.0 : v1.0fixes

git push origin --delete myExample

git remote add upstream https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web.git



# Pull Request
used to check before any changes are proposed to master

goto pull request tab

click on create a new pull request

base must be master branch and comapre with feature1

Then create a pull request

once its created u have to merge pull request if its cool

Then u will get this message:
```
Pull request successfully merged and closed
You’re all set—the feature1 branch can be safely deleted.
```
After this delete the feature branch

Note:
Always merge and delete the feature branches

# Merge Tool
* https://www.youtube.com/watch?v=1MVQYSlgXrI

It shows 3 panes

1st : whats changed in local

2nd : what was the base version

3rd : what was changed in the remote branch

And below it shows to edit the file 

In insert mode delete the lines with <<<<<<<<<HEAD & ======== & SHA1 numbers and resolve the conflict manually

Then exit the editor and 3 panes 

Then commit the same in the terminal

Then push to remote


# Webhooks
GitHub webhooks in Jenkins are used to trigger the build whenever a developer commits something to the master branch.


Let’s see how to add build a webhook in GitHub and then add this webhook in Jenkins.

* Go to your project repository.
* Go to "settings" in the right corner.
* Click on "webhooks."
* Click "Add webhooks."
* Write the Payload URL as
https://228b9f82.ngrok.io/github-webhook/
(tunnel link/github-webhook/)


Open ngrok
run the command with the port where jenkins is running :
```
ngrok http 8080
```
Copy the http:// part and paste it at the payload URL.

Remember the exposed tunnel is active till ngrok is active in background.

Here, Payload URL is the URL where our Jenkins is running add github-webhook to tell GitHub that it is a webhook.

Now go to the Jenkins pipeline and select "GitHub hook trigger for GITScm polling"

Content type: What kind of data we want in our webhook. I have selected JSON data.
Secret: Used to secure our webhook we can provide a secret in our webhook and ensure that only applications having this webhooks can use it.
SSL verification: This SSL Checker will help you diagnose problems with your SSL certificate installation. You can verify the SSL certificate on your web server to make sure it is correctly installed, valid, trusted and doesn’t give any errors to any of your users.
Which events would you like to trigger this webhook?

Just the push event: Only send data when someone pushed into my repository.
Send me everything: If there is any pull or push event in our repository we will get notified.
Let me select individual events: We can configure for what events we want our data.
Click Create and a webhook will be created.

Here https://228b9f82.ngrok.io/ is the port or IP where my Jenkins is running.

Here is a problem you have to take care of if you are running Jenkins on localhost then writing https://localhost:8080/github-webhook/ will not work because Webhooks can only work when they are exposed to the internet.

So if you want to make your  localhost:8080 expose to the internet then we can use tool

Write GitHub-webhook to the ngrok tool refer to this link.

### Some Links for Webhook

https://ngrok.com/download

https://ngrok.com/docs

https://dzone.com/articles/adding-a-github-webhook-in-your-jenkins-pipeline


# Invite Collaborator

Project > Settings > Manage Access > Invite Collab

A mail will be sent to collab .
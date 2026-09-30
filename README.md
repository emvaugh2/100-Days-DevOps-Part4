# 100 Days of DevOps with KodeKloud - Days 31 - 40


Greetings! Welcome back. We'll stick with this formatting right now for our challenge. We have a few more Git labs to finish up but after that, we'll be going into Docker almost until Day 50 so lets get that started. I'm pretty familiar with Docker at this point so I'm not worried about those. Lets knock out the next 10 days. 



## Day 40: Docker EXEC Operations
## Day 39: Create a Docker Image From Container
## Day 38: Pull Docker Image
## Day 37: Copy File to Docker Container
## Day 36: Deploy Nginx Container on Application Server
## Day 35: Install Docker Packages and Start Docker Service

Alright I'm going to go above and beyond and knock this lab out really quickly. I'm very familiar with Docker at this point. I've also used podman for the RHCSA exam so I know my way around the block a little bit. 

For this lab, we need to install docker-ce and docker compose. Then we need to start the docker service. Like with all services, I'll use some variation of `systemctl enable --now docker`. We already know how to install packages using dnf. Lets do that and see what results we get. 

UPDATE: I didnt get anything when I tried to download `docker-ce`. I asked AI where to find this package. I had to do a few different downloads. Here were the commands
- sudo dnf install -y dnf-plugins-core
- sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
- sudo dnf install -y docker-ce docker-ce-cli containerd.io

Then I just started and enabled docker. The docker compose package was automatically installed with the third command. Alright the dnf-plugins-core install allowed us to use the config-manager command. I'm actually just used to these commands already being built into the labs that I'm using. What does the config-manager do? Well in this case, it allowed us to add another repo to our default repos in CentOS. In Linux, you can create and add your own repos whenever you want. There's literally a file you can edit to do this. config-manager basically just does it for you. So we downloaded the Docker repo. My thing is, I didn't know you could add repos to these labs. I tried to do that recently and it didn't work. 

Docker CE - Docker community edition. This is the actual docker engine. To actually use this engine, you need the CLI associated with it, hence docker-ce-cli. Containerd is the worker and docker is the manager. Containerd talks to the kernel to manage the containers. It's the containers runtime. 

## Day 34: Git Hook

Our final Git task! Lets get this handled. So this is similar to a regular hook. A hook is basically when an event happens, it automatically triggers some type of job to run. So there are pre-commit and pre-push hooks for example. The pre-commit says before a commit happens, Git triggers a script. Same with push. This is mainly for automation. 

In the lab, we need to make a git hook called `post-update` so that whenever any changes are pushed to the master branch, it creates a release tag with name release-YYYY-MM-DD. I don't even know what a release tag is in the context of git. I also just used the man pages for git-hook and the examples are not clear. So I'm going to google this. 

I was told to run `git init` and create your file. The hooks are typically stored in /.git/hooks. I didn't know that. I'm seeing a bunch of files here already named pre-merge, post-update, pre-rebase, etc. So there's already a post-update.sample file. I'm going to go here and...well I cat'd the file. It says to enable this hook, rename this file to "post-update". So apparently I need to write a Bash script for this. Gotta love it. I think I'll just do a `echo "release-" and date +%F. I also don't know wha ta release tag is exactly so I'm going to lean on AI for my understanding of this moving forward. 

UPDATE: Apparently git tag is a whole different topic. So the hook needs to generate the tag name and then the git tag should be created. I also need to create the hook in the actual remote repo directories, not the local one. I changed directories over to there, fixed the file name, and made a script with `git tag release-$(date +%F)`. I kept the `exec git update-server-info` line in there as well. I also had to make the file executable using `chmod +x post-update`. Now lets test it out. You actually have to switch back to the remote repo directory to see the tag. I used the command `git --git-dir=/opt/games.git tag` to check. 

All of that worked! So the workflow is create the hook in the remote repo, push the changes in the local repo, check for the tags in the remote repo. 

NOW WE'RE DONE WITH GIT. HOW NICE. I'll review these labs and do them occassionally to keep my Git skips up to par. I have some Anki cards for review so that will help in the meantime. 



## Day 33: Resolve Git Merge Conflicts

Okay so the title is self explanatory. You can have two developers working on the same file and do a commit. Obviously this would cause some conflicts so we need to learn how to correct this in order to keep that application running. Alright for the lab, we first need to log into the storage server `ststor01` as the user max. They want us to push the changes from the story-blog repo to the origin repo which is `/sarah/story-blog.git`. I fixed the typo as requested using vi. I did a git add, commit, and push. I ran into an error message at this point. It's telling me to do a git pull before pushing again. I ran the pull request but I encountered a merge conflict. Once I cat'd the file again, I saw the `<<<<< HEAD` and `=====` lines in the file. So apparently everything below the HEAD line and above the === line is our local version. I went ahead and removed the lines Git added and made sure the file looked exactly how the lab wanted it to (4 story titles and the typo fixed). I then did add, commit, and push again. No error messages. I hit submit and everything worked perfectly. 

I'm going to ask AI to explain the file change markers for me though because I didn't understand that fully. 

UPDATE: Still a bit confusing but I think I need to see this in a few different contexts. I can at least see that there's a change in the file so that's good enough for me right now. 

## Day 32: Git Rebase

The catch up is almost over! So what is git rebase? Conceptually, git rebase takes your branch's commits and replays them on top of a newer base commit. Say you have a master branch that has commits A, B, and C. You can have a feature branch from the master branch that has A, B, and C as well. You can do commits D and E on the feature branch but say someone is working on the master branch and they do commits F, G, and H. If you do a rebase on your feature branch, Git will move your changes to after H. So the master branch will have A, B, C, F, G, H, D and E. This is to make sure the history looks clean. Think of it as a timeline. The master branch will see commits 1, 2, 3, 6, 7 and 8 in order although 4 and 5 came before the other ones but they were on another branch. So instead of trying to merge them based on their timeless, rebase just sticks those commits 4 and 5 at the end of the master branch's commits. 

Okay, lets get this lab started. It was pretty simple. We just needed to rebase our feature branch based on the master branch. I ran the command `git rebase master` on the feature branch. Then, I push those changes using `git push --force origin feature`. That was literally it. My biggest thing was the verification check. I didn't know what the initial environment was supposed to look like and then I couldn't see how it changed either besides the success message I received after I rebased my branch. I'll ask AI about this. 

UPDATE: So AI gave me the command `git log --oneline --graph --all` to see where the trees for the master and feature deviate. If you look at the output, you have `| /` which looks like a tree path. The | symbol shows a branch and the / shows another branch. 

Syntax explanation: `* c44700c (HEAD -> feature, origin/feature) Add new feature` means you're currently on the feature branch. HEAD points to the feature. You can see the tree branch when using the --graph command and it will show you which branches have which commits. Once you rebase your branch, it will show you the "new" commits on top of the master. After you push the changes, you're new --oneline output will show you the changes in the order that they were rebased in. That's how you can verify that it worked properly. 

Also, the --oneline flag just condenses the information from the log output. It makes it a bit easier to read. Think git log as the detailed version and git log --oneline as the summary. 

## Day 31: Git Stash

Alright lets kick off the first day! (Although I'm doing 3 labs today because I didn't get anything done this weekend). 

We're talking git stash. The concept (according to AI) is basically a temporary shelf for uncommitted work. Say you want to switch tasks without committing your unfinished work. I believe this is where git stash comes into play. 

For this lab, we have to go to the cloned git repo and restore some of the stash changes with stash@{1}. I don't really know what this means. I ran `sudo git stash` and it said no local changes to be made. I also gig gist status and log and I didn't see much information. I'm going to ask AI for some clear instructions. Once again, this will be a learning lab. Can't know how to do everything starting off. 

UPDATE: Okay we need to run `git stash list`. This showed me there's a stash for stash@{0} and stash@{1}. You can also use the commands `git stash show stash@{1}` and `git stash show -p stash@{1}` to see exactly what changes are affected by those particular stash. Also, the stashes can be by the same developer. Think of it more like a sticky note. You can think of the higher the number stash (for example, stash@{13}) is the most recent stash of that commit line. Okay cool. We'll definitely need to review this to keep it fresh. 

The command to actually restore the stash is `git stash apply stash@{#}` so for our cash, it will be {1}. Once you do that, this is essentially the same as you adding the file CHANGES back to your current branch. Not the commits. You'll still had to add the changes to the branch to be tracked officially using `git add .`, then `git commit -m "whatever message"` and then `git push origin master`. Make sure you do `git remote -v` to make sure you're pushing to the correct remote repo. 

That concludes this lab! Remember, stashes are uncommitted CHANGES, not commits. You can make a bunch of changes and if you just want to add (basically track) them later because you want to review them or you're unsure, you can stash them. Then, you can apply them when you're ready. It's saving the changes temporarily off to the side. 

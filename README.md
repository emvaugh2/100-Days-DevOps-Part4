# 100 Days of DevOps with KodeKloud - Days 31 - 40


Greetings! Welcome back. We'll stick with this formatting right now for our challenge. We have a few more Git labs to finish up but after that, we'll be going into Docker almost until Day 50 so lets get that started. I'm pretty familiar with Docker at this point so I'm not worried about those. Lets knock out the next 10 days. 



## Day 40: Docker EXEC Operations
## Day 39: Create a Docker Image From Container
## Day 38: Pull Docker Image
## Day 37: Copy File to Docker Container
## Day 36: Deploy Nginx Container on Application Server
## Day 35: Install Docker Packages and Start Docker Service
## Day 34: Git Hook
## Day 33: Resolve Git Merge Conflicts
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

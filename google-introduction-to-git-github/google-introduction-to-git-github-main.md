**‘Google: Introduction to Git and Github’**  
**Notes**  
Coursera

05/06/25											Thursday

**Differentiating b/w files:**  
diff tells difference between files  
example:  
5c5 on top means 5th line changed to something in 5th line  
11a13 indicates “added”  
\< means line removed  
\> means line added

\-u : provides better interface   
\++ means line added  
\-- means lined removed

more tools:  
wdiff, meld, KDiff3, vimdiff

**Creating .diff files:**  
diff \-u *\<old file\> \<new file\>* \> change.diff 

**Patching files with .diff files:**  
patch *\<old file\>* \< *\<diff file\>*

**VCS: (Or SCM)**   
Version Control System or Source Control Management lets you go back to the stable version of your program to control the errors and fixes from that point. It’s like checkpoints.   
Most popular example of vcs: Git. 

**Creating a repository:**  
Two ways:  
	git init \= initialises  
	git clone \= copies from remote location  
	

**Git Directory & Working Tree:**  
The git directory contains all the changes and their history and it acts as a database for all the changes tracked in Git.  
The working tree contains the current versions of the files and it acts as a sandbox where we can edit the current versions of the files.

**Adding file to staging area:**  
Staging area: The staging area which is also known as the index is a file maintained by Git that contains all of the information about what files and changes are going to go into your next commit.  
git add \<filename\>

**Checking status of staging area:**  
git status

**Commiting:**  
git commit  
This opens a text editor to add a description about our commit. 

git commit \-m \<commit message\>  
The m flag passes the commit message directly from the command to the editor, so no editor opens up specially. 

06/06/25												Friday

**Snapshots, Tracked & Untracked Files:**  
Snapshots: Each time you commit, git takes a snapshot of the current state of your project at that time. Combined, these snapshots make up the history of your project. 

Tracked files are part of snapshots and untracked files aren’t part of the snapshots yet, which is the usual case for new files.   
Each tracked file can be in one of three stages: Modified, Staged and Committed.  
File edited but not added using *git add*: Modified.  
File edited and added to staging area using *git add*: Staged.  
File edit, staged using *git add*, and committed using *git commit*: Committed.

**Anatomy of a Git Commit Message & Logging:**  
The first line is around 50 characters.  
Blank line.   
Next lines provide a detailed explanation of the changes, what’s interesting, anything difficult to understand. Each line should be within 72 characters. 

git log: shows the history of who committed, with a commit message, and a commit identifier. 

**Seeing git configs:**  
git config \-l

**Skipping staging for tracked files:**  
git commit \-a   
The \-a flag commits all the tracked files automatically without needing to stage them. This only works on files which are being tracked. So if you make a change or modify a file (which is already being tracked) then this flag allows git to commit without needing to “add” (using git add) them first. 

**HEAD:**  
Head is a bookmark showing the current snapshot of your project. 

**More information about changes \+ Patch flag:**  
git log \-p   
This flag shows an output similar to *diff \-u*.   
You can use page up, page down and arrow keys to navigate through the pages of the output. It basically shows associated patches.

git show \<id\>  
Uses the commit ID and displays information about that commit. 

git log \--stat   
Shows more details like what files were changed in the log.

git log \--graph  
Shows ASCII graph of commit and merge history.

git log \--oneline  
Prints each commit in a single line.

git diff   
Shows the changes to an unstaged file in a *diff \-u* type format.

git diff \--staged  
Shows the changes to a staged file.

git add \-p   
Shows the changes you’re about to add before committing. 

**Housekeeping, Renaming, Deleting, Ignoring:**  
git rm \<filename\>  
Removes the file. The change still needs to be committed though. 

git mv \<old name\> \<new name\>  
Renames the file. The change needs to be committed.

.gitignore   
This file contains the filenames or filename patterns you want git to ignore. 

08/06/25											Sunday

**Reverting changes:**  
git checkout \<filename\>   
Check out the latest file from the current snapshot.   
git checkout \<filename\> \-p   
The p flag asks you change by change if you want to go back to the previous snapshot or not. 

**Unstaging our changes:**  
git reset HEAD \<filename\>

git reset HEAD \<filename\> \-p  
Asks you which changes you want to reset. 

**Amending commits:**  
git commit \--amend   
Modifies the latest commit.  
Avoid amending commits that have already been made public.

**Rollbacks & Reverts:**  
git revert HEAD   
Reverts the latest commit by creating an anti-commit.   
git revert \<commid ID\>  
Reverts the specified commit by creating an anti-commit.

**Creating branches:**  
git branch  
Shows all the branches  
git branch \<branch name\>

**Using branches:**  
git checkout \<branch name\>

git checkout \-b \<new branch name\>  
Creates a new branch and switches to it in one line. 

git branch \-d \<branch name\>  
Deletes the branch.

**Merging:**  
Two types:  
Fast-forward  
Three-way merge 

git merge \<branch name\>

git merge \--abort  
Aborts and tries to go back to pre-merge state.

09/06/25											        Monday

**Cloning a github repo:**  
git clone \<https clone url\>  
To clone you need to enter your github username and password.  
The password needs to be your PAT (Personal Access Token) which is generated by you in “Developer Settings” in github.

**Avoiding entering the password everytime:**  
Two ways:  
1\. Caching the credentials (for 15 mins):  
git config \--global credential.helper cache  
Increasing the time limit  
git config \--global credential.helper ‘cache \--timeout=3600’  
Sets timeout to 3600s or 1 hour. 

2\. Using an SSH key pair.

**Pushing and Pulling:**  
git push  
Pushes the snapshots to the remote repo.  
git pull  
Pulls the contents from remote repo.

**Working with remotes:**  
git remote \-v  
Shows the current remote branches.  
git remote show \<assigned branch name (usually origin)\>  
Shows more details about the current remote branches

git branch \-r  
Shows which remote branch git is pointing to.

**Fetching data:**  
git fetch  
Fetches the data but doesn’t merge it with the local repo. If we’re happy with the changes, we merge the data.   
git pull   
Fetches the data and merges it with the local repo.

10/06/25											        Tuesday

**Pushing remote branches:**  
git push \-u origin \<branch name to create\>  
Creates a remote copy of your local branch which you pushed.

**Rebasing:**  
git rebase \<name of branch to rebase to\> 


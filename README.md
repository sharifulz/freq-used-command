--------------------- Git Basic Starts-----------------------------------
=========================================================================
Step 0. Working Dir, Stage Dir, Stash, Commit Dir, Push
Step 1. Git initialize, Create 2 Branch, Switch Branch
Step 2. Add file, edit file on branch1 , 3 commit, switch commit
Step 3. Add file, edit file on branch2 , 3 commit, switch commit
Step 4. Merge with main branch
Step 5. Merge specific commit (cherry pic)
Step 6. Git rebase, revert, reset
Step 7. Git Remote

Note: git revert
	What it does: Creates a new commit that undoes the changes made by a previous commit.
	Use when: You want to undo something without rewriting Git history (safe for shared/public branches).
	
	git revert <commit_hash>

Note: git rebase
	What it does: Moves or replays your commits onto another base commit (linearizes history).
	Use when: You want a cleaner, linear commit history or you're updating a feature branch.
	
git rebase main

git reset
	What it does: Moves the current branch pointer to a different commit. You can optionally change the working directory and staging area too.

	Types:

		--soft: Keeps changes in staging.

		--mixed: Keeps changes in working directory but unstaged.

		--hard: Discards all changes and resets everything to a past commit.

	git reset --soft HEAD~1    # Undo last commit, keep changes staged
	git reset --mixed HEAD~1   # Undo last commit, keep changes unstaged
	git reset --hard HEAD~1    # Undo last commit, discard changes

========================================================================================================================
Fork
Clone
PAT & SSH

--------------------- Git Basic Ends ------------------------------------
##########################################################################################################################


--------------------- Git clone using SSH Start----------------------------
===========================================================================

Step 1: 
Github Acc: shariful.w3@gmail.com
Gitlab Acc  shaarifulz@gmail.com	


Step 2: 
For GitHub; Account 1:
ssh-keygen -t rsa -b 4096 -C "shariful.w3@gmail.com" -f ~/.ssh/id_rsa_account1

For Gitlab; Account 2:
ssh-keygen -t rsa -b 4096 -C "shariful.w3@gmail.com" -f ~/.ssh/id_rsa_account2

For GitHub; Account 3: 
ssh-keygen -t rsa -b 4096 -C "shaarifulz@gmail.com" -f ~/.ssh/id_rsa_account3

Step 3: 
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_account1
ssh-add ~/.ssh/id_rsa_account2

Step 4: Add the ssh key on git remote platforms (GitHub-acc1, GitLab-acc2, GitHub-acc3)

Step 5: Update the ssh config file:

nano ~/.ssh/config

# GitHub Account 1
Host github.com-account1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_account1
  IdentitiesOnly yes

# GitLab Account 2
Host github.com-account2
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_account2
  IdentitiesOnly yes

# GitHub Account 3
Host github.com-account3
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_account3
  IdentitiesOnly yes

*Note: If you dont have config in .ssh directory the create one:
mkdir -p ~/.ssh
touch ~/.ssh/config
chmod 600 ~/.ssh/config


Step 6: Clone any private or public repo

git clone git@<<Host mentioned in  ssh config file>>:shariful-w3/microservices-config.git

#github
git clone git@github.com-account1:shariful-w3/microservices-config.git
#gitlab
git clone git@gitlab.com-account2:shaarifulz/portableportal.git
#github
git clone git@github.com-account3:sharifulz/python-practice-problems.git

--------------------- Git clone using SSH End ------------------------------
git@gitlab.com:shaarifulz/devops1.git
##########################################################################################################################

git clone git@gitlab.com-account2:shariful-w3/DevOps1.git


##########################################################################################################################

git config --global init.defaultBranch
git config --global init.defaultBranch main

git log --all --oneline

git checkout br1
git cherry-pick ae5c3e9
git merge br1

Some more commands:

Pushing local code to remote repo:
Step 1: Go to local working directory

Step 2: 
git init
git add .
git commit -m "first commit"

Step 3: Add remote repo, using ssh configurations
git remote set-url origin git@github.com-account1:shariful-w3/DevOps1Jenkins.git

git remote add origin git@github.com-account1:shariful-w3/DevOps1Jenkins.git

git remote set-url origin git@gitlab.com-account2:shaarifulz/devops1.git

git remote set-url origin git@github.com-account1:shariful-w3/office-man-tool.git

git@gitlab.com:shaarifulz/devops1.git
Step 4 : Push
git push -u origin main


.ssh/conf file:

# GitHub Account 1
Host github.com-account1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_account1
  IdentitiesOnly yes

# GitLab Account 2
Host gitlab.com-account2
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_account2
  IdentitiesOnly yes

# GitHub Account 3
Host github.com-account3
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_account3
  IdentitiesOnly yes






--------------------- Git Hooks Start --------------------------------------
============================================================================
Step 1: 
cd your-springboot-repo/ // go to project directory
Create a Java file with some code

public class HellowWorld {
    public static void main(String[] args) {
        System.out.println("Hello Work");
    }
}


Step 2: Create a pre-commit file
touch .git/hooks/pre-commit


Step 3: Make the pre-commit file executable
chmod +x .git/hooks/pre-commit


Step 4: nano the pre-commit file and use validation rules like below.
Below code will check if any syout exist in any java file even in comment mode. If exist then it will not let you proceed to commit,

#!/bin/bash

echo "🔍 Running pre-commit checks..."

# Find staged Java files
files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.java$')

for file in $files; do
  if grep -q "System.out.println" "$file"; then
    echo "🚫 Error: 'System.out.println' found in $file"
    echo "Please remove debug statements before committing."
    exit 1
  fi
done

echo "✅ Pre-commit checks passed."
exit 0

Step 5: Try commit in the repo


More validations:
1. pre-commit
	a. pre-commit: Disallow TODOs or FIXME in committed code

		grep -rnw 'src/' -e 'TODO\|FIXME' && echo "Remove TODOs before commit!" && exit 1

	b. pre-commit: Prevent large files from being committed

		find . -size +5M | grep -q . && echo "Too large files found!" && exit 1

2. commit-msg
	a. Enforce commit message format (like Jira ticket or Conventional Commits):
	
# Inside .git/hooks/commit-msg
pattern="^(feat|fix|docs|style|refactor|test|chore)\([a-z]+\): .{10,}$"
if ! grep -qE "$pattern" "$1"; then
  echo "❌ Commit message format is invalid!"
  exit 1
fi



--------------------- Git Hooks End--------------------------------------
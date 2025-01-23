- Git is an version control system.
- GitHub is a online tool which uses the power to GitHub to create a collaborative workspace for open and closed source
applications.

# Git commands

```bash
git init
git add <file name> or 
git add .
git commit -m "<name of the commit>"    //Use this command to register staging changes to the commit
git remote add origin <repo>
git push -u origin master         //for first time

git pull origin master            //pull all the latest commit from the github
git push origin master

git commit --amend                // to uncommit a commit
git rm --cached <filename>        //to unadd file or to bring back whole file to the untracked area
git restore --cached <filename>   //to bring back only the new changes made in the file to the untracked area

git status
git log                    //Lists down all the commits in the repository
ls -a
ls                         //shows folders inside
esc :wq enter              //to exit git prompt terminal
cd <folder name>
cd ..                      //to get back to previous folder
git cat-file -t <hash>     //shows the type of the hash
git cat-file -p <hash>     //shows the content of the hash

rm -rf <file path>         //to delete file
rm rf .git                 //delete project from git
cat <file name>            //it shows the content of the file
touch <file name>          //it creates a file named filename
mkdir <folder name>        //it creates a folder named foldername
git remote                 //Lists down all the remote connection names
git remote -v
git remote remove origin       //removes the origin
git remote rename <old-name> <new-name>   //This renames the remote connection. Note - The name of the remote connection is always used to establish a connection between the two repos.

git checkout -b <branchname>   //creates a new branch
git checkout <branchname>      //to switch between branchs
git merge <branchname>         //to merge branch to master/main
git clone <repo>       
```

# Recommended Practice to do -
  - make changes
  - git add <file>
  - git commit
  - git pull
  - git push

# Different Areas in Git
- Working area -> Files which are not being currently handled by git are in the working area.It means changes done to these files are not managed by git. When on using the git status command if there are untracked files then these are the files currently in the working area.

- Staging area -> Files in the staging area represent what all files are going to be the part of the next version. The staging area is the place were git knows what all changes are going to be done in the previous version to the next version.

- Repository area -> This area contains all the details of the previously registered versions. Git knows and manages the version history of all the files in this area.

- Difference between rm and restore

- git rm is used to move back the entire file to an untracked state whereas git restore is used to move the files from working and staging area.

- Merge conflicts are very common scenario. Merge conflicts can occur if multiple people try to make changes to the same file and try to collaborate.

# How Git is Internally Works
- Git is heavily dependent on Hashing and Tree/Graph data structures for its working.
- Git is like key value store
- Key - Hash of the data
- Value - Data Git used a cryptographic hashing function SHA-1 (Secure Hashing Algorithm ) which for a given data outputs a 40 digit Hexadecimal number which remains same for given set of data.
- After this Git compresses the data in form of blob and stores some meta data about it. Note - BLOB - It stand for Big Large Binary Object. Inside the block it stores the characters size, delimiters and the actual data.

- Inside the .git folder all the files added to the git are stored inside the objects folder in form of key value pairs wherein the first 2 digits of the hash are the folder name whereas the last 38 digits are value of key value pair (filename).

- `tree .git ->` Used this command to visualise the entire .git folder structure.

- `git cat-file -p < entire hash value >` -> Use this command to print the contents of the files added to git with the respective hash value.

- The directed tree structure of Git stores the data in the following manner -

    - It uses the hash as pointers to store
    - To store BLOBs
    - Also internal trees in maintained.
    - Also stores the meta-data
    - Type of meta-data
    - Directory name or filename
    - Mode of file(.exe, .txt etc.) Note - It also manages the directory structures as these points can point to - different trees and blobs as well.

# Branches
Branch is just a pointer to a particular commit.

- `git checkout -b "name of the new branch"` -> Creates a new branch with the given name
- `git checkout "name of the branch"` - > Used to switch to the entered branch.
- `git log --all --decorate --oneline --graph` -> To get a visual representation of all the branches.
- `git log --graph` -> To get the logs in form of a graph view.
- `git branch` -> Shows a list of the branches present where * symbol represents the current branch.


- Merging Branches

- `git merge <branch name>` -> This merges the current branch with the mentioned branch in the command.

- `git switch <branch-name>` -> This command is also used to change the branch apart from the git checkout command.

- `git rebase <branch-name to be merged>` -> Rebase command makes a copy of the branch to be merged and then attached it to the head of the master branch and moves the branch pointer to it.

- `git rebase --interactive <name of the branch to which this branch has to be added>` - > On using this command an interactive mode in vim opens up where all the commits of the branch to be squashed are listed.
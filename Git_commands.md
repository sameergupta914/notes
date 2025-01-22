# Git commands

```bash
git init
git add <file name> or 
git add .
git commit -m "<name of the commit>"
git remote add origin <repo>
git push -u origin master         //for first time

git pull origin master            //pull all the latest commit from the github
git push origin master

git commit --amend                // to uncommit a commit
git rm --cached <filename>        //to unadd file or to bring back whole file to the untracked area
git restore --cached <filename>   //to bring back only the new changes made in the file to the untracked area

git status
git log
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
git remote -v
git remote remove origin       //removes the origi

git checkout -b <branchname>   //creates a new branch
git checkout <branchname>      //to switch between branchs
git merge <branchname>         //to merge branch to master/main
git clone <repo>       
```
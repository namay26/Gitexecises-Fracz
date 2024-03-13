### 1.
> The first exercise is to push a commit that is created when you run the git start command. Just try git verify after you have initialized the exercises and be proud of passing the first one.
---
Here you have to verify your commit using `git verify`.

### 2.
> There are two files created in the root project directory - A.txt and B.txt. The goal is to commit only one of them.
---
We have to add only one file to the staging area so that we are able to push it. For this we can use the `git add` command to add it to staging area and then `git commit -m` to push the added files.  

    $ git add A.txt
    $ git commit -m "Add A.txt"
    $ git verify

### 3.
> There are two files created in the root project directory - A.txt and B.txt. They are both added to the staging area. The goal is to commit only one of them.
---
We firstly have to reset the staging area using `git reset`, then add A.txt using `git add` and finally commit it.
    
    $ git reset
    $ git add A.txt
    $ git commit -m "Add file"

### 4.
> It is often good idea to tell Git which files it should track and which it should not. Developers almost always do not want to include generated files, compiled code or libraries into their project history. Your task is to create and commit configuration that would ignore:  
  all files with exe extension  
  all files with o extension  
  all files with jar extension  
  the whole libraries directory  
---
We have to create a .gitignore file which can be done by using `touch` command and then we hav to add the conditions to it. After this, we must push it.

    $ touch .gitignore
    $ nano .gitignore
      *.exe
      *.o
      *.jar
      libraries/
    $ git add .gitignore
    $ git commit -m "Add .gitignore"

### 5.
> You are currently on chase-branch branch. There is also escaped branch that has two more commits. You want to make chase-branch to point to the same commit as the escaped branch.
---
You have to merge the escaped branch.

    $ git merge escaped

### 6.
> Merge conflict appears when you change the same part of the same file differently in the two branches you're merging together. Conflicts require developer to solve them by hand.
---
When we will try to directly merge the branch, we will get the following output.

    $ git merge another-piece-of-work
    Auto-merging equation.txt
    CONFLICT (content): Merge conflict in equation.txt
    Automatic merge failed; fix conflicts and then commit the result.

Hence, we need to solve the conflict by hand.
    
    $ nano equation.txt

Edit the text file by removing the conflict dividers and then writing what you wish to have in your commit.

    $ git add equation.txt
    $ git commit -m "merge"

### 7.
> You are working hard on a regular issue while your boss comes in and wants you to fix a bug. State of your current working area is a total mess so you don't feel comfortable with making a commit now. However, you need to fix the found bug ASAP.
  Git lets you to save your work on a side and continue it later. Find appropriate Git tool and use it to handle the situation appropriately.
  Look for a bug to remove in bug.txt.
  After you commit the bugfix, get back to your work. Finish it by adding a new line to bug.txt
---
Firstly you need to use `git stash` to save your work. Then look for bug.txt, open it with `nano` and remove the bug, add it to the working stage and commit it. After that you need to use `git stash pop` to get you working space again with the applied changes and then use `nano` to complete your work. Stage the file again and commit it.

    $ git stash
    $ nano bug.txt
      (Remove the BUG Line)
    $ git add .
    $ git commit -m "Remove bug"
    $ git stash pop
    $ nano bug.txt
      (Finish your work)
    $ git add .
    $ git commit -m "Work completed"

### 8.
> You were working on a regular issue while your boss came in and told you to fix recent bug in an application. Because your work on the issue hasn't been done yet, you decided to go back where you started and do a bug fix there.
---
We need to use the `git rebase` command which allows us to have linear branch by applying the commits to the tip of the base branch.

    $ git rebase hot-bugfix

### 9.
> File ignored.txt is ignored by rule in .gitignore but is tracked because it had been added before the ignoring rule was introduced.
  Remove it so changes in ignored.txt file are not tracked anymore.
---
We need to use `git rm` to remove the ignored.txt file.

    $ git rm ignored.txt
    $ git commit -m "Remove ignored.txt"

### 10.
> You have committed a File.txt but then you realized the filename should be all lowercase: file.txt. Change the filename.
  This one is tricky on Windows, or in any filesystem that treats File.txt and file.txt as the same files.
---
For this we need to change the name of the file. This can be done using `git mv` command.

    $ git mv File.txt file.txt

### 11.
> You have committed file.txt but you realized you made a typo - you wrote wordl instead of world.
  Edit previous commit so no one would realize you haven't checked the file before committing it.
  Pay attention to the commit message, too!
---
In this excercise, we are required to fix the typo. You need to firstly do it locally using `nano` then you need to add the text file to the working stage. Following this, you must use `git commit --amend` so that the previous commit is amended. Make sure to fix the typo in the commit message too ;)

    $ nano file.txt
      (fix the typo)
    $ git add .
    $ git commit --amend -m "Add Hello World"

### 12.
> You should have finished your work a week ago. However, you had some more important things to do so you have committed the work just now.
  As a git expert, change the date of the last commit. Don't be modest - make it look like it was committed in 1987!
---
Dates of a commit can be changed by using `--date` flag in `git commit --amend` command.

    $ git commit --amend --date="1987"

### 13.
> While you were working you noticed a typographic error in file.txt - you wrote wordl instead of world.
  Unfortunately, you have made another commit on top of the typo so simple git commit --amend is not enough.
  Fix the typographic error by amending commit in history. Pay attention to the commit message, too!
---
A relatively difficult exercise. What I did here was to eit the typo in `file.txt` first using `nano`. Then I added the file on the work space and made a commit. Now if we do `git log` we see:

    pick d64a712 Add Hello wordl
    pick 768dce9 Further work on Hello world
    pick c21a554 Add Hello world

What we want to do is merge the last commit with the one with the typo. For this we can use the command

    $ git rebase -i HEAD~3

and then change the log of commits as

    pick d64a712 Add Hello wordl
    f c21a554 Add Hello world
    pick 768dce9 Further work on Hello world

This way when the rebase is complete, the typo is fixed. But we run upon the following merge conflict.

    $ git rebase -i HEAD~3
    Auto-merging file.txt
    CONFLICT (content): Merge conflict in file.txt
    error: could not apply 3ea038d... Add Hello world
    hint: Resolve all conflicts manually, mark them as resolved with
    hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
    hint: You can instead skip this commit: run "git rebase --skip".
    hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
    Could not apply 3ea038d... Add Hello world

To fix this, we must sort the conflict. We do `nano` and fix the conflict manually.

    $ nano file.txt

After fixing the conflict, we must add the fixed file to the working space. 

    $ git add .

And then, we must continue the rebase.

    $ git rebase --continue

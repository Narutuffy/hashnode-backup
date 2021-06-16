## Strange Git got Goods, yep my git snippet's


Coming from a SVN versioning system to GIT has been a strange trip,  at first SVN commands made more sense like checkout and update but then when you think about it there kinda backwards.  Where as with Git , I'm noticing it's more like *nix cli commands to deal with files, commits, etc.  Some examples, ( btw this article is for my personal memory, if it helps you awesome. )

 - Remove a file

~~~

#Note pass -f to remove folder
git rm filename

~~~


 - Rename / Move a file

~~~

git mv filename

~~~
    
 - Add File or Files for Commit

~~~

    git add ./
    git add filename


~~~

 - Commit with message

    
~~~

    git commit -m ""

    # Better commit but know your editor it normally opens VIM, NANO, or whatever you setup
    git commit

~~~
    
 - Pull From Server
~~~

    git pull

    git pull remote branchname

~~~


 - Pull Branch From Server

~~~

    #example git checkout -b < new_branch > origin/< new_branch >
    git checkout -b newbranch origin/newbranch

~~~
     
 - Push to server, replace master with branch


~~~

    git push origin master
    git push remote branch

~~~
    
 - Switch branches

~~~

    git checkout branchname

~~~  

 - Create a tag

~~~
      
  git tag tagname
  git push origin :refs/tags/tagname

~~~
    
 - Push a tag

~~~
      
git push --tags

~~~

 - Delete a tag

This one had me for a file, strange one.

~~~
      
    git tag -d tagname
    git push origin :refs/tags/tagname


~~~
        
 - Adding Remote Repo
~~~

     git remote add **alias** **url**
    
    #Example
     git remote add upstream git://github.com/octocat/Spoon-Knife.git
     git fetch upstream
     git merge upstream/develop
      

~~~


There's more but that'll do for thist post.

Highly suggest learning git-flow
## Intro to Git and Github

### Git Config

Git is a local program for performing version control which is usually paired with a remote Git server for saving code off site. GitHub is a web-based Git repository which your local Git program is capable of interfacing with. Git and GitHub are therefore two seperate systems and Git needs to be configured correctly in order for your code changes to be tracked and attributed correctly on GitHub. You should run the following commands on any new system you are committing from before you start working (these can be run from any directory and only have to be run once per system):

```sh
git config --global user.name "<github-username>"
git config --global user.email <github-registered-email>
```

GitHub will use the email that you configure with your Git client to track which users are making which changes. This means that you'll need to use the email address you've registered with GitHub in the above configurations (otherwise you may see commits from an anonymous user). In this course we look at commits and who made them to make sure partners in the projects and labs are contributing equally, so having misattributed commits may lead to point deductions. If you have a few which are misattribued this is not an issue but you should configure your client quickly after you notice the problem to correct the issue.


### Git Init & Clone

New Git repositories can be created either locally using the Git client or through GitHub directly. Make sure you are back at your home directory (`cd ~`) and run the following commands on the terminal in `hammer`:

```sh
mkdir lab-01
cd lab-01
```

Then to initialize that directory as a Git project, use the following Git command:

```sh
git init
```

This will create a hidden `.git` repository (which you can see with `ls -a`) that holds all the information that Git uses to keep track of your files and changes. The folder is hidden because it begins with a period (recall earlier in the lab) and is typically not modified by users directly.

However, you use a different method if you want to receive a copy of an already existing repository, which you will do for all of the labs and assignments in this course. Rather than initializing a new repository, you will make a “clone” of a repository that already exists. Start by moving out of the Git project you just created and removing that directory:

```sh
cd ..
rm -rf lab-01
```

Now, go to the upper right of this page and click the “Code” dropdown. A box will appear that should say “Clone” and the HTTPS tab should be selected with an HTTPS link displayed (if it says “Clone with SSH”, then be careful to click the tab to the left that says “HTTPS”).

Copy the link in the box below, this is the GitHub repository url which you will use to clone that repository. Now, run the following command:

```sh
git clone <github-url>
```

Make sure to replace the above `<github-url>` with the url that you copied from the “Clone or download” box. This will create a new folder named `lab-01-intro-to-sct-...` with some additional text based on your username/groupname. This new directory is a copy of the GitHub repository, and is already initialized as a Git project. Move into this new directory and we can begin modifying it.

> Note: README.md files are special in GitHub repositories. The contents of the README.md file in the repository's root will actually be rendered along with a list of files for anyone who visits the repository. The hash (#) at the beginning of the line is part of GitHub Markdown which specifies that this line should be a title. [You can read more about GitHub Markdown here](https://guides.github.com/features/mastering-markdown/).

### Git Status, Add & Commit

Remember our program and its source/header files from earlier? Go ahead and move all of those files (including src/header files) into the new `lab-01-intro-to-sct-...` folder.

Git doesn’t automatically keep track of new files for us. Instead, we have to tell Git to start tracking these new files. Run the following command:

```sh
git status
```

The `status` command lists the current status of your Git repository, mostly showing whcih files have changes. In the output, there is a section labeled "untracked files." Notice the files in that section. This means that Git knows these files exist, but isn’t currently keeping track of changes to them. We want to keep track of all the `.cpp` `.hpp` files and `CMakeLists.txt`, but not the `area_calculator` file since that should be recompiled to run correctly on different machines. It is important to note that Git does not automatically save changes to your files either locally or on GitHub. When you have made a set of changes that you want to save, you will have to use the commands we are going to introduce below so you will use these commands very often.

We don’t want Git to continue to tell us that `area_calculator` is untracked, but luckily Git has a solution for this problem. You can create a file called `.gitignore` that will contain a list of all of the files that you want Git to ignore when it tells you what is/isn’t tracked and modified. To ignore a file, add the name of that file on a new line of `.gitignore`. Now create a file named `.gitignore` and add `area_calculator` to it. Now run `git status` again and take a look at the output. Notice that `area_calculator` is no longer listed, only the `.cpp`, `.hpp`, `CMakeLists.txt` and the new `.gitignore` files are listed as untracked. Now, we can add all of these files to our project with the following commands:

```sh
git add header/rectangle.hpp
git add src/rectangle.cpp
git add src/main1.cpp
git add src/new_main.cpp
git add CMakeLists.txt
git add .gitignore
```

> Note: Do not add executables, object files, or temporary files which are re-generated during compilation to your git repo, only add source files and other necessary files for your submission (these are listed in the assignment specifications). Tracking executables uses LOTS of disk space and they are unlikely to work on other peoples machines. **If we see these files in your Git repos, your grade on the assignment will be docked 20%**. You should use a `.gitignore` file so that they don’t appear in your Git status or accidentally get added to your repository.  The `.gitignore` file supports wildcards, so that the entry `*.o` will cause all object files to be ignored.

Now, when we run Git status, there is a section labeled "Changes to be committed" with the files that were just added underneath it. This means that Git now thinks these files are part of the project, and will begin to track changes to it.

Whenever we finish a task in our repo, we "commit" our changes. This tells Git to save the current state of the repo, made up of all the files we’ve added, so that we can come back to it later if we need to. Commit your changes using the command:

```sh
git commit -m "Add initial files"
```

Every commit needs a "commit message" that describes what changes were made in the repo. Writing clear, succinct, informative commit messages is one of the keys to using Git effectively. In this case we passed the `-m` flag to Git so we could write the commit message in the command line. If we did not pass a flag, then Git would have opened the Vim editor for us to type a longer commit message which is useful when you are commiting more major changes which require more explanation.

That was not a very informative commit message, so lets edit it to be something more informative. Anytime you need to modify the last commit that you made, you need to amend it, which is done through the following command:

```sh
git commit --amend
```

Running this allows you to edit the last commit that you made, including which files were included in that commit. Anything that you have done a `git add` to before running `git commit --amend` will be added to the last commit in addition to allowing you to edit the commit message.

> Note: the --amend flag will let you add anything to the previous commit, even if it's not a good idea. Because of this you should use it carefully and for situations where you have forgotten a minor change (README, comment, better commit message) or your last commit did not actually function correctly and you need to “patch” it.

We didn’t write an informative commit message, so we should modify it to be something more useful. Run the `git commit --amend` command. You should see something like the following open in either Vim or Emacs.

```
My first commit
  
# Please enter the commit message for your changes. Lines starting
# with ‘#' will be ignored, and an empty message aborts the commit.
#
# On branch brrcrites/lab-01
# Changes to be committed:
#   added:   header/rectangle.hpp
#   added:   src/rectangle.cpp
#   added:   src/main1.cpp
#   added:   .gitignore
#
# Changes not staged for commit:
#
# Untracked files:
#
```

While you can write any message that you want here, GitHub will do some automatic formatting based on past good practices for writing commit messages that you should adhere to. 

* The first line will be formatted as a subject line, and cut off at 50 characters. You should therefore pick a concise subject message that is < 50 characters long 
* You should separate the first line from the rest of your body text with a blank line
* The body of your message (anything else you add after the subject but before the commented lines) should describe what changes occurred and why, rather than how you did it. This is often formatted as a bulleted list using a * as the bullet.

Following these rules will make your commits nicely formatted on GitHub and easier to understand and process. Lets update our commit message to the following:

```
Add rectangle area program

* Added Rectangle class files and main file that calculates the rectangle's area
  
# Please enter the commit message for your changes. Lines starting
# with ‘#' will be ignored, and an empty message aborts the commit.
#
# On branch brrcrites/lab-01
# Changes to be committed:
#   added:   header/rectangle.hpp
#   added:   src/rectangle.cpp
#   added:   src/main1.cpp
#   added:   .gitignore
#
# Changes not staged for commit:
#
# Untracked files:
#
```

After editing the file with the above message, exit your editor and the commit should update.

> Note: The -m flag should only be used when writing very short commit messages, and otherwise you should use the interactive mode to have both a subject and commit body. For some more tips on writing effective git commit messages, read [this blog post](https://chris.beams.io/posts/git-commit/).

> Tip: There are many useful shortcuts that can be used to speed up common workflows.  Multiple files can be added at once: `git add file1 file2 file3`.  You can also specify files to commit directly: `git commit file1 file2 file3`.  You can commit all modified files with `git commit -a`, but be very careful to check first that you actually want to commit all modified files in one commit.

Let's make one more commit so we'll have something to play with. Update your `main1.cpp` to calculate one more rectangle's area:

```c++
#include <iostream>
#include "../header/rectangle.hpp"

using namespace std;

int main()
{
    Rectangle rect1, rect2;
    rect1.set_width(3);
    rect1.set_height(4);

    rect2.set_width(4);
    rect2.set_height(2);

    cout << "Rectangle 1 area: " << rect1.area() << endl;
    cout << "Rectangle 2 area: " << rect2.area() << endl;

    return 0;
}
```

Now run:

```sh
git commit -m "Add one more rectangle and compute its area"
```

Uh oh! We got an error message saying: "no changes added to the commit".

Every time you modify a file, you must explicitly tell Git to add the file again if you want that file included in the commit. This is because sometimes programmers want to commit only some of the modified files. 

Run `git status` to see the files that are currently being tracked. We can add the changes to the `main1.cpp` file, and verify it is added, with:

```sh
$ git add main1.cpp
$ git status
```

We can then commit the changes using:

```sh
$ git commit -m "Add one more rectangle and compute its area"
```

> Note: `git add` is used for several *different* tasks, such as telling git to start tracking a new file, "staging" a modified file for a subsequent commit, or signifying that a conflict has been resolved (we will visit conflicts later).  `git rm` and `git mv` can be used to remove or move files; they work the same way as the regular `rm` and `mv` commands, but they also tell git to make the corresponding changes to the repository.

### Git Push & Pull

While git is a version control system (VCS), GitHub is a remote repository which is an important distinction for two reasons. The first is that up until now all the work you’ve done has only been saved locally, so if there is a problem with your computer you would have no backup and therefore no way to recover the files. The second is that because all the changes are local, there is no way for people collaborating with you to see or receive your changes. Go to your GitHub repository for this lab, and you should see that none of the work you've done is listed.

Since we cloned the remote repository from GitHub directly, our local repository is already associated with a remote repository (usually referred to as “remote” or “upstream”). In order to send the changes we’ve made locally to GitHub, we just need to “push” them up to the server (do this now).

```sh
$ git push
```

This will push all the commits you've made since your last push assuming there haven't been any changes to the remote GitHub repo that your local Git doesn't know about. If there have been you will need to "pull" and "merge" those changes into your local repository, but we will cover those steps in a future lab when we cover Git and GitHub in more depth.



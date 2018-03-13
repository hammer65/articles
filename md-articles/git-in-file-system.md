# Using GIT Within a File System

You can use git without using github. If you need privacy or if you aren't ready to share your creation you can still use the versioning features. You can store your main repository on a flash drive and take it wherever you want.

You then push your commits to it just as if it were a server. Here is how. Go to your flash drive with the command line and create a bare repository. Not like you normally would however you would include the `--bare` flag

```bash
git init --bare myproject.git
```

*"myproject" will be the name of your repo*

Then with your terminal go to the directory where you want to work on the project. if it isn't a git repository already make it one by typing

```bash
git init
```

Then hook it up to your origin (you can call it something other than origin) repository which is on your flash drive or other stoage device.

```bash
git remote add origin /path/to/your/git/file/myproject.git
```

You can then push to this repo.

```bash
git push origin master
```
or if you are working on a different branch from the master branch
```bash
git push origin mybranch
```

You can work with the repo any way you would work with any other repo. In fact it works very much like this when using a remote repo as well. Say over SSH

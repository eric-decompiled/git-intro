# Introduction to Git

Repo: (https://github.com/eric-decompiled/git-intro)

## What is Git?
  Version control, used to manage code basis. Alternatives exist, but git is by far, the most popular
## Why use Git?
  Lets you have a historical record of how the code base evolved, and collaborate with others
## Installing Git
 Depends on your OS:
 [Instructions here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Configuring Git

- Set user name + email
  `git config --global user.name`
  `git config --global user.email`

(These can be overrode per repo by leaving out `--global`)

- Set editor, should default to something though
  `git config --global core.editor vim`

`git config --list` shows current config

## (Optional) set up some shortcuts
```sh
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.sw switch
```

[Reference](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)

## Git Basics

- Navigate to a directory you want to version control
- Initialize a new git repository with: `git init`
- This creates a `.git` folder that will store the information git needs to work.
- Note: Make sure you are not already in a git directory (Does `git status` work?). Nested git repos should be avoided

## Basic commands
  Commands for basic usage

  `git status`           See what files have changed
  `git diff`             Show file changes from previous change
  `git add -a`           Add all files in "staging" area
  `git add src/someFile` You can be selective in what being changed
  `git add src/*`        `*` acts as a wildcard to add everything in `src/`
  `git diff --staged`    I almost always run this before commit
  `git commit`           Write staging area to the history

(demo/work along)

## ⚠️ WARNING: NEVER COMMIT CREDENTIALS!

- If you do accidentally commit, rotate the credential. Git keeps a "ref log" of everything, it can be difficult to truly remove

## Ignoring files
Make a `.gitignore` file, put dev secrets in a `.env`. Add `.env` to `.gitignore`

Each project should have a `.gitignore` which contains:

- Ignore `.env`,
- library files (.e.g `node_modules`)
- build output


e.g.
```
.env
node_modules/
dist/
```

### with dotenv

dotenv is a library implemented for most languages that will auto load `.env` variables

.env:
```
MY_API_KEY=someSecret
```

### (0 tech version)

You don't need a library if you are willing to source secrets

```
export MY_API_KEY=someSecret
```

then
```sh
source .env
```

### Accessing secrets
In code use `apiKey = os.getEnv('MY_API_KEY')`

### Creating global ignore

Coder's personal editor shouldn't be in a shared repo. Consider setting up a global ignore

e.g. `.vscode`, vim's `.swp`, MacOS `.DS_STORE`, etc.

[Guide](`https://gist.github.com/subfuzion/db7f57fff2fb6998a16c`)

## Viewing history
  `git log`
  `git switch --detach $COMMIT_HASH` This will "restore" the commit

## Git Remote

Git histories can be synced across computers. Your local repository can define remotes to sync with

There are many services that will host a git server and provide a GUI and additional tooling.

Examples:
  - GitHub (The most popular, owned by Microsoft)
  - Gitea (Open source. Good if you want to host your own)
  - GitLab (GitHub competitor)

### Adding a remote repository

First create a repository on GitHub, then:

  `git remote add origin ssh://...`
  - URL will be copied from the UI

  - (Can use https://, but i think it needs your username/password each time)
  - SSH will require an SSH key set

### Syncing with remote repository

  - `git push origin main` (Push your current changes to the `main` branch)
  - `git pull origin main` (Fetch the latest changes from the `main` branch)

  Note: The names `origin` and `main` are used by convention. You can call branches and remote repos whatever you want

### Git Branching

  Generally developers will not commit to `main`, but create a branch

  A branch is a named pointer that will advance when branches are committed to

### Creating branches
  to make a new branch use:

  `git switch -c alpha` (make and changes to alpha branch, based off of current branch)

  A branch name should be short but descriptive

### Switching between branches

To change branches use `git switch $BRANCH_NAME`,

### Git Merging

`git merge` lets branches be synced

You can merge branches locally:
- First switch to the branch that should be updated
`git switch main`

- Then merge the branch you want to update with
`git merge alpha`

This will update `main` with the latest changes from `alpha`

### Industry Practice

Normally a "Pull Request" is created and work is reviewed. The merge will be done on the remote repo

In addition to a coworker reviewing your code, automated checks maybe performed on the code:

  - A linter maybe used to enforce a consistent style in the code base
  - Automated unit tests are ran. If a test fails it should block the PR
  - Static analysis tools maybe used (detect unsafe methods like `eval()`, flag complicated functions)

### Git Conflict
When two people edit the same line of code in two branches git will not be able to tell which version it should use

It will mark the code with both versions. The correct version must be manually chosen

Careful, bugs can result from incorrect resolutions. Even correct resolutions can break other parts of the code

### Git Stashing
Git will not delete files, and will often refuse to pull or switch branches when there are changed files

`git stash` hides current changes
`git stash pop` restores it

Helpful when switching branches, or syncing a remote branch, e.g.

```sh
git pull origin main ## errors due to changes
git stash
git pull origin main ## should be ok
git stash pop ## restores in progress work
```

`git stash pop` can result in conflicts. If this happens the stash will remain intact so you can clear out the staging area

## Advanced git

Here are some more complex commands for git, they aren't essential for day to day use though:

- `git rebase` helps keep git history clean by "rewriting history". Should never be done to shared branches
- `git commit --amend` Lets you rewrite the last commit (You will need to "force push" to update a remote)
- `git cherry-pick $COMMIT_HASH` applies commit to working branch. Helpful for porting bug fixes to older versions
- `git bisect` binary search to find where a bug was introduced
- `git revert` undoes a commit by applying an opposite change set
- `git tag` allows commits to be easily found, e.g. `v3.0.1`
- `git checkout` (legacy) can be used in many ways but is hard to explain

## Further Reading

How to write good commit messages: https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages/
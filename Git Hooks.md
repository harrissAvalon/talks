# Becoming a CI Cook with Git Hooks pt. 1
---
###  Git Hooks are scripts that run automatically every time a particular event occurs in a Git repository.

 They are useful for encouraging a commit policy, altering the project environment depending on the state of the repository, and implementing continuous integration.


## Setting up git hooks
1.  Initialize the repo
```bash
git init
```
2. Locate the scripts inside the `.git/hooks` directory of a particular repo
3. Ensure the script has the correct permissions to run
```
chmod +x <script>
```

## Git hooks are either client-side or server-side
### Client
- pre-commit
- prepare-commit-msg
- commit-msg
- post-commit
- post-checkout
- pre-rebase

### Server
- pre-receive
- update
- post-receive

**Client-side** hook scripts ensure developers follow standards and automate moving code that meet quality standards for deployments/integration. **Server-side** hooks enforce policies to adhere to quality.
## Git hooks and continuous integration
Ensuring that code meets quality is just as important as writing it. You can utilize git hooks to ensure code shared in a team is verified through automated builds and testing, allowing teams to detect problems early.

Let's look at two local-side git hooks for example.
### Pre-commit hook
- run automated tests
- ensure unnecessary whitespace is removed
- remind developers to include helpful commit messages
#### Example: JS validation before committing
Let's create a pre-commit hook that validates javascript files through  [JSLint](www.JSLint.com). We first need to start by downloading the JSLint package from NPM.
```
npm install -g jslint
```
Next lets create a pre-commit file in the hooks directory
```
nano .git/hooks/pre-commit
```
Now we will add in a script  to run .js files through JSLint

```bash
#!/bin/sh

# put all js filenames into a variable, if none then exit the hook without an error
files=$(git diff --cached --name-only --diff-filter=ACM | grep ".js$")
if [ "$files" = "" ]; then
    exit 0
fi

pass=true

echo "\nValidating JavaScript:\n"

# run JSlint for each js file, printing if the file passed or failed
for file in ${files}; do
    result=$(jslint ${file} | grep "${file} is OK")
    if [ "$result" != "" ]; then
        echo "\t\033[32mJSLint Passed: ${file}\033[0m"
    else
        echo "\t\033[31mJSLint Failed: ${file}\033[0m"
        pass=false
    fi
done

echo "\nJavaScript validation complete\n"

# if a file did not pass the linter, let the user know and exit with an error preventing a commit, otherwise allow the commit to proceed
if ! $pass; then
    echo "\033[41mCOMMIT FAILED:\033[0m Your commit contains files that should pass JSLint but do not. Please fix the JSLint errors and try again.\n"
    exit 1
else
    echo "\033[42mCOMMIT SUCCEEDED\033[0m\n"
fi
```
Lastly we're going to change the permissions of the file.

```bash
chmod +x .git/hooks/pre-commit
```
When you commit the JS files that are staged for the commmit will run through the JSlinter for validation. If they pass standards, you code will then be committed.

### Post-commit hook

- deploying changes
- notify of changes


#### Example: Deploying changes with a post-commit hook
Create a post-commit file in the hooks directory
```
nano .git/hooks/post-commit
```
Add in a script to commit changed files to web server (don't forget to change the `/Path/to/repo/.git` variable).

```bash
#push automatically to WebServer
#!/bin/bash
unset GIT_INDEX_FILE
sudo git --work-tree=/Library/WebServer/Documents --git-dir=/Path/to/repo/.git checkout -f
```
Change the permissions of the file
```
chmod +x .git/hooks/post-commit
```
Now I'll add a change to my index.html file and commit it
```bash
echo "<p>Here is a change.</p>" >> index.html
git add .
git commit -m "First change"
```

After the commit, the post-commit script will utilize my local web server folder to deploy my changes to, allowing me to immediately test my changes after a commit.
## Why is this important?
Automation can remove human errors from your workflow. Here are just a few ways you can utilize git hooks and the respective hook name.


- ACLs & testing [**pre-commit**]
- Faster deployments [**post-commit**]
- more helpful commit messages [**prepare-commit-msg**]
- ensure branch is ready for commits [**post-checkout**]
- prevent feature branches that are already merged to 'dev' branch from getting rebased [**pre-rebase**]
- reject code that doesn't adhere to the teams standards [**pre-receive**]
- enable push to deploy for faster releases [**post-receive**]

Git hooks can also be source controlled through git and creating a symbolic link to the file in .git/hooks. For example:
```bash
ln -s /path/to/repo/pre-commit-jslint /path/to/repo/.git/hooks/pre-commit
```

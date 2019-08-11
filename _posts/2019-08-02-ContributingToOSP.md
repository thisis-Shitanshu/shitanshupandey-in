---
layout: post
title:  "Contrubting to Open Source Project"
categories: OpenSource GitHub
---

1. Look for CONTRIBUTING.md file.
1. Fork the Repository.
1. Clone the Forked Repo locally.
1. To keep your copy of repository up to date with original repository (Upstream repository);
    - add original copy of repository as **git remote**, so we can easily fetch and pull from it.
    ```
        $ git remote add upstream URL_ORIGINAL_REPOSITORY
    ```
    - To get information about upstream/ remote.
    ```
        $ git fetch upstream
    ```
    - You should have your local copy of master to point to upstream/master, so whenever you pull changes into master you will get latest changes from upstream.
    ```
        $ git branch --set-upstream-to=upstream/master master
    ```
1. Create a branch.
```
    $ git checkout -b BRANCH_NAME
```
1. Install dependencies, run test, run build.
```
    $ npm install
    $ npm t
    $ npm run build
    $ npm run test:watch
```
1. **MAKE CHANGES**
1. Make Pull Request.
```
    $ git add .
    $ git status
    $ git commit -m "Message"

    // Push to your remote repository/ fork
    $ git push origin BRANCH_NAME
    
    // OR first
    $ git push --set-upstream origin BRANCH_NAME
    $ git push --u origin BRANCH_NAME
        // Later you can just use to push to branch
        $ git push
```
1. On the forked repository online, you will see your pushed changes instantly.
    - You can Compare & Pull Request, or
    - Create a New Pull Request
1. Compare changes across fork:
    - Change your **head fork** to your fork.
    - Change branch to the one you just pushed.
    - Write message.
    - Create pull request.


# More Tips
1. Check Travis build before you push up changes, you can run the scripts in .travis.yml locally.
1. Use commit message standards if present in CONTRIBUTING.md.

## To skip git hooks when performing commit:
```
    $ git commit -m "MESSAGE" --no-verify
    $ git push
```

## Rebase a git Pull Request branch:
- Sometimes original branch has updates and our branch falls behind.
- If changes in the remote repository conflict with our repository, we get **merge conflict**.
```
    // Update local machine knowledge of upstream
    $ git fetch upstream

    // Replay our commits onto the upstream/master remote
    $ git rebase upstream/master

    // Manually fix merge conflict
    // Continue rebase
    $ git rebase --continue

    // Use force to push but never use it on master
    $ git push -f
```


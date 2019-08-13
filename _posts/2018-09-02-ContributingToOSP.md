---
layout: post
title: "Contributing to Open Source Project"
categories: OpenSource GitHub
published: true
---

When starting to contribute to any project always look for CONTRIBUTING.md file. (If not present, you can submit an issue for it. So that contributing to that project becomes convenient.)

## #Steps
1. Fork the Repository.
1. Clone the Forked Repo locally.
1. To keep your copy of repository up to date with the original repository (Upstream repository):
    - add `git remote`, so we can easily fetch and pull from the original copy of repository.
    ```
    $ git remote add upstream URL_ORIGINAL_REPOSITORY
    ```
    - To get information about upstream/ remote.
    ```
    $ git fetch upstream
    ```
    - You should have your local copy of master pointing to upstream/master, so whenever you pull changes from the master you will get the latest changes from upstream.
    ```
    $ git branch --set-upstream-to=upstream/master master
    ```
1. Create a new branch to add the changes to.
```
 $ git checkout -b BRANCH_NAME
```
1. Install dependencies, run tests, run build.
```
$ npm install
$ npm t
$ npm run build
$ npm run test:watch
```
1. **ADD THE CHANGES!!!**
1. Make the Pull Request:
    - Commit the changes.
    ```
    $ git add .
    $ git status
    $ git commit -m "Message"
    ```
    - Push to your remote repository/ fork,
        ```
        $ git push origin BRANCH_NAME
        ```
        - or first
        ```
        $ git push --set-upstream origin BRANCH_NAME
        $ git push --u origin BRANCH_NAME
        ```
        - and later you can just use this command to push changes
        ```
        $ git push
        ``` 
1. **GitHub.com:** On the forked repository online, you will see your pushed changes instantly.
    - You can Compare & Pull Request, or
    - Create a New Pull Request
1. Compare changes across fork:
 - Change your **head fork** to your fork.
 - Change branch to the one you just pushed.
 - Write message.
 - Create a pull request.


## #More Tips
- Check Travis build before you push up changes:
    - You can run the scripts in .travis.yml locally.
- Use commit message standards if present in CONTRIBUTING.md.
- To skip git hooks when performing commit:
```
$ git commit -m "MESSAGE" --no-verify
$ git push
```

## #Rebase a Git branch
Sometimes the original branch has updates and our branch falls behind.
If changes in the remote repository conflict with our repository, we get `merge conflict`.
- Update local machine knowledge of upstream.
```
$ git fetch upstream
```
- Replay our commits onto the upstream/master remote.
```
$ git rebase upstream/master
```
- If conflict exists, manually fix merge conflict.
- Continue rebase.
```
$ git rebase --continue
```
- Push the changes.
- You can use force to push but never use it on the master.
```
$ git push -f
```
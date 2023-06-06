# Git and version control

## Create new branch for each features with that card number or ticket number

## Create new branch for hotfixes

## Use detailed, well-written commit messages with card number or ticket number when committing code into VCS.
- An even better thing to do is to use `git commit -v` flag to open a commit message editor and enter your message and description and in the bottom attach a link to the trello card / trello comment of that task.

## Keep commits clean. Don't mix features/bug fixes
- For each commit or group of commits it should either finish a feature or fix a bug.  
- One commit shouldn't contain the fix for multiple issues or multiple features or the mix between issue and a feature.  
- If we revert one commit / PR it should exactly remove that code/feature from the codebase.
- Try using `git add -p` to make things easier when adding changes to a commit.

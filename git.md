# Git and version control

## Don't push broken code into VCS
- If you know your code is not working properly or has issues, don't push it into VCS.
- Make small, incremental commits in working condition on feature branches.

## Don't push directly into master
- Never push into master or main directly unless you know what you're doing.

## Keep master/main an equivalent of production ready code
- master or main branch should have working code that is deployed on production.
- master should always be on par with production deployments.

## Keep a base branch for development activities
- Create a development/staging/pre-production branches based on your product and server setup.
- Always use development as base branch and use PRs to merge up and above to higher environments

## Create new branch for each features with that card number or ticket number
- Create new branches from development for each new feature that you're working on

## Create new branch for hotfixes
- When fixing issues in production, create a branch from production base branch and then create a PR

## Use detailed, well-written commit messages with card number or ticket number when committing code into VCS.
- An even better thing to do is to use `git commit -v` flag to open a commit message editor and enter your message and description and in the bottom attach a link to the trello card / trello comment of that task.

## Keep commits clean. Don't mix features/bug fixes
- For each commit or group of commits it should either finish a feature or fix a bug.  
- One commit shouldn't contain the fix for multiple issues or multiple features or the mix between issue and a feature.  
- If we revert one commit / PR it should exactly remove that code/feature from the codebase.
- Don't add all files you changed with `git add .`, Try using `git add -p` to make things easier when adding changes to a commit.

## Know what you're doing
- A lot of commits on git are reversible, but very difficult to do so.
- Don't copy and paste git commands without understanding them first.
- When in doubt, see if there is a `dry-run` flag available for that command, this will run the command and will tell us what will happen but will not make any actual changes.
- If there is no `dry-run`, discuss with your team to learn more about a command.

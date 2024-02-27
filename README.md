Git Exercise
License:CC BY-NC-SA 4.0

Guidelines
Fork this repo and clone your forked repo locally. Make sure you fork ALL branches, not only main
Answer the below questions, commit your results and push them (make sure you push all the involved branches).
Check GitHub Actions in your forked repo (you may need to enable it) for test results. Make sure you pass the automatic tests of the Students Presubmit Fork Tests workflow.
Branches
Create the following git commit tree. You can add any file you want in each commit. The message for each commit is denoted in the graph (c1, c2,..., c12). Note the (lightweight) tags in the merge commits and commit c8. Make sure to create exactly these names for branches and commit messages.

The parent commit of c1 should be the last commit in branch main after a fresh clone of this repo (commit with message start here).


Notes:

If you've messed up the repo, you can always checkout branch main and run git reset --hard <commit-id> where <commit-id> is the commit hash from which you need to start.
By default, your tags aren't being pushed to remote. Make sure to push your tags using the --tags flag in the git push command.
Test it locally
git checkout main
cd test
bash branches.sh
Merge Conflict
It's highly recommended to use a conflict merge tool (like the built-in one in PyCharm and VSCode).

Your team colleagues, John Doe and Narayan Nadella, are working together on the same task. Each one of them is working on his own git branch.

John Doe developed under origin/feature/version1 branch.
Narayan Nadella developed under origin/feature/version2 branch.
Both checked out from the same main branch.

You decide to create a new branch called feature/myfeature and merge the work of John and Narayan into your branch. You'll encounter a conflict.

From mian branch, create and checkout feature/myfeature branch.
Merge origin/feature/version1 into your branch, take a look on the merge changes.
Merge origin/feature/version2 into your branch, and resolve the conflicts according to the below guidelines:
The flask webserver code under app.py should have a total of 8 endpoints in the following order: /, /status, /blog, /pricing, /contact, /chat, /services, /internal.
Narayan mistakenly has coded a bad port number for the service, John's branch port is correct.
Narayan knows better than John about the price of the service.
Test it locally
git checkout main
cd test
bash conflict.sh
Pre-commit and sensitive data
In this repo, there is a commit which contains credentials of strong identity in AWS. The file contains the credentials might look like:

AWS_ACCESS_KEY_ID=AKIA6BJMA3TKBADSHFXZ
AWS_SECRET_ACCESS_KEY=op7N48fxIFxh06ToUwZd33emso/QKZWb/2M5fgTX
Your goal is to find this commit, and completely remove it from the history.

Here is an illustration of the vulnerable commit (the true branch name is not some_branch):


And after your fix:


Note that commits behind the vulnerable commit should remain untouched (like commit1), while commit ahead the vulnerable commit might change (like some other commit 2 and some other commit 3, instead of commit 2 and commit 3).

Commit-wise, you are free to do whatever you wish for commits that are coming after the vulnerable commit, as far as the content of the branch remain the same. The branch content should be identical to what it was before your fix, except the vulnerable file that was committed in the VULNERABLE_COMMIT commit.

There are many approaches to solve it, some are using git reset --hard, git rebase or git cherry-pick. Find your preferred way. You should find the branch contains the vulnerable data, learn its structure and data, and remove the vulnerable commit carefully, without loosing data committed in other commits.

The commit shouldn't be found also in remote branches. Since you've changed the commit history, you may be needing to --forcefully push your fixed branch to remote.

In order to prevent this vulnerability in the future, integrate pre commit into your repo, and add a plugin that blocks any commits that contains AWS credentials data. Verify that the tool is working - try to commit the below text and make sure pre-commit is blocking you. If you were able to commit it, git reset your working branch to the commit before the vulnerable commit, and try again.

Test it locally
git checkout main
bash test/sensitive_data.sh
Git workflows and remote
Gitflow is a branching model for Git that provides a structure for managing feature development and releases in a software project. It defines specific branches for each stage of the development process and enforces rules about how and when code can be merged between them.

In the Gitflow model, the main branches are:

main: This branch represents the production-ready code and should always contain the latest stable release. This branch should be protected in GitHub, no one is able to push code into it directly.
dev: This branch is used for ongoing development of the application and should contain the latest features that are being worked on.
Feature branches (starts with feature/...): These branches are used for developing new features and should be branched off from the dev branch. Once the feature is complete, it is merged back into dev via a Pull Request. If everything is ok and ready to be deployed in production, the branch owner opens a Pull Request from the branch into main.
Release branches (starts with release-*) for preparing releases. Once the code in a release branch is stable. It is merged into both dev and main. Any necessary bug fixes for the release are done in this branch.
Your goal is to implement gitflow workflow in this repo.

Tip: You can always start over again by deleting the dev, release and feature branches (also from remote if needed), and use the git reset --hard <commit> command to reset the main branch to certain commit, while <commit> is the commit id last before you start this question.

First, create the following protection rules in your GitHub repo:

dev branch is not allowed to being pushed directly, only via Pull Request.
main branch is read-only, only the release manager (you) can push it directly.
From main, checkout new dev branch.

From dev create some feature branch.

Create a Pull Request from your feature branch into dev, review the PR, and finally merge it into dev (don't use fast-forward!).

Observe the created GitHub action job that responsible to "deploy" the updated branch dev into Development environment (you may need to enable GitHub actions in your account - it's free).

From the merge commit created by the PR, create a release branch. You can call it release-v1.2 for example.

Commit some more changes in the release branch. Those changes are simulate some tiny fixes you've received from QA, some typos the product manager has fixed, and release specific content.

Push your release branch. Observe who is the GitHub action jos "deploying" this release to Test environment (also known as Stage env). Make your fixes if needed.

Once you're satisfied with results, ask your release manager (which is you) to merge your new release into main, push main and observe how the created GitHub action job is "deploying" the release to ☠️Production env☠️.

In GitHub actions, workflows are defined using YAML syntax in .github/workflows directory of your repository. Take a look into .github/workflows/prod.yaml file, which defines Production deployment pipeline. Edit the workflow yaml file, such that the workflow is aborted when the user triggered it is not the release manager.

Merge two git repositories
In a company implementing typical DevOps pipelines, different teams may be responsible for developing separate microservices of a larger application, each residing in its own Git repository. You have been assigned the task of merging two different Git repositories, each containing separate microservices, into a single monorepo. The repositories were maintained by separate teams and have separate commit histories. Your goal is to preserve the entire commit history of both repositories while merging the code into a single Git repository, ensuring that the microservices remain functional and properly integrated with each other.

Merge the GitExerciseOther repo into this (GitProject) repository. The main branch of the monorepo should have the following file structure:

GitProject
└── serviceA/
        ├── [service A files...]
    serviceB/
        └── [service B files...]
Notes
You are to choose what to do in the files of the GitExerciseOther repo that don't under serviceB directory.
You can commit any further change (e.g. move files into some directory) after the history of the GitExerciseOther repo has been successfully merged into this repo.
In case of conflicts during the merge, you should prefer this repo's version.
Test it locally
git checkout main
cd test
bash merge_repos.sh
Gook Luck

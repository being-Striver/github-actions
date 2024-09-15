# Github Actions
-------------------------------
A tool that lets you automate your software development workflows.
It allows you to react to some events that can happen in your repository or outside of your repository and run a workflow in response of that event.


## What is workflow?
Workflows are configurable automated processes that you can set up in your repository in order to perform a certain task.
  - testing your code
  - deploying an app
  - publishing a package
  - sending a slack message

Three main building blocks:
- Workflows
- jobs
- steps

You can add workflows to your github repository.
Workflows can contains one or more jobs.
One job can contains one or more steps.

NOTE: All workflows should reside inside ".github/workflows/*".

How to run your workflow using github actions?
-- As soon as workflow will be commited to github repository, github will identify those workflows in .github/workflows subfolder and auto populates it.

If you need to run multiple shell commands (or multi-line commands, e.g., for readability), you can easily do so by adding the pipe symbol (|) as a value after the run: key.

Like this:
...
run: |
    echo "First output"
    echo "Second output"
This will run both commands in one step.




# Workflow triggers
----------------------------
Workflow triggers are events that cause a workflow to run. 


Available events:
--------------------------
- repository related:
   - push, fork, watch , pull_request, issues, discussion, issue_comments
- others:
   - workflow_dispatch: manually trigger workflow
   - repository_dispatch: REST API requests triggers workflow
   - schedule: workflow is scheduled
   - workflow_call: can be called by other workflow


Github Workflow UI:
--------------------------
command to check workflow using cli : gh workflow view "First Workflow".

NOTE: You can disable workflow using UI. It will prevent from running whenever there is a push on repository.

You can enable or disable workflow using cli. command: gh enable workflow "name of workflow"


Debugging in github actions:
----------------------------------
You can add more logs using variables and secret by adding new variable to repository. You can go to settings where you can add variables and secrets in order to get more logs.

SKipping the workflow:
----------------------------
Sometimes you might want to skip workflow whenever there is a push to repository.Sometimes you might want to skip your workflow for specific commit.
Workflows that would otherwise be triggered using on: push or on: pull_request won't be triggered if you add any of the following strings to the commit message in a push, or the HEAD commit of a pull request:
 - [skip ci]
 - [ci skip]
 - [no ci]
 - [skip actions]
 - [actions skip]


Note: Skip instructions only apply to the push and pull_request events. For example, adding [skip ci] to a commit message won't stop a workflow that's triggered on: pull_request_target from running.

#Introduction to workflow commands:
------------------------------------------
Github actions can communicate with the runner machine running your job. 




# GITHUB_SHA
-----------------------
GITHUB_SHA is a predefined environment variable in GitHub Actions that contains the commit SHA (Secure Hash Algorithm) of the commit that triggered the workflow. A commit SHA is a unique identifier (hash) that Git generates for each commit, and it helps you pinpoint the exact state of the code at the time the workflow is run.


# GITHUB_REF
--------------------------
GITHUB_REF is a predefined environment variable in GitHub Actions that refers to the Git reference of the event that triggered the workflow. This reference can be a branch, tag, or a pull request reference.

# Difference between GITHUB_SHA and GITHUB_REF
------------------------------------------------
- Use GITHUB_SHA when you need to reference a specific commit.
- Use GITHUB_REF when you need to reference the branch or tag that triggered the workflow.


# what is actions/checkout@v3?
------------------------------------------
actions/checkout@v3 is the third major version of the checkout action in GitHub Actions, used to check out (or clone) your repository code onto the runner (the virtual machine) where your workflow is executed. This allows your workflow to access the source code of the repository so that it can run builds, tests, or deployments.
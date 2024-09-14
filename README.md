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
- 
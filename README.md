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


# Expressions and context
------------------------------------
Expressions in GitHub Actions are used to evaluate and compute values within workflow files. They allow you to create conditional logic (like if statements) and compute dynamic values.

Syntax: Expressions are enclosed in ${{ <expression>}}. Inside this syntax, you can use variables, functions, and operators.

An expression can be any combination of literal values, references to a context, or functions. You can combine literals, context references, and functions using operators.

Common Expression Functions:
- success(): Returns true if the previous step succeeded.
- failure(): Returns true if the previous step failed.
- contains(): Checks if a string contains a substring.
- startsWith(): Checks if a string starts with a specific prefix.


Contexts provide information about workflow events, runner environments, jobs, steps, and more. They are key-value pairs that give you access to metadata and allow you to use them in expressions.

github contexts:
- ${{ github.repository }}: The name of the repository (owner/repo).
- ${{ github.event_name }}: The event that triggered the workflow (e.g., push, pull_request).
- ${{ github.actor }}: The GitHub username who initiated the workflow.
- ${{ github.ref }}: The branch or tag that triggered the workflow.


env contexts:
- ${{ env.MY_ENV_VAR }}: Access the custom environment variable MY_ENV_VAR.

job contexts:
- ${{ env.MY_ENV_VAR }}: Access the custom environment variable MY_ENV_VAR.

Runner contexts:
- ${{ runner.os }}: The operating system of the runner (e.g., Linux, Windows, macOS).

steps contexts:
- ${{ steps.step_id.outputs.output_name }}: Get the output from a specific step.

matrix contexts:
- ${{ matrix.os }}: The operating system defined in the matrix for this run


By using **IF** key, we can control the workflow. We can define **IF** key at job level and step level.
Whatever inside **IF** key, github will evaluate it as an expression. You can either put "${{}}" or you can skip it.

For using more expressions, its better to refer official doc where all listed expressions are given.



## Default and custom environment variables:
---------------------------------------------------
What is the difference between env variables and context?
- ENV variables are evaluated on runner machine while context will be evaluated before workflow sending to runner machine.

${{github.ref}}  -- this is context. this will be sent to runner machine after evaluating.
$GITHUB_REF  -- this is env variable. it will be evaluated on runner machine.


During the execution of workflow, runner machine generates some temporary files that can be used to perform certain actions.
You can make an environment variable available to any subsequent steps in a workflow job by defining or updating the environment variable and writing this to the GITHUB_ENV environment file. The step that creates or updates the environment variable does not have access to the new value, but all subsequent steps in a job will have access.

echo "{environment_variable_name}={value}" >> "$GITHUB_ENV"


You can use multiline strings.You can use delimeter syntax.
{name}<<{delimiter}
{value}
{delimiter}

example:
  steps:
  - name: Set the value in bash
    id: step_one
    run: |
      {
        echo 'JSON_RESPONSE<<EOF'
        curl https://example.com
        echo EOF
      } >> "$GITHUB_ENV"


Setting an output parameter:
   echo "{name}={value}" >> "$GITHUB_OUTPUT"

Adding a job summary:
- echo "{markdown content}" >> $GITHUB_STEP_SUMMARY


Multiline markdown content:
For multiline Markdown content, you can use >> to continuously append content for the current step
- name: Generate list using Markdown
  run: |
    echo "This is the lead in sentence for the list" >> $GITHUB_STEP_SUMMARY
    echo "" >> $GITHUB_STEP_SUMMARY # this is a blank line
    echo "- Lets add a bullet point" >> $GITHUB_STEP_SUMMARY
    echo "- Lets add a second bullet point" >> $GITHUB_STEP_SUMMARY
    echo "- How about a third one?" >> $GITHUB_STEP_SUMMARY



# Configurations variables and secrets:
-----------------------------------------------
In the previous section, we have learned how to define env variables at workflow level.What if we need same environment variable in another workflow.This is where configuration variables come into picture. Configuration variables can be accessible throughout the repository or even for multiple repository throughout organization. It means whatever workflow files are there, they would be able to access it.
You can declare it in github environments and secrets.
These can be defined at two level.
- repo level
- org level
- at environment level















# job artifacts and outputs
-------------------------------------
Artifacts allow you to share data between jobs in a workflow and store data once that workflow has completed. An artifact is a file or collection of files produced during a workflow run. For example, you can use artifacts to save your build and test output after a workflow run has ended. All actions and workflows called within a run have write access to that run's artifacts.

Github provides two actions that you can use to upload and download build artifacts.

Artifacts and caching are similar because they provide the ability to store files on GitHub, but each feature offers different use cases and cannot be used interchangeably.



# Advanced github actions features
---------------------------------------
## What's timeout-minutes?
timeout-minutes is the maximum number of minutes to let a job run before GitHub automatically cancels it.
The default value is 360.

## Why should you set timeout-minutes?
The default value of timeout-minutes is 360, but this is too long for most GitHub Actions jobs.
Even if processes are stuck for some reason, jobs keeps running until the timeout.
This wastes resources uselessly.
By setting timeout-minutes properly, you can notice the issue and resolve it by retrying jobs quickly.

## What is continue-on-error?
The **continue-on-error** keyword is used to allow a job or a step to continue running even if it encounters an error. Normally, if a step fails, the job and workflow stop executing. By setting continue-on-error: true, the workflow will not fail immediately, and the subsequent steps or jobs will continue running.

## use cases:
-------------------------
This feature is helpful when you want to proceed with the workflow even if a non-critical step or job fails. 
You can use continue-on-error at both the job level and the step level.

1. You may want to allow non-critical tests(like integration tests) to fail but still proceed with deployment.
2. If you run a linter or code style check and don't want the failure to block the build.
3. If you use **needs** to define dependencies between jobs, the dependent job will still run if the upstream job fails but has **conitnue-on-error: true**.


## matrix:
-----------------

## What is github actions matrix strategy?
The matrix strategy in GitHub Actions allows you to define a matrix of values that will run the same job multiple times with different configurations. This is useful when you want to run the same job with different versions of a language, different operating systems, or different inputs based on what your job is doing.

## Defining a matrix in your github actions workflow
--------------------------------------------------------
The key, strategy.matrix is defined below the job ID you're currently defining. Conceptually, the matrix strategy is a dictionary of keys and values that you want to run your job against. The keys are the names of the variables you want to use in your job, and the values are the different configurations you want to run your job against.


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
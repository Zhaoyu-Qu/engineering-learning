# Github Actions Notes

## Concepts
- Workflow: a configurable automated process that will run one or more jobs when triggered. It is defined by a YMAL file stored in .github/workflows. The YMAL file specifies events that trigger the workflow, defines jobs and specifies steps to run within each job. A repository can have multiple workflows. A workflow can be triggered by an event, by a schedule, by posting to a REST API, or manually. 
- Event: an activity in a repository that triggers a workflow run.
- Job: a set of steps. Each job runs inside its own runner. Jobs can run paprallel by default or be configured to run sequentially. However, steps inside a job are always run sequentially.
- Runner: an isolated environment that executes the steps of a job defined in a workflow. A runner could be a virtual machine (VM), a container (such as a Docker container), or even your own physical machine. Wherever your runner is located, the code to execute is still the same - it just needs to be executed somewhere. The code doesn't care where it gets executed.
    - GitHub-Hosted Runner: GitHub Actions will always provision a seperate VM for each job. You may choose to run job steps directly in a VM, or the VM can run a container which executes your code. Once again, GitHub Actions will always create an independent VM for every single job.
    - Self-Hosted Runner: could be your on-premises infrastructure, or web services. If it's a VM or a physical machine, you will need to install the GitHub Actions runner software to register the machine as a self-hosted runner. If it's a container, then the container should run the software so it is recognised as a self-hosted runner.
- Step: a step is either a shell script or an action. Since a job is a set of steps and each job is assigned one runner, naturally all steps in the same job also run in the same runner in order. These steps can share data with each other since they are in the same runner.
  - Shell script steps are defined directly in the workflow YAML file under the run: keyword.
  - Action steps are referenced via uses: and run pre-built code.
- Action: an action is a small, self-contained piece of software designed to perform a complex but frequently repeated task within a CI/CD workflow. You can write your own actions, or find them in the GitHub Marketplace.

## QuickStart
1. Create a new repository on GitHub
2. In the repository directory, create a workflow file `.github/workflows/github-actions-demo.yml`.
   - The workflow files can be given any names, but they must be put in a directory called `.github/workflows` to be discovered.
3. Copy the following YAML contents into the github-actions-demo.yml file:
```
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```
4. Commit changes. This will update the workflow file and the push action itself will trigger the workflow.
5. On GitHub, navigate to the main page of the repository. Click Actions and examine the results.

### Note:
- The commit always happens first. After the changes have become part of the repository, GitHub emits events (e.g., push) based on the new state of the repository. Lastly, GitHub Actions sees the event and checks .github/workflows/ to decide if any workflows should run.
- uses: Tells GitHub to use a pre-built action (like a reusable plugin).
- actions/checkout Refers to the official GitHub "checkout" action (maintained by GitHub) - equivalent to git clone repo then cd repo.
- @v4 Pins the action to version 4 (stable). Always specify a version.

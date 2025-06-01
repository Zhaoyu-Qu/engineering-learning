# Concepts
- **Workflow**: a configurable automated process that will run one or more jobs when triggered. It is defined by a YMAL file stored in .github/workflows. The YMAL file specifies events that trigger the workflow, defines jobs and specifies steps to run within each job. A repository can have multiple workflows. A workflow can be triggered by an event, by a schedule, by posting to a REST API, or manually. 
- **Event**: an activity in a repository that triggers a workflow run.
- **Job**: a set of steps. Each job runs inside its own runner. Jobs can run paprallel by default or be configured to run sequentially. However, steps inside a job are always run sequentially.
- **Runner**: an isolated environment that executes the steps of a job defined in a workflow. A runner could be a virtual machine (VM), a container (such as a Docker container), or even your own physical machine. Wherever your runner is located, the code to execute is still the same - it just needs to be executed somewhere. The code doesn't care where it gets executed.
    - GitHub-Hosted Runner: GitHub Actions will always provision a seperate VM for each job. You may choose to run job steps directly in a VM, or the VM can run a container which executes your code. Once again, GitHub Actions will always create an independent VM for every single job.
    - Self-Hosted Runner: could be your on-premises infrastructure, or web services. If it's a VM or a physical machine, you will need to install the GitHub Actions runner software to register the machine as a self-hosted runner. If it's a container, then the container should run the software so it is recognised as a self-hosted runner.
- **Step**: a step is either a shell script or an action. Since a job is a set of steps and each job is assigned one runner, naturally all steps in the same job also run in the same runner in order. These steps can share data with each other since they are in the same runner.
  - Shell script steps are defined directly in the workflow YAML file under the `run:` keyword.
  - Action steps are referenced via `uses:` and run pre-built code.
- **Action**: an action is a small, self-contained piece of software designed to perform a complex but frequently repeated task within a CI/CD workflow. You can write your own actions, or find them in the GitHub Marketplace.

# Mechanism
GitHub Actions is an automation platform built into GitHub. Under the hood, it consists of:
- A **workflow** engine (on GitHub’s servers)
  - It Parses and validates your workflow file, schedules jobs and assigns runners, sends instructions to the runners, monitors status and collects output, and lastly, sends notifications.
  - It decides what to run and in what order, but does not execute code or run scripts - that's the runners' job.
  - It does not check exit codes. Instead, it relies on the runners to report the outcome.
- A **runner agent** (software installed in a virtual machine)
  - When the runner starts, the runner agent pulls down your repository, read the `.github/workflows/*.yml` file and executes each step in order.
  - It translates human-readable steps into shell scripts so the OS can execute them.
  - It is designed to capture the exit code of every command, log stdout and stderr, send status updates back to GitHub's workflow engine and mark steps as failed or succeeded.
  - When the job is done, the runner agent finishes any cleanup and then self-terminates (the VM is deleted).
- A **job scheduler** that determines when and where a job should run. Think of it as a traffic controller. For example, when you push a commit, the scheduler queues a workflow run and assigns jobs to available runners, based on capacity, availability, and concurrency limits.
- An **orchestrator** that oversees the lifecycle and coordination of jobs and runners. For example, it ensures that jobs start in the right order (respecting needs:), cancels workflows if you push new commits too quickly, and tears down resources after the job finishes.

When triggered, GitHub Actions evaluates your YAML workflow files using the workflow engine, which determines the jobs and steps to run. Execution is then delegated to a GitHub-hosted runner, which is an ephemeral virtual machine.
Each step in a job is executed by the runner. If a step returns a non-zero exit code, the runner marks that step as failed and reports the result back to the workflow engine, which updates the UI.

By default, a failed step causes the entire job to stop immediately — unless explicitly configured otherwise using `continue-on-error: true`.

## You cannot host your app on a GitHub-hosted runner
GitHub-hosted runners are not meant to host applications for the following reasons:
- Ephemeral: The runner is destroyed after each job — nothing is persisted.
- Time-limited: Jobs can only run for a limited time (6 hours for free tier, less for others).
- No public IP: You can’t publicly access apps running inside runners like you would on AWS or Heroku.
- Costly & not scalable: Even if you hacked it to keep something running, it would be very unreliable and expensive.

# QuickStart
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

## Note:
- The commit always happens first. After the changes have become part of the repository, GitHub emits events (e.g., push) based on the new state of the repository. Lastly, GitHub Actions sees the event and checks .github/workflows/ to decide if any workflows should run.
- uses: Tells GitHub to use a pre-built action (like a reusable plugin).
- actions/checkout Refers to the official GitHub "checkout" action (maintained by GitHub) - equivalent to git clone repo then cd repo.
- @v4 Pins the action to version 4 (stable). Always specify a version.

# Write Workflows
## Workflow basics
A workflow must contain the following basic components:
- One or more events that will trigger the workflow.
- One or more jobs, each of which will execute on a runner machine and run a series of one or more steps.
- Each step can either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.

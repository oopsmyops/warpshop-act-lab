# Warpshop Lab: The Expert Challenge

Welcome to the expert challenge! You've mastered the basics, now let's tackle a more realistic and complex workflow. Your mission is to debug a multi-stage, matrix-driven CI pipeline using advanced `act` features.

---

## Your Expert Mission

The workflow on this branch is designed to build a report, test it across multiple Node.js versions, and then package the final results. It's currently broken in several ways.

### Step 1: Fix the Build Artifacts

The first job, `build-report`, is supposed to create a report and upload it as an artifact for the next job to use. It's failing.

1.  **Run the `build-report` job in isolation.** Use the `-j` flag to target only this job:
    ```bash
    act -j build-report
    ```
2.  **Observe the error.** The `Generate Report` step is failing because a dependency is missing.
3.  **Fix the workflow.** Edit `.github/workflows/main.yml`. You need to add a step **before** the "Generate Report" step to install the Node.js dependencies. Use an `npm install` command.
4.  **Verify your fix.** Run `act -j build-report` again. It should now succeed. Check your local file system: `act` creates an `artifacts` directory where the `test-report.json` file should now be located.

### Step 2: Debug the Matrix Job

The `run-tests` job uses a matrix strategy to test against multiple Node.js versions, but one of them is misconfigured.

1.  **Run the `run-tests` job.** This job depends on the artifact from the first job. `act` handles this automatically.
    ```bash
    act -j run-tests
    ```
2.  **Observe the error.** The matrix will expand into two jobs. You'll see the job for Node.js 18 succeeds, but the job for Node.js "latest" fails with a strange error. It seems "latest" isn't a valid version for the `actions/setup-node` action.
3.  **Fix the workflow.** In `main.yml`, change `node-version: ['18', 'latest']` to a valid LTS version, for example, `node-version: ['18', '20']`.
4.  **Verify your fix.** Run `act -j run-tests` again. Both matrix jobs should now pass successfully.

### Step 3: Simulate a Pull Request Event

The final job, `summarize-for-pr`, should **only** run on a pull request, not on a push. Your final task is to verify this logic locally.

1.  **Run `act` with the default `push` event.**
    ```bash
    act
    ```
    Observe the output. You will see that the `summarize-for-pr` job is **skipped**. This is correct behavior!

2.  **Simulate a `pull_request` event.** Now, tell `act` to run the workflow as if it were triggered by a pull request.
    ```bash
    act pull_request
    ```
3.  **Verify the outcome.** This time, the `summarize-for-pr` job should run and succeed, printing a summary message.

---

## Mission Accomplished, Expert!

You've successfully debugged a complex CI pipeline involving job dependencies, artifacts, matrix strategies, and event-based logic—all without a single commit. You are now an `act` power user!
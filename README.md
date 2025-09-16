# Warpshop Lab: The Expert Challenge

Welcome to the expert challenge! You've mastered the basics, now let's tackle a more realistic and complex workflow. Your mission is to debug a multi-stage, matrix-driven CI pipeline using advanced `act` features.

---

## Your Expert Mission

The workflow on this branch is designed to build a report, test it across multiple Node.js versions, and then package the final results. It's currently broken in several ways.

### Step 1: Fix the Build Artifacts

The first job, `build-report`, is supposed to create a report and upload it as an artifact for the next job to use. It's failing.

**Run the `build-report` job in isolation.** Use the `-j` flag to target only this job:
    ```bash
    act -j build-report --artifact-server-path /tmp/artifacts
    ```

### Step 2: Debug the Matrix Job

The `run-tests` job uses a matrix strategy to test against multiple Node.js versions, but one of them is misconfigured.

**Run the `run-tests` job.** This job depends on the artifact from the first job. `act` handles this automatically.
    ```bash
    act -j run-tests --artifact-server-path /tmp/artifacts
    ```

### Step 3: Simulate a Pull Request Event

The final job, `summarize-for-pr`, should **only** run on a pull request, not on a push. Your final task is to verify this logic locally.

1.  **Run `act` with the default `push` event.**
    ```bash
    act --artifact-server-path /tmp/artifacts
    ```
    Observe the output. You will see that the `summarize-for-pr` job is **skipped**. Try simulating a `pull_request` event.

---
## Need a Hint?

If you get stuck, check out the [`HINTS.md`](./HINTS.md) file in this repository. It contains troubleshooting tips and guidance for each step of the challenge.

---

## Mission Accomplished, Expert!

You've successfully debugged a complex CI pipeline involving job dependencies, artifacts, matrix strategies, and event-based logic, all without a single commit. You are now an `act` power user!
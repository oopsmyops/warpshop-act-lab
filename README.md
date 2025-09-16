# Warpshop: Presenter's Demo Script (`demo` branch)

This document is your script for the live demo portion of the "Beyond 'Push and Pray'" Warpshop. The workflow in this branch is fully functional and designed to demonstrate several key features of `act`.

---

## Pre-Flight Check (Do this before the session!)

1.  Make sure you are on the `demo` branch:
    ```bash
    git checkout demo
    ```
2.  Ensure **Docker Desktop is running**.
3.  **Warm up the cache:** Run `act` once to pull the necessary Docker images so you don't have to wait for a download during the live demo.
    ```bash
    # This will pull the images and run the push workflow
    act
    ```

---

## Part 1: The "Happy Path" (The `push` event)

**Goal:** Show a complex, multi-job workflow running successfully in seconds.

1.  **Run `act` with no arguments.** This simulates the default `push` event.

    ```bash
    act
    ```

2.  **Explain what's happening on screen.** Point out the following:
    *   The `build-report` job runs first.
    *   The `run-tests` job starts after `build-report` finishes.
    *   The `run-tests` job expands into a **matrix**, running two jobs in parallel for different Node.js versions.
    *   **Crucially, point out that the `summarize-for-pr` job is SKIPPED.** This is because of the `if: github.event_name != 'pull_request'` condition.

---

## Part 2: Testing Conditional Logic (The `pull_request` event)

**Goal:** Demonstrate how `act` can be used to test different event triggers and conditional job logic.

1.  **Introduce the concept of simulating events.** Say: "But what happens on a pull request? We don't have to create a real PR to find out. We can just tell `act` to simulate it."

2.  **Run `act` with the `pull_request` event.**

    ```bash
    act pull_request
    ```

3.  **Explain the new outcome.** This is a powerful moment. Point out:
    *   The `build-report` and `run-tests` jobs are now **SKIPPED**.
    *   Only the `summarize-for-pr` job runs, because its `if` condition (`github.event_name == 'pull_request'`) is now met.
    *   This shows how you can validate complex, event-driven logic without ever leaving your terminal.

---

## Part 3: The "Aha!" Moment (Breaking and Fixing)

**Goal:** Show the core value proposition: instant debugging of syntax errors.

1.  **Tell the audience:** "Now, let's see what happens when we make a mistake, which is what we *really* use this for."

2.  **Intentionally break the workflow.** Open `.github/workflows/main.yml`. Go to the `run-tests` job and **mis-indent** the `name: Download Report Artifact` step (un-indent it by two spaces).

    ```yaml
    # BEFORE (Correct)
    steps:
      - uses: actions/checkout@v4
      - name: Download Report Artifact

    # AFTER (Broken)
    steps:
      - uses: actions/checkout@v4
    - name: Download Report Artifact
    ```

3.  **Run `act` again.**

    ```bash
    act
    ```

4.  **Show the instant error.** Point to the terminal output. `act` will immediately fail with a clear YAML parsing error, often pointing to the exact line number. Say: "This is the magic. We got this error in one second. On GitHub, we would have waited five minutes just to see this."

5.  **Fix the indentation live** in your editor and save the file.

6.  **Run `act` one last time.**

    ```bash
    act
    ```

7.  **Show the immediate success.** The workflow will now run perfectly again. Conclude by saying: "And just like that, we're back to a working pipeline. That entire debug cycle took us 30 seconds instead of 10-15 minutes of pushing and waiting."

---

## Demo Complete

You have now successfully demonstrated the core pillars of using `act`:
-   Running a full CI pipeline locally.
-   Testing conditional logic based on events.
-   Instantly catching and fixing syntax errors.
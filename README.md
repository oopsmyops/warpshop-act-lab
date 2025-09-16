# Warpshop Lab: Testing GitHub Actions Locally

Welcome to the hands-on lab! Your mission is to use the `act` tool to find and fix the errors in this repository's GitHub Actions workflow, all without committing a single line of code.

## Prerequisites

Before you begin, please ensure you have the following installed and running:

1.  **Docker Desktop:** Must be running in the background. ([Install Docker](https://www.docker.com/products/docker-desktop/))
2.  **`act`:** The local runner tool. ([Install `act`](https://github.com/nektos/act#installation))

---

## Your 15-Minute Challenge

### Step 1: Clone This Repository

Open your terminal and clone this repository to your local machine:

```bash
git clone https://github.com/your-username/warpshop-act-lab.git
cd warpshop-act-lab
```

### Step 2: The Main Goal (Fix the Syntax Error)

The workflow in this repository is broken. Your first task is to find and fix the syntax error.

1.  **Run `act` for the first time.** In your terminal, at the root of the project, simply run:

    ```bash
    act
    ```

2.  **Observe the error.** You will see an immediate error message from `act` indicating that the workflow file (`.github/workflows/main.yml`) is invalid. It will likely point you to a specific line. This is the **instant feedback loop** in action!

3.  **Fix the file.** Open `.github/workflows/main.yml` in your code editor. Find the error (hint: it's a common **YAML indentation mistake**) and correct it.

4.  **Verify your fix.** Run `act` again.
    *   **Expected Outcome:** The workflow will now start running! It will likely succeed on the `build` job but fail on the `test` job's "Notify" step. This is progress! You have fixed the syntax.

### Step 3: The Bonus Challenge (Handle the Secret)

The workflow is now failing because a step requires a secret that isn't available locally.

1.  **Create a `.secrets` file.** In the root of the `warpshop-act-lab` directory, create a new file named `.secrets`.

2.  **Add the secret to the file.** Add the following line to your new `.secrets` file and save it:

    ```
    NOTIFICATION_TOKEN=this-is-my-local-secret-for-testing
    ```

3.  **Run `act` with the secrets file.** Now, tell `act` to use your secrets file with the `--secret-file` flag:

    ```bash
    act --secret-file .secrets
    ```

4.  **Observe the success!** You should now see all jobs complete successfully in your terminal. The "Notify" step will print the secret you provided.

---

## Congratulations!

You have successfully debugged a GitHub Actions workflow entirely on your local machine. You've fixed a syntax error and handled secrets, all without a single "fix ci" commit. You're no longer pushing and praying!
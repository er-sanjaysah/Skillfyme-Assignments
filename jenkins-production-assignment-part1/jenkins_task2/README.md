# Jenkins Declarative Pipeline – Task 2

This project demonstrates a **Jenkins Declarative Pipeline** with dynamic parameters and advanced error handling.

## Features

- Accepts runtime parameters:
  - **Git Branch** (default: `main`)
  - **Simulate Failure** (`true/false`)
- Includes **4 pipeline stages**:
  1. **Checkout** – Fetches code from Git and handles invalid branch errors.
  2. **Build** – Simulates build using shell command.
  3. **Test** – Fails intentionally when the boolean parameter is `true`.
  4. **Deploy** – Runs only if all previous stages succeed.

## Error Handling

- Test stage retries **2 times** before marking the build as failed.
- Post-build actions:
  - On **failure**: Simulates sending an email using `echo`.
  - On **success**: Archives build logs.

## Usage

1. Create a Jenkins Pipeline job.
2. Add the provided `Jenkinsfile` to your repository.
3. Run the job and choose parameters.
4. Observe retry logic, failure handling, and post-build actions.

---

Designed for learning Jenkins pipelines, retries, and post conditions.

## Author

**Sanjay Sah**

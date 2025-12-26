# Jenkins Multi-Job Dependency Pipeline

This project demonstrates **Jenkins Freestyle job dependencies** with parameterized deployment and manual approval.

## Job Flow

```
Job A ➜ Job B ➜ Job C
```

### Job A – Fetch Code

* Clones code from a public Git repository

### Job B – Test Simulation

* Runs after Job A
* Simulates unit tests using `echo`

### Job C – Deploy (Parameterized)

* Runs only if Job B succeeds
* Parameter: `DEPLOY_ENV`

  * `dev` → Deploying to Development
  * `stage` → Deploying to Staging
  * `prod` → Deploying to Production (Requires Approval)

## Approval Logic

* If Job B fails, Job C waits for **manual approval** before retry

---

**Author:** Sanjay Sah

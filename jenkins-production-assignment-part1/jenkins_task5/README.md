# Task 5: Real-World Edge Case Simulation

This task demonstrates how Jenkins handles **real-world failure scenarios** such as agent disconnection and pipeline timeouts.

## 1. Agent Disconnection Mid-Build

* Executed a **long-running job** using `sleep 120`
* Manually disconnected the Jenkins agent during execution

**Observed behavior:**

* The pipeline either **resumed after agent reconnection** or
* **Failed gracefully** with a clear log message

## 2. Pipeline Timeout Handling

* Configured a **global pipeline timeout of 5 minutes**
* If any stage exceeded the limit:

  * The build was **aborted automatically**
  * A failure status was recorded for notification

---

**Author:** Sanjay Sah

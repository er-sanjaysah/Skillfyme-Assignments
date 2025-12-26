# Task 4: Plugin Management & Performance Optimization

This task focuses on **Jenkins plugin troubleshooting** and **basic performance tuning** to ensure system stability and efficient resource usage.

## 1. Plugin Troubleshooting

### ThinBackup Plugin

* Installed the **ThinBackup plugin**
* Performed a **manual Jenkins backup** to safeguard configuration and job data

### Plugin Conflict Simulation

* Installed an **older version of the Git plugin** to simulate a conflict
* Resolved the issue by **upgrading to the latest Git plugin version**

## 2. Performance Tuning

### Build Cleanup

* Configured Jenkins jobs to **discard old builds**
* Retained only the **last 5 builds** to save disk space

### JVM Heap Size

* Set Jenkins JVM heap size to **2GB**
* Simulated configuration in `jenkins.xml`

### Disk Space Cleanup (Simulation)

**Commands used (Docker-based Jenkins):**

```bash
sudo su
df -h /var/lib/docker/volumes/jenkinsdir_jenkins_home/_data
rm -rf /var/lib/docker/volumes/jenkinsdir_jenkins_home/_data/workspace/*
rm -rf /var/lib/docker/volumes/jenkinsdir_jenkins_home/_data/jobs/*/builds/*
```

---

**Author:** Sanjay Sah

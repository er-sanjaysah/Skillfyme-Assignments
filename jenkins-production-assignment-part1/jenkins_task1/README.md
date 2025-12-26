# Jenkins High Availability & Security Setup

## Objective

Build a **fault-tolerant, scalable, and secure Jenkins setup** that simulates a real-world CI/CD environment with high availability, role-based access control, load distribution, and resource constraints.

This setup covers:

* High-availability Jenkins Master–Agent architecture
* Automatic agent reconnection and load balancing
* Resource-constrained agent simulation
* Secure Jenkins with RBAC and IP-based access restrictions

---

## Architecture Overview

* **Jenkins Controller (Master)**: Central orchestration node
* **Agent 1 (Normal Agent)**: Standard resources
* **Agent 2 (Constrained Agent)**: Limited to **1 CPU & 1GB RAM**
* **Security**: Matrix-based RBAC + IP restrictions

Jobs are distributed dynamically across agents based on availability.

---

## Task 1: Jenkins Infrastructure Setup

### 1. High-Availability Jenkins Master–Agent Setup

#### Agents Deployment

* Two Jenkins agents deployed on **separate containers**
* Connected using **JNLP** agents

#### Agent Auto-Reconnect

* Enabled `Keep this agent online as much as possible`
* Jenkins automatically reconnects agents if they go offline

#### Load Balancing

* Jobs use the `agent any` directive
* Jenkins scheduler distributes builds evenly across agents

#### Resource Constraint Simulation

* One agent configured with:

  * **CPU**: 1 core
  * **RAM**: 1GB
* Used to simulate real-world resource bottlenecks

Example (Docker-based constrained agent):

```bash
services:
  agent-fast:
    image: jenkins/inbound-agent
    container_name: agent-fast
    restart: unless-stopped
    environment:
      - JENKINS_URL=http://jenkins-master:8080
      - JENKINS_AGENT_NAME=agent1
      - JENKINS_SECRET=< paste here agent token Token >
    networks:
      - jenkins-net

networks:
  jenkins-net:
    external: true
```

---

## Task 2: Secure Jenkins

### 1. Role-Based Access Control (RBAC)

Enabled using **Matrix Authorization Strategy Plugin**.

#### Roles Created

| Role          | Permissions                                             |
| ------------- | ------------------------------------------------------- |
| **Admin**     | Full access (configure Jenkins, jobs, nodes, users)     |
| **Developer** | Trigger builds, view jobs & logs (no job configuration) |
| **Auditor**   | Read-only access (view jobs, logs, build history)       |

Permissions configured under:

```
Manage Jenkins → Security → Authorization → Matrix-based security
```

---

### 2. IP-Based Access Restriction

To restrict Jenkins access to trusted networks only:

#### Option 1: Reverse Proxy (Recommended)

* Jenkins placed behind **NGINX**
* Only allowed IPs can access Jenkins

Example (NGINX):

```nginx
server {
    listen 80;

    location / {
            #allow 192.168.1.0/24;   # Office network
            #allow 10.0.0.5;         # Bastion / VPN IP
        allow 10.234.250.62;
        allow 47.15.119.177;
        deny all;

        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```

#### Option 2: Firewall-Level Restriction

```bash
sudo ufw allow from 192.168.1.0/24 to any port 8080
sudo ufw deny 8080
```

---

## Validation & Testing

* ✔ Agents reconnect automatically after restart
* ✔ Jobs distributed across available agents
* ✔ Constrained agent slows builds as expected
* ✔ Developers cannot modify jobs
* ✔ Auditors have read-only access
* ✔ Jenkins accessible only from allowed IPs

---

## Edge Cases Handled

* Agent downtime & auto-recovery
* Resource exhaustion on low-capacity agent
* Unauthorized access attempts
* Accidental job modification prevention

---

## Conclusion

This Jenkins setup closely mirrors a **production-grade CI/CD environment**, demonstrating:

* High availability
* Scalability
* Security best practices
* Real-world operational constraints

---

## Author

**Sanjay Sah**


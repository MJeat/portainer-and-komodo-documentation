[Certain] Don't start with CTFd immediately. Build the management skill first, then progressively introduce **one Docker challenge → multiple challenges → full CTFd deployment**.

## 1. Portainer Roadmap

| Phase                           | What you build/test                                                  | Objective                                                                    |
| ------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **P1 — Basics**                 | Install Portainer locally + connect local Docker                     | Understand Portainer UI, Docker hosts, containers, images, volumes, networks |
| **P2 — Manual Docker**          | Create `nginx`, Ubuntu, Node containers manually                     | Learn how Portainer maps to normal Docker commands                           |
| **P3 — Container Management**   | Start/stop/restart/delete containers, inspect logs/resources         | Become comfortable managing containers without CLI                           |
| **P4 — Networks & Volumes**     | Create custom Docker network + persistent volume                     | Understand how containers communicate and store data                         |
| **P5 — Docker Compose**         | Deploy a small multi-container app with Compose                      | Learn how Portainer manages stacks                                           |
| **P6 — CTFd**                   | Deploy CTFd + database through Docker Compose                        | Build your first managed CTFd environment                                    |
| **P7 — One Challenge Server**   | CTFd + **1 Docker challenge container**                              | Understand the CTFd → challenge-container workflow                           |
| **P8 — Multiple Challenges**    | Deploy 3–5 isolated challenge containers                             | Practice managing challenge lifecycle and isolation                          |
| **P9 — Competition Simulation** | CTFd + multiple challenge containers + reset/restart testing         | Simulate an actual organizer's operational workflow                          |
| **P10 — Failure Testing**       | Kill containers, restart Docker, consume resources, recover services | Learn how to recover during a competition                                    |

**Portainer end goal:**

```text
Portainer
    │
    ├── CTFd
    ├── Database
    ├── Challenge 1
    ├── Challenge 2
    ├── Challenge 3
    └── Challenge 4
```

---

## 2. Komodo Roadmap

[Likely] Komodo should come **after Portainer**, because you'll already understand what you're trying to automate/manage.

| Phase                            | What you build/test                                          | Objective                                                           |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------- |
| **K1 — Basics**                  | Install Komodo locally                                       | Understand servers, projects, deployments and Komodo's architecture |
| **K2 — Server Management**       | Connect/manage a local Docker server                         | Learn how Komodo interacts with Docker                              |
| **K3 — Manual Deployment**       | Deploy a simple Docker container                             | Understand Komodo's basic deployment workflow                       |
| **K4 — Git Deployment**          | Connect a Git repository → deploy Docker project             | Learn Git-based deployment                                          |
| **K5 — Compose Deployment**      | Deploy a multi-container Compose application                 | Manage an application as a complete deployment                      |
| **K6 — CTFd**                    | Deploy CTFd + database through Komodo                        | Recreate your Portainer CTFd environment                            |
| **K7 — Challenge Deployment**    | Deploy one Docker challenge through Komodo                   | Understand challenge deployment through the platform                |
| **K8 — Multiple Challenges**     | Manage several challenge containers                          | Test centralized challenge management                               |
| **K9 — Update/Restart Testing**  | Change Git code → redeploy challenge                         | Simulate fixing/updating challenges during development              |
| **K10 — Competition Simulation** | Full CTFd + multiple challenges + failure/redeployment tests | Determine whether Komodo fits your real CTF operations              |

### Final comparison test

After completing both:

| Scenario                   | Portainer | Komodo       |
| -------------------------- | --------- | ------------ |
| Manually manage containers | Test      | Test         |
| Manage Docker Compose      | Test      | Test         |
| Run CTFd                   | Test      | Test         |
| Run multiple challenges    | Test      | Test         |
| Restart failed challenge   | Test      | Test         |
| Update challenge from Git  | Test      | **Key test** |
| Manage many deployments    | Test      | **Key test** |
| Competition operations     | Evaluate  | Evaluate     |

**[Certain] Your real objective isn't learning two dashboards.** It's finding out which workflow lets you **deploy, isolate, reset, monitor, and recover CTF challenges fastest during an actual competition.**
